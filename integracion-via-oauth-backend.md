---
description: Acciones a realizar por el equipo de backend
---

# 🧑‍💻 Integración vía Oauth: backend

Esta guía está diseñada para ayudar al equipo de backend a integrar el proceso de autenticación OAuth. A lo largo de este documento, se proporcionan pasos detallados y ejemplos de código para facilitar la implementación.

### Introducción

La integración mediante OAuth permite a los usuarios autenticarse de manera segura con Wealth Reader. Este proceso garantiza que las solicitudes provienen de un origen confiable y protege la información del usuario durante la autenticación.

## Acciones a realizar por el equipo de backend

Esta guía está diseñada para ayudar al equipo de backend a integrar el proceso de autenticación OAuth utilizando Wealth Reader. Las siguientes acciones son necesarias:

* Autorizar el dominio desde donde se iniciarán las solicitudes OAuth.
* Generar los valores `nonce`, `state`, y `code_verifier` en formato hexadecimal.
* Construir la query con los parámetros correctos y redirigir al usuario para la autenticación.
* Esperar la respuesta en la URL de callback con los parámetros `nonce` y `code`.
* Enviar una solicitud `POST` al servidor de tokens para completar el desafío OAuth y obtener el token de acceso.

***

## 1. Autorizar el Dominio indicando que el tipo de integración será oauth

El primer paso para comenzar con la integración del widget es autorizar el dominio donde se integrará. Esto garantiza que las solicitudes provienen de un origen seguro.

Puedes autorizar tu dominio mediante la API utilizando una solicitud `POST` a la siguiente URL:

```
https://api.wealthreader.com/domains/
```

A continuación, se muestra un ejemplo de cómo realizar esta solicitud usando `curl`:

```bash
curl --location 'https://api.wealthreader.com/domains/' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'method=add' \
--data-urlencode 'api_key={{apiKey}}' \
--data-urlencode 'domain=https://www.example.com/oauth/success.php' \
--data-urlencode 'access_type=oauth'
```

***

## 2. Generar valores criptográficos para el challenge y `wr_conf`

Para iniciar el desafío OAuth, primero debes generar un `challenge_code`, que será enviado como parte de la query. Este código, junto con otros parámetros, se genera en formato hexadecimal.

### Valores necesarios:

* **redirect\_uri**: La página a la que se redirige al cliente tras completar el flujo, codificada con url\_encode.
* **nonce**: Cadena aleatoria de 41 caracteres convertida a formato hexadecimal (41x2 caracteres).
* **state**: Cadena aleatoria de 41 caracteres convertida a formato hexadecimal.
* **code\_verifier**: Cadena aleatoria de 41 caracteres convertida a formato hexadecimal.
* **challenge\_code**: SHA\_256 del valor hexadecimal de code\_verifier.
* wr\_conf: Es la configuración del selector de bancos que verá el cliente. Los parámetros para configurar este json están detallados en [Integración vía iframe (1 de 2)](integracion-via-iframe-1-de-2-frontend.md).

El siguiente ejemplo muestra cómo crear una configuración JSON que posteriormente se debe convertir a formato hexadecimal para usar en la query:

```json
{
  "operation_id": "operation_id",
  "entities_to_display": [],
  "wait_full_response": false
}
```

Tras convertir este JSON a hexadecimal, ya tendrás generados todos los valores. Ver ejemplo en la siguiente sección.

Es fundamental guardar todos estos valores para poder recuperar la respuesta del banco al finalizar el flujo.

***

## 3. Construir la Query con los valores generados

Una vez que hayas generado los valores necesarios, puedes construir la query para que el cliente acceda a la página de autenticación. A continuación, un ejemplo de la query construida:

```
https://oauth.wealthreader.com/oauth2/?challenge_code=de166ee425eec5495291a70e6b934e65e7bb09849fe99e7a28a5025f9c90f788
&code_challenge_method=S256
&redirect_uri=https://example.com/oauth/success.php
&response_type=code
&state=6f5a6d34494d61434a6e714e746e45644c44777531724e596e7532464b643947703168493879397366
&nonce=7065556651485978744d7669633373686c44427532556d71506170425178724d584738693548365444
&wr_conf=7b226f7065726174696f6e5f6964223a226368616c6c656e67655f636f6465222c22656e7469746965735f746f5f646973706c6179223a5b5d2c22776169745f66756c6c5f726573706f6e7365223a66616c73657d
```

En este ejemplo, el cliente es redirigido a la página de autenticación donde el servidor de autenticación procesa la solicitud con los valores enviados. Una vez que el usuario autoriza la solicitud, el flujo continúa hacia el callback.

***

## 4. Recepción de la Solicitud GET en la URL de Callback

Después de que el usuario autoriza el acceso, el servidor de autenticación redirige al cliente hacia la URL de callback especificada (por ejemplo, `https://example.com/oauth/success.php`).

Esta solicitud GET tendrá incorporados los valores de los parámetros `nonce` y `code`, que son necesarios para completar el desafío.

```bash
https://example.com/oauth/success.php?nonce=7065556651485978744d7669633373686c44427532556d71506170425178724d584738693548365444
&code=1234567890abcdef1234567890abcdef1234567890a
```

***

## 5. Completar el Desafío OAuth

Una vez que hayas recibido los parámetros `nonce` y `code` en la URL de callback, es necesario completar el desafío realizando una solicitud POST al siguiente endpoint para obtener la respuesta del banco:

```
https://oauth.wealthreader.com/token/
```

Esta solicitud debe incluir los siguientes parámetros:

* **grant\_type**: Debe ser `authorization_code`.
* **redirect\_uri**: La misma `redirect_uri` utilizada al construir la query.
* **code**: El código recibido en la solicitud GET.
* **code\_verifier**: El `code_verifier` que generaste previamente.

### Ejemplo de solicitud POST:

```bash
curl --location --request POST 'https://oauth.wealthreader.com/token/' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=authorization_code' \
--data-urlencode 'redirect_uri=https://example.com/oauth/success.php' \
--data-urlencode 'code=1234567890abcdef1234567890abcdef1234567890a' \
--data-urlencode 'code_verifier=6e37325953774b514b4f6a553374484d67423656524a4e31584c64327030384b4b4a7452666f506d6b'
```

Si los valores son correctos, el servidor devolverá la respuesta del banco junto con un token de acceso que puedes utilizar para autenticar futuras solicitudes en nombre del usuario. La estructura de la respuesta está documentada en [Integración vía iframe (2 de 2): backend](integracion-via-iframe-2-de-2-backend.md).

***

### 6. Código de Ejemplo

Puedes descargar el código de ejemplo en distintos lenguajes para generar los parámetros necesarios y completar el desafío OAuth. El archivo contiene ejemplos en **Java**

[Descargar código de ejemplo en JAVA (ZIP)](https://drive.google.com/file/d/1nyWASQVkhFHAgTTeqq0HJe70fucg8D2E/view?usp=sharing)
