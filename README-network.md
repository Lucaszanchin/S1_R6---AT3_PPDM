# Documentação: Status de Rede no Expo (expo-network)

## Sobre a biblioteca

A biblioteca **Expo Network** permite consultar informações relacionadas à conexão de rede do dispositivo em aplicações React Native utilizando Expo.

Neste projeto, ela foi utilizada para verificar o estado da conexão do dispositivo e identificar informações como o **tipo de conexão** e se o dispositivo está conectado a uma rede.

---

## Tecnologias utilizadas

* React Native
* Expo
* JavaScript
* Visual Studio Code
* Android Studio
* Expo Network

---

## 1. Instalação

Para utilizar a biblioteca no projeto, foi utilizado o comando:

```bash
npx expo install expo-network
```

---

## 2. Importação

Depois da instalação, a biblioteca foi importada utilizando:

```javascript
import * as Network from 'expo-network';
```

Dessa forma, podemos utilizar os métodos disponibilizados pela biblioteca através do objeto `Network`.

## 3. Verificando o estado da conexão

Para verificar o estado atual da conexão de rede, foi utilizado:

```javascript
const estado = await Network.getNetworkStateAsync();
```

O método retorna informações sobre a conexão atual do dispositivo.

Podemos verificar, por exemplo, se o dispositivo está conectado:

```javascript
setConectado(estado.isConnected);
```

Também podemos identificar o tipo de conexão:

```javascript
setTipoConexao(estado.type);
```

### Verificação da conexão

![Estado da conexão](./assets/Captura%20de%20tela%202026-08-31%20160425.png)

---

## 4. Identificando o tipo de conexão

A propriedade `type` informa o tipo de conexão utilizada pelo dispositivo.

Alguns exemplos são:

* Wi-Fi
* Dados móveis
* Ethernet
* Nenhuma conexão

No projeto, essa informação é utilizada para apresentar ao usuário qual é o tipo de rede utilizado.

Exemplo:

```javascript
const estado = await Network.getNetworkStateAsync();

if (estado.isConnected) {
    setTipoConexao(estado.type);
} else {
    setTipoConexao('Sem conexão');
}
```

---

## 5. Tratamento de erros

Também foi utilizado `try/catch` para tratar possíveis erros durante a consulta das informações da rede.

```javascript
try {
    const estado = await Network.getNetworkStateAsync();

    setConectado(estado.isConnected);

    if (estado.isConnected) {
        setTipoConexao(estado.type);
    } else {
        setTipoConexao('Sem conexão');
    }

} catch (error) {
    setErro('Não foi possível verificar a conexão.');
}
```

Dessa forma, caso aconteça algum problema durante a consulta, o aplicativo consegue apresentar uma mensagem para o usuário.

---

## 6. Resultado

Depois de executar o aplicativo, as informações sobre a conexão podem ser apresentadas na tela.

Exemplo:

```text
Conectado: Sim
Tipo de conexão: Wi-Fi
```

Caso não exista conexão:

```text
Conectado: Não
Tipo de conexão: Sem conexão
```

### Aplicativo funcionando

![Aplicativo funcionando](./assets/Captura%20de%20tela%202026-08-31%20153559.png)

---

## Conclusão

A biblioteca `expo-network` permite consultar informações da rede de forma simples dentro de uma aplicação Expo.

Durante a atividade, foi possível aprender como:

* Instalar a biblioteca;
* Importar o `expo-network`;
* Verificar se o dispositivo está conectado;
* Identificar o tipo de conexão;
* Utilizar funções assíncronas;
* Tratar possíveis erros;
* Apresentar as informações da rede na aplicação.
