---
description: Acciones a realizar por el equipo de frontend
---

# 👨‍💻 Integración vía iframe (1 de 2): frontend

El widget consiste en un contenedor que se carga mediante una llamada a un JavaScript. Realiza este checklist para integrar la parte de front:

* [ ] Si tú no has sido la persona encargada de realizar el alta en Wealth Reader, asegúrate con el responsable de que ha recibido el email confirmando el alta. En este paso previo habréis elegido alguna de las opciones de funcionamiento del widget, como la URL de callback a la que el widget enviará los datos, entre otras opciones.
* [ ] En el lugar donde quieres que aparezca el selector de entidades inserta este código:

```javascript
 <script>
   //operation_id: es un id generado por vosotros que os volverá en la respuesta al callback
   //entities_to_display: array de entidades a mostrar. Si es array vacío se muestran todas las entidades. Para recuperar todos los códigos de entidades: https://api.wealthreader.com/entities/
    const wr_conf = {
        "operation_id": "5f5b5c5c-5f5b-5c5c-5f5b-5c5c5f5b5c5c",
        "entities_to_display": ["andbank", "bancamarch", "bankinter", "bbva", "caixabank", "degiro", "ing", "santander_emp", "triodos"],
        "wait_full_response": false
    }
 
    window.addEventListener("message", (e) => {
        var data = e.data;

        if (data === "flow completed") {
            console.log("El widget ha notificado que el flujo ha finalizado");
            //Aquí puedes escribir el código con lo que debe suceder cuando el flujo ha finalizado
        }
    });
</script>

<iframe id="wr-iframe" title="Wealth Reader widget" width="100%" frameBorder="0" referrerpolicy="origin"></iframe>
<script src="https://widget.wealthreader.com/js/load.js"></script>
```

<table><thead><tr><th width="198">Parámetro</th><th width="116" data-type="checkbox">Requerido</th><th>Descripción</th></tr></thead><tbody><tr><td>operation_id</td><td>true</td><td>Id de operación asignado generado por el cliente. Este id se enviará junto con la respuesta a la URL de callback previamente acordada.</td></tr><tr><td>entities_to_display</td><td>false</td><td>Entidades a mostrar en el widget. Si no se especifica ningún valor se muestran todas.<br><br>Puedes obtener el listado completo de entidades en: <a href="https://api.wealthreader.com/entities/">https://api.wealthreader.com/entities/</a> Este método y el resto están descritos en la definición OpenAPI, al final del documento. </td></tr><tr><td>wait_full_response</td><td>false</td><td><p>Si es true, envía la información completa: activos y sus transacciones. En caso contrario el widget solamente enviará el listado de productos, pero no las transacciones.</p><p></p><p>La razón para querer desactivar esta opción es dar al usuario la sensación de inmediatez, pero no recomendamos desactivarlo porque el tiempo de espera de recuperar la información completa también es corto, del orden de segundos.</p><p></p><p> El valor por defecto es true.</p></td></tr><tr><td>date_from</td><td>false</td><td><p>Fecha desde la que obtener transacciones en formato AAAA-MM-DD.</p><p></p><p>Ejemplo: 2024-01-15.</p><p></p><p>Este parámetro solo aplica cuando wait_full_response está activado.</p><p><br>Importante: Para rangos superiores a 89 días en bancos europeos, el banco puede solicitar autenticación adicional (doble factor) que el usuario deberá completar directamente en el widget. En estos casos, el proceso puede tardar varios minutos debido al mayor número de consultas necesarias para recuperar todo el historial solicitado.</p></td></tr><tr><td>product_types</td><td>false</td><td><p>Permite filtrar el tipo de productos a recuperar.</p><p></p><p>Acepta una lista de tipos de producto separados por coma, de las siguientes opciones:</p><ul><li>accounts: cuentas corrientes</li><li>portfolios: carteras de inversión</li><li>cards: tarjetas</li><li>receipts: recibos</li><li>loans: préstamos</li><li>factoring: factoring</li><li>confirming: confirming</li><li>properties: propiedades</li><li>invoices: facturas</li><li>files: archivos (Norma 43, 19...)</li></ul><p></p><p>Ejemplo: accounts,cards,loans</p><p><br>Si no se especifica, se recuperan todos los tipos asociados a su api_key.</p></td></tr><tr><td>default_login</td><td>false</td><td>Si tiene un valor de entity_code válido, se abrirá directamente el formulario para la recogida de consentimiento de esa entidad. El listado completo de entidades se puede obtener en <a href="https://api.wealthreader.com/entities/">https://api.wealthreader.com/entities/</a></td></tr><tr><td>default_login_entity_country</td><td>false</td><td>Necesario solamente cuando default_login tiene un valor</td></tr><tr><td>token</td><td>false</td><td>Se trata de un valor opcional. Al proporcionar un valor válido, el widget preselecciona el banco asociado al token y solicita al usuario que inicie sesión. Esto tiene utilidad cuando un token ha sido invalidado debido a un cambio de contraseña o por un desafío de autenticación de doble factor.</td></tr><tr><td>psd2</td><td>false</td><td>Valor por defecto es true. Indica si debe mostrar en el listado las entidades soportadas por PSD2. El parámetro se toma en cuenta solamente cuando entities_to_display no tiene ningún valor.</td></tr><tr><td>nonpsd2</td><td>false</td><td>Valor por defecto es true. Indica si debe mostrar en el listado las entidades soportadas por canal alternativo a PSD2, con información más completa. El parámetro se toma en cuenta solamente cuando entities_to_display no tiene ningún valor.</td></tr></tbody></table>

* [ ] Abre tu aplicación. Si todo es correcto verás el selector de bancos. En caso contrario, utiliza cualquiera de las tres formas de soporte que ofrecemos.
