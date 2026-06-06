# 🗄️ Modelo de Base de Datos

SaludYa utiliza un modelo **relacional** implementado con **SQLite** (`better-sqlite3`). El archivo de base de datos (`saludya.db`) y sus tablas se crean automáticamente en el primer arranque del backend, desde `backend/database.js`.

## Tablas

### Tabla `usuarios`

Almacena los datos de acceso y personales de todos los usuarios del sistema (pacientes, médicos y administradores).

| Campo | Tipo | Restricciones | Descripción |
|-------|------|---------------|-------------|
| `id` | INTEGER | PK, AUTOINCREMENT | Identificador único |
| `nombre` | TEXT | — | Nombre completo |
| `email` | TEXT | UNIQUE | Correo electrónico (único) |
| `password` | TEXT | — | Contraseña |
| `telefono` | TEXT | — | Número de teléfono |
| `tipo_id` | TEXT | — | Tipo de identificación (CC, TI…) |
| `numero_id` | TEXT | — | Número de identificación |
| `rh` | TEXT | — | Grupo sanguíneo |

### Tabla `citas`

Registra las citas médicas agendadas por los pacientes.

| Campo | Tipo | Restricciones | Descripción |
|-------|------|---------------|-------------|
| `id` | INTEGER | PK, AUTOINCREMENT | Identificador único |
| `paciente_email` | TEXT | — | Correo del paciente (relación con `usuarios.email`) |
| `especialidad` | TEXT | — | Especialidad médica |
| `medico` | TEXT | — | Nombre del médico asignado |
| `fecha` | TEXT | — | Fecha de la cita (formato `YYYY-MM-DD`) |
| `hora` | TEXT | — | Hora de la cita (formato `HH:MM`) |

## Diagrama Entidad–Relación (conceptual)

```
┌─────────────────────────┐                ┌─────────────────────────┐
│        USUARIOS         │                │          CITAS          │
├─────────────────────────┤                ├─────────────────────────┤
│ id            (PK)      │                │ id            (PK)      │
│ nombre                  │  1        N    │ paciente_email   (FK*)  │
│ email         (UNIQUE)  │───────────────<│ especialidad            │
│ password                │   realiza      │ medico                  │
│ telefono                │                │ fecha                   │
│ tipo_id                 │                │ hora                    │
│ numero_id               │                │                         │
│ rh                      │                │                         │
└─────────────────────────┘                └─────────────────────────┘
```

> (*) La relación entre `citas.paciente_email` y `usuarios.email` es **lógica** (por correo), no una clave foránea declarada en el esquema actual.

## Cardinalidad

- Un **usuario** (paciente) puede solicitar **muchas citas**.
- Una **cita** pertenece a **un solo paciente**.

## Entidades del diseño original

El diseño completo del proyecto contemplaba además las entidades **Pacientes**, **Médicos**, **Agenda médica/disponibilidad** y **Recordatorios**. En la versión implementada (MVP) se consolidó la información de acceso en la tabla `usuarios` y se mantuvo la tabla `citas`, dejando las demás entidades planificadas para futuras ampliaciones.

## Inicialización y datos de demostración

Al arrancar, `database.js` inserta tres usuarios con `INSERT OR IGNORE` (evitando duplicados en reinicios):

| id | nombre | email | rol (por correo) |
|----|--------|-------|------------------|
| 1 | Paciente Demo | `demo@saludya.com` | Paciente |
| 2 | Administrador Demo | `admin@saludya.com` | Administrador |
| 3 | Medico Demo | `medico@saludya.com` | Médico |

Todas con la contraseña `123456`.

## Migración a MySQL (futuro)

El modelo relacional es directamente trasladable a MySQL. Bastaría con:

1. Sustituir `better-sqlite3` por `mysql2`.
2. Adaptar los tipos (`INTEGER PRIMARY KEY AUTOINCREMENT` → `INT PRIMARY KEY AUTO_INCREMENT`, `TEXT` → `VARCHAR`).
3. Configurar la cadena de conexión mediante variables de entorno.

La estructura de tablas y las consultas SQL permanecen prácticamente iguales.
