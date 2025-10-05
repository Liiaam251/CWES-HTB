# Marcos de Desarrollo y API

## Marcos de Desarrollo Web
Los **frameworks de desarrollo web** facilitan la creación de aplicaciones web modernas al proporcionar funcionalidades comunes preconstruidas, como el registro de usuarios y la integración con el frontend.

Algunos de los frameworks más populares:

- **Laravel (PHP):** Popular en startups por su potencia y facilidad de desarrollo.
- **Express (Node.js):** Usado por PayPal, Yahoo, Uber, IBM y MySpace.
- **Django (Python):** Usado por Google, YouTube, Instagram, Mozilla y Pinterest.
- **Rails (Ruby):** Usado por GitHub, Hulu, Twitch, Airbnb y anteriormente Twitter.

> Los sitios web grandes suelen usar una combinación de diferentes frameworks y servidores web.

---

## API en el Desarrollo Web
Una **API** (Interfaz de Programación de Aplicaciones) permite que los componentes **frontend** y **backend** intercambien datos y funciones.

Las APIs reciben solicitudes HTTP del frontend, las procesan en el backend y devuelven respuestas para que el frontend las renderice.

Ejemplo: una API meteorológica permite solicitar el clima de una ciudad usando una URL con el nombre o ID de la ciudad, devolviendo los datos en formato JSON.

---

## Parámetros de Consulta
Para enviar datos al backend se utilizan **parámetros HTTP**:

- **GET:** Incluye los parámetros en la URL.  
  Ejemplo: `/search.php?item=apples`
- **POST:** Envía los datos en el cuerpo de la solicitud HTTP.

Ejemplo de POST:
```http
POST /search.php HTTP/1.1
...SNIP...

item=apples
```

---

## Estándares de API

### SOAP (Simple Object Access Protocol)
- Intercambia datos usando **XML**.
- Útil para transferir datos estructurados, objetos con estado o incluso datos binarios.
- Es más complejo de usar y requiere solicitudes extensas.

Ejemplo de solicitud SOAP:
```xml
<?xml version="1.0"?>

<soap:Envelope
xmlns:soap="http://www.example.com/soap/soap/"
soap:encodingStyle="http://www.w3.org/soap/soap-encoding">

<soap:Header>
</soap:Header>

<soap:Body>
  <soap:Fault>
  </soap:Fault>
</soap:Body>

</soap:Envelope>
```

---

### REST (Representational State Transfer)
- Utiliza **rutas de URL** para enviar datos, por ejemplo: `/search/users/1`.
- Devuelve datos en **JSON**, **XML** o incluso datos sin procesar.
- Más sencillo y eficiente para consultas y acciones frecuentes.

Ejemplo de respuesta JSON:
```json
{
  "100001": {
    "date": "01-01-2021",
    "content": "Welcome to this web application."
  },
  "100002": {
    "date": "02-01-2021",
    "content": "This is the first post on this web app."
  },
  "100003": {
    "date": "02-01-2021",
    "content": "Reminder: Tomorrow is the ..."
  }
}
```

Métodos HTTP comunes en REST:
- **GET:** Obtener datos.
- **POST:** Crear nuevos datos (no idempotente).
- **PUT:** Crear o reemplazar datos existentes (idempotente).
- **DELETE:** Eliminar datos.

