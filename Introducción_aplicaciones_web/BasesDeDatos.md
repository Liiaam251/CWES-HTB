# Bases de Datos

Las aplicaciones web utilizan bases de datos **back-end** para almacenar diversos contenidos e información: imágenes, archivos, publicaciones, actualizaciones, nombres de usuario y contraseñas.  
Esto permite almacenar y recuperar datos de forma rápida y habilitar contenido dinámico para cada usuario.

Los desarrolladores suelen buscar en una base de datos: **velocidad, escalabilidad, tamaño y coste**.

---

## Tipos de Bases de Datos

### 1. Relacionales (SQL)
Almacenan datos en **tablas, filas y columnas**.  
Cada tabla puede tener **claves únicas** para crear relaciones entre ellas (**esquema**).

**Ejemplo:**  
- Tabla `users`: id, username, first_name, last_name  
- Tabla `posts`: id, user_id, date, content  

Podemos usar `id` de `users` como clave para vincularla con `user_id` de `posts` y recuperar información de usuario para cada publicación.

Esto permite:
- Recuperar rápidamente los datos relacionados con un elemento específico.
- Facilitar la gestión de datos.
- Mayor velocidad y fiabilidad para datos estructurados.

**Bases de datos SQL más comunes:**  
- **MySQL:** la más utilizada en Internet, gratuita y de código abierto.  
- **MSSQL:** de Microsoft, muy usada en servidores Windows/IIS.  
- **Oracle:** muy fiable, cara pero optimizada para grandes empresas.  
- **PostgreSQL:** gratuita, de código abierto y extensible.  
- Otras: **SQLite, MariaDB, Amazon Aurora, Azure SQL**.

---

### 2. No Relacionales (NoSQL)
No utilizan tablas, filas, columnas ni esquemas.  
Son **muy escalables y flexibles**, ideales para datos poco estructurados.

**Modelos de almacenamiento más comunes:**
- **Clave-Valor:** datos en formato JSON/XML con un par `clave: valor`.
- **Basado en Documentos:** almacena objetos JSON complejos con metadatos.
- **Columna Ancha:** optimiza almacenamiento por columnas.
- **Gráfico:** ideal para relaciones complejas (como redes sociales).

**Ejemplo Key-Value en JSON:**

```json
{
  "100001": { "date": "01-01-2021", "content": "Welcome to this web application." },
  "100002": { "date": "02-01-2021", "content": "This is the first post on this web app." },
  "100003": { "date": "02-01-2021", "content": "Reminder: Tomorrow is the ..." }
}
```

**Bases de datos NoSQL más comunes:**  
- **MongoDB:** Document-Based, gratuita y de código abierto.  
- **ElasticSearch:** optimizada para búsquedas rápidas y grandes conjuntos de datos.  
- **Apache Cassandra:** escalable, gratuita y precisa con valores erróneos.  
- Otras: **Redis, Neo4j, CouchDB, Amazon DynamoDB**.

---

## Uso en Aplicaciones Web

1. Se instala y configura la base de datos en el **servidor back-end**.
2. Las aplicaciones web la utilizan para **almacenar y recuperar datos**.

**Ejemplo en PHP con MySQL:**

```php
$conn = new mysqli("localhost", "user", "pass");
$sql = "CREATE DATABASE database1";
$conn->query($sql);

$conn = new mysqli("localhost", "user", "pass", "database1");
$query = "select * from table_1";
$result = $conn->query($query);
```

**Ejemplo de búsqueda de usuarios:**

```php
$searchInput = $_POST['findUser'];
$query = "select * from users where name like '%$searchInput%'";
$result = $conn->query($query);

while($row = $result->fetch_assoc()){
    echo $row["name"]."<br>";
}
```

---

## Riesgos
Un mal manejo del código puede abrir la puerta a **vulnerabilidades de inyección SQL**, poniendo en riesgo la seguridad de los datos.
