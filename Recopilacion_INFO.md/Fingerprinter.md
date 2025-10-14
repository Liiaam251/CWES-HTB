
# Toma de huellas dactilares (Web Fingerprinting) — Apuntes con conceptos y comandos

## ¿Qué es y por qué importa?
La **toma de huellas dactilares** identifica tecnologías y versiones detrás de un sitio o aplicación web (servidor web, CMS, frameworks, OS, WAF, TLS, etc.).
- **Ataques dirigidos:** permite elegir exploits y técnicas acordes a la tecnología real.
- **Detectar misconfiguraciones/obsolescencia:** banners, headers y respuestas anómalas delatan software desactualizado o por defecto.
- **Priorizar objetivos:** centrar esfuerzos en lo más vulnerable o valioso.
- **Perfil completo:** combinada con otras fases de recon (DNS, subdominios, vhosts) ofrece una visión holística de la superficie de ataque.

> Ética/legal: realiza estas técnicas **solo con autorización**. Un escaneo sin permiso puede ser ilegal.

## Técnicas principales
1) **Banner grabbing:** leer cabeceras y mensajes iniciales de servicios (HTTP, TLS) para extraer nombre/versión.
2) **Análisis de headers HTTP:** `Server`, `X-Powered-By`, `Link`, `Set-Cookie`, `Strict-Transport-Security`, etc.
3) **Probing específico:** peticiones diseñadas que revelan comportamientos únicos (p. ej., endpoints de WordPress).
4) **Análisis de contenido:** rutas, HTML, JS y patrones que delatan CMS, frameworks o librerías.
5) **Enumeración de TLS:** certificados, SANs, cifrados y versiones negociadas (SNI/ALPN).

## Flujo práctico (ejemplo: `inlanefreight.com`)
### 1) Encadenar redirecciones y ver banners
```bash
# Solo cabeceras (HEAD). Comienza en HTTP y observa redirecciones
curl -I inlanefreight.com

# Sigue la redirección a HTTPS (captura headers de cada salto con -L y muestra cabeceras)
curl -s -D- -o /dev/null -L inlanefreight.com

# Pedir directamente HTTPS y luego el host con www
curl -I https://inlanefreight.com
curl -I https://www.inlanefreight.com
```
**Qué buscamos:** `Server: Apache/2.4.41 (Ubuntu)`, `X-Redirect-By: WordPress`, enlaces `wp-json`, etc.

### 2) ¿Hay WAF?
```bash
# Instalación (si no lo tienes)
pip3 install git+https://github.com/EnableSecurity/wafw00f

# Detección
wafw00f inlanefreight.com
```
Indica si hay WAF (p. ej., **Wordfence**), lo que afecta al resto del reconocimiento.

### 3) Fingerprinting de pila/headers con Nmap
```bash
# Detección de versiones y SO + scripts útiles
nmap -sV -O -p 80,443 --script http-headers,http-title,ssl-cert,ssl-enum-ciphers inlanefreight.com
```
- `-sV`: versión de servicios.
- `-O`: hipótesis de sistema operativo (requiere privilegios en muchas distros).
- Scripts: headers/títulos HTTP, certificado TLS y cifrados soportados.

### 4) Fingerprinting de web con Nikto (solo módulos de identificación)
```bash
# Instalación rápida si fuera necesario
sudo apt update && sudo apt install -y perl
git clone https://github.com/sullo/nikto
cd nikto/program && chmod +x ./nikto.pl

# Escaneo de identificación
nikto -h inlanefreight.com -Tuning b
```
**Salida típica:** servidor, presencia de **WordPress**, headers faltantes (HSTS, X-Content-Type-Options), archivos como `license.txt`, etc.

### 5) WhatWeb (firma de tecnologías desde CLI)
```bash
# Detección ligera
whatweb inlanefreight.com

# Modo agresivo (más pruebas)
whatweb -a 3 https://www.inlanefreight.com
```
Identifica CMS, frameworks, librerías y servicios asociados con base en firmas.

