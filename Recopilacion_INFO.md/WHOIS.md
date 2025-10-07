# WHOIS

## Qué es WHOIS
WHOIS es un **protocolo de consulta y respuesta** utilizado para acceder a bases de datos que almacenan información sobre **recursos de internet registrados**.  
Principalmente se usa para obtener datos sobre **nombres de dominio**, pero también puede proporcionar detalles sobre **bloques de direcciones IP** y **sistemas autónomos**.

Es como una **guía telefónica de internet**, permitiendo consultar **quién es el propietario o responsable** de recursos en línea.

### Ejemplo de uso en consola:
```bash
whois inlanefreight.com
```

### Ejemplo de salida:
```
Domain Name: inlanefreight.com
Registry Domain ID: 2420436757_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.registrar.amazon
Registrar URL: https://registrar.amazon.com
Updated Date: 2023-07-03T01:11:15Z
Creation Date: 2019-08-05T22:43:09Z
...
```

---

## Información común en un registro WHOIS:
- **Domain Name:** nombre de dominio (por ej.: `ejemplo.com`).
- **Registrar:** empresa donde se registró el dominio (GoDaddy, Namecheap, etc.).
- **Registrant Contact:** persona u organización que registró el dominio.
- **Administrative Contact:** responsable de administrar el dominio.
- **Technical Contact:** persona encargada de los aspectos técnicos.
- **Creation and Expiration Dates:** fecha de creación y fecha de caducidad del dominio.
- **Name Servers:** servidores que traducen el dominio en una dirección IP.

---

## Historia de WHOIS
- Creado en los **años 70 por Elizabeth Feinler** y su equipo en el **NIC (Network Information Center)** de Stanford.
- Surgió para gestionar los crecientes recursos de **ARPANET** (predecesor de internet).
- Inicialmente fue un **directorio básico** que almacenaba información de usuarios, hosts y dominios.

---

## Importancia de WHOIS para Web Recon
Los datos de WHOIS son fundamentales para el **reconocimiento web** y las **pruebas de penetración**:

1. **Identifying Key Personnel:**  
   - Puede mostrar **nombres, correos electrónicos y teléfonos** de los responsables del dominio.  
   - Útil para ataques de **ingeniería social** o campañas de **phishing**.

2. **Discovering Network Infrastructure:**  
   - Incluye detalles técnicos como **servidores de nombres** y **direcciones IP**.  
   - Ayuda a detectar **puntos de entrada** y **configuraciones incorrectas**.

3. **Historical Data Analysis:**  
   - Con servicios como **WhoisFreaks** se pueden ver **cambios históricos** en la propiedad, los contactos y la infraestructura técnica.  
   - Permite **rastrear la evolución** de la presencia digital del objetivo.

