
# Apuntes: Subdominios

## ¿Qué son los Subdominios?
Los **subdominios** son extensiones del dominio principal que se utilizan para organizar o separar distintas secciones de un sitio web.  
Ejemplos comunes:
- `blog.example.com` → blog corporativo  
- `shop.example.com` → tienda online  
- `mail.example.com` → servicio de correo electrónico  

## Importancia para el Reconocimiento Web
Los subdominios pueden contener recursos valiosos o inseguros que no están visibles en el sitio principal.  
Algunos ejemplos:
- **Entornos de desarrollo y pruebas**: subdominios para probar nuevas funciones que a menudo carecen de medidas de seguridad.
- **Portales de inicio de sesión ocultos**: paneles administrativos que no están enlazados públicamente.
- **Aplicaciones heredadas**: sistemas antiguos en subdominios que pueden tener vulnerabilidades conocidas.
- **Información sensible**: documentos internos, archivos de configuración u otros datos expuestos accidentalmente.

## Enumeración de Subdominios
La **enumeración de subdominios** es el proceso de identificar y listar los subdominios existentes de un dominio objetivo.  
Desde el punto de vista de DNS, los subdominios suelen representarse con:
- Registros **A** (IPv4) o **AAAA** (IPv6) → Asignan el subdominio a una IP.  
- Registros **CNAME** → Alias que apuntan a otros dominios/subdominios.

Existen dos enfoques principales:

### 1. Enumeración Activa
Consiste en interactuar directamente con los servidores DNS del objetivo.  
Métodos:
- **Transferencia de zona DNS (AXFR)**: un servidor mal configurado puede filtrar todos los subdominios (rara vez funciona por seguridad).  
- **Fuerza bruta (Brute-force)**: probar listas de posibles subdominios comunes o personalizados.  

**Herramientas activas:**
- `dnsenum`
- `ffuf`
- `gobuster`

Ventajas: más exhaustiva.  
Desventajas: más detectable y puede generar alertas.

### 2. Enumeración Pasiva
Recopila información de fuentes externas sin interactuar con el servidor del objetivo.  
Métodos:
- **Registros de Transparencia de Certificados (CT logs)**: los certificados SSL/TLS suelen listar subdominios en el campo SAN (Subject Alternative Name).  
- **Motores de búsqueda**: usar operadores como `site:subdominio.dominio.com` en Google o DuckDuckGo.  
- **Bases de datos y herramientas online**: agregan datos DNS históricos y públicos.  

Ventajas: más discreta.  
Desventajas: puede ser incompleta.

## Estrategia Recomendada
Combinar **enumeración activa y pasiva** ofrece mejores resultados:
- Activa → más descubrimientos técnicos.
- Pasiva → menos detectable y útil para un reconocimiento inicial.

---
**Resumen:** La enumeración de subdominios es clave para descubrir puntos de entrada ocultos, entornos de desarrollo y servicios olvidados que pueden representar vulnerabilidades en un pentest.
