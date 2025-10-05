# Servidores Web

## Definición
Un **servidor web** es una aplicación que se ejecuta en el **back-end** y gestiona el tráfico HTTP entre el navegador del cliente y la aplicación web. Generalmente utiliza los puertos **80/TCP (HTTP)** y **443/TCP (HTTPS)**.

## Flujo de trabajo
El servidor web:
1. Acepta solicitudes HTTP desde el cliente.
2. Procesa y enruta la solicitud al recurso correspondiente.
3. Responde al cliente con el contenido y un **código de estado HTTP**.

### Códigos de estado comunes:
- **200 OK:** Solicitud exitosa.
- **301 Moved Permanently:** El recurso fue movido permanentemente.
- **302 Found:** Redirección temporal.
- **400 Bad Request:** Sintaxis inválida en la solicitud.
- **401 Unauthorized:** Acceso no autenticado.
- **403 Forbidden:** Acceso prohibido.
- **404 Not Found:** El recurso no existe.
- **405 Method Not Allowed:** Método no permitido.
- **408 Request Timeout:** La solicitud excedió el tiempo de espera.
- **500 Internal Server Error:** Error interno del servidor.
- **502 Bad Gateway:** Respuesta inválida de una puerta de enlace.
- **504 Gateway Timeout:** Tiempo de espera de puerta de enlace agotado.

### Ejemplo con `cURL`:
- Obtener solo encabezados:
```bash
curl -I https://academy.hackthebox.com
```
- Obtener el contenido completo de la página:
```bash
curl https://academy.hackthebox.com
```

## Tipos de Servidores Web

### 1. **Apache**
- Es el servidor web más usado en internet (alrededor del **40%** de los sitios).
- Compatible con **PHP**, **Python**, **Perl**, **.NET** y scripts CGI como Bash.
- Open source, ampliamente documentado y soportado.
- Usado por empresas como **Apple**, **Adobe**, **Baidu**.
- Ideal para startups y pequeñas empresas por su facilidad de desarrollo.

### 2. **NGINX**
- Segundo más popular, aloja cerca del **30%** de los sitios web.
- Optimizado para manejar **muchas solicitudes simultáneas** con bajo consumo de memoria y CPU gracias a su arquitectura **asíncrona**.
- Elegido por sitios de alto tráfico: **Google**, **Facebook**, **Twitter**, **Cisco**, **Intel**, **Netflix**, **HackTheBox**.
- Open source y de alto rendimiento.

### 3. **IIS (Internet Information Services)**
- Tercer servidor web más popular (**~15%** de los sitios web).
- Desarrollado por **Microsoft**, optimizado para integrarse con **Windows Server** y **Active Directory**.
- Usado comúnmente para aplicaciones desarrolladas con **.NET Framework**, pero también compatible con **PHP** y servicios **FTP**.
- Ejemplos: **Microsoft**, **Office365**, **Skype**, **Stack Overflow**, **Dell**.

