---
description: Acciones a realizar por el equipo de backend
---

# 👩‍💻 Integración vía iframe (2 de 2): backend

Esta sección debe realizarse después de completar del checklist de frontend. Los pasos son los siguientes

*   [ ] **Crear la URL de callback**\
    Crea en tu entorno una URL de callback donde Wealthreader enviará los datos obtenidos a través del widget.

    Esta URL debe poder recibir peticiones `POST` con un cuerpo en formato JSON.

    Como confirmación de que los datos se han recibido correctamente, tu endpoint debe responder con HTTP `200` y el siguiente cuerpo:

```json
{
    "status": "ok"
}
```

* [ ] **Asociar el dominio, la URL de callback y la API key**

Desde el área de clientes de Wealth Reader, asocia:

* el dominio desde el que se cargará el widget;
* la URL de callback donde recibirás los datos;
* tu `api_key`.

Puedes hacerlo desde: [https://www.wealthreader.com/clients/](https://www.wealthreader.com/clients/)

* [ ] Para probar el flujo completo, abre la página que carga el Widget que ha creado tu compañero de front y escribe datos reales o utiliza alguno de los usuarios mock que te hemos proporcionado en el email de bienvenida. Si no tienes este email, pídelo a support@wealthreader.com
* [ ] Existen usuarios "mock" con los que probar el flujo de datos sin necesidad de realizar una conexión real. Introduciendo MOCKDATA como usuario con cualquier contraseña, se recibirán datos de ejemplo correctos anonimizados. Existen otros usuarios mock que recrean errores, como MOCKLOGINKO. Puede pedir a nuestro soporte el listado completo.
* [ ] Hasta aquí, has completado la integración en un caso de uso de "one shot", donde no se necesitan refrescos de la información. Si tu caso de uso requiere refrescar la información, ya sea mediante un proceso batch nocturno, u ofreciendo un botón de refrescar al usuario final, necesitarás volver a llamar a la API de Wealth Reader con los datos obtenidos. Para tal fin, utiliza la siguiente documentación, prestando especial atención a la gestión de códigos de error:
  * [ ] Especificación de la API [OpenAPI V3](https://www.wealthreader.com/api-reference/es/)
  * [ ] [Colección Postman](https://www.postman.com/wealtreader/wealth-reader-api-definition/overview)

{% hint style="info" %}
Si tienes alguna dificultad al programar la URL de callback y quieres localizar la causa del problema, crear una URL de callback en un servicio como [https://pipedream.com/](https://pipedream.com/).
{% endhint %}
