# Subdomain Brute-Force Enumeration - Apuntes

## Pasos del Proceso

## Herramientas Populares

| Herramienta       | Descripción |
|-------------------|-------------|
| **dnsenum**       | Enumeración de DNS completa: registros, subdominios, zona, WHOIS. |
| **fierce**        | Descubrimiento recursivo de subdominios, detección de comodines. |
| **dnsrecon**      | Combina varias técnicas de reconocimiento, salida personalizable. |
| **amass**         | Mantenida activamente, integra diversas fuentes OSINT y fuerza bruta. |
| **assetfinder**   | Ligera y rápida, ideal para escaneos simples. |
| **massdns**       | Fuerza bruta potente y flexible para grandes volúmenes. |

---
## dnsenum

**dnsenum** es una herramienta muy utilizada que permite:
- **DNS Record Enumeration:** obtiene registros A, AAAA, MX, NS, TXT, etc.
- **Zone Transfer Attempts:** intenta transferencias de zona si el servidor lo permite.
- **Subdomain Brute-Forcing:** fuerza bruta de subdominios usando wordlists.
- **Google Scraping:** busca subdominios adicionales desde resultados de Google.
- **Reverse Lookup:** identifica dominios asociados a una IP.
- **WHOIS Lookups:** recopila datos WHOIS del dominio objetivo.

---
## Ejemplo de Uso

```bash
dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -r
```

- `--enum` → activa varias opciones de enumeración.
- `-f` → indica la ruta a la **wordlist** de subdominios.
- `-r` → habilita la fuerza bruta **recursiva**.

---
## Ejemplo de Salida

```bash
dnsenum VERSION:1.2.6

-----   inlanefreight.com   -----

Host's addresses:
inlanefreight.com. 300 IN A 134.209.24.248

Brute forcing with /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt:

www.inlanefreight.com. 300 IN A 134.209.24.248
support.inlanefreight.com. 300 IN A 134.209.24.248
...
done.
```
