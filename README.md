# 📚 SaludYa — Documentación Técnica

Repositorio de **documentación técnica** del proyecto **SaludYa**, una aplicación web para la gestión y agendamiento de citas médicas en consultorios de pequeña y mediana demanda.

Aquí se centraliza toda la documentación que facilita la comprensión, instalación y mantenimiento del sistema: descripción, arquitectura, modelo de datos, documentación de la API (Swagger), documentación del código (JSDoc) y la Wiki del proyecto.

> 📦 **Repositorio del código:** https://github.com/Alejandro-OrtizG/saludYaCICD
> 🌐 **Aplicación (Vercel):** https://salud-ya-cicd.vercel.app/
> 🔌 **API (Render):** https://saludyacicd.onrender.com/

---

## 🗂️ Contenido de este repositorio

| Carpeta / archivo                             | Descripción                                                                                          |
| --------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| [`docs/`](./docs)                             | Documentación detallada (instalación, arquitectura, base de datos, API, CI/CD, equipo)               |
| [`api/`](./api)                               | Documentación Swagger navegable (`index.html`) y especificación [`openapi.yaml`](./api/openapi.yaml) |
| [`codigo-documentado/`](./codigo-documentado) | Código del backend comentado con **JSDoc** + documentación HTML generada                             |
| [`wiki/`](./wiki)                             | Páginas listas para publicar en la **Wiki** de GitHub                                                |
| [`INSTRUCCIONES.md`](./INSTRUCCIONES.md)      | Cómo publicar cada pieza (Wiki, GitHub Pages, repo de código)                                        |

### Accesos directos a la documentación

- 🚀 [Instalación y ejecución](./docs/01-instalacion-y-ejecucion.md)
- 🏗️ [Arquitectura](./docs/02-arquitectura.md)
- 🗄️ [Modelo de base de datos](./docs/03-modelo-de-base-de-datos.md)
- 📡 [Documentación de la API](./docs/04-documentacion-de-la-api.md)
- ⚙️ [Despliegue y CI/CD](./docs/05-despliegue-y-cicd.md)
- 👥 [Metodología y equipo](./docs/06-metodologia-y-equipo.md)

---

## Descripción del proyecto

En muchos consultorios médicos la asignación de citas se realiza de forma manual (agendas físicas, llamadas, registros informales), lo que provoca duplicación de citas, pérdida de información, errores de horario y dependencia del personal administrativo. **SaludYa** digitaliza este proceso ofreciendo:

- **Registro e inicio de sesión** con distinción por rol.
- **Gestión de roles**: administrador, médico y paciente, cada uno con su panel.
- **Agendamiento de citas** por especialidad, médico, fecha y hora.
- **Visualización de citas** del paciente.
- **Administración de médicos** desde el panel de administrador.
- **Persistencia de datos** en una base de datos relacional.

Desarrollado bajo metodología **SCRUM** para la cátedra _Proyecto de Software_ de la Corporación Universitaria Iberoamericana.

---

## Tecnologías utilizadas

| Capa                 | Tecnología                                       | Versión    |
| -------------------- | ------------------------------------------------ | ---------- |
| Frontend             | React + React Router (Create React App)          | 19.x / 6.x |
| Backend              | Node.js + Express                                | 22.x / 5.x |
| Base de datos        | SQLite (`better-sqlite3`)                        | 11.x       |
| Documentación API    | swagger-jsdoc + swagger-ui-express (OpenAPI 3.0) | —          |
| Documentación código | JSDoc                                            | 4.x        |
| Gestor de paquetes   | pnpm (monorepo con workspaces)                   | 10.x       |
| CI/CD                | GitHub Actions                                   | —          |
| Hosting              | Vercel (frontend) / Render (backend)             | —          |

