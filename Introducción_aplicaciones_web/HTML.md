# Exposición de Datos Confidenciales

La **exposición de datos confidenciales** ocurre cuando información sensible está disponible en **texto plano** para el usuario final en el **código fuente del frontend** de una aplicación web.  
Aunque los componentes **front-end** no acceden directamente al núcleo **back-end**, las vulnerabilidades en ellos pueden exponer datos críticos y permitir ataques como acceso no autorizado o explotación de funcionalidades sensibles.

---

##  Importancia
- El **frontend** incluye HTML, CSS y JavaScript visibles en el navegador.
- Si hay datos expuestos (credenciales, hashes, enlaces internos, etc.), pueden ser utilizados por un atacante para **escalar privilegios** o comprometer el backend.
- Por eso, durante las evaluaciones de seguridad web, se debe **revisar el código fuente visible al usuario**.

---

##  Cómo Ver el Código Fuente
- Clic derecho en la página → **“View Page Source”**.
- Atajo de teclado: **CTRL + U**.
- Si el clic derecho está deshabilitado, usar:
  - Herramientas de desarrollo del navegador (**F12 → pestaña Sources**).
  - Proxies web como **Burp Suite** para inspeccionar el contenido.
- Ejemplo: `view-source:https://www.google.com/`

---

##  Información que Puede Estar Expuesta
- **Credenciales de prueba**:
  ```html
  <!-- TODO: remove test credentials test:test -->
  ```
- **Hashes de contraseñas**.
- **Enlaces a directorios internos**.
- **Páginas de depuración o funcionalidades ocultas**.
- Información sensible sobre usuarios u otros sistemas conectados.

**Ejemplo de código vulnerable:**
```html
<form action="action_page.php" method="post">
  <!-- TODO: remove test credentials test:test -->
  <input type="text" name="username" required>
  <input type="password" name="password" required>
</form>
```

---

##  Riesgos
- Un atacante puede usar credenciales de prueba no eliminadas.
- Páginas ocultas o rutas internas pueden servir como puerta de entrada.
- Podría permitir **escalar privilegios** y acceder a paneles de administración u otros servicios críticos.

---

##  Prevención
1. **Eliminar comentarios innecesarios** y datos sensibles en el código visible.
2. Mantener solo el **código estrictamente necesario** en el frontend.
3. **Clasificar los tipos de datos** y controlar qué se expone al cliente.
4. Revisar regularmente el código del cliente antes de producción.
5. Usar **ofuscación de JavaScript** o empaquetado para dificultar la lectura automática del código.
6. Evitar dejar credenciales, claves API, endpoints internos o rutas de depuración.

---

##  Resumen
- La exposición de datos sensibles en el **frontend** puede dar pistas a un atacante.
- **Primera acción en un pentest web**: revisar el código fuente (HTML y JS).
- Buscar **credenciales, comentarios, enlaces internos o funciones ocultas**.
- Mantener el frontend limpio y seguro ayuda a prevenir fugas de información crítica.
