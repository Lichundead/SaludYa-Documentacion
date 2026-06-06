# 👥 Metodología y Equipo

## Metodología SCRUM

El proyecto se desarrolló bajo el marco ágil **SCRUM**, organizando el trabajo en sprints de dos semanas que permitieron entregar funcionalidades de forma progresiva y priorizar las características clave: autenticación de usuarios, gestión de citas y dashboards por rol.

Cada sprint siguió el ciclo:

1. **Planificación:** definición de tareas y objetivos.
2. **Desarrollo:** implementación de funcionalidades.
3. **Revisión:** evaluación de los avances.
4. **Retrospectiva:** análisis del proceso para identificar mejoras.

## Roles del equipo

| Rol | Responsabilidad |
|-----|-----------------|
| **Product Owner** | Define y prioriza los requerimientos del sistema. |
| **Scrum Master** | Guía al equipo en la aplicación de la metodología y elimina impedimentos. |
| **Equipo de desarrollo** | Análisis, diseño y construcción de la solución. |

## Equipo

Proyecto desarrollado para la cátedra **Proyecto de Software** — Ingeniería de Software, Corporación Universitaria Iberoamericana.

| Integrante | Aporte principal |
|------------|------------------|
| Yaridiveth Soler Manjarres | Creación del repositorio y estrategia de ramas; módulos DashboardPaciente, CrearMedico y App.js |
| Diego Alejandro Ortiz Granados | Componentes Login y registro de pacientes; documento ADR y descripción de componentes |
| Sebastian Vallejo Ricaurte | Historias de usuario, tablero Trello, agendamiento de citas y recuperación de contraseña |
| Laura Lorena Forero Romero | Definición de la estructura general del sistema y componentes de visualización |
| Andrés Josué Guerrero Ortiz | Estrategia de validación de disponibilidad de citas (backend + reglas de negocio) |

**Docente:** Tatiana Cabrera

## Herramientas de trabajo

| Herramienta | Uso |
|-------------|-----|
| **Trello** | Gestión de tareas y seguimiento del progreso ([tablero](https://trello.com/b/KVnppLKI/saludya)) |
| **GitHub** | Control de versiones y trabajo colaborativo |
| **Figma** | Prototipado de interfaces antes de programarlas |
| **Visual Studio Code** | Entorno de desarrollo |
| **GitHub Actions** | Integración y despliegue continuo |

## Estrategia de ramas

- **`main`**: versión estable del proyecto.
- **`develop`**: integración de los avances antes de pasar a producción.
- Ramas por integrante/funcionalidad que se fusionan (merge) hacia la rama de desarrollo una vez finalizada la tarea.

Esta estrategia mantiene el orden, evita conflictos y facilita el seguimiento del trabajo de cada integrante.

## Estado de funcionalidades

| Funcionalidad | Estado |
|---------------|--------|
| Sistema de autenticación | ✅ Implementado |
| Gestión de pacientes | ✅ Implementado |
| Gestión de médicos | ✅ Implementado |
| Dashboards (admin, médico, paciente) | ✅ Implementado |
| Agendamiento de citas | ✅ Implementado |
| Persistencia en base de datos | ✅ Implementado |
| Backend Node.js + Frontend React | ✅ Implementado |
| Recuperación de contraseña completa | 🟡 Parcial |
| Despliegue avanzado | 🟡 Parcial |
| Notificaciones automáticas por correo | ❌ No implementado |

Las funcionalidades pendientes (notificaciones automáticas, recuperación de contraseña por correo, gestión avanzada de disponibilidad e integración con APIs externas) quedaron fuera del alcance por restricciones de tiempo y por requerir servicios externos no disponibles en el marco académico. El sistema quedó estructuralmente preparado para incorporarlas en futuras iteraciones.
