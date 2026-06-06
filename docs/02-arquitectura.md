# 🏗️ Arquitectura

SaludYa implementa una arquitectura **cliente–servidor basada en API REST**, separando la aplicación en tres capas independientes: frontend, backend y base de datos.

## Visión general

```
┌──────────────────────────┐      HTTP / JSON       ┌──────────────────────────┐
│         FRONTEND          │  ───────────────────▶  │          BACKEND          │
│   React (SPA) en Vercel   │                        │  Node.js + Express (REST) │
│                           │  ◀───────────────────  │        en Render          │
│  - Login / Registro       │                        │  - /login   - /register   │
│  - Dashboards por rol     │                        │  - /usuario - /citas      │
│  - Agendar / ver citas    │                        │                           │
└──────────────────────────┘                        └────────────┬─────────────┘
                                                                  │
                                                                  ▼
                                                     ┌──────────────────────────┐
                                                     │       BASE DE DATOS       │
                                                     │   SQLite (better-sqlite3) │
                                                     │   Tablas: usuarios, citas │
                                                     └──────────────────────────┘
```

## Capas del sistema (Modelo C4 — Nivel de contenedores)

### Frontend (React)
Capa de presentación. Es la interfaz gráfica con la que interactúan los usuarios. Construida con React y React Router (aplicación de página única, SPA). Se encarga de:

- Mostrar formularios de registro e inicio de sesión.
- Permitir la navegación entre vistas según el rol.
- Visualizar citas médicas y disponibilidad.
- Capturar la información ingresada por el usuario y enviarla a la API mediante `fetch`.

### Backend (Node.js + Express)
Capa de lógica. Procesa las solicitudes del frontend y ejecuta las reglas de negocio a través de una API REST. Sus responsabilidades:

- Autenticación y registro de usuarios.
- Consulta de perfiles.
- Creación y consulta de citas médicas.
- Comunicación con la base de datos.

### Base de datos (SQLite)
Capa de persistencia. Almacena de forma estructurada los usuarios y las citas. El modelo es relacional y migrable a MySQL.

## Decisiones de arquitectura (ADR)

| Decisión | Elección | Justificación |
|----------|----------|---------------|
| Lenguaje | JavaScript (frontend y backend) | Un solo lenguaje en todo el stack simplifica el desarrollo y el mantenimiento. |
| Framework frontend | React | Interfaces dinámicas, componentes reutilizables, gran ecosistema. |
| Framework backend | Express (Node.js) | Creación ágil de rutas y API REST escalable. |
| Arquitectura | Cliente–servidor / API REST | Separa frontend y backend, permitiendo desarrollo y despliegue independientes. |
| Base de datos | SQLite (`better-sqlite3`) | Motor relacional embebido, sin servidor; ideal para entornos académicos y de despliegue gratuito. El diseño relacional permite migrar a MySQL sin cambios de modelo. |
| Gestor de paquetes | pnpm (workspaces) | Monorepo eficiente que centraliza frontend y backend. |

> **Nota:** El documento de diseño original contempla MySQL como motor objetivo. La implementación usa SQLite por simplicidad; el modelo relacional es equivalente.

## Servicios externos

El diseño contempla la integración de servicios de correo electrónico para el envío de recordatorios y notificaciones de citas. Esta funcionalidad quedó planificada para iteraciones futuras (no implementada en la versión actual).

## Rutas del frontend (React Router)

| Ruta | Vista | Rol |
|------|-------|-----|
| `/` | Login | Todos |
| `/recover` | Recuperar contraseña | Todos |
| `/register-paciente` | Registro de paciente | Público |
| `/dashboard-admin` | Panel de administrador | Administrador |
| `/crear-medico` | Crear cuenta médica | Administrador |
| `/dashboard-medico` | Panel del médico | Médico |
| `/dashboard-paciente` | Panel del paciente | Paciente |
| `/agendar-cita` | Agendar nueva cita | Paciente |
| `/perfil` | Perfil del paciente | Paciente |
