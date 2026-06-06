# 📡 Documentación de la API

La API REST de SaludYa está construida con **Express** y documentada con **Swagger / OpenAPI 3.0**.

- **Swagger UI (local):** http://localhost:3001/api-docs
- **Swagger UI (producción):** https://saludyacicd.onrender.com/api-docs
- **Especificación estática:** [`openapi.yaml`](https://github.com/Alejandro-OrtizG/saludYaCICD/blob/main/openapi.yaml)

**URL base**
- Local: `http://localhost:3001`
- Producción: `https://saludyacicd.onrender.com`

Todas las peticiones y respuestas usan **JSON**.

---

## Endpoints

### 🔐 `POST /login`
Autentica a un usuario validando correo y contraseña.

**Cuerpo de la petición**
```json
{
  "email": "demo@saludya.com",
  "password": "123456"
}
```

**Respuesta `200`**
```json
{
  "success": true,
  "user": {
    "id": 1,
    "nombre": "Paciente Demo",
    "email": "demo@saludya.com",
    "telefono": "3000000000",
    "tipo_id": "CC",
    "numero_id": "12345678",
    "rh": "O+"
  }
}
```
Si las credenciales no coinciden: `{ "success": false }`.

---

### 📝 `POST /register`
Registra un nuevo usuario (paciente). El correo debe ser único.

**Cuerpo de la petición**
```json
{
  "nombre": "Juan Pérez",
  "email": "juan@correo.com",
  "password": "claveSegura123",
  "telefono": "3001234567",
  "tipo_id": "CC",
  "numero_id": "1090123456",
  "rh": "O+"
}
```

**Respuesta `200`**
```json
{ "success": true, "id": 4 }
```
Si el correo ya existe u ocurre un error: `{ "success": false }`.

---

### 👤 `GET /usuario/{email}`
Obtiene los datos de un usuario por su correo.

**Parámetro de ruta**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `email` | string | Correo del usuario a consultar |

**Ejemplo:** `GET /usuario/demo@saludya.com`

**Respuesta `200`**
```json
{
  "success": true,
  "user": { "id": 1, "nombre": "Paciente Demo", "email": "demo@saludya.com" }
}
```

---

### 📅 `POST /citas`
Crea una nueva cita médica.

**Cuerpo de la petición**
```json
{
  "paciente_email": "demo@saludya.com",
  "especialidad": "Medicina general",
  "medico": "Paula García",
  "fecha": "2026-06-15",
  "hora": "09:30"
}
```

**Respuesta `200`**
```json
{ "success": true, "id": 7 }
```

> El frontend valida que la fecha no caiga en fin de semana antes de enviar la petición.

---

### 📋 `GET /citas/{email}`
Lista todas las citas de un paciente.

**Parámetro de ruta**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `email` | string | Correo del paciente |

**Ejemplo:** `GET /citas/demo@saludya.com`

**Respuesta `200`**
```json
{
  "success": true,
  "citas": [
    {
      "id": 1,
      "paciente_email": "demo@saludya.com",
      "especialidad": "Medicina general",
      "medico": "Paula García",
      "fecha": "2026-06-15",
      "hora": "09:30"
    }
  ]
}
```
Si el paciente no tiene citas, `citas` es un arreglo vacío.

---

## Tabla resumen

| Método | Endpoint | Descripción | Tag |
|--------|----------|-------------|-----|
| `POST` | `/login` | Inicia sesión | Autenticación |
| `POST` | `/register` | Registra un usuario | Autenticación |
| `GET` | `/usuario/{email}` | Consulta un usuario | Usuarios |
| `POST` | `/citas` | Crea una cita | Citas |
| `GET` | `/citas/{email}` | Lista citas de un paciente | Citas |

## Cómo se genera la documentación

La especificación OpenAPI se construye automáticamente con **`swagger-jsdoc`** a partir de las anotaciones `@openapi` escritas en los comentarios de `backend/server.js`, y se sirve con **`swagger-ui-express`** en la ruta `/api-docs`. La configuración (info, servidores y esquemas `Usuario` y `Cita`) está en `backend/swagger.js`.
