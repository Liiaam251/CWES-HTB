# Vulnerabilidades Web Comunes

Cuando realizamos pruebas de penetración en aplicaciones web podemos encontrar vulnerabilidades causadas por configuraciones incorrectas o errores de desarrollo. A continuación se describen algunas de las vulnerabilidades más comunes incluidas en el Top 10 de OWASP.

## 1. Autenticación/Control de Acceso Rotos

- **Broken Authentication:** Permite a un atacante eludir las funciones de autenticación, como iniciar sesión sin credenciales válidas o escalar privilegios (por ejemplo, un usuario normal que se convierte en administrador).
- **Broken Access Control:** Permite a los atacantes acceder a páginas o funciones restringidas, como que un usuario normal acceda al panel de administración.

**Ejemplo:**  
Un sistema vulnerable (College Management System 1.2) permite iniciar sesión usando en el campo de correo electrónico:
```
' or 0=0 #
```
y cualquier contraseña.

---

## 2. Carga de Archivos Maliciosos

Cuando una aplicación permite la carga de archivos y no valida correctamente su tipo o extensión, un atacante podría subir un script malicioso (por ejemplo, un archivo `PHP`) que permita ejecutar comandos en el servidor.

**Ejemplo:**  
El plugin de WordPress *Responsive Thumbnail Slider 1.0* puede explotarse subiendo archivos con doble extensión como:
```
shell.php.jpg
```
Esto permite ejecutar código arbitrario en el servidor.

---

## 3. Inyección de Comandos

Sucede cuando la aplicación ejecuta comandos locales del sistema operativo con datos proporcionados por el usuario sin sanitización adecuada. Un atacante puede inyectar comandos adicionales para ejecutar en el servidor.

**Ejemplo:**  
El plugin de WordPress *Plainview Activity Monitor 20161228* permite inyectar comandos en el parámetro `ip`:
```
127.0.0.1 | whoami
```

---

## 4. Inyección SQL (SQLi)

Ocurre cuando la aplicación utiliza entradas de usuario en consultas SQL sin sanitización ni validación adecuadas, lo que permite a los atacantes modificar la consulta y ejecutar código malicioso contra la base de datos.

**Ejemplo:**  
Una consulta vulnerable:
```php
$query = "SELECT * FROM users WHERE name LIKE '%$searchInput%'";
```
Permite que un atacante inyecte código SQL adicional para eludir autenticación o manipular datos.

**Caso real:**  
El sistema *College Management System 1.2* presentaba una SQL Injection que permitía iniciar sesión como administrador y extraer datos de la base de datos.

