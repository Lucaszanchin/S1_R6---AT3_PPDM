#  Documentação: Geolocalização no Expo (expo-location)

Este documento demonstra como foi configurada a biblioteca expo-location para capturar a posição via GPS no aplicativo, respeitando as boas práticas de desenvolvimento mobile.

## 1. Instalação e Configuração

Para instalar a biblioteca no projeto Expo, foi utilizado o seguinte comando no terminal:
```bash
npx expo install expo-location
```

## 2. Sobre a Biblioteca (expo-location)

A biblioteca **expo-location** é o módulo oficial do ecossistema Expo responsável por permitir que o aplicativo interaja com os serviços de localização nativos do dispositivo móvel (como GPS, redes Wi-Fi e antenas de telefonia).

Ela simplifica a captura de coordenadas geográficas em dispositivos Android e iOS de forma unificada, gerenciando tanto a solicitação de permissões de privacidade quanto a leitura de posição em tempo real.

### Principais Recursos Utilizados no Projeto

* **Location.requestForegroundPermissionsAsync()**: Solicita ao usuário a autorização para acessar a localização enquanto o aplicativo estiver aberto em primeiro plano. Retorna o status da permissão (granted para aceito ou denied para negado).

* **Location.getCurrentPositionAsync()**: Consulta o hardware do dispositivo e retorna um objeto contendo a posição geográfica atual com métricas como **latitude**, **longitude**, **altitude** e **nível de precisão**.

## 3. Exemplo de Código

```javascript
import react, {useEffect, useState} from "react";
import { StyleSheet, Text, TouchableOpacity, View } from "react-native";
import { SafeAreaView } from "react-native-safe-area-context";
import * as Location from 'expo-location'

export default function PosicaoGPSScreen() {

    const [location, setLocation] = useState(null);
    const [errorMsg, setErrorMsg] = useState(null);

    useEffect(() => {
        // Função assíncrona que pede a permissão para pegar a logalização e buscar suas coordenadas
        async function getCurrentLocation() {
            // Solicita a localização ao usuário 
            const { status } = await Location.requestForegroundPermissionsAsync();
            
            // Se a permissão for negada essa mensagem é mostrada
            if (status !== 'granted') {
                setErrorMsg("Permissão negada a localização!")
                return;
            }

            // Caso passe pela aprovação do usuário ele começa a busca da latitude e longitude
            const tempLocation = await Location.getCurrentPositionAsync();
            setLocation(tempLocation);
        }

        getCurrentLocation();

    }, []);

    // Mensagem que aparece quando está carregando a localização
    let text = 'Aguardando...';

    if (errorMsg) {
        text = errorMsg;
    } else if (location) {
        text = JSON.stringify(location);
    }

    return (
        <SafeAreaView style={styles.safeArea}>
            <View style={styles.container}>
                <View style={styles.header}>
                    <Text style={styles.titleScreen}>Posição Atual</Text>
                    <Text style={styles.paragraph}>{text}</Text>
                </View>
            </View>
        </SafeAreaView>
    );
}
```

## 4. Resultado Visual (Telas do App)

Abaixo estão as capturas de tela demonstrando o fluxo de funcionamento no dispositivo:

**1. Solicitação de Permissão:**
![Tela de Carregamento](./assets/Captura%20de%20tela%202026-08-31%20153520.png)

**2. Exibição das Coordenadas (Latitude e Longitude):**
![Coordenadas na Tela](./assets/Captura%20de%20tela%202026-08-31%20153547.png)