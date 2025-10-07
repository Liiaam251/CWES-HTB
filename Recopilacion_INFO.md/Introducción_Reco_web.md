# Introducción a Web Reconnaissance

Web Reconnaissance es la **base de una evaluación de seguridad exhaustiva**. Consiste en recopilar información sobre un sitio web o aplicación web objetivo antes de realizar análisis más avanzados.  
Es una parte esencial de la fase de **Information Gathering** en un pentest.

---

##  Objetivos principales
- **Identifying Assets:** Descubrir componentes públicos como páginas web, subdominios, IPs, tecnologías.
- **Discovering Hidden Information:** Encontrar archivos de respaldo, configuraciones, documentación interna.
- **Analysing the Attack Surface:** Examinar tecnologías, configuraciones y posibles puntos de entrada.
- **Gathering Intelligence:** Recopilar datos útiles para explotación o ingeniería social (personal, emails, etc.).

Los **atacantes** usan esta información para preparar ataques dirigidos; los **defensores** para corregir vulnerabilidades proactivamente.

---

##  Tipos de Reconocimiento

###  Activo
Implica **interactuar directamente con el objetivo**. Proporciona información más completa pero con mayor riesgo de ser detectado.

| Técnica | Descripción | Ejemplo | Herramientas | Riesgo |
|---------|-------------|---------|--------------|--------|
| **Port Scanning** | Identificar puertos y servicios abiertos. | Escanear con Nmap los puertos 80/443. | Nmap, Masscan, Unicornscan | Alto |
| **Vulnerability Scanning** | Buscar vulnerabilidades conocidas. | Escanear con Nessus SQLi/XSS. | Nessus, OpenVAS, Nikto | Alto |
| **Network Mapping** | Mapear topología de red. | `traceroute` para ver saltos de red. | Traceroute, Nmap | Medio-Alto |
| **Banner Grabbing** | Obtener banners para saber software/versión. | Examinar banner HTTP puerto 80. | Netcat, cURL | Bajo |
| **OS Fingerprinting** | Identificar SO del objetivo. | `nmap -O` para detectar Windows/Linux. | Nmap, Xprobe2 | Bajo |
| **Service Enumeration** | Identificar versiones de servicios. | `nmap -sV` para versiones Apache/Nginx. | Nmap | Bajo |
| **Web Spidering** | Rastrear páginas y directorios. | Usar Spider de Burp/ZAP. | Burp Spider, ZAP Spider, Scrapy | Bajo-Medio |

 **Ventaja:** visión directa y detallada.  
 **Desventaja:** puede activar IDS/firewalls y levantar sospechas.

---

###  Pasivo
Se realiza **sin interactuar directamente con el objetivo** usando solo fuentes públicas. Es más sigiloso pero menos completo.

| Técnica | Descripción | Ejemplo | Herramientas | Riesgo |
|---------|-------------|---------|--------------|--------|
| **Search Engine Queries** | Buscar información con motores de búsqueda. | Google “Target employees”. | Google, DuckDuckGo, Shodan | Muy bajo |
| **WHOIS Lookups** | Consultar registro de dominios. | WHOIS de dominio para ver dueño/contacto. | whois CLI, servicios online | Muy bajo |
| **DNS Analysis** | Analizar registros DNS y subdominios. | `dig` o `dnsenum` para descubrir subdominios. | dig, nslookup, host, dnsenum | Muy bajo |
| **Web Archive Analysis** | Revisar versiones antiguas de webs. | Wayback Machine para ver cambios. | Wayback Machine | Muy bajo |
| **Social Media Analysis** | Buscar información en redes sociales. | LinkedIn empleados objetivo. | LinkedIn, Twitter, Facebook | Muy bajo |
| **Code Repositories** | Buscar datos en repos públicos. | Buscar credenciales expuestas en GitHub. | GitHub, GitLab | Muy bajo |

 **Ventaja:** discreto y difícil de detectar.  
 **Desventaja:** limitado a información pública disponible.

---

##  Nota
El primer paso práctico será **WHOIS**, para conocer registros de dominios, propiedad e infraestructura digital.

---

## 📜 Resumen
- Reconocimiento web = **fase inicial** del pentest.  
- Se divide en **Activo** (directo, más info, más riesgo) y **Pasivo** (discreto, menos info).  
- Es esencial para conocer el **attack surface** antes de la explotación.
