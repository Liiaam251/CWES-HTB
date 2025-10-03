# Apuntes: Solicitudes HTTP POST y Cookies

## 1. Introducción a POST
- **POST** se usa cuando se requiere transferir archivos o enviar datos fuera de la URL.
- Diferencias clave respecto a **GET**:
  - **No registra en URL:** útil para cargas de archivos grandes.
  - **Menos restricciones de codificación:** permite datos binarios en el cuerpo.
  - **Mayor cantidad de datos:** las URLs suelen estar limitadas a ~2000 caracteres.

## 2. Formularios de Inicio de Sesión
- POST envía credenciales dentro del **cuerpo** de la solicitud.
- Ejemplo de datos enviados en un login:
  ```bash
  username=admin&password=admin
  ```
- Con `cURL`:
  ```bash
  curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/
  ```
- Usar `-L` para seguir redirecciones:
  ```bash
  curl -X POST -d 'username=admin&password=admin' -L http://<SERVER_IP>:<PORT>/
  ```

## 3. Cookies Autenticadas
- Tras iniciar sesión, el servidor devuelve una **cookie** con el encabezado `Set-Cookie`.
- Permite mantener la sesión sin reautenticación.
- Para usarla con `cURL`:
  ```bash
  curl -b 'PHPSESSID=<COOKIE_VALUE>' http://<SERVER_IP>:<PORT>/
  ```
- También se puede usar el encabezado `Cookie`:
  ```bash
  curl -H 'Cookie: PHPSESSID=<COOKIE_VALUE>' http://<SERVER_IP>:<PORT>/
  ```
- En el navegador, se pueden ver y modificar cookies desde la pestaña **Storage → Cookies**.

## 4. Datos JSON en POST
- Algunas funciones, como **City Search**, usan datos JSON en el cuerpo de la solicitud.
- Ejemplo de carga útil:
  ```json
  {"search":"london"}
  ```
- Con `cURL`, incluyendo **Content-Type** y **cookie** autenticada:
  ```bash
  curl -X POST -d '{"search":"london"}'   -b 'PHPSESSID=<COOKIE_VALUE>'   -H 'Content-Type: application/json'   http://<SERVER_IP>:<PORT>/search.php
  ```
- La respuesta puede ser en formato JSON:
  ```json
  ["London (UK)"]
  ```

## 5. Consejos
- Usar **Copy → Copy as cURL** o **Copy as Fetch** desde las herramientas de desarrollo para replicar solicitudes.
- Borrar solicitudes previas en la pestaña **Network** para analizar solo las relevantes.
- Probar solicitudes sin cookies o sin encabezados para ver diferencias en el comportamiento.

## 6. Uso de Fetch en Navegador
- Desde la pestaña **Console** en DevTools, se pueden enviar solicitudes con `fetch()`:
  ```javascript
  fetch("http://<SERVER_IP>:<PORT>/search.php", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Cookie": "PHPSESSID=<COOKIE_VALUE>"
    },
    body: JSON.stringify({"search":"london"})
  })
  .then(response => response.json())
  .then(data => console.log(data));
  ```
