# Documentação: Geolocalização no Expo (expo-location)

## Sobre a biblioteca

A biblioteca **Expo Location** permite acessar informações relacionadas à localização do dispositivo em aplicações desenvolvidas com React Native e Expo.

Neste projeto, a biblioteca foi utilizada para obter a localização atual do dispositivo, principalmente informações como **latitude, longitude e altitude**.

---

## Tecnologias utilizadas

* React Native
* Expo
* JavaScript
* Visual Studio Code
* Android Studio
* Expo Location

---

## 1. Instalação

Primeiramente, foi necessário instalar a biblioteca `expo-location` no projeto.

Para realizar a instalação, foi utilizado o comando:

```bash
npx expo install expo-location
```

## 2. Importação

Depois da instalação, a biblioteca foi importada no arquivo responsável pela tela de localização.

```javascript
import * as Location from 'expo-location';
```

A utilização de `* as Location` permite acessar os recursos disponibilizados pela biblioteca através do objeto `Location`.

## 3. Solicitação de permissão

Antes de obter a localização do dispositivo, é necessário solicitar permissão ao usuário.

Foi utilizado o método:

```javascript
const { status } = await Location.requestForegroundPermissionsAsync();
```

Depois da solicitação, o código verifica se a permissão foi concedida.

```javascript
if (status !== 'granted') {
    setErro('Permissão de localização negada.');
    return;
}
```

Caso o usuário não permita o acesso à localização, o programa interrompe a execução e apresenta uma mensagem de erro.

### Solicitação de permissão

![Permissão de localização](../assets/gps-03.png)

---

## 4. Obtendo a localização

Depois que a permissão é concedida, podemos obter a localização atual do dispositivo.

```javascript
const location = await Location.getCurrentPositionAsync({});
```

O resultado possui informações dentro de `location.coords`, como:

* Latitude
* Longitude
* Altitude
* Velocidade
* Precisão

Neste projeto, essas informações são utilizadas para apresentar os dados da localização na tela.

Exemplo:

```javascript
setLatitude(location.coords.latitude);
setLongitude(location.coords.longitude);
setAltitude(location.coords.altitude);
```

## 5. Tratamento de erros

Foi utilizado tratamento de erros para evitar que o aplicativo seja encerrado caso ocorra algum problema ao tentar obter a localização.

```javascript
try {
    const { status } =
        await Location.requestForegroundPermissionsAsync();

    if (status !== 'granted') {
        setErro('Permissão de localização negada.');
        return;
    }

    const location =
        await Location.getCurrentPositionAsync({});

    setLatitude(location.coords.latitude);
    setLongitude(location.coords.longitude);
    setAltitude(location.coords.altitude);

} catch (error) {
    setErro('Não foi possível obter a localização.');
}
```

O `try/catch` permite identificar possíveis erros durante a execução e apresentar uma mensagem para o usuário.

## 6. Resultado

Após permitir o acesso à localização, o aplicativo consegue obter os dados do dispositivo e apresentar as informações na tela.

Exemplo das informações apresentadas:

```text
Latitude: -22.XXXXXX
Longitude: -47.XXXXXX
Altitude: XX metros
```

### 📸 Aplicativo funcionando

![Aplicativo funcionando](./assets/Captura%20de%20tela%202026-08-31%20153547.png);

---

## ✅ Conclusão

A biblioteca `expo-location` facilita o acesso aos recursos de localização em aplicações React Native desenvolvidas com Expo.

Durante a atividade, foi possível aprender como:

* Instalar a biblioteca;
* Importar o `expo-location`;
* Solicitar permissão de localização;
* Obter latitude, longitude e altitude;
* Utilizar funções assíncronas;
* Tratar erros e permissões negadas;
* Apresentar as informações na tela do aplicativo.
