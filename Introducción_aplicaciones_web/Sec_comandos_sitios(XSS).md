# Secuencias de Comandos entre Sitios (XSS)

Las vulnerabilidades de **HTML Injection** a menudo se pueden aprovechar para realizar ataques de **Cross-Site Scripting (XSS)** mediante la inyección de **código JavaScript** que se ejecuta en el cliente.  
Cuando el código se ejecuta en el equipo de la víctima, un atacante puede acceder a su cuenta o incluso comprometer su equipo.

En la práctica, **XSS es muy similar a HTML Injection**, pero con la diferencia de que XSS implica ejecutar **JavaScript** para realizar ataques avanzados.

---

## Tipos de XSS

| Tipo          | Descripción |
|---------------|-------------|
| **Reflected XSS** | La entrada del usuario se refleja en la página después de ser procesada (ej: resultados de búsqueda, mensajes de error). |
| **Stored XSS**    | La entrada del usuario se almacena en la base de datos y luego se muestra al recuperarla (ej: comentarios, publicaciones). |
| **DOM XSS**       | La entrada del usuario se procesa directamente en el navegador y se inserta en el objeto **DOM** (ej: campos de nombre, títulos de página). |

---

## Ejemplo Práctico de XSS

En el ejemplo visto de **HTML Injection**, no había sanitización de la entrada.  
Por ello, la página puede ser vulnerable a XSS.

Carga útil usada:

```javascript
#"><img src=/ onerror=alert(document.cookie)>
```

### Resultado
- Al introducir esta carga, aparece una ventana de alerta mostrando el valor de la **cookie** del usuario.
- Esto ocurre porque el navegador interpreta la entrada como un nuevo objeto **DOM** y ejecuta el **JavaScript**.

---

