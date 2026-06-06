# 🚀 Instalación y Ejecución

## Requisitos previos

- **Node.js 22** o superior — https://nodejs.org/
- **pnpm 10** o superior (`corepack enable` o `npm install -g pnpm`)
- **Git**

## Instalación

```bash
git clone https://github.com/Alejandro-OrtizG/saludYaCICD.git
cd saludYaCICD
corepack enable          # habilita pnpm si no está activo
pnpm install             # instala dependencias de frontend y backend
```

El proyecto es un **monorepo** gestionado con pnpm workspaces (`pnpm-workspace.yaml`), por lo que `pnpm install` instala las dependencias de ambos servicios en una sola ejecución.

## Variables de entorno

Crea un archivo `frontend/.env` con la URL del backend:

```env
# Desarrollo local
REACT_APP_API_URL=http://localhost:3001

# Producción
# REACT_APP_API_URL=https://saludyacicd.onrender.com
```

Opcionalmente, en `backend/.env` puedes definir el puerto:

```env
PORT=3001
```

> Los archivos `.env` están en `.gitignore` y nunca deben subirse al repositorio.

## Ejecución local

Abre **dos terminales**, una para cada servicio.

### Terminal 1 — Backend (API)

```bash
cd backend
pnpm start
```

- API disponible en **http://localhost:3001**
- Documentación Swagger en **http://localhost:3001/api-docs**
- En el primer arranque se crea el archivo `saludya.db` con las tablas y los usuarios de demostración.

### Terminal 2 — Frontend (React)

```bash
cd frontend
pnpm start
```

- Aplicación disponible en **http://localhost:3000**

## Cuentas de demostración

| Rol | Correo | Contraseña |
|-----|--------|-----------|
| Administrador | `admin@saludya.com` | `123456` |
| Médico | `medico@saludya.com` | `123456` |
| Paciente | `demo@saludya.com` | `123456` |

> El rol se determina por el contenido del correo: si incluye `admin` se redirige al panel de administrador, si incluye `medico` al panel médico, y en cualquier otro caso al panel del paciente.

## Scripts disponibles

### Frontend (`frontend/`)

| Comando | Acción |
|---------|--------|
| `pnpm start` | Inicia el servidor de desarrollo |
| `pnpm run build` | Genera la versión optimizada de producción |
| `pnpm test` | Ejecuta las pruebas con Jest |
| `pnpm lint` | Analiza el código con ESLint |

### Backend (`backend/`)

| Comando | Acción |
|---------|--------|
| `pnpm start` | Inicia el servidor Express |
