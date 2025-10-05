# Interceptar solicitudes con Burp Suite

## 1. Activar el Proxy
- Abre **Burp Suite → Proxy → Intercept**.
- Verifica que el botón **“Intercept is on”** esté activado.
- Configura el navegador para usar el proxy de Burp:
  - Host: `127.0.0.1`
  - Puerto: `8080` (por defecto).

## 2. Capturar solicitudes
- Una vez activado el proxy, cualquier **solicitud HTTP/HTTPS** que realice el navegador será capturada.
- En la pestaña **“Proxy → HTTP history”** podrás ver un registro de todas las solicitudes que pasan por el proxy.

## 3. Editar solicitudes
- Las solicitudes interceptadas aparecen en **“Intercept”** antes de enviarse al servidor.
- Se puede **modificar cualquier parte de la solicitud**:
  - **Línea de solicitud:** método (`GET`, `POST`, etc.) y URL.
  - **Cabeceras:** como `Host`, `User-Agent`, `Cookie`.
  - **Cuerpo:** datos enviados en formularios o JSON.
- Tras editar, presiona **“Forward”** para enviarla al servidor o **“Drop”** para descartarla.

## 4. Filtros de interceptación
- En **“Proxy → Options → Intercept Client Requests”** puedes establecer reglas para interceptar solo ciertas solicitudes, por ejemplo:
  - Tipo de contenido.
  - Métodos HTTP específicos.
  - Palabras clave en la URL.
  - Rutas concretas de la aplicación web.

## 5. Uso típico
- **Probar validaciones:** Cambiar parámetros para verificar el manejo de datos en el servidor.
- **Detectar vulnerabilidades:** SQLi, XSS, CSRF, etc.
- **Forzar errores:** Modificar cabeceras o métodos HTTP.

## 6. Buenas prácticas
- Realizar estas pruebas únicamente en sistemas autorizados.
- Guardar siempre copias de las solicitudes originales.
- Usar los filtros para reducir el ruido y concentrarse en las solicitudes relevantes.
