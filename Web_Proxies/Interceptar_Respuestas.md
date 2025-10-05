# Interceptar Respuestas con BurpSuite

## Objetivo
En algunos casos, necesitamos **interceptar las respuestas HTTP del servidor** antes de que lleguen al navegador. Esto es útil para:
- Cambiar la apariencia de una página web específica.
- Habilitar campos deshabilitados u ocultos.
- Modificar formularios para permitir mayor control en pruebas de penetración.

---

## Pasos para Interceptar Respuestas en BurpSuite

### 1. Habilitar Interceptación de Respuestas
1. Abrir **BurpSuite**.
2. Ir a **Proxy > Proxy settings**.
3. Activar la opción **Intercept Response** en *Response interception rules*.
4. Asegurarse de marcar también la opción para actualizar el encabezado `Content-Length` si se requiere.

---

### 2. Interceptar y Editar
1. Habilitar de nuevo la **interceptación de solicitudes**.
2. Actualizar la página en el navegador con **CTRL + SHIFT + R** para forzar una recarga completa.
3. Cuando Burp capture la **solicitud**, hacer clic en **Forward**.
4. Veremos la **respuesta interceptada** en Burp.

---

### 3. Ejemplo Práctico
- Originalmente, un formulario solo permitía valores numéricos en el campo IP:

```html
<input type="number" id="ip" name="ip" min="1" max="255" maxlength="3"
    oninput="javascript: if (this.value.length > this.maxLength) 
    this.value = this.value.slice(0, this.maxLength);" required>
```

- Modificamos el campo para aceptar texto y mayor longitud:

```html
<input type="text" id="ip" name="ip" min="1" max="255" maxlength="100"
    oninput="javascript: if (this.value.length > this.maxLength) 
    this.value = this.value.slice(0, this.maxLength);" required>
```

5. Presionar **Forward** nuevamente.
6. Regresar al navegador y verificar el cambio aplicado: ahora el campo permite cualquier valor.

---

## Ejercicio Sugerido
- Usar la misma técnica para habilitar un botón HTML deshabilitado.
- Insertar la carga útil directamente desde el navegador una vez modificado el formulario.

---

## Otras Opciones
- Burp incluye en **Proxy > Proxy settings > Response modification rules** opciones como **Unhide hidden form fields** para habilitar automáticamente campos ocultos.
- También es posible mostrar los **comentarios HTML** en las páginas para descubrir pistas adicionales.

---

## ZAP HUD (Alternativa)
- ZAP permite interceptar solicitudes y respuestas de forma similar a Burp.
- Al interceptar, podemos usar el botón **Step** para enviar la solicitud e interceptar la respuesta.
- El **botón de bombilla** permite habilitar campos deshabilitados u ocultos sin interceptar manualmente.
- También cuenta con la función **Comments** para revelar comentarios ocultos en el código fuente.
