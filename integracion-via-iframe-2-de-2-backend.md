---
description: Acciones a realizar por el equipo de backend
---

# 👩‍💻 Integración vía iframe (2 de 2): backend

Esta sección debe realizarse a continuación del checklist de frontend. Los pasos son los siguientes

* [ ] Crea la URL en tu entorno donde recibirás los datos obtenidos por el widget.
* [ ] En el body de respuesta, como confirmación de que has recibido correctamente los datos, incluye

```json
{
    "status": "ok"
}
```

*   [ ] Asocia el dominio donde cargas el widget con tu URL de callback y tu api\_key:<br>

    ```bash
    # Define las variables
    API_KEY='12345678'
    DOMAIN='http://desarrollo.cliente.es' #no puede ser una IP ni incluir la palabra localhost
    URL_CALLBACK='https://desarrollo.cliente.es/hooks/wealthreader'
    #Valores posibles, ['widget','oauth']
    ACCESS_TYPE='widget'


    # Ejecuta el comando curl utilizando las variables
    curl --location "https://api.wealthreader.com/domains/" \
    --header "Content-Type: application/x-www-form-urlencoded" \
    --data-urlencode "method=add" \
    --data-urlencode "tokenize=1" \
    --data-urlencode "api_key=${API_KEY}" \
    --data-urlencode "domain=${DOMAIN}" \
    --data-urlencode "url_callback=${URL_CALLBACK}" \
    --data-urlencode "access_type=${ACCESS_TYPE}"
    ```


* [ ] Para probar el flujo completo, abre la página que carga el Widget que ha creado tu compañero de front y escribe datos reales o utiliza alguno de los usuarios mock que te hemos proporcionado en el email de bienvenida. Si no tienes este email, pídelo a support@wealthreader.com
* [ ] Existen usuarios "mock" con los que probar el flujo de datos sin necesidad de realizar una conexión real. Introduciendo MOCKDATA como usuario con cualquier contraseña, se recibirán datos de ejemplo correctos anonimizados. Existen otros usuarios mock que recrean errores, como MOCKLOGINKO. Puede pedir a nuestro soporte el listado completo.
* [ ] Hasta aquí, has completado la integración en un caso de uso de "one shot", donde no se necesitan refrescos de la información. Si tu caso de uso requiere refrescar la información, ya sea mediante un proceso batch nocturno, u ofreciendo un botón de refrescar al usuario final, necesitarás volver a llamar a la API de Wealth Reader con los datos obtenidos. Para tal fin, utiliza la siguiente documentación, prestando especial atención a la gestión de códigos de error:
  * [ ] Especificación de la API [OpenAPI V3](https://www.wealthreader.com/api-reference/es/)
  * [ ] [Colección Postman](https://www.postman.com/wealtreader/wealth-reader-api-definition/overview)

{% hint style="info" %}
Si tienes alguna dificultad al programar la URL de callback y quieres localizar la causa del problema, crear una URL de callback en un servicio como [https://pipedream.com/](https://pipedream.com/).
{% endhint %}
