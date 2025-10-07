# DNS – Domain Name System

## Introducción
El **Domain Name System (DNS)** actúa como un traductor entre nombres de dominio legibles por humanos (por ejemplo, `www.example.com`) y direcciones IP (por ejemplo, `192.0.2.1`).  
Facilita la navegación en internet al evitar tener que memorizar direcciones IP.

---

## Cómo funciona el DNS
1. **Consulta inicial (DNS Query)**: el equipo consulta la caché local o el solucionador DNS del ISP.
2. **Solucionador (Recursive Lookup)**: consulta su propia caché o inicia búsqueda en la jerarquía DNS.
3. **Servidor raíz (Root Name Server)**: redirige al servidor TLD.
4. **Servidor TLD (TLD Name Server)**: apunta al servidor autoritativo correspondiente al dominio.
5. **Servidor autoritativo (Authoritative Name Server)**: devuelve la dirección IP correcta.
6. **Respuesta**: el solucionador envía la IP al cliente y la guarda en caché.
7. **Conexión**: el equipo ya puede conectarse al servidor web.

---

## Archivo Hosts
Permite asignar nombres de host a IP manualmente.  
Ubicación:
- **Windows:** `C:\Windows\System32\drivers\etc\hosts`
- **Linux / macOS:** `/etc/hosts`

Formato:
```
<IP>      <hostname>   [alias]
```

Ejemplos:
```
127.0.0.1       localhost
127.0.0.1       myapp.local        # Desarrollo local
192.168.1.10    devserver.local    # Pruebas
0.0.0.0         unwanted-site.com  # Bloquear sitio
```

---

## Conceptos Clave de DNS
- **Zona DNS (Zone):** conjunto de dominios/subdominios gestionados por la misma entidad.
- **Servidor Autoritativo:** contiene la información final y real de un dominio.
- **Registros DNS:** almacenan diferentes tipos de datos asociados a dominios.

---

## Tipos Comunes de Registros DNS
| Tipo   | Descripción                                      | Ejemplo                          |
|--------|--------------------------------------------------|----------------------------------|
| A      | Nombre de host a IPv4                            | `www.example.com -> 192.0.2.1`   |
| AAAA   | Nombre de host a IPv6                            | `www.example.com -> 2001:db8::1` |
| CNAME  | Alias de un dominio hacia otro                   | `blog.example.com -> web.example.net` |
| MX     | Servidor de correo                                | `example.com -> mail.example.com` |
| NS     | Servidor de nombres                               | `example.com -> ns1.example.com` |
| TXT    | Texto arbitrario (SPF, verificación de dominio)   | `"v=spf1 mx -all"`               |
| SOA    | Información administrativa de la zona            | Servidor principal, TTL, etc.    |
| SRV    | Define host y puerto de un servicio               | `_sip._udp.example.com`          |
| PTR    | Búsqueda inversa de IP a nombre de host           | `1.2.0.192.in-addr.arpa -> www.example.com` |

---

## Comandos Clave para DNS
### Consultas básicas
- **`nslookup <dominio>`** – Consulta rápida de IP de un dominio.
- **`nslookup <IP>`** – Realiza búsqueda inversa.

### Herramientas de análisis avanzadas
- **`dig <dominio>`** – Consulta DNS detallada.
- **`dig <dominio> ANY`** – Muestra todos los registros DNS disponibles.
- **`dig @<servidor_DNS> <dominio>`** – Consulta usando un servidor DNS específico.
- **`host <dominio>`** – Consulta simple de registros DNS.
- **`host -t MX <dominio>`** – Consulta registros MX de correo.

### Enumeración de subdominios
- **`dnsenum <dominio>`** – Enumeración automática de subdominios.
- **`dnsrecon -d <dominio>`** – Reconocimiento de DNS más exhaustivo.
- **`fierce -dns <dominio>`** – Rastrea subdominios de forma agresiva.

---

## Importancia del DNS en Web Recon
- **Descubrir activos:** subdominios, servidores de correo, registros TXT, etc.
- **Mapear infraestructura:** ayuda a entender la red y detectar configuraciones incorrectas.
- **Monitoreo de cambios:** nuevos subdominios o cambios en registros pueden indicar puntos de entrada o información sensible.
