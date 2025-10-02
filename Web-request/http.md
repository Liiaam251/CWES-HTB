#  Apuntes: Protocolo HTTP y uso de cURL

##  ¿Qué es HTTP?
- **HTTP (HyperText Transfer Protocol)** es el protocolo de aplicación usado para **comunicar clientes y servidores web**.
- Permite **solicitar recursos** (páginas, imágenes, APIs, etc.) de un servidor.
- Usa normalmente el **puerto 80** (HTTP) o **443** (HTTPS).
- Es fundamental en hacking web, pentesting y CTFs, porque todo el tráfico web se basa en HTTP/S.

---

##  URL y sus Componentes
Una URL indica **dónde está el recurso** y cómo acceder a él.

Ejemplo:  
```
http://admin:password@inlanefreight.com:80/dashboard.php?login=true#status
```

| Componente   | Ejemplo                          | Descripción                                                                 |
|--------------|----------------------------------|-----------------------------------------------------------------------------|
| **Scheme**   | `http://` o `https://`           | Protocolo usado (HTTP o HTTPS).                                            |
| **User Info**| `admin:password@`                | Credenciales opcionales para autenticación.                                |
| **Host**     | `inlanefreight.com`              | Nombre de dominio o IP del servidor.                                       |
| **Port**     | `:80`                            | Puerto donde escucha el servidor (80 por defecto para HTTP, 443 para HTTPS).|
| **Path**     | `/dashboard.php`                 | Ruta o archivo solicitado en el servidor.                                  |
| **Query**    | `?login=true`                     | Parámetros enviados al servidor, separados con `&`.                        |
| **Fragment** | `#status`                        | Referencia interna dentro de la página (solo para el navegador).          |

>  Mínimo obligatorio: **Scheme + Host**

---

##  Flujo de una petición HTTP
1. El **cliente (navegador o cURL)** escribe la **URL**.
2. Se resuelve el **FQDN (dominio)** mediante **DNS** para obtener una **IP**.
   - Primero revisa `/etc/hosts`.
   - Si no existe, pregunta al servidor DNS configurado.
3. El cliente hace una **solicitud GET** al servidor web (por defecto en puerto 80/443).
4. El servidor responde con el recurso (ejemplo: `index.html`) y un **código de estado** (`200 OK`, `404 Not Found`, etc.).

---

##  Archivo `/etc/hosts`
Permite mapear manualmente dominios a IPs en tu máquina.  
Útil en CTFs para acceder a dominios internos.

Ejemplo:
```
10.10.10.5   inlanefreight.htb
```

---

##  Herramienta: cURL
**cURL (Client URL)** es una **herramienta en CLI** para enviar peticiones HTTP (y otros protocolos).

 **Uso básico:**
```bash
curl http://inlanefreight.com
```
- Descarga y muestra en la terminal el **código fuente HTML** de la página.

>  **cURL NO renderiza HTML/CSS/JS**, solo muestra el contenido crudo.

---

##  Descargar archivos con cURL
```bash
curl -O http://inlanefreight.com/index.html
```
- **`-O`**: Guarda el archivo con el **mismo nombre** que en el servidor (`index.html`).

```bash
curl -o pagina.html http://inlanefreight.com/index.html
```
- **`-o <archivo>`**: Guarda con el **nombre que elijas** (`pagina.html`).

---

##  Silenciar salida
```bash
curl -s -O http://inlanefreight.com/index.html
```
- **`-s`**: “silent mode”. Oculta la barra de progreso y mensajes de estado.

---

##  Autenticación
```bash
curl -u admin:1234 http://inlanefreight.com/secure
```
- **`-u usuario:contraseña`**: Autenticación básica HTTP.

---

##  Mostrar cabeceras HTTP
```bash
curl -i http://inlanefreight.com
```
- **`-i`**: Incluye **headers de la respuesta HTTP**.

---

##  Enviar datos (POST)
```bash
curl -d "usuario=admin&pass=123" http://inlanefreight.com/login
```
- **`-d`**: Envía **datos con POST**.

---

##  Cambiar User-Agent
```bash
curl -A "Mozilla/5.0" http://inlanefreight.com
```
- **`-A`**: Define el **User-Agent** enviado al servidor (útil para evasión y pruebas).

---

##  Verbose mode
```bash
curl -v http://inlanefreight.com
```
- **`-v`**: Muestra detalles del **handshake y cabeceras enviadas/recibidas**.

---

##  Ayuda de cURL
```bash
curl -h
curl --help all
man curl
```
- **`-h`**: Ayuda básica.
- **`--help all`**: Muestra **todas las opciones**.
- **`man curl`**: Manual completo.

---

##  Comandos clave en resumen
| Comando                               | Función                                         |
|---------------------------------------|-------------------------------------------------|
| `curl <url>`                          | Petición básica a un recurso.                   |
| `curl -O <url>`                       | Descarga con el **nombre original**.            |
| `curl -o <archivo> <url>`             | Descarga con **nombre personalizado**.          |
| `curl -s <url>`                        | Silencia barra de progreso.                     |
| `curl -u user:pass <url>`              | Autenticación HTTP básica.                      |
| `curl -i <url>`                        | Muestra cabeceras HTTP en la salida.            |
| `curl -d "a=1&b=2" <url>`              | Envía **datos con POST**.                       |
| `curl -A "<UA>" <url>`                 | Modifica el **User-Agent**.                     |
| `curl -v <url>`                        | Modo verbose, muestra detalles de la conexión.  |
| `curl --help all` / `man curl`         | Documentación completa de cURL.                 |

---

##  Tips para CTFs
- HTTP es **clave en pentesting web**: la mayoría de las vulnerabilidades viajan en las **peticiones HTTP**.
- Conocer las **URL, parámetros y cabeceras** te permite:
  - Descubrir rutas ocultas (`robots.txt`, `/admin`).
  - Probar **inyecciones SQL/XSS/LFI**.
  - Automatizar fuerza bruta y fuzzing.
- **cURL** es imprescindible para interactuar rápido con el servidor sin depender del navegador.
