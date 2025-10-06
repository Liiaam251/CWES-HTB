# Apuntes: Burp Intruder - Fuzzing y Fuerza Bruta

##  Descripción General
Burp Intruder es la herramienta de **fuzzing web y fuerza bruta** integrada en Burp Suite.  
Permite automatizar ataques sobre:
- Directorios web
- Subdominios
- Parámetros y valores
- Formularios de login
- Cualquier dato en las solicitudes HTTP

>  Nota: La **Community Version** de Burp Intruder está limitada a **1 solicitud/segundo**, lo que la hace muy lenta para listas largas.  
La **Pro Version** no tiene limitaciones de velocidad y añade funciones avanzadas.

---

##  Flujo de Trabajo Básico
1. **Enviar solicitud al Intruder**
   - Capturar una solicitud en el **Proxy History**
   - Click derecho → **Send to Intruder** o atajo `CTRL+I`
2. **Abrir pestaña Intruder**
   - Ir a `Intruder` o atajo `CTRL+SHIFT+I`
3. **Configurar el objetivo (Target)**
   - Automáticamente se completa con la información de la solicitud
4. **Configurar Posiciones (Positions)**
   - Marcar con `§` las variables a fuzzear (ejemplo: `/DIRECTORY/`)
   - Tipo de ataque inicial: **Sniper**
5. **Configurar Cargas Útiles (Payloads)**
   - Cargar listas de palabras (ejemplo: `common.txt` de SecLists)
   - Opcional: añadir elementos manualmente
6. **Configurar Procesamiento (Payload Processing)**
   - Añadir reglas (ejemplo: omitir líneas que empiezan con `.` usando regex `^\..*$`)
7. **Configurar Codificación (Payload Encoding)**
   - Mantener habilitada la codificación URL
8. **Opciones de Ataque (Settings)**
   - Activar **Grep - Match** y filtrar por `200 OK`
9. **Iniciar el ataque**
   - Click en **Start Attack**
10. **Analizar resultados**
    - Ordenar por `Status`, `Length` o `200 OK`

---

##  Modos de Ataque (Attack Types)
- **Sniper**: Un solo marcador de carga útil. Prueba cada valor en un punto.
- **Battering Ram**: Múltiples posiciones, mismo valor probado en todas al mismo tiempo.
- **Pitchfork**: Varias posiciones, prueba diferentes listas de palabras en paralelo.
- **Cluster Bomb**: Combinaciones de múltiples listas de palabras en distintas posiciones.

---

## 🛠️ Tipos de Carga Útil (Payload Types)
- **Simple List**: Lista básica de palabras a probar.
- **Runtime file**: Similar a *Simple List* pero lee línea por línea para ahorrar memoria.
- **Character Substitution**: Sustituye caracteres por otros (útil para pruebas avanzadas).
- **Numbers**: Genera secuencias numéricas.
- **Brute Forcer**: Genera combinaciones de caracteres para ataques de fuerza bruta.
- **Date**: Genera fechas en rangos específicos.
- **Null Payloads**: Envía solicitudes sin carga útil (para tests de comportamiento).
- Otros: Hash, IP ranges, entre muchos más.

---

## ⚙️ Opciones Útiles
- **Grep - Match**: Marca respuestas que contengan ciertas cadenas (por ejemplo, `200 OK`)
- **Grep - Extract**: Extrae partes específicas de respuestas extensas
- **Resource Pool**: Controla los recursos de red usados para ataques grandes
- **Payload Encoding**: Asegura la correcta codificación de caracteres especiales

---

##  Consejos Prácticos
- Usar `Runtime file` con listas muy grandes para evitar problemas de memoria.
- Activar `Grep - Match` para encontrar resultados exitosos (códigos 200).
- Combinar con listas de **SecLists** ubicadas en `/opt/useful/seclists/`.
- Usar `CTRL+I` para enviar al Intruder rápidamente.
- Usar **Cluster Bomb** para pruebas en múltiples parámetros.
- En la **Community Version**, usar para pruebas cortas (consultas rápidas).

---

##  Lista de Comandos y Atajos Importantes
| Acción | Comando/Atajo | Descripción |
|--------|---------------|-------------|
| Enviar solicitud a Intruder | `CTRL+I` | Mueve la solicitud desde el Proxy/Repeater a Intruder |
| Abrir pestaña Intruder | `CTRL+SHIFT+I` | Acceso rápido a la pestaña Intruder |
| Agregar marcador | Botón **Add §** | Marca la posición a fuzzear |
| Iniciar ataque | Botón **Start Attack** | Comienza el ataque con la configuración actual |
| Limitar líneas en listas | Payload Processing → **Skip if matches regex** | Excluye líneas según regex (por ejemplo: `^\..*$`) |
| Cargar lista de palabras | **Load** | Carga archivo de lista de palabras |
| Añadir palabra | **Add** | Agrega palabra manualmente |
| Borrar entradas | **Clear / Remove** | Limpia lista de palabras |
| Guardar resultados | **Save Results** | Exporta resultados del ataque |

---

##  Ejemplo Rápido
1. Interceptar `/DIRECTORY/`
2. Marcar `§DIRECTORY§`
3. Cargar lista `common.txt`
4. Configurar `Grep - Match` con `200 OK`
5. Ejecutar ataque → Identificar directorios válidos (`/admin/`)
