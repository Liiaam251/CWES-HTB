
# Hosts virtuales (VHosts) — Apuntes con conceptos y comandos

## ¿Qué problema resuelven los VHosts?
Un servidor con **una sola IP** puede alojar **muchos sitios**. Para saber qué contenido servir en cada petición, el servidor web se guía por el **encabezado HTTP `Host`** que envía el navegador. Así, `www.ejemplo.com` y `blog.ejemplo.com` pueden convivir en la misma máquina pero mostrar **sitios totalmente distintos**.

## DNS vs. servidor web (dos capas distintas)
- **DNS (capa de nombres):** traduce `nombre → IP`. Decide **a qué servidor** llega el tráfico.
- **Servidor web (capa HTTP/HTTPS):** una vez allí, decide **qué sitio** entregar según el `Host` pedido.
- Por eso a veces **apunta todo al mismo IP** en DNS, y el **servidor web** separa sitios con VHosts.

## Concepto de `Host` header y SNI (TLS)
- En **HTTP**, el cliente envía `Host: sitio.ejemplo.com`. El servidor busca un **VirtualHost** que coincida.
- En **HTTPS**, además del `Host` header, el cliente usa **SNI (Server Name Indication)** en el handshake TLS para que el servidor seleccione el **certificado correcto**. Sin SNI, solo funcionaría bien un certificado (o habría “mismatch”).

## VHosts vs. subdominios
- **Subdominios** (DNS): entradas como `blog.ejemplo.com` que resuelven a una IP. Organización por nombre.
- **VHosts** (servidor web): bloques de configuración que asocian nombres (dominios/subdominios) a **rutas de archivos** y políticas (logs, rewrites, proxy, etc.).

## Acceso sin DNS (archivo `hosts` local)
Si no existe el registro DNS o es interno, puedes “simularlo” localmente:
```bash
# Linux/macOS
echo "203.0.113.10  intranet.ejemplo.com" | sudo tee -a /etc/hosts

# Windows (PowerShell admin)
Add-Content -Path "C:\Windows\System32\drivers\etc\hosts" -Value "203.0.113.10`t intranet.ejemplo.com"
```
Esto **no crea** un VHost; solo evita la resolución DNS, apuntando ese nombre a una IP desde tu equipo.

## ¿Qué pasa si no hay coincidencia?
Si la petición llega con un `Host` que **no coincide** con ningún VHost, el servidor puede responder con el **“default vhost”** (primero que carga) o un 404/400. Esto a veces filtra información (títulos por defecto), por eso conviene **uniformar** la respuesta del vhost por defecto.

## Ejemplos de configuración (Apache, name-based)
```apacheconf
<VirtualHost *:80>
    ServerName www.example1.com
    DocumentRoot /var/www/example1
</VirtualHost>

<VirtualHost *:80>
    ServerName www.example2.org
    DocumentRoot /var/www/example2
</VirtualHost>

<VirtualHost *:80>
    ServerName www.another-example.net
    DocumentRoot /var/www/another-example
</VirtualHost>
```
Cada bloque asocia un **nombre** a un **DocumentRoot**. Con HTTPS tendrías, además, certificados y posiblemente `ServerAlias`, redirecciones, etc.

## Tipos de virtual hosting
- **Name-based:** distingue por `Host`. Lo más usado y eficiente. Con TLS usa **SNI**.
- **IP-based:** cada sitio tiene su **IP**. Aísla mejor, pero consume direcciones.
- **Port-based:** sitios en puertos distintos (80, 8080, 8443). Requiere poner el **puerto** en la URL.

## Descubrimiento de VHosts (fuzzing del `Host`)
> Úsalo con autorización. Puede activar IDS/WAF.

**Idea:** Enviar muchas peticiones a **la misma IP** cambiando el `Host` header para ver cuáles devuelven respuestas “válidas” (códigos 200/301/302, tamaños diferentes, títulos distintos).

### Señales útiles durante el fuzzing
- **Código HTTP** distinto del de “default vhost” (ej., el default devuelve 404 y uno candidato da 200/301).
- **Tamaño** de respuesta o **título** cambiado.
- **Redirecciones** hacia rutas/lugares específicos del sitio (p. ej., `/forum`).
- Ojo con **wildcard vhosts**: algunos servidores responden “200” a cualquier nombre; filtra por tamaño/título.

## Validación manual rápida
```bash
# Solo cabeceras (código, location, server, etc.)
curl -I -H "Host: intranet.ejemplo.com" http://203.0.113.10

