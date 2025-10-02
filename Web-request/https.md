# Apuntes: Protocolo seguro de transferencia de hipertexto (HTTPS)

## 1. ¿Qué es HTTPS?
- **HTTPS (HyperText Transfer Protocol Secure)** es la versión cifrada de HTTP.
- Usa **TLS/SSL** para cifrar el tráfico entre cliente (navegador, curl) y servidor.
- Protege los datos contra ataques **Man-in-the-Middle (MiTM)**.
- Puerto por defecto: **443**.
- Los sitios HTTPS se identifican con `https://` en la URL y el **candado** en el navegador.

 **Diferencia con HTTP:**
- HTTP (puerto 80) envía los datos en **texto plano** (cualquier persona en la red puede verlos).
- HTTPS (puerto 443) envía los datos **cifrados**, haciendo imposible leerlos sin la clave.

---

## 2. Flujo de una conexión HTTPS
1. El cliente solicita la web (`http://ejemplo.com`).
2. El servidor redirige al **puerto 443** usando código **301 Moved Permanently**.
3. Se realiza el **handshake TLS**:
   - El cliente envía **Client Hello** (detalles de cifrado compatible).
   - El servidor responde con **Server Hello** (incluye su certificado SSL).
   - Intercambio de claves y verificación de certificados.
4. Se establece el canal cifrado y comienza el **tráfico HTTP normal pero cifrado**.

 HTTPS evita que terceros lean contraseñas, cookies, etc., durante el transporte.

---

## 3. Riesgos y ataques
- **Degradación HTTPS → HTTP:** Un atacante fuerza que la conexión pase a HTTP (texto plano).
- **Certificados no válidos:** Si el certificado no está verificado por una CA confiable, el navegador o `curl` bloquean la conexión.
- Se recomienda usar **DNS cifrado (DoH/DoT)** o una **VPN** para evitar fugas de las URL visitadas.

---

## 4. Uso de cURL con HTTPS

### 4.1. Enviar solicitud HTTPS básica
```bash
curl https://inlanefreight.com
```
- `curl` maneja automáticamente el **handshake TLS**.
- Si el certificado SSL es válido, la solicitud se completa y devuelve el HTML.

---

### 4.2. Omitir validación de certificado SSL
Cuando el certificado es inválido o caducado (por ejemplo, en entornos locales):
```bash
curl -k https://inlanefreight.com
```
- La bandera **`-k`** permite ignorar los errores de validación de certificados SSL.
- Úsalo solo en entornos de práctica, **no en producción** (riesgo MiTM).

---

### 4.3. Descargar un recurso HTTPS
```bash
curl -O https://inlanefreight.com/index.html
```
- **`-O`** descarga el archivo usando el mismo nombre del archivo remoto.

---

### 4.4. Silenciar el progreso
```bash
curl -s -O https://inlanefreight.com/index.html
```
- **`-s`** oculta la barra de progreso y mensajes de estado.

---

### 4.5. Ver ayuda completa de cURL
```bash
curl -h
```
o
```bash
curl --help all
```
- Muestra todas las banderas disponibles.
- Para ayuda específica sobre HTTPS:  
  ```bash
  curl --help http
  ```

---

## 5. Comandos importantes resumidos
| Comando | Descripción |
|---------|-------------|
| `curl https://URL` | Solicitud HTTPS estándar. |
| `curl -k https://URL` | Ignorar error de certificado SSL. |
| `curl -O https://URL/archivo` | Descarga el archivo remoto manteniendo su nombre. |
| `curl -s -O https://URL/archivo` | Descarga en modo silencioso. |
| `curl -h` | Ayuda básica de cURL. |
| `curl --help all` | Lista completa de opciones de cURL. |

---

## 6. Puntos clave para CTF
- HTTPS protege datos sensibles, pero aún se puede inspeccionar el **handshake** y las redirecciones.
- El **código 301** indica redirección de HTTP a HTTPS.
- Útil probar conexiones con `curl` y detectar errores de certificados con `-k`.
- Wireshark en HTTP puede mostrar contraseñas en texto plano; en HTTPS, el tráfico aparece **cifrado**.
