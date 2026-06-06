# ⚙️ Despliegue y CI/CD

SaludYa cuenta con un pipeline de **Integración y Despliegue Continuo** implementado con **GitHub Actions**. El workflow se encuentra en [`.github/workflows/ci-cd.yml`](https://github.com/Alejandro-OrtizG/saludYaCICD/blob/main/.github/workflows/ci-cd.yml) y se ejecuta automáticamente en cada `push` y `pull request` sobre la rama `main`.

## Disparadores

```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
```

De esta forma, cualquier cambio propuesto se valida antes de integrarse al código principal.

## Jobs del pipeline

### 1. `frontend`
Ejecutado en `ubuntu-latest` sobre el directorio `frontend/`:

1. **Checkout** del código.
2. **Setup Node** versión 22.
3. **Instalar pnpm** (`corepack enable`).
4. **Instalar dependencias** (`pnpm install --no-frozen-lockfile`).
5. **Lint** con ESLint (`pnpm lint`).
6. **Pruebas** con Jest (`CI=true pnpm test --watchAll=false`).
7. **Build de producción** (`pnpm run build`).

### 2. `backend`
Ejecutado en `ubuntu-latest` sobre el directorio `backend/`:

1. **Checkout** del código.
2. **Setup Node** versión 22.
3. **Instalar pnpm**.
4. **Instalar dependencias**.
5. **Verificación de sintaxis** del servidor (`node --check server.js`).

### 3. `deploy`
Depende de que `frontend` y `backend` finalicen correctamente (`needs: [frontend, backend]`) y solo se ejecuta sobre la rama `main`. Prepara el despliegue hacia las plataformas de hosting.

```yaml
deploy:
  needs: [frontend, backend]
  if: github.ref == 'refs/heads/main'
```

## Flujo del pipeline

```
        push / pull_request → main
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
   ┌─────────┐        ┌─────────┐
   │ frontend │        │ backend │
   │ lint     │        │ check   │
   │ test     │        │ syntax  │
   │ build    │        │         │
   └────┬────┘        └────┬────┘
        └────────┬─────────┘
                 ▼
            ┌─────────┐
            │ deploy   │  (solo en main)
            └─────────┘
```

## Despliegue en producción

| Servicio | Plataforma | URL |
|----------|-----------|-----|
| Frontend | Vercel | https://salud-ya-cicd.vercel.app/ |
| Backend | Render | https://saludyacicd.onrender.com/ |

Ambas plataformas publican automáticamente las nuevas versiones cada vez que se integran cambios en la rama principal.

## Build automático

La compilación del frontend se realiza con:

```bash
pnpm run build
```

Esto verifica que la aplicación genere correctamente una versión optimizada para producción antes de desplegarse.

## Análisis estático (Lint)

```bash
pnpm lint
```

ESLint identifica errores de sintaxis y malas prácticas en el código antes de permitir el despliegue.

## Pruebas automáticas

```bash
CI=true pnpm test --watchAll=false
```

Jest ejecuta las pruebas configuradas en el proyecto React para validar el funcionamiento de componentes críticos.
