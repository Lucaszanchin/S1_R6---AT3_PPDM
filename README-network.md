# Documentação: Status de Rede no Expo (expo-network)

Este documento detalha o uso da biblioteca expo-network para monitorar a conexão com a internet, identificando se o dispositivo está conectado via Wi-Fi ou Dados Móveis.

## 1. Instalação

A biblioteca foi adicionada ao projeto através do comando:
```bash
npx expo install expo-network
```

## 2. Sobre a Biblioteca (expo-network)

Ela permite que a aplicação identifique em tempo real se o aparelho está online, se a conexão ativa é Wi-Fi ou Dados Móveis, qual é o endereço IP atribuído e se o Modo Avião está ativado.

### Principais Recursos Utilizados no Projeto

* **Network.getNetworkStateAsync()**: Retorna um objeto contendo o estado da rede, incluindo o tipo de interface ativa (WIFI, CELLULAR, UNKNOWN) e a confirmação de alcance à internet (isInternetReachable).
* **Network.getIpAddressAsync()**: Obtém o endereço IP atual do dispositivo na rede em que está conectado.
* **Network.isAirplaneModeEnabledAsync()**: Verifica se o Modo Avião está ativado no sistema operacional (suportado em dispositivos Android).
* **Network.addNetworkStateListener()**: Registra um ouvinte (listener) em tempo real que detecta alterações na conexão (ex: quando o Wi-Fi cai ou o usuário liga o Modo Avião) e atualiza a interface automaticamente.


## 3. Código de Exemplo 
```javascript

export default function RedesWifiScreen() {
    const [info, setInfo] = useState(null);
    const [errorMsg, setErrorMsg] = useState(null);
    const [loading, setLoading] = useState(false);

    // Função assíncrona responsável por buscar as informações de rede do dispositivo
    const carregaRede = useCallback(async () => { 
        setLoading(true);
        setErrorMsg(null);

        try {
            // Obtém o estado geral da rede (conectado, Wi-Fi, dados móveis, etc.)
            const stateWifi = await Network.getNetworkStateAsync();

            let ip = "Indisponível";
            let airplane = false;

            // Busca do endereço IP com tratamento de erro isolado
            try {
                ip = await Network.getIpAddressAsync();
            } catch (error) {
                ip = "Indisponível";
            }

            // Verificação do modo avião com tratamento de erro isolado
            try {
                airplane = await Network.isAirplaneModeEnabledAsync();
            } catch (error) {
                airplane = false;
            }

            // Atualiza o estado central da tela
            setInfo({
                type: stateWifi.type ?? Network.NetworkStateType.UNKNOWN,
                isConnected: stateWifi.isConnected ?? false,
                isInternetReachable: stateWifi.isInternetReachable ?? false,
                ipAddress: ip,
                isAirplaneMode: airplane,
            });
        } catch (error) {
            // Tratamento genérico caso ocorra falha no módulo de rede
            setErrorMsg(
                "Não foi possível obter as informações da rede."
            );
        } finally {
            setLoading(false);
        }
    }, []);

    useEffect(() => {
        carregaRede();

        // Adiciona um listener para atualizar as informações automaticamente quando o status da rede mudar
        const subscription = Network.addNetworkStateListener(() => {
            carregaRede();
        });

        return () => subscription.remove();
    }, [carregaRede]);

    const isWifi = info?.type === Network.NetworkStateType.WIFI;
    const tipoLabel = info?.type ?? "Indisponível";
    const conexaoLabel = info?.isConnected ? "Conectado" : "Sem conexão";
    const internetLabel = info?.isInternetReachable ? "Disponível" : "Indisponível";
    const modoAviaoLabel = info?.isAirplaneMode ? "Ativado" : "Desativado";
}
```

## 4. Resultado Visual (Telas do App)

O aplicativo responde dinamicamente dependendo da conexão do dispositivo.

**1. Aparelho Conectado ao Wi-Fi:**
![Status Conectado](./assets/Captura%20de%20tela%202026-08-31%20153559.png)

**2. Aparelho Desconectado (Modo Avião/Offline):**
![Status Offline](./assets/Captura%20de%20tela%202026-08-31%20160425.png)