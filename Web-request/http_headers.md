# Apuntes: Encabezados HTTP

Estos apuntes resumen el funcionamiento de los **encabezados HTTP**, sus tipos principales y los comandos prácticos para visualizarlos o configurarlos durante pruebas web y CTF.

---

##  Tipos de Encabezados HTTP

Los encabezados HTTP son pares `Nombre: Valor` que transmiten **información entre el cliente y el servidor**.  
Algunos se usan en **solicitudes (Request)**, otros en **respuestas (Response)** y otros son **comunes** a ambos.

### 1.  Encabezados Generales
Se utilizan tanto en solicitudes como en respuestas y describen **el mensaje**, no el contenido.
| Encabezado | Ejemplo | Descripción |
|------------|---------|-------------|
| **Date** | `Date: Wed, 16 Feb 2022 10:38:44 GMT` | Fecha y hora de origen del mensaje. Se recomienda UTC. |
| **Connection** | `Connection: close` | Indica si la conexión debe mantenerse abierta (`keep-alive`) o cerrarse (`close`) tras la respuesta. |

---

### 2.  Encabezados de Entidad
Describen **el contenido** transferido en el mensaje. Usados en respuestas y solicitudes `POST` o `PUT`.
| Encabezado | Ejemplo | Descripción |
|------------|---------|-------------|
| **Content-Type** | `Content-Type: text/html` | Tipo de recurso transferido. Incluye `charset=UTF-8`. |
| **Media-Type** | `Media-Type: application/pdf` | Similar a Content-Type, especifica el formato de datos. |
| **Boundary** | `boundary="b4e4fbd93540"` | Marca los límites entre partes de un mismo mensaje (p. ej. formularios). |
| **Content-Length** | `Content-Length: 385` | Tamaño (bytes) del contenido. |
| **Content-Encoding** | `Content-Encoding: gzip` | Indica transformaciones como compresión. |

---

### 3.  Encabezados de Solicitud
Enviados por el **cliente**. No describen el contenido, sino la **petición**.
| Encabezado | Ejemplo | Descripción |
|------------|---------|-------------|
| **Host** | `Host: www.inlanefreight.com` | Indica el host o IP al que se solicita el recurso. Clave para enumeración de hosts virtuales. |
| **User-Agent** | `User-Agent: curl/7.77.0` | Identifica el cliente (navegador, versión, SO). |
| **Referer** | `Referer: http://www.inlanefreight.com/` | Página de origen de la solicitud. Manipulable, cuidado con la seguridad. |
| **Accept** | `Accept: */*` | Tipos de contenido que el cliente acepta. `*/*` acepta todo. |
| **Cookie** | `Cookie: PHPSESSID=b4e4fbd93540` | Envía cookies almacenadas (sesiones, preferencias). |
| **Authorization** | `Authorization: BASIC cGFzc3dvcmQK` | Método para enviar credenciales o tokens al servidor. |

---

### 4.  Encabezados de Respuesta
Enviados por el **servidor** con la respuesta al cliente.
| Encabezado | Ejemplo | Descripción |
|------------|---------|-------------|
| **Server** | `Server: Apache/2.2.14 (Win32)` | Muestra el software y versión del servidor web. Útil para enumeración. |
| **Set-Cookie** | `Set-Cookie: PHPSESSID=b4e4fbd93540` | Envía cookies al cliente para futuras solicitudes. |
| **WWW-Authenticate** | `WWW-Authenticate: BASIC realm="localhost"` | Informa sobre el tipo de autenticación requerida. |

---

### 5.  Encabezados de Seguridad
Refuerzan la protección del sitio frente a ataques.
| Encabezado | Ejemplo | Descripción |
|------------|---------|-------------|
| **Content-Security-Policy** | `script-src 'self'` | Permite solo scripts desde dominios confiables (previene XSS). |
| **Strict-Transport-Security** | `max-age=31536000` | Fuerza el uso de HTTPS, evitando downgrade a HTTP. |
| **Referrer-Policy** | `origin` | Controla si el navegador debe enviar el `Referer`. Protege info sensible. |

---

##  Comandos Prácticos con cURL

### Ver solo los **encabezados de respuesta**
```bash
curl -I https://www.inlanefreight.com
```
> Envía una solicitud `HEAD` y muestra **solo los headers**.

---

### Ver **encabezados + cuerpo de respuesta**
```bash
curl -i https://www.inlanefreight.com
```
> Muestra encabezados y **HTML/código** devuelto por el servidor.

---

### Ver **solicitud y respuesta completa (verbose)**
```bash
curl -v https://www.inlanefreight.com
```
> Muestra todos los detalles de la transacción (útil para debugging y pentesting).

---

### Cambiar el **User-Agent**
```bash
curl https://www.inlanefreight.com -A "Mozilla/5.0"
```
> Modifica el encabezado `User-Agent`.

---

### Añadir encabezados personalizados
```bash
curl https://www.inlanefreight.com -H "Referer: https://google.com"
```
> Añade un encabezado específico a la solicitud.

---

##  DevTools del Navegador
- Abrir con **F12** o **CTRL+SHIFT+I**.
- Pestaña **Network** → muestra todas las solicitudes HTTP/HTTPS.
- Se pueden inspeccionar:
  - Método (GET, POST…)
  - Código de estado (200, 404…)
  - Encabezados de solicitud y respuesta.
- Opción **Raw** → muestra el contenido sin procesar.
- Pestaña **Cookies** → muestra cookies de cada solicitud.

---

##  Resumen Rápido
- Los encabezados son clave para entender y manipular el tráfico web.
- `curl -I` → solo headers.
- `curl -i` → headers + body.
- `curl -v` → verbose (toda la transacción).
- `-A` → cambiar `User-Agent`.
- `-H` → añadir encabezados personalizados.
- DevTools es esencial para analizar solicitudes/respuestas en aplicaciones web.