> **Nota:** el documento de diseño contempla **MySQL** como motor objetivo; la implementación usa **SQLite** por simplicidad y portabilidad. El modelo relacional es equivalente y migrable. Ver [detalles](./docs/03-modelo-de-base-de-datos.md#migración-a-mysql-futuro).

---

## Arquitectura general

SaludYa sigue una arquitectura **cliente–servidor basada en API REST** en tres capas independientes:

```
┌──────────────────────────┐      HTTP / JSON       ┌──────────────────────────┐
│         FRONTEND          │  ───────────────────▶  │          BACKEND          │
│   React (SPA) en Vercel   │                        │  Node.js + Express (REST) │
│                           │  ◀───────────────────  │        en Render          │
│  - Login / Registro       │                        │  - /login   - /register   │
│  - Dashboards por rol     │                        │  - /usuario - /citas      │
│  - Agendar / ver citas    │                        │                           │
└──────────────────────────┘                        └────────────┬─────────────┘
                                                                  ▼
                                                     ┌──────────────────────────┐
                                                     │   SQLite (better-sqlite3) │
                                                     │   Tablas: usuarios, citas │
                                                     └──────────────────────────┘
```

Explicación completa (diagramas C4, ER y decisiones ADR) en [docs/02-arquitectura.md](./docs/02-arquitectura.md).

---

## Guía de instalación (resumen)

Requisitos: **Node.js 22+**, **pnpm 10+**, **Git**.

```bash
# Clonar el repositorio del CÓDIGO
git clone https://github.com/Alejandro-OrtizG/saludYaCICD.git
cd saludYaCICD
corepack enable
pnpm install
```

Ejecución local (dos terminales):

```bash
# Terminal 1 — Backend (API + Swagger en /api-docs)
cd backend && pnpm start          # http://localhost:3001

# Terminal 2 — Frontend (React)
cd frontend && pnpm start         # http://localhost:3000
```

Antes de iniciar el frontend, crea `frontend/.env` con:

```env
REACT_APP_API_URL=http://localhost:3001
```

Guía detallada en [docs/01-instalacion-y-ejecucion.md](./docs/01-instalacion-y-ejecucion.md).

---

## Documentación de la API (Swagger)

La API REST está documentada con **Swagger / OpenAPI 3.0**. Hay tres formas de consultarla:

1. **En vivo** (con el backend corriendo): http://localhost:3001/api-docs o https://saludyacicd-54ta.onrender.com/api-docs
2. **Navegable desde este repo** (GitHub Pages): publica la carpeta `api/` y abre `index.html`
3. **Especificación estática**: [`api/openapi.yaml`](./api/openapi.yaml), importable en https://editor.swagger.io/.

| Método | Endpoint           | Descripción                    |
| ------ | ------------------ | ------------------------------ |
| `POST` | `/login`           | Autentica a un usuario         |
| `POST` | `/register`        | Registra un nuevo usuario      |
| `GET`  | `/usuario/{email}` | Obtiene un usuario por correo  |
| `POST` | `/citas`           | Crea una cita médica           |
| `GET`  | `/citas/{email}`   | Lista las citas de un paciente |

Detalle en [docs/04-documentacion-de-la-api.md](./docs/04-documentacion-de-la-api.md).

---

## Documentación del código (JSDoc)

El backend está comentado con **JSDoc** (descripción, `@param`, `@returns`) en cada función y módulo. En [`codigo-documentado/`](./codigo-documentado) encontrarás:

- Los archivos fuente comentados (`server.js`, `swagger.js`, `database.js`).
- La documentación **HTML generada** con JSDoc en [`codigo-documentado/jsdoc-html/`](./codigo-documentado/jsdoc-html).

Para regenerarla:

```bash
cd backend
npx jsdoc server.js database.js swagger.js -d ./docs-html
```

---

## Wiki

La carpeta [`wiki/`](./wiki) contiene las páginas listas para la **Wiki de GitHub** de este repositorio. Una vez publicada estará en:

`https://github.com/Lichundead/SaludYa-Documentacion/wiki`

---

## Equipo

Ingeniería de Software — Corporación Universitaria Iberoamericana · Cátedra _Proyecto de Software_.

- Yaridiveth Soler Manjarres
- Diego Alejandro Ortiz Granados
- Sebastian Vallejo Ricaurte
- Laura Lorena Forero Romero
- Andrés Josué Guerrero Ortiz

**Docente:** Tatiana Cabrera

## Licencia

MIT
