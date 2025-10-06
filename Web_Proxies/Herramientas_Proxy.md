# Herramientas de Proxy

Un aspecto importante del uso de proxies web es la posibilidad de interceptar las solicitudes web realizadas por herramientas de línea de comandos y aplicaciones de cliente pesado. Esto nos proporciona transparencia sobre las solicitudes web realizadas por estas aplicaciones y nos permite utilizar todas las funciones de proxy que hemos utilizado con las aplicaciones web.

## Uso de Proxies Web
- Los proxies web se configuran como intermediarios entre una aplicación/herramienta y el servidor back-end.
- Permiten capturar y analizar solicitudes HTTP de aplicaciones que no son navegadores.
- Ejemplos: Burp Suite o ZAP.

**Nota**: El uso de proxies puede ralentizar las herramientas, por lo que deben usarse principalmente para investigación y pruebas.

## Proxychains
Proxychains es una herramienta en Linux que enruta el tráfico de cualquier herramienta de línea de comandos hacia un proxy especificado.

1. Editar `/etc/proxychains.conf`
2. Comentar la última línea y añadir:
   ```
   #socks4         127.0.0.1 9050
   http 127.0.0.1 8080
   ```
3. Usar la opción `-q` para modo silencioso.
4. Ejemplo con `curl`:
   ```bash
   proxychains -q curl http://SERVER_IP:PORT
   ```

En Burp Suite, podremos ver la solicitud interceptada en el panel de Proxy.

## Metasploit
Podemos enrutar el tráfico de módulos de Metasploit por el proxy:

1. Iniciar msfconsole:
   ```bash
   msfconsole
   ```
2. Seleccionar módulo:
   ```bash
   use auxiliary/scanner/http/robots_txt
   ```
3. Configurar proxy:
   ```bash
   set PROXIES HTTP:127.0.0.1:8080
   ```
4. Establecer host y puerto:
   ```bash
   set RHOST SERVER_IP
   set RPORT PORT
   ```
5. Ejecutar:
   ```bash
   run
   ```

La solicitud pasará por el proxy y podrá ser analizada en Burp o ZAP.
