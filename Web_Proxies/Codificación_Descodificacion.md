# Codificación y Decodificación en Burp Suite y ZAP

## Importancia
Al modificar y enviar solicitudes HTTP personalizadas, a menudo es necesario **codificar** y **decodificar** datos para que el servidor web los interprete correctamente.

## Codificación de URL
Es crucial que los datos de la solicitud estén **codificados en URL** y los encabezados configurados adecuadamente.  
Si no se codifican:
- Los **espacios** pueden interpretarse como fin de datos.
- El carácter **&** como delimitador de parámetros.
- El carácter **#** como identificador de fragmento.

### Cómo codificar en Burp Suite
- Seleccionar texto → clic derecho → `Convert Selection > URL > URL-encode key characters`.
- O usar atajo **CTRL+U**.
- También se puede habilitar la opción para codificar automáticamente mientras se escribe.

> En ZAP, los datos de la solicitud suelen codificarse automáticamente en segundo plano.

Existen otros métodos como **Full URL-Encoding** o **Unicode URL**, útiles para caracteres especiales.

## Decodificación
Las aplicaciones web a menudo **codifican datos** que necesitamos **decodificar** para entender su contenido.  
Algunos formatos comunes:
- HTML
- Unicode
- Base64
- ASCII Hex

### Cómo decodificar
- En **Burp Suite**: pestaña **Decoder**, pegar el texto y elegir `Decode as > [tipo]`.
- En **ZAP**: usar `Encoder/Decoder/Hash` con atajo **CTRL+E**.

#### Ejemplo práctico
Cookie en **Base64**:
```
eyJ1c2VybmFtZSI6Imd1ZXN0IiwgImlzX2FkbWluIjpmYWxzZX0=
```
Decodificada muestra:
```json
{"username":"guest","is_admin":false}
```

Podemos modificar el valor y recodificarlo para pruebas de escalación de privilegios:
```json
{"username":"admin","is_admin":true}
```

## Inspector de Burp
Burp incluye la herramienta **Inspector**, disponible en **Proxy** o **Repeater**, que permite codificar y decodificar directamente las solicitudes y respuestas.

## Consejos
- Crear pestañas personalizadas en el codificador/decodificador de ZAP con `Agregar nueva pestaña`.
- En Burp, se puede re-codificar la **salida decodificada** usando otro método de codificación.

## Resumen
- **Codificar URL** evita errores en solicitudes HTTP.
- **Decodificar datos** revela contenido oculto, útil para pruebas de penetración.
- Permite modificar valores sensibles (como cookies) y recodificarlos para nuevos intentos.
