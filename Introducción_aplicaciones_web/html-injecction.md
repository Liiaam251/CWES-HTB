# Inyección de HTML

## Introducción
La **inyección de HTML** es un tipo de vulnerabilidad en el front-end que surge cuando la aplicación no valida ni limpia adecuadamente la **entrada del usuario** antes de mostrarla en la página web.  
Aunque a menudo la validación se realiza en el back-end, es **crucial validar y limpiar los datos tanto en el front-end como en el back-end**, ya que algunas entradas nunca llegan al servidor y son procesadas únicamente en el cliente.

## Concepto
Cuando el navegador muestra directamente el contenido proporcionado por el usuario sin filtrarlo, se abre la posibilidad de que el atacante inserte código **HTML** malicioso.  
Esto puede incluir:
- Formularios falsos de inicio de sesión para robar credenciales.
- Scripts que cambien la apariencia del sitio (defacement).
- Inserción de anuncios o imágenes maliciosas.

## Riesgos
Si no hay sanitización, un atacante puede controlar cómo se renderiza su entrada y ejecutar acciones como:
- Cambiar el aspecto visual del sitio web.
- Insertar elementos engañosos (por ejemplo, un formulario que envía datos a un servidor malicioso).
- Afectar la reputación de la empresa mediante el **defacement** de la página.

## Ejemplo Práctico
Página sencilla que pide el nombre al usuario y lo muestra en la pantalla.

### Código HTML vulnerable
```html
<!DOCTYPE html>
<html>
<body>
    <button onclick="inputFunction()">Click to enter your name</button>
    <p id="output"></p>

    <script>
        function inputFunction() {
            var input = prompt("Please enter your name", "");
            if (input != null) {
                document.getElementById("output").innerHTML = "Your name is " + input;
            }
        }
    </script>
</body>
</html>
```

El problema está en la línea:
```javascript
document.getElementById("output").innerHTML = "Your name is " + input;
```
Se inserta directamente el contenido del usuario en el DOM usando `innerHTML`, sin filtrar.

### Prueba de Inyección
Si ingresamos el siguiente código como "nombre":
```html
<style>
    body { background-image: url('https://academy.hackthebox.com/images/logo.svg'); }
</style>
```
El sitio web cambiará su fondo. Esto demuestra que podemos **inyectar código HTML** arbitrario.

## Prevención
- Validar y limpiar siempre las entradas del usuario en el **front-end** y el **back-end**.
- Evitar el uso de `innerHTML` para mostrar datos no confiables.
- Utilizar funciones seguras como `textContent` para mostrar solo texto plano.
- Implementar librerías de sanitización de entrada.