# Contenido completo (para ver título/HTML)
curl -H "Host: intranet.ejemplo.com" http://203.0.113.10

# Cambiando puerto (p. ej., 81)
curl -I -H "Host: intranet.ejemplo.com" http://203.0.113.10:81
```

## Fuzzing con herramientas

### 1) Gobuster (modo vhost)
```bash
# Sintaxis general
gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain

# Flags útiles
# -t <N>     -> hilos
# -k         -> ignorar errores TLS
# -o out.txt -> guardar salida
# --append-domain -> requerido en versiones nuevas para construir FQDN (host.dominio)
```

**Ejemplo (puerto 81 y SecLists):**
```bash
gobuster vhost -u http://inlanefreight.htb:81   -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt   --append-domain
```

> Nota sobre `--append-domain`: en versiones nuevas es **obligatorio** para que a cada palabra (`forum`, `dev`, …) le añada el **dominio base** (`forum.inlanefreight.htb`). En versiones antiguas se gestionaba distinto.

### 2) FFUF (fuzzing del header `Host`)
```bash
ffuf -u http://203.0.113.10/      -H "Host: FUZZ.inlanefreight.htb"      -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt      -mc 200,301,302 -fs 0
```
- `-mc` filtra por códigos (acepta 200/301/302).  
- `-fs 0` evita respuestas vacías por tamaño cero; puedes ajustar a un tamaño base para filtrar wildcard.

### 3) Nmap (NSE http-vhosts)
```bash
nmap -p80,81,443 --script http-vhosts      --script-args http-vhosts.domain=inlanefreight.htb 203.0.113.10
```
Útil para detectar vhosts conocidos/obvios y comportamiento por dominio.

### 4) Bucle bash con `curl` (rápido y manual)
```bash
for h in dev test staging admin; do
  curl -s -o /dev/null -w "%{http_code} %{size_download} %{redirect_url} -> $h\n"        -H "Host: $h.inlanefreight.htb" http://203.0.113.10
done
```
Mira cambios en **código**, **tamaño** o **redirección** por cada candidato.

## Buenas prácticas (defensa)
- Configurar un **default vhost** neutro (mismo código/longitud) para no delatar nombres válidos.
- Usar **SNI/certificados** correctos y evitar mensajes de error reveladores.
- Registrar y alertar por **Host** anómalos, límites de tasa, WAF/ratelimiting.
- Separar sitios sensibles (admin) con **autenticación** y **segmentación**.

---
## Resumen de **comandos** y para qué sirven

1. `echo "IP dominio" | sudo tee -a /etc/hosts`  
   Simula resolución DNS en tu equipo (nombre → IP).

2. `Add-Content ... hosts` (Windows)  
   Edita el archivo `hosts` en Windows para el mismo fin.

3. `curl -I -H "Host: <vhost>" http://IP[:puerto]`  
   Verifica rápidamente si existe un VHost (códigos/headers).

4. `curl -H "Host: <vhost>" http://IP[:puerto]`  
   Obtiene el HTML para ver títulos y diferencias de contenido.

5. `gobuster vhost -u http://IP[:puerto] -w <wordlist> --append-domain [-t N] [-k] [-o out.txt]`  
   Fuzzing automatizado del header `Host` para descubrir VHosts válidos.

6. `ffuf -u http://IP/ -H "Host: FUZZ.<dominio>" -w <wordlist> -mc 200,301,302 -fs 0`  
   Fuzzing flexible del `Host` con filtros por código/tamaño para evitar “wildcards”.

7. `nmap -p80,81,443 --script http-vhosts --script-args http-vhosts.domain=<dominio> <IP>`  
   Enumeración de VHosts usando NSE de Nmap (apoya reconocimiento inicial).

8. Bucle bash con `curl` para candidatos (`dev`, `test`, `staging`, `admin`)  
   Prueba manual rápida de varios nombres con el header `Host`.
