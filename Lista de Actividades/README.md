# Sistema de Gestión de Incidentes

Proyecto académico orientado al desarrollo de un sistema para el registro, consulta y visualización de incidentes utilizando una arquitectura basada en **API REST y arquitectura MVC**.

El sistema permite registrar incidentes, consultarlos mediante filtros y visualizarlos en un mapa geográfico.

---

# Tecnologías utilizadas

## Backend
- C#
- .NET
- API REST
- Swagger

## Base de datos
- SQL Server

## Frontend

## Herramientas
- Git
- Visual Studio / VS Code
- Swagger

---

# Estructura del Proyecto (EDT / WBS)

El proyecto se divide en 10 fases principales:

1. Inicio del proyecto  
2. Análisis de requerimientos  
3. Diseño del sistema  
4. Configuración del entorno de desarrollo  
5. Desarrollo Backend (API)  
6. Desarrollo Frontend  
7. Integración del sistema  
8. Pruebas del sistema  
9. Validación del proyecto  
10. Cierre del proyecto  

---

# Lista de Actividades del Proyecto

| ID | Actividad | EDT | Recursos | Predecesora | Sucesora |
|----|----------|-----|----------|-------------|-----------|
| A1 | Identificar stakeholders del proyecto | 1.1.1 | Equipo | - | A2 |
| A2 | Definir problema a resolver | 1.1.2 | Equipo | A1 | A3 |
| A3 | Definir alcance del proyecto | 1.1.3 | Equipo | A2 | A4 |
| A4 | Definir objetivos generales del sistema | 1.1.4 | Equipo | A3 | A5 |
| A5 | Definir objetivos específicos | 1.1.5 | Equipo | A4 | A6 |
| A6 | Definir restricciones del proyecto | 1.1.6 | Equipo | A5 | A7 |
| A7 | Definir entregables del proyecto | 1.1.7 | Equipo | A6 | A8 |
| A8 | Elaborar documento de visión del sistema | 1.1.8 | Analista | A7 | A9 |
| A9 | Validar alcance con el equipo y docente | 1.1.9 | Equipo | A8 | A10 |
| A10 | Identificar actores del sistema | 2.1.1 | Analista | A9 | A11 |
| A11 | Definir casos de uso principales | 2.1.2 | Analista | A10 | A12 |
| A12 | Documentar casos de uso del sistema | 2.1.3 | Analista | A11 | A13 |
| A13 | Definir flujo principal de cada caso de uso | 2.1.4 | Analista | A12 | A14 |
| A14 | Definir flujos alternativos | 2.1.5 | Analista | A13 | A15 |
| A15 | Priorizar requerimientos funcionales | 2.1.6 | Equipo | A14 | A16 |
| A16 | Validar requerimientos con el equipo | 2.1.7 | Equipo | A15 | A17 |
| A17 | Definir requerimientos de seguridad | 2.2.1 | Backend | A16 | A18 |
| A18 | Definir requerimientos de rendimiento | 2.2.2 | Backend | A17 | A19 |
| A19 | Definir requerimientos de disponibilidad | 2.2.3 | Backend | A18 | A20 |
| A20 | Definir requerimientos de usabilidad | 2.2.4 | Frontend | A19 | A21 |
| A21 | Definir requerimientos de escalabilidad | 2.2.5 | Backend | A20 | A22 |
| A22 | Definir requerimientos de compatibilidad | 2.2.6 | Backend | A21 | A23 |
| A23 | Documentar estándares de desarrollo | 2.2.7 | Equipo | A22 | A24 |
| A24 | Seleccionar arquitectura del sistema | 3.1.1 | Backend | A23 | A25 |
| A25 | Diseñar arquitectura en capas (MVC) | 3.1.2 | Backend | A24 | A26 |
| A26 | Definir estructura del backend | 3.1.3 | Backend | A25 | A27 |
| A27 | Definir estructura del frontend | 3.1.4 | Frontend | A26 | A28 |
| A28 | Definir patrón de acceso a datos | 3.1.5 | Backend | A27 | A29 |
| A29 | Definir estructura de API REST | 3.1.6 | Backend | A28 | A30 |
| A30 | Diseñar diagrama de componentes | 3.1.7 | Arquitecto | A29 | A31 |
| A31 | Diseñar diagrama de despliegue | 3.1.8 | Arquitecto | A30 | A32 |
| A32 | Identificar entidades principales | 3.2.1 | Backend | A31 | A33 |
| A33 | Definir atributos de cada entidad | 3.2.2 | Backend | A32 | A34 |
| A34 | Definir relaciones entre entidades | 3.2.3 | Backend | A33 | A35 |
| A35 | Diseñar modelo conceptual | 3.2.4 | Backend | A34 | A36 |
| A36 | Diseñar modelo lógico | 3.2.5 | Backend | A35 | A37 |
| A37 | Normalizar tablas | 3.2.6 | BD | A36 | A38 |
| A38 | Diseñar modelo físico | 3.2.7 | BD | A37 | A39 |
| A39 | Definir claves primarias | 3.2.8 | BD | A38 | A40 |
| A40 | Definir claves foráneas | 3.2.9 | BD | A39 | A41 |

---

# Arquitectura del Sistema

El sistema utiliza una arquitectura basada en **MVC (Model - View - Controller)**.

## Capas del sistema

### Model
Contiene las entidades del sistema y la lógica de negocio.

### Controller
Gestiona las solicitudes HTTP de la API.

### View
Representa la interfaz gráfica del sistema.

---

# API REST

La API permite gestionar incidentes mediante operaciones CRUD:

- Crear incidentes
- Consultar incidentes
- Actualizar incidentes
- Eliminar incidentes
- Filtrar por fecha
- Filtrar por localidad

La documentación de la API se genera automáticamente mediante **Swagger**.

---

# Pruebas del sistema

Se realizan tres tipos principales de pruebas:

### Pruebas unitarias
Verifican el funcionamiento de componentes individuales.

### Pruebas de integración
Verifican la interacción entre módulos.

### Pruebas funcionales
Validan el funcionamiento completo del sistema.

---

# Documentación

El proyecto incluye:

- Documentación de arquitectura
- Documentación de base de datos
- Documentación de API
- Manual técnico
- Manual de usuario

---

# Entrega final

La entrega final incluye:

- Código fuente del sistema
- Documentación técnica
- Documentación de usuario
- Presentación del proyecto
