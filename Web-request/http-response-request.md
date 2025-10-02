#  Solicitudes y Respuestas HTTP

##  Concepto General
Las comunicaciones **HTTP** constan de dos partes principales:
- **Solicitud HTTP (Request)**: enviada por el **cliente** (por ejemplo, navegador o `cURL`).
- **Respuesta HTTP (Response)**: enviada por el **servidor** (por ejemplo, Apache, Nginx).

El cliente especifica el **recurso** (URL, ruta, parámetros, encabezados, etc.) que desea, y el servidor procesa la solicitud y devuelve una **respuesta HTTP** con los datos y un **código de estado**.

---

##  Solicitud HTTP
Ejemplo básico de **solicitud HTTP GET**:

```
GET /users/login.html HTTP/1.1
Host: inlanefreight.com
User-Agent: Mozilla/5.0
Cookie: PHPSESSID=c4ggt4jull9obt7aupa55o8vbf
```

### Componentes principales:
| **Campo**  | **Ejemplo**                     | **Descripción**                               |
|------------|----------------------------------|-----------------------------------------------|
| `Method`   | `GET`                             | Verbo HTTP que indica la acción a realizar.   |
| `Path`     | `/users/login.html`               | Ruta del recurso al que se accede.            |
| `Version`  | `HTTP/1.1`                         | Versión del protocolo HTTP.                   |
| **Headers**| `Host`, `User-Agent`, `Cookie`    | Pares clave-valor que contienen información adicional sobre la solicitud. |
| **Body**   | (opcional)                        | Contiene los datos que se envían (por ejemplo, formularios en `POST`).      |

 En **HTTP/1.x** las solicitudes se envían como **texto plano** con saltos de línea.
En **HTTP/2.x** se envían como **datos binarios** más eficientes.

---

##  Respuesta HTTP
Ejemplo de **respuesta HTTP**:

```
HTTP/1.1 200 OK
Date: Tue, 21 Jul 2020 05:20:15 GMT
Server: Apache/2.4.41 (Ubuntu)
Set-Cookie: PHPSESSID=m4u64rqlpfthrvvb12ai9voqgf
Content-Type: text/html; charset=UTF-8

<!DOCTYPE html>
<html> ... </html>
```

###  Componentes principales:
- **Versión:** `HTTP/1.1`
- **Código de estado:** `200 OK` → indica que la solicitud fue exitosa.
- **Headers de respuesta:** Información del servidor, cookies, tipo de contenido, etc.
- **Cuerpo de respuesta:** Suele ser el **HTML** de la página, pero puede incluir JSON, imágenes, PDFs, etc.

---

##  cURL — Inspección de Solicitudes y Respuestas

El comando `curl` permite enviar solicitudes HTTP y ver la respuesta.  
Por defecto muestra solo el **cuerpo de la respuesta**, pero con banderas podemos inspeccionar más detalles:

###  Comandos útiles de cURL
```bash
# Enviar solicitud GET básica
curl http://inlanefreight.com

# Mostrar solicitud y respuesta completas (verbose)
curl -v http://inlanefreight.com

# Verbose más detallado
curl -vvv http://inlanefreight.com

# Guardar el contenido recibido en un archivo con el nombre remoto
curl -O http://inlanefreight.com/index.html

# Guardar el contenido en un archivo específico
curl -o pagina.html http://inlanefreight.com

# Descargar en modo silencioso (sin mostrar progreso)
curl -s -O http://inlanefreight.com/index.html

# Incluir encabezados de respuesta en la salida
curl -i http://inlanefreight.com
```

###  Ejemplo `curl -v`
```bash
curl http://inlanefreight.com -v
```
Salida abreviada:
```
> GET / HTTP/1.1
> Host: inlanefreight.com
> User-Agent: curl/7.65.3
> Accept: */*
< HTTP/1.1 401 Unauthorized
< Server: Apache/X.Y.ZZ (Ubuntu)
< Content-Type: text/html; charset=iso-8859-1
```

 La bandera **`-v`** muestra **solicitud completa** y **respuesta completa**, ideal para análisis de pruebas de penetración.  
 La bandera **`-vvv`** ofrece aún **más detalle técnico**.

---

##  Herramientas de Desarrollo del Navegador (DevTools)
Los navegadores como **Chrome** y **Firefox** incluyen **DevTools**, útiles para analizar solicitudes/respuestas.

- Para abrir: `Ctrl + Shift + I` o `F12` → pestaña **Network**.
- Al recargar la página, se ven todas las solicitudes enviadas.
- Se puede ver:
  - Método (`GET`, `POST`, etc.)
  - Código de estado (`200`, `404`, etc.)
  - URL solicitada y rutas.
  - Filtrar por URLs con **Filter URLs**.
  - Ver **cuerpo de respuesta** y **código fuente crudo** con la pestaña **Response → Raw**.

 Esto permite inspeccionar cómo el navegador interactúa con el servidor, útil para pruebas de seguridad y debugging.

---

##  Resumen
- **HTTP Request** → contiene Método, Ruta, Versión, Headers y opcionalmente Body.
- **HTTP Response** → contiene Código de estado, Headers y opcionalmente Body (HTML, JSON, etc.).
- `curl -v` es clave para ver la comunicación completa.
- **DevTools** facilita inspeccionar solicitudes y respuestas desde el navegador.
