# Excavando DNS

## Herramientas DNS
El reconocimiento DNS implica el uso de herramientas para consultar servidores DNS y extraer información valiosa.

### Herramientas principales
- **dig**: Consultas DNS avanzadas y detalladas. Ideal para registros A, MX, NS, TXT, etc.
- **nslookup**: Búsquedas DNS básicas. Útil para registros A, AAAA y MX.
- **host**: Búsquedas rápidas y concisas de registros A, AAAA y MX.
- **dnsenum**: Enumeración automatizada de DNS, ataques de diccionario y fuerza bruta.
- **fierce**: Reconocimiento de DNS y descubrimiento de subdominios.
- **dnsrecon**: Combina múltiples técnicas de reconocimiento de DNS con soporte para varios formatos de salida.
- **theHarvester**: Recopilación OSINT incluyendo registros DNS y correos electrónicos.
- **Servicios online**: Herramientas web para consultas rápidas cuando no hay CLI disponible.

---

## Comandos principales con `dig`
- `dig domain.com` – Consulta predeterminada de registro A.
- `dig domain.com A` – Recupera dirección IPv4.
- `dig domain.com AAAA` – Recupera dirección IPv6.
- `dig domain.com MX` – Encuentra los servidores de correo.
- `dig domain.com NS` – Lista los servidores de nombres autorizados.
- `dig domain.com TXT` – Recupera registros TXT.
- `dig domain.com CNAME` – Recupera registro de nombre canónico.
- `dig domain.com SOA` – Recupera el registro de inicio de autoridad.
- `dig @1.1.1.1 domain.com` – Consulta usando un servidor DNS específico.
- `dig +trace domain.com` – Muestra toda la ruta de resolución DNS.
- `dig -x <IP>` – Realiza búsqueda inversa (PTR).
- `dig +short domain.com` – Muestra solo la respuesta de la consulta.
- `dig +noall +answer domain.com` – Muestra únicamente la sección de respuesta.
- `dig domain.com ANY` – Solicita todos los registros (a menudo limitado por servidores).

---

## Ejemplo de salida de `dig google.com`
```
; <<>> DiG 9.18.24 <<>> google.com
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 16449
;; flags: qr rd ad; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0
;; QUESTION SECTION:
;google.com.   IN  A
;; ANSWER SECTION:
google.com. 0  IN  A  142.251.47.142
;; Query time: 0 msec
;; SERVER: 172.23.176.1#53(172.23.176.1)
;; WHEN: Thu Jun 13 10:45:58 SAST 2024
;; MSG SIZE  rcvd: 54
```

---

## Notas clave de la salida
- **HEADER**: Muestra tipo de consulta (QUERY), estado (NOERROR) e ID.
- **flags**: 
  - `qr`: respuesta de consulta
  - `rd`: recursión deseada
  - `ad`: datos auténticos
- **QUESTION SECTION**: Pregunta realizada.
- **ANSWER SECTION**: Respuesta con registro solicitado.
- **TTL (0)**: Tiempo de vida en caché.
- **SERVER**: Servidor DNS que respondió.
- **Query time**: Tiempo de la consulta.
- **MSG SIZE**: Tamaño del mensaje DNS recibido.

---

## Uso de `+short`
Para respuestas simples:
```bash
dig +short hackthebox.com
104.18.20.126
104.18.21.126
```

---

## Precauciones
- Algunos servidores limitan consultas excesivas.
- Respetar límites y permisos antes de hacer enumeración DNS intensiva.