### 6) TLS/SNI y certificado con OpenSSL
```bash
# Ver datos del certificado (CN, SANs, fechas, emisor)
openssl s_client -connect www.inlanefreight.com:443 -servername www.inlanefreight.com </dev/null 2>/dev/null | openssl x509 -noout -subject -issuer -dates -ext subjectAltName
```
Útil para confirmar **SNI**, **SANs** (nombres alternativos) y vigencia del cert.

## Herramientas de apoyo
- **Wappalyzer / BuiltWith / Netcraft:** perfilado web desde navegador/online.
- **WhatWeb:** CLI con base de firmas extensa.
- **Nmap:** servicio/SO y scripts NSE (HTTP/TLS).
- **Nikto:** identificación + hallazgos de seguridad comunes.
- **wafw00f:** detección de WAF y fingerprint parcial.

## Señales e interpretaciones comunes
- `Server:`, `X-Powered-By:` o `Link:` con rutas `wp-json` → posible **WordPress**.
- Redirecciones 301/302 a `www.` o a HTTPS → flujos de reescritura activos (a veces WordPress/Apache/Nginx).
- Certificado Let's Encrypt + SANs con `www.` y raíz → despliegue moderno automatizado.
- Headers de seguridad ausentes (HSTS, X-Content-Type-Options, CSP) → **endurecimiento** pendiente.
- Versiones desactualizadas (p. ej., `Apache/2.4.41`) → revisar changelogs/cves.

## Buenas prácticas defensivas (hardening)
- Minimizar **exposición de banners** (eliminar o generalizar `Server`/`X-Powered-By` donde sea viable).
- Activar **headers de seguridad** (HSTS, X-Content-Type-Options, CSP, Referrer-Policy, etc.).
- Mantener **parches/actualizaciones** al día (servidor, CMS, plugins).
- WAF bien configurado (sin falsos positivos excesivos, registro/alerta).
- Usar TLS fuerte (cifrados modernos, renovar certificados a tiempo).

---
## Resumen de **comandos** y para qué sirven

1. `curl -I inlanefreight.com`  
   Banner grabbing básico (cabeceras HTTP/1.1), detecta redirecciones iniciales y `Server`.

2. `curl -s -D- -o /dev/null -L inlanefreight.com`  
   Sigue redirecciones (`-L`) y muestra **todas** las cabeceras de cada salto (`-D-`).

3. `curl -I https://inlanefreight.com`  
   Cabeceras ya en HTTPS (posibles cambios: HSTS, `X-Redirect-By`, etc.).

4. `curl -I https://www.inlanefreight.com`  
   Cabeceras del host canónico con `www` (suele ser el destino final).

5. `pip3 install git+https://github.com/EnableSecurity/wafw00f`  
   Instala **wafw00f** (si no está) para detectar WAFs.

6. `wafw00f inlanefreight.com`  
   Detecta presencia/tipo de **WAF** (p. ej., Wordfence).

7. `nmap -sV -O -p 80,443 --script http-headers,http-title,ssl-cert,ssl-enum-ciphers inlanefreight.com`  
   Fingerprinting de servicios/OS y scripts de HTTP/TLS (headers, título, cert, cifrados).

8. `sudo apt update && sudo apt install -y perl`  
   Prerrequisito común para **Nikto** (si no viene instalado).

9. `git clone https://github.com/sullo/nikto`  
   Descarga **Nikto**.

10. `cd nikto/program && chmod +x ./nikto.pl`  
    Prepara el ejecutable de **Nikto**.

11. `nikto -h inlanefreight.com -Tuning b`  
    Nikto en modo de **identificación** (huellas) sin pruebas intrusivas adicionales.

12. `whatweb inlanefreight.com`  
    Detección rápida de tecnologías con **WhatWeb**.

13. `whatweb -a 3 https://www.inlanefreight.com`  
    Modo agresivo de WhatWeb (más firmas y comprobaciones).

14. `openssl s_client -connect www.inlanefreight.com:443 -servername www.inlanefreight.com | openssl x509 -noout -subject -issuer -dates -ext subjectAltName`  
    Inspecciona **certificado TLS** (CN/SANs/emisor/fechas) usando SNI.
