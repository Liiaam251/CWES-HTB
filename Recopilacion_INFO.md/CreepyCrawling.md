# Bichos espeluznantes 🕷️ — Rastreo web paso a paso (para novatos)

## 1) ¿Qué es el “rastreo web”?
“Rastrear” una web es **visitar automáticamente** sus páginas para **descubrir enlaces, archivos e información pública** (como imágenes, PDFs, scripts, etc.) y guardarlo ordenado para analizarlo después.

- **No es hacking** por sí mismo: solo **lee contenido público**.
- Sirve para **mapear** la web, entender su estructura y encontrar **puntos de interés** (por ejemplo, páginas de login, documentos, políticas).

## 2) Herramientas populares (muy breve)
- **Burp Suite Spider**: parte de la suite de pruebas web Burp. Mapea contenido y ayuda a encontrar fallos.
- **OWASP ZAP**: alternativa gratuita y de código abierto. Tiene un “spider” (araña) para descubrir páginas.
- **Scrapy (Python)**: un **framework** para crear **tus propios** rastreadores y extraer datos.
- **Apache Nutch (Java)**: pensado para **rastreo a gran escala** (muchos dominios, gran volumen).

> Tip: Cada herramienta tiene su estilo. En este documento **usaremos Scrapy + ReconSpider** porque es sencillo de replicar y entender.

## 3) Reglas de oro (ética y legalidad)
- **Pide permiso** al dueño del sitio si no es un laboratorio o tu dominio.
- **No satures** el servidor con demasiadas peticiones.
- Respeta las leyes y los Términos de Servicio.
- En entornos de práctica (HTB, etc.) sigue las instrucciones del módulo/laboratorio.

## 4) Tu primer rastreo con Scrapy + ReconSpider

### 4.1. Instalar Scrapy
Necesitas Python 3 y `pip`. En una terminal:
```bash
pip3 install scrapy
```
Esto descarga e instala Scrapy y dependencias.

### 4.2. Descargar ReconSpider
ReconSpider es una **araña** (script) que automatiza el reconocimiento en un dominio de ejemplo (p. ej., `inlanefreight.com` del curso).

```bash
# Descarga el paquete de ReconSpider
wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip

# Extrae el contenido
unzip ReconSpider.zip
```

### 4.3. Ejecutar la araña
Lanza la araña contra el dominio de práctica (o **tu** dominio con permiso):

```bash
python3 ReconSpider.py http://inlanefreight.com
```
Cambia `inlanefreight.com` por el dominio objetivo **autorizado**.  
La herramienta rastreará el sitio y guardará los resultados en un archivo JSON llamado **`results.json`** (o similar, según versión).

## 5) ¿Qué aparece en `results.json`?
Un ejemplo simplificado de la **estructura** que suele generarse:

```json
{
  "emails": ["lily.floid@inlanefreight.com", "cvs@inlanefreight.com"],
  "links": ["https://www.inlanefreight.com/index.php/offices/"],
  "external_files": ["https://www.inlanefreight.com/wp-content/uploads/2020/09/goals.pdf"],
  "js_files": ["https://www.inlanefreight.com/wp-includes/js/jquery/jquery-migrate.min.js?ver=3.3.2"],
  "form_fields": [],
  "images": ["https://www.inlanefreight.com/wp-content/uploads/2021/03/AboutUs_01-1024x810.png"],
  "videos": [],
  "audio": [],
  "comments": ["<!-- #masthead -->"]
}
```

### ¿Qué significa cada cosa?
- **emails**: direcciones de correo encontradas en el contenido público.
- **links**: enlaces (URLs) localizados dentro del sitio.
- **external_files**: archivos públicos (PDFs, docs, etc.).
- **js_files**: ficheros **JavaScript** que usa la web.
- **form_fields**: campos de formularios detectados (usuario, email, etc.).
- **images / videos / audio**: recursos multimedia.
- **comments**: **comentarios HTML** dentro del código fuente (a veces dan pistas).

> Con esto puedes **entender la arquitectura** de la web y detectar “puntos de interés” (p. ej., `/login`, políticas, documentación, etc.).

## 6) Paso a paso súper corto (resumen)
1. **Instala Scrapy**: `pip3 install scrapy`  
2. **Descarga** ReconSpider: `wget -O ReconSpider.zip ...`  
3. **Extrae**: `unzip ReconSpider.zip`  
4. **Ejecuta**: `python3 ReconSpider.py http://tu-dominio-con-permiso`  
5. **Abre** `results.json` en tu editor y **revisa** cada sección.

## 7) Siguientes pasos (si quieres avanzar)
- Filtra resultados por **subdominios** (auth., login., accounts.).  
- Mira si hay una carpeta `/.well-known/` (metadatos públicos estándar).  
- Guarda los hallazgos en un documento: páginas clave, rutas de login, políticas, archivos relevantes.  
- Si dominas Python, adapta ReconSpider o crea tu propia araña con **Scrapy** para extraer exactamente lo que te interese.

## 8) Glosario rápido
- **Araña (spider)**: programa que recorre páginas y extrae datos.
- **Endpoint**: URL que hace algo concreto (login, API, descargar archivo).
- **Metadatos**: datos **sobre** el sitio (no el contenido principal), útiles para integraciones y análisis.

## 9) Problemas típicos (y soluciones rápidas)
- **No tengo Python/pip** → Instala Python 3 desde su web oficial o tu gestor de paquetes.
- **Permisos denegados** → Asegúrate de tener permiso para rastrear ese dominio.
- **Bloqueos por el sitio** → Baja la velocidad de peticiones o respeta `robots.txt` si procede.
- **El JSON está vacío** → Prueba con otra URL inicial (homepage), revisa tu conexión, o comprueba que la araña se ejecutó sin errores.

### Recordatorio final
Usa estas técnicas **solo donde esté permitido** (tus dominios, entornos de laboratorio o con autorización explícita). El objetivo de estos apuntes es **aprender** y **analizar** información pública de forma responsable.
