# `/.well-known` para principiantes (apuntes sencillos)

## 1) ¿Qué es `/.well-known`?
Es una carpeta especial en los sitios web: `https://tu-dominio.com/.well-known/`
Ahí se guardan **archivos públicos con información** sobre el sitio (políticas, configuración de seguridad, dónde están ciertos servicios). Sirve para que las aplicaciones encuentren esa información siempre en el **mismo lugar**.

Piensa en ello como un “tablón de avisos” estándar del sitio.

## 2) ¿Para qué sirve?
- Para que navegadores, apps y herramientas encuentren **metadatos** sin buscar por todo el sitio.
- Para publicar contactos de seguridad, políticas de correo, detalles de inicio de sesión con proveedores externos, etc.
- Para la gente de ciberseguridad, ayuda a **mapear** rápidamente cómo está montado un servicio (sin contraseñas ni secretos).

## 3) Ejemplos famosos dentro de `/.well-known`
- `security.txt`: cómo contactar con el equipo de seguridad del sitio si encuentras un fallo.
- `openid-configuration`: dice dónde están los endpoints de inicio de sesión con **OpenID Connect** (autenticación).
- `mta-sts.txt`: política para hacer **más seguro** el correo del dominio.
- `assetlinks.json`: vincula una web con su app móvil oficial.
- `change-password`: ruta estándar para llevar al usuario a la página de **cambiar contraseña**.

No todos los sitios tienen estos archivos, pero muchos servicios modernos sí.

## 4) ¿Cómo lo miro en la práctica?
Puedes probar en cualquier dominio (mejor el tuyo) desde el navegador o con `curl`:

```bash
# Abrir el índice (si existe)
https://ejemplo.com/.well-known/

# Ver security.txt
https://ejemplo.com/.well-known/security.txt
# A veces está en la raíz también:
https://ejemplo.com/security.txt

# OpenID Connect (descubrimiento)
https://ejemplo.com/.well-known/openid-configuration

# Política MTA-STS (correo)
https://ejemplo.com/.well-known/mta-sts.txt

# Asset Links (apps móviles)
https://ejemplo.com/.well-known/assetlinks.json
```

Tip: A veces esto está en **subdominios** como `auth.ejemplo.com`, `login.ejemplo.com` o `accounts.ejemplo.com`. Pruébalos si el principal no responde.

## 5) ¿Qué información aparece?
Depende del archivo. Un ejemplo típico (muy resumido) del de OpenID Connect devuelve un JSON así:

```json
{
  "issuer": "https://ejemplo.com",
  "authorization_endpoint": "https://ejemplo.com/oauth2/authorize",
  "token_endpoint": "https://ejemplo.com/oauth2/token",
  "userinfo_endpoint": "https://ejemplo.com/oauth2/userinfo",
  "jwks_uri": "https://ejemplo.com/oauth2/jwks"
}
```

Traducción rápida:
- **authorization_endpoint**: dónde empieza el inicio de sesión.
- **token_endpoint**: dónde se obtienen los tokens tras iniciar sesión.
- **userinfo_endpoint**: dónde preguntar por los datos básicos del usuario ya autenticado.
- **jwks_uri**: dónde están las **claves públicas** para verificar firmas (sin secretos).

## 6) ¿Es peligroso publicar esto?
No, si está **bien configurado**. Son **metadatos públicos** pensados para ayudar a integrar servicios.
Consejos básicos si administras un sitio:
- No pongas **secretos** en estos archivos (solo información pública).
- Revisa que las URLs apunten a entornos correctos (no a “staging” o internos).
- En OpenID Connect, usa algoritmos seguros (p. ej., RS256) y valida bien el **issuer** y la **audiencia**.
- Mantén políticas al día (por ejemplo, `security.txt` con un email válido).

## 7) Glosario súper breve
- **Endpoint**: una dirección URL donde un servicio hace algo concreto (autorizar, dar tokens, etc.).
- **Metadatos**: datos “sobre” el servicio (no el contenido principal).
- **OpenID Connect (OIDC)**: estándar para autenticar usuarios (va “encima” de OAuth 2.0).
- **JWKS**: conjunto de claves públicas usadas para validar firmas de tokens.

## 8) Checklist rápido para tus pruebas
- [ ] ¿Existe la carpeta `/.well-known/` en el dominio o subdominio?
- [ ] ¿Hay `security.txt` con contacto válido?
- [ ] ¿Usa login externo? Prueba `/.well-known/openid-configuration`.
- [ ] ¿Tiene correo propio? Mira `/.well-known/mta-sts.txt`.
- [ ] ¿Hay app móvil oficial? Prueba `/.well-known/assetlinks.json`.
- [ ] ¿Evita revelar rutas internas o información innecesaria?

## 9) Para saber más (si te pica la curiosidad)
- RFC 8615 — Well‑Known URIs (estándar que lo define)
- RFC 9116 — `security.txt`
- RFC 8461 — MTA‑STS (correo seguro)
- “OpenID Connect Discovery” (documentación de OIDC)
- Registro oficial de IANA de `/.well-known` (lista completa de sufijos)
