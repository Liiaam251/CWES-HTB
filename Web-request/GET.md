#  HTTP GET, Autenticación Básica y Parámetros GET

##  1. Método GET
- Al visitar una **URL**, el navegador envía por defecto una **solicitud GET** para obtener los recursos alojados en esa URL.
- Tras recibir la página inicial, el navegador puede enviar otras solicitudes (GET, POST, etc.) para cargar imágenes, CSS, JS, etc.
- Estas solicitudes pueden observarse en la pestaña **Network/Red** de las **DevTools** (`Ctrl+Shift+I` → pestaña **Network**).

>  **Ejercicio:** visita cualquier web y revisa las solicitudes en **Network** para comprender su funcionamiento.

---

##  2. Autenticación Básica HTTP
Algunos sitios requieren credenciales para acceder a un recurso protegido (por ejemplo, `admin:admin`).  
Esta autenticación es gestionada por el **servidor web** y no por un formulario de login de la aplicación.

Ejemplo de URL protegida:
```
http://<SERVER_IP>:<PORT>/
```

Cuando accedemos sin credenciales:
```bash
curl -i http://<SERVER_IP>:<PORT>/
```
Respuesta:
```
HTTP/1.1 401 Authorization Required
WWW-Authenticate: Basic realm="Access denied"
Content-Type: text/html; charset=UTF-8

Access denied
```

 **Explicación:**  
- El **código 401** indica que se requieren credenciales.
- El encabezado `WWW-Authenticate` confirma el uso de **Basic HTTP Auth**.

---

##  3. Métodos para Autenticarse

### Opción 1: Con la bandera `-u` de cURL
```bash
curl -u admin:admin http://<SERVER_IP>:<PORT>/
```
 Devuelve la página correctamente.

---

### Opción 2: Incluyendo las credenciales en la URL
```bash
curl http://admin:admin@<SERVER_IP>:<PORT>/
```
 **No recomendado en entornos reales** por ser poco seguro (credenciales visibles en la URL).

---

### Opción 3: Usando el encabezado `Authorization`
Las credenciales se codifican en **Base64**:  
`admin:admin` → `YWRtaW46YWRtaW4=`

```bash
curl -H "Authorization: Basic YWRtaW46YWRtaW4=" http://<SERVER_IP>:<PORT>/
```
 También otorga acceso al recurso.

---

##  4. Analizando Encabezados con `-v`
Para inspeccionar la solicitud y respuesta completas:
```bash
curl -v http://admin:admin@<SERVER_IP>:<PORT>/
```
Ejemplo de parte de la solicitud:
```
> GET / HTTP/1.1
> Authorization: Basic YWRtaW46YWRtaW4=
```
- El encabezado **Authorization** se añade automáticamente cuando usamos `-u`.
- Con métodos modernos, este encabezado suele usar tokens JWT (`Bearer <token>`).

---

##  5. Parámetros GET
Una vez autenticados, la web puede tener funcionalidades (como búsqueda) que usen parámetros GET.

Ejemplo de URL con parámetro:
```
http://<SERVER_IP>:<PORT>/search.php?search=le
```
- `search=le` es el **parámetro GET** enviado en la URL.
- Podemos inspeccionar esta solicitud en la pestaña **Network** de DevTools.

 Para replicarla con cURL:
```bash
curl 'http://<SERVER_IP>:<PORT>/search.php?search=le' -H 'Authorization: Basic YWRtaW46YWRtaW4='
```
Respuesta:
```
Leeds (UK)
Leicester (UK)
```

---

##  6. Uso de DevTools
1. Abre **DevTools → Network** (`Ctrl+Shift+I` → pestaña **Red**).
2. Borra solicitudes antiguas con el icono 🗑️ antes de buscar algo.
3. Escribe el término en el buscador de la web.
4. Observa la nueva **solicitud GET** enviada (ej. `/search.php?search=le`).

 Clic derecho en la solicitud → **Copy → Copy as cURL**  
Obtendrás el comando exacto para reproducirla en terminal.

 También puedes usar **Copy as Fetch** y ejecutarlo en la **Consola de JavaScript** (`Ctrl+Shift+K`) para repetir la solicitud desde el navegador.

---

##  7. Resumen de Comandos Útiles
```bash
# Solicitud GET normal
curl http://<SERVER_IP>:<PORT>/

# Mostrar encabezados de respuesta
curl -i http://<SERVER_IP>:<PORT>/

# Autenticación básica con usuario/contraseña
curl -u admin:admin http://<SERVER_IP>:<PORT>/

# Autenticación usando el encabezado Authorization
curl -H "Authorization: Basic YWRtaW46YWRtaW4=" http://<SERVER_IP>:<PORT>/

# Solicitud GET con parámetro
curl 'http://<SERVER_IP>:<PORT>/search.php?search=le' -H 'Authorization: Basic YWRtaW46YWRtaW4='

# Ver solicitud y respuesta completas
curl -v http://admin:admin@<SERVER_IP>:<PORT>/
```
