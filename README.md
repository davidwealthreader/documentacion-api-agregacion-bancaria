# 🖐️ Introducción

Bienvenido. Esta guía paso a paso te permitirá integrar el Widget de Wealth Reader en tu aplicación de forma sencilla. Sigue estos pasos y podrás comenzar a utilizar el Widget en tu aplicación en poco tiempo.

Este es un ejemplo del resultado final: [https://widget.wealthreader.com/demo-all/](https://widget.wealthreader.com/demo-all/)

El flujo es el siguiente:

Una página en el cliente invoca al widget, que muestra el listado de bancos. Uno de los parámetros proporcionado por el cliente es el operation\_id, necesario más adelante para  poder asociar la respuesta generada por el widget.

El usuario selecciona su entidad y hace login. El widget gestiona cualquier interacción con el usuario, como podría ser  informarle de un password incorrecto o resolver un desafío de doble factor, hasta conseguir obtener la información con éxito. El widget realiza una llamada a la API de Wealth Reader y con la información recuperada realiza dos acciones:

* En el backend, envía la información completa a la URL de callback previamente acordada.
* En el frontend, el widget, que está contenido en un iframe, envía un mensaje a un listener configurado en la página padre, informando de la finalización de la ejecución, para que se pueda realizar cualquier otra acción en el front, como cerrar el selector de bancos o redirigirle a una página de éxito.
