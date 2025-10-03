# API CRUD

## Introducción
Las APIs permiten interactuar con bases de datos usando métodos HTTP. Un ejemplo común es `api.php` que permite gestionar la tabla **city** en la base de datos.

Ejemplo:
```bash
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london ...
```

## CRUD
CRUD describe las 4 operaciones principales sobre datos:
| Operación | Método HTTP | Descripción |
|-----------|-------------|-------------|
| **Create** | POST    | Agrega datos a la tabla |
| **Read**   | GET     | Lee los datos especificados |
| **Update** | PUT     | Actualiza los datos de una fila específica |
| **Delete** | DELETE  | Elimina la fila especificada |

---

## Leer (Read)
Podemos leer datos especificando la tabla y el recurso:

```bash
curl http://<SERVER_IP>:<PORT>/api.php/city/london
```

Resultado en JSON:
```json
[{"city_name":"London","country_name":"(UK)"}]
```

Para formatear mejor la salida:
```bash
curl -s http://<SERVER_IP>:<PORT>/api.php/city/london | jq
```

Leer todos los datos:
```bash
curl -s http://<SERVER_IP>:<PORT>/api.php/city/ | jq
```

---

## Crear (Create)
Para crear nuevas entradas usamos `POST` con datos JSON:

```bash
curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City","country_name":"HTB"}' -H 'Content-Type: application/json'
```

Verificar la creación:
```bash
curl -s http://<SERVER_IP>:<PORT>/api.php/city/HTB_City | jq
```

---

## Actualizar (Update)
Para modificar datos usamos `PUT` y especificamos el recurso:

```bash
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City","country_name":"HTB"}' -H 'Content-Type: application/json'
```

Comprobar los cambios:
```bash
curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq
```

---

## Borrar (Delete)
Para eliminar un recurso:
```bash
curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City
```

Confirmar que fue eliminado:
```bash
curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq
# Resultado: []
```

---

## Notas
- **PATCH** se usa para actualizar parcialmente una entrada, mientras que **PUT** actualiza todo el recurso.
- Algunas APIs permiten **Update** para crear una entrada si no existe.
- En aplicaciones reales, el acceso suele requerir **cookies** o **tokens de autorización** (ej. JWT).
