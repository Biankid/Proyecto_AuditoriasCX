# ProyectoFINAL_AuditoriasCX
# Proyecto de Base de Datos para el Equipo de Auditores de CX

## Introducción

En el entorno empresarial actual, las auditorías de customer experience (CX) se han convertido en un componente crucial para mejorar la satisfacción del cliente y optimizar las operaciones internas. La gestión de estas auditorías, que involucra a múltiples empresas y equipos de auditores, requiere una estructura sólida de datos que permita manejar eficientemente la información relacionada con los casos auditados, las condiciones laborales, los indicadores de desempeño, y las relaciones con los clientes.

El presente proyecto tiene como objetivo diseñar y desarrollar una base de datos que centralice toda la información necesaria para la administración de auditorías de customer experience, asegurando la integridad de los datos y facilitando el acceso rápido y preciso a la información relevante. Este sistema mejorará la visibilidad del desempeño de los auditores y de las empresas auditadas, optimizando la toma de decisiones estratégicas y operativas

## Objetivo

Desarrollar una base de datos relacional que permita al equipo de auditores de customer experience gestionar eficientemente todos los aspectos relacionados con las auditorías, incluyendo la asignación de proyectos, el seguimiento del desempeño mediante indicadores clave, la administración de las condiciones laborales y de pago, y la generación de informes detallados. Esta base de datos debe facilitar la centralización de la información, mejorar la calidad del análisis de datos, reducir la duplicación de información y brindar soporte en la toma de decisiones estratégicas y operativas del equipo.

## Diagramas de entidad relación de la base de datos

### Diagrama de Entidad-Relación - Se adjunta en el final del script general, DER simplificado, aquí el detalle:

- **Relaciones:**

- `Empresa a Auditor:` Uno a Muchos (Una empresa puede tener varios auditores).
- `Auditor a Caso:`Uno a Muchos (Un auditor puede manejar varios casos).
- `Caso a CasoKPI:` Uno a Muchos (Un caso puede tener varios KPIs asociados).
- `KPI a CasoKPI:` Uno a Muchos (Un KPI puede aplicarse a varios casos).
- `Auditor a AuditorProyecto:` Uno a Muchos (Un auditor puede estar asignado a varios proyectos).
- `Caso a Case_Escalations:` Uno a Muchos (Un caso puede tener varias escalaciones).
- `Caso a Customer_Satisfaction:` Uno a Uno (Cada caso tiene una evaluación de satisfacción del cliente).
- `Auditor a Training_Sessions:` Uno a Muchos (Un auditor puede participar en varias sesiones de capacitación).
- `Auditor a Performance_Metrics:` Uno a Muchos (Un auditor puede tener varias métricas de desempeño).
- `Caso a Data_Corrections:` Uno a Muchos (Un caso puede tener varias correcciones de datos).
- `Caso a Audit_Schedules`: Uno a Muchos (Un caso puede tener varios cronogramas de auditoría).
- `Condicion a Empresa, Auditor, Caso, Payment_Conditions:` Uno a Muchos (Una condición puede estar asociada con varias empresas, auditores, casos, y condiciones de pago).

## Listado de tablas

### Empresa

Almacena información sobre las empresas que proporcionan los servicios de auditoría.
- **Atributos**:
  - `id_empresa`: Identificador único de la empresa.
  - `razon_social`: Nombre legal de la empresa.
  - `direccion`: Dirección física de la empresa.
  - `localidad`: Ciudad o localidad donde se encuentra la empresa.
  - `telefono`: Número de teléfono de la empresa.
  - `correo`: Dirección de correo electrónico de la empresa.
  - `id_condicion`: Referencia a la tabla `Condicion` para las condiciones de pago.

| PK/FK | Atributo     | TIPO      | Tamaño | NULIDAD   | AUTOINC. | DEFAULT | DESCRIPCIÓN                              |
|-------|--------------|-----------|--------|-----------|----------|---------|------------------------------------------|
| PK    | id_empresa   | INT       |        | NOT NULL  | SI       |         | Identificador único de la empresa        |
|       | razon_social | VARCHAR   | 255    | NOT NULL  | NO       |         | Nombre legal de la empresa               |
|       | direccion    | VARCHAR   | 255    | NOT NULL  | NO       |         | Dirección física de la empresa           |
|       | localidad    | VARCHAR   | 255    | NOT NULL  | NO       |         | Ciudad o localidad                       |
|       | telefono     | VARCHAR   | 20     | NOT NULL  | NO       |         | Número de teléfono                       |
|       | correo       | VARCHAR   | 255    | NOT NULL  | NO       |         | Correo electrónico                       |
| FK    | id_condicion | INT       |        | NOT NULL  | NO       |         | Referencia a la tabla `Condicion`        |

### Auditor

Contiene información sobre los auditores que realizan las auditorías de casos de customer experience.
- **Atributos**:
  - `id_auditor`: Identificador único del auditor.
  - `nombre`: Nombre del auditor.
  - `apellido`: Apellido del auditor.
  - `telefono`: Número de teléfono del auditor.
  - `correo`: Dirección de correo electrónico del auditor.
  - `id_empresa`: Referencia a la tabla `Empresa` indicando a qué empresa pertenece el auditor.
  - `id_condicion`: Referencia a la tabla `Condicion` para las condiciones de trabajo.

| PK/FK | Atributo     | TIPO      | Tamaño | NULIDAD   | AUTOINC. | DEFAULT | DESCRIPCIÓN                              |
|-------|--------------|-----------|--------|-----------|----------|---------|------------------------------------------|
| PK    | id_auditor   | INT       |        | NOT NULL  | SI       |         | Identificador único del auditor          |
|       | nombre       | VARCHAR   | 255    | NOT NULL  | NO       |         | Nombre del auditor                       |
|       | apellido     | VARCHAR   | 255    | NOT NULL  | NO       |         | Apellido del auditor                     |
|       | telefono     | VARCHAR   | 20     | NOT NULL  | NO       |         | Número de teléfono                       |
|       | correo       | VARCHAR   | 255    | NOT NULL  | NO       |         | Correo electrónico                       |
| FK    | id_empresa   | INT       |        | NOT NULL  | NO       |         | Referencia a la tabla `Empresa`          |
| FK    | id_condicion | INT       |        | NOT NULL  | NO       |         | Referencia a la tabla `Condicion`        |

### Caso

Registra los casos de auditoría realizados por los auditores.
- **Atributos**:
  - `id_caso`: Identificador único del caso.
  - `id_auditor`: Referencia a la tabla `Auditor` indicando qué auditor está asignado al caso.
  - `fecha_inicio`: Fecha de inicio del caso.
  - `fecha_fin`: Fecha de finalización del caso.
  - `descripcion`: Descripción del caso.
  - `id_condicion`: Referencia a la tabla `Condicion` para las condiciones aplicables al caso.

| PK/FK | Atributo      | TIPO      | Tamaño | NULIDAD   | AUTOINC. | DEFAULT | DESCRIPCIÓN                              |
|-------|---------------|-----------|--------|-----------|----------|---------|------------------------------------------|
| PK    | id_caso       | INT       |        | NOT NULL  | SI       |         | Identificador único del caso             |
| FK    | id_auditor    | INT       |        | NOT NULL  | NO       |         | Referencia a la tabla `Auditor`          |
|       | fecha_inicio  | DATE      |        | NOT NULL  | NO       |         | Fecha de inicio del caso                 |
|       | fecha_fin     | DATE      |        | NOT NULL  | NO       |         | Fecha de finalización del caso           |
|       | descripcion   | TEXT      |        | NOT NULL  | NO       |         | Descripción del caso                     |
| FK    | id_condicion  | INT       |        | NOT NULL  | NO       |         | Referencia a la tabla `Condicion`        |

### KPI

Define los indicadores clave de desempeño (KPIs) utilizados para evaluar los casos de auditoría.

- **Atributos**:
  - `id_kpi`: Identificador único del KPI.
  - `descripcion`: Descripción del KPI.
  - `tipo`: Tipo de KPI (Cuantitativo o Cualitativo).

| PK/FK | Atributo     | TIPO      | Tamaño | NULIDAD   | AUTOINC. | DEFAULT | DESCRIPCIÓN                              |
|-------|--------------|-----------|--------|-----------|----------|---------|------------------------------------------|
| PK    | id_kpi       | INT       |        | NOT NULL  | SI       |         | Identificador único del KPI              |
|       | descripcion  | VARCHAR   | 255    | NOT NULL  | NO       |         | Descripción del KPI                      |
|       | tipo         | VARCHAR   | 50     | NOT NULL  | NO       |         | Tipo de KPI (Cuantitativo o Cualitativo) |

### CasoKPI

Asocia los KPIs con los casos específicos y almacena sus valores.

- **Atributos**:
  
  - `id_caso`: Referencia a la tabla `Caso` indicando el caso evaluado.
  - `id_kpi`: Referencia a la tabla `KPI` indicando el KPI utilizado.
  - `valor`: Valor del KPI para el caso específico.

| PK/FK | Atributo     | TIPO      | Tamaño | NULIDAD   | AUTOINC. | DEFAULT | DESCRIPCIÓN                              |
|-------|--------------|-----------|--------|-----------|----------|---------|------------------------------------------|
| PK,FK | id_caso      | INT       |        | NOT NULL  | NO       |         | Referencia a la tabla `Caso`             |
| PK,FK | id_kpi       | INT       |        | NOT NULL  | NO       |         | Referencia a la tabla `KPI`              |
|       | valor        | DECIMAL   | 10, 2  | NOT NULL  | NO       |         | Valor del KPI para el caso específico    |

### Condicion

Almacena información sobre las condiciones de pago y trabajo disponibles.
- **Atributos**:
  - `id_condicion`: Identificador único de la condición.
  - `descripcion`: Descripción de la condición.
  - `precio`: Precio asociado a la condición.
  - `plazo`: Plazo de pago en días.

| PK/FK | Atributo     | TIPO      | Tamaño | NULIDAD   | AUTOINC. | DEFAULT  | DESCRIPCIÓN                              |
|-------|--------------|-----------|--------|-----------|----------|----------|------------------------------------------|
| PK    | id_condicion | INT       |        | NOT NULL  | SI       |          | Identificador único de la condición      |
|       | descripcion  | VARCHAR   | 255    | NOT NULL  | NO       |          | Descripción de la condición              |
|       | precio       | DECIMAL   | 10, 2  | NOT NULL  | NO       | 5000.00  | Precio asociado a la condición           |
|       | plazo        | INT       |        | NOT NULL  | NO       | 0        | Plazo de pago en días                    |

### AuditorProyecto

Asocia los auditores con los proyectos en los que están trabajando.

- **Atributos**:
  - `id_auditor`: Referencia a la tabla `Auditor` indicando el auditor asignado.
  - `id_proyecto`: Referencia al proyecto específico.
  - `descripcion`: Descripción del proyecto.

| PK/FK | Atributo     | TIPO      | Tamaño | NULIDAD   | AUTOINC. | DEFAULT | DESCRIPCIÓN                              |
|-------|--------------|-----------|--------|-----------|----------|---------|------------------------------------------|
| PK,FK | id_auditor   | INT       |        | NOT NULL  | NO       |         | Referencia a la tabla `Auditor`          |
| PK,FK | id_proyecto  | INT       |        | NOT NULL  | NO       |         | Referencia al proyecto específico        |
|       | descripcion  | TEXT      |        | NOT NULL  | NO       |         | Descripción del proyecto                 |


### Training_Sessions

Almacena información sobre las sesiones de capacitación para los auditores.

- **Atributos:**
- 
- `id_session`: Identificador único de la sesión de capacitación.
- `id_auditor`: Referencia al auditor participante.
- `fecha`: Fecha de la sesión.
- `tema`: Tema o contenido de la sesión.
- `duracion`: Duración de la sesión.
- `feedback`: Comentarios o evaluación de la sesión.

| PK/FK | Atributo     | TIPO    | Tamaño | NULIDAD  | AUTOINC. | DEFAULT | DESCRIPCIÓN                           |
|-------|--------------|---------|--------|----------|----------|---------|---------------------------------------|
| PK    | id_session   | INT     |        | NOT NULL | SI       |         | Identificador único de la sesión      |
| FK    | id_auditor   | INT     |        | NOT NULL | NO       |         | Referencia al auditor                 |
|       | fecha        | DATE    |        | NOT NULL | NO       |         | Fecha de la sesión                    |
|       | tema         | VARCHAR | 255    | NOT NULL | NO       |         | Tema o contenido de la sesión         |
|       | duracion     | TIME    |        | NOT NULL | NO       |         | Duración de la sesión                 |
|       | feedback     | TEXT    |        | NULL     | NO       |         | Comentarios o evaluación de la sesión |

### Case_Escalations

Registra los casos que han sido escalados durante las auditorías.

- **Atributos:**
- 
- `id_escalation`: Identificador único de la escalación.
- `id_caso`: Referencia al caso auditado.
- `razon`: Razón de la escalación.
- `fecha_escalacion`: Fecha en la que se escaló el caso.
- `id_auditor_responsable`: Auditor responsable de la escalación.

| PK/FK | Atributo              | TIPO    | Tamaño | NULIDAD   | AUTOINC. | DEFAULT | DESCRIPCIÓN                             |
|-------|-----------------------|---------|--------|-----------|----------|---------|-----------------------------------------|
| PK    | id_escalation         | INT     |        | NOT NULL  | SI       |         | Identificador único de la escalación    |
| FK    | id_caso               | INT     |        | NOT NULL  | NO       |         | Referencia al caso auditado             |
|       | razon                 | VARCHAR | 255    | NOT NULL  | NO       |         | Razón de la escalación                  |
|       | fecha_escalacion      | DATE    |        | NOT NULL  | NO       |         | Fecha de la escalación                  |
| FK    | id_auditor_responsable| INT     |        | NOT NULL  | NO       |         | Auditor responsable de la escalación    |


### Customer_Satisfaction

Almacena información sobre la satisfacción del cliente con respecto a las auditorías realizadas

- **Atributos:**
- `id_satisfaction`: Identificador único de la satisfacción.
- `id_caso`: Referencia al caso auditado.
- `rating`: Calificación otorgada por el cliente.
- `comentarios`: Comentarios adicionales del cliente.
- `fecha`: Fecha de la evaluación de satisfacción.

| PK/FK | Atributo           | TIPO    | Tamaño | NULIDAD  | AUTOINC. | DEFAULT | DESCRIPCIÓN                                  |
|-------|--------------------|---------|--------|----------|----------|---------|----------------------------------------------|
| PK    | id_satisfaction    | INT     |        | NOT NULL | SI       |         | Identificador único de la satisfacción       |
| FK    | id_caso            | INT     |        | NOT NULL | NO       |         | Referencia al caso auditado                  |
|       | rating             | INT     |        | NOT NULL | NO       |         | Calificación del cliente                     |
|       | comentarios        | TEXT    |        | NULL     | NO       |         | Comentarios adicionales del cliente          |
|       | fecha              | DATE    |        | NOT NULL | NO       |         | Fecha de la evaluación de satisfacción       |

### Performance_Metrics

Métricas que evaluan el desempeño de los auditores.

-**Atributos**:
- `id_metric`: Identificador único de la métrica.
- `id_auditor`: Referencia al auditor.
- `id_kpi`: Referencia al KPI.
- `periodo`: Período de evaluación.
- `valor`: Valor del KPI en el período.

| PK/FK | Atributo     | TIPO    | Tamaño | NULIDAD  | AUTOINC. | DEFAULT | DESCRIPCIÓN                                    |
|-------|--------------|---------|--------|----------|----------|---------|------------------------------------------------|
| PK    | id_metric    | INT     |        | NOT NULL | SI       |         | Identificador único de la métrica              |
| FK    | id_auditor   | INT     |        | NOT NULL | NO       |         | Referencia al auditor                          |
| FK    | id_kpi       | INT     |        | NOT NULL | NO       |         | Referencia al KPI                              |
|       | periodo      | VARCHAR | 50     | NOT NULL | NO       |         | Período de evaluación                          |
|       | valor        | DECIMAL |        | NOT NULL | NO       |         | Valor del KPI en el período                    |


### Audit_Schedules

Cronograma de auditorías asignadas.

-**Atributos**:
- `id_schedule`: Identificador único del cronograma.
- `id_auditor`: Referencia al auditor.
- `id_caso`: Referencia al caso auditado.
- `fecha_programada`: Fecha programada para la auditoría.
- `estado`: Estado de la auditoría (pendiente, completada, etc.).

| PK/FK | Atributo           | TIPO    | Tamaño | NULIDAD  | AUTOINC. | DEFAULT | DESCRIPCIÓN                                  |
|-------|--------------------|---------|--------|----------|----------|---------|----------------------------------------------|
| PK    | id_schedule        | INT     |        | NOT NULL | SI       |         | Identificador único del cronograma          |
| FK    | id_auditor         | INT     |        | NOT NULL | NO       |         | Referencia al auditor                       |
| FK    | id_caso            | INT     |        | NOT NULL | NO       |         | Referencia al caso auditado                 |
|       | fecha_programada   | DATE    |        | NOT NULL | NO       |         | Fecha programada para la auditoría          |
|       | estado             | VARCHAR | 50     | NOT NULL | NO       |         | Estado de la auditoría (pendiente, completada, etc.) |


### Data_Corrections

Registros de correcciones de datos en casos.

-**Atributos**:
- `id_correction`: Identificador único de la corrección.
- `id_caso`: Referencia al caso auditado.
- `descripcion`: Descripción de la corrección.
- `fecha`: Fecha de la corrección.
- `id_auditor`: Auditor que realizó la corrección.

| PK/FK | Atributo       | TIPO    | Tamaño | NULIDAD  | AUTOINC. | DEFAULT | DESCRIPCIÓN                                |
|-------|----------------|---------|--------|----------|----------|---------|--------------------------------------------|
| PK    | id_correction  | INT     |        | NOT NULL | SI       |         | Identificador único de la corrección       |
| FK    | id_caso        | INT     |        | NOT NULL | NO       |         | Referencia al caso auditado                |
|       | descripcion    | TEXT    |        | NOT NULL | NO       |         | Descripción de la corrección               |
|       | fecha          | DATE    |        | NOT NULL | NO       |         | Fecha de la corrección                     |
| FK    | id_auditor     | INT     |        | NOT NULL | NO       |         | Auditor que realizó la corrección          |

### Audit_FollowUps

Seguimiento de las auditorías.

-**Atributos**:
- `id_followup`: Identificador único del seguimiento.
- `id_caso`: Referencia al caso auditado.
- `fecha`: Fecha del seguimiento.
- `observaciones`: Observaciones realizadas durante el seguimiento.
- `id_auditor`: Auditor responsable del seguimiento.

| PK/FK | Atributo          | TIPO    | Tamaño | NULIDAD  | AUTOINC. | DEFAULT | DESCRIPCIÓN                                    |
|-------|-------------------|---------|--------|----------|----------|---------|------------------------------------------------|
| PK    | id_followup       | INT     |        | NOT NULL | SI       |         | Identificador único del seguimiento            |
| FK    | id_caso           | INT     |        | NOT NULL | NO       |         | Referencia al caso auditado                    |
|       | fecha             | DATE    |        | NOT NULL | NO       |         | Fecha del seguimiento                          |
|       | observaciones     | TEXT    |        | NULL     | NO       |         | Observaciones realizadas durante el seguimiento|
| FK    | id_auditor        | INT     |        | NOT NULL | NO       |         | Auditor responsable del seguimiento            |

### Payment_Conditions

Condiciones de pago específicas.

-**Atributos**:

- `id_payment`: Identificador único del pago.
- `id_condicion`: Referencia a la tabla Condicion.
- `id_auditor`: Referencia al auditor.
- `fecha_acuerdo`: Fecha del acuerdo de pago.
- `monto`: Monto acordado.

| PK/FK | Atributo         | TIPO    | Tamaño | NULIDAD  | AUTOINC. | DEFAULT | DESCRIPCIÓN                                   |
|-------|------------------|---------|--------|----------|----------|---------|-----------------------------------------------|
| PK    | id_payment       | INT     |        | NOT NULL | SI       |         | Identificador único del pago                 |
| FK    | id_condicion     | INT     |        | NOT NULL | NO       |         | Referencia a la tabla `Condicion`            |
| FK    | id_auditor       | INT     |        | NOT NULL | NO       |         | Referencia al auditor                        |
|       | fecha_acuerdo    | DATE    |        | NOT NULL | NO       |         | Fecha del acuerdo de pago                    |
|       | monto            | DECIMAL |        | NOT NULL | NO       |         | Monto acordado                               |

# Listado de Vistas

## 1.AuditoriasPorEmpresa

*Descripción:*
Esta vista proporciona un listado de todas las auditorías realizadas por cada empresa, incluyendo información detallada sobre el auditor y el caso de auditoría.

*Objetivo:*
Facilitar la obtención de un resumen de auditorías por empresa, permitiendo a los usuarios ver qué auditorías se han realizado y quiénes fueron los auditores asignados.

*Tablas Compuestas:*
  - `Empresa`
  - `Auditor`
  - `Caso`

## 2.Vista KPIResumen

*Descripción:*
Esta vista proporciona un resumen de los KPIs asociados a cada caso.

*Objetivo:*
Evaluar rápidamente el rendimiento de los casos basados en los KPIs.

*Tablas Compuestas:*

  - `Caso`
  - `KPI`
  - `CasoKPI`

## 3. Vista AuditorDesempeno

*Descripción:*
Proporciona un resumen del desempeño de cada auditor basado en las métricas de desempeño y los KPIs asociados.

*Objetivo:*
Evaluar el desempeño de los auditores mediante un análisis de sus métricas y KPIs, facilitando la identificación de áreas de mejora o reconocimiento.

*Tablas Compuestas:*

- `Auditor`
- `Performance_Metrics`
- `KPI`

## 4. Vista EscalacionesPorAuditor

*Descripción:*
Muestra todas las escalaciones de casos por auditor, incluyendo la razón y la fecha de la escalación.

*Objetivo:*
Permitir a los usuarios revisar las escalaciones que ha manejado cada auditor, para analizar las razones y la frecuencia de estas escalaciones.

*Tablas Compuestas:*
- `Case_Escalations`
- `Auditor`
- `Caso`

## 5. Vista SatisfaccionPorCaso

*Descripción:*
Proporciona un resumen de la satisfacción del cliente por cada caso de auditoría, incluyendo las calificaciones y comentarios.

*Objetivo:*
Facilitar el análisis de la satisfacción del cliente con respecto a los casos auditados, permitiendo a los usuarios identificar patrones o áreas de atención.

*Tablas Compuestas:*
- `Customer_Satisfaction`
- `Caso`

# Triggers

## 1. Trigger: AuditUpdateTimestamp

**Descripción**: Actualiza la fecha de modificación de un caso cada vez que se actualiza algún registro en la tabla Caso.

**Objetivo**: Mantener un registro preciso de cuándo se actualizan los casos, lo cual es útil para auditorías y seguimiento de cambios.

## 2. Trigger: CheckSatisfactionRating

**Descripción:** Valida que la calificación de satisfacción (rating) esté dentro del rango permitido (por ejemplo, 1 a 5) antes de insertar un registro en la tabla Customer_Satisfaction.

**Objetivo:** Garantizar que las calificaciones ingresadas estén dentro de los límites definidos para mantener la integridad de los datos.

# Funciones

## 1. Función: CalculateAverageKPI
**Descripción:** Calcula el promedio de un KPI específico para un auditor dado en un período.

**Objetivo:** Proporcionar una evaluación rápida del rendimiento de un auditor en relación con un KPI específico durante un período.

## 2. Función: GetCaseDuration
**Descripción:** Calcula la duración en días de un caso desde su inicio hasta su fin.

**Objetivo:** Ayudar a determinar la duración de los casos de auditoría para el análisis de eficiencia y tiempos de resolución.

### Scripts creacion de tablas, triggers, vistas, inserción de datos, informes:

```
+----------------+     +------------------+     +----------------+     +----------------+
|    Empresa     |     |     Auditor       |     |     Caso       |     |      KPI       |
+----------------+     +------------------+     +----------------+     +----------------+
| id_empresa (PK)|<----| id_empresa (FK)  |     | id_caso (PK)   |     | id_kpi (PK)    |
| razon_social   |     | id_auditor (PK)  |     | id_auditor (FK)|     | descripcion    |
| direccion      |     | nombre           |     | fecha_inicio   |     | tipo           |
| localidad      |     | apellido         |     | fecha_fin      |     +----------------+
| telefono       |     | telefono         |     | descripcion    |     |    CasoKPI     |
| correo         |     | correo           |     | id_condicion(FK)|    +----------------+
| id_condicion (FK)|   | id_condicion (FK)|     +----------------+     | id_caso (FK)   |
+----------------+     +------------------+     | id_kpi (FK)    |     | id_kpi (FK)    |
                                              | valor          |     | valor          |
                                              +----------------+     +----------------+
                                                      |
                                                      |
                                                +----------------+
                                                |   CasoKPI      |
                                                +----------------+
                                                | id_caso (FK)   |
                                                | id_kpi (FK)    |
                                                | valor          |
                                                +----------------+

+----------------+     +----------------+     +----------------+     +----------------+
| AuditorProyecto|     | Training_Sessions|   | Case_Escalations| | Customer_Satisfaction |
+----------------+     +----------------+     +----------------+     +----------------+
| id_auditor (FK)|     | id_session (PK) |     | id_escalation (PK)| | id_satisfaction (PK)|
| id_proyecto (FK)|    | id_auditor (FK) |     | id_caso (FK)       | | id_caso (FK)       |
| descripcion    |     | fecha           |     | razon             | | rating             |
+----------------+     | tema            |     | fecha_escalacion  | | comentarios        |
                       | duracion        |     | id_auditor_responsable(FK)| fecha             |
                       | feedback        |     +----------------+     +----------------+
                       +----------------+ 

+----------------+     +----------------+     +----------------+
| Performance_Metrics | | Audit_Schedules | | Data_Corrections |
+----------------+     +----------------+     +----------------+
| id_metric (PK)   |   | id_schedule (PK)|   | id_correction (PK)|
| id_auditor (FK)  |   | id_auditor (FK) |   | id_caso (FK)      |
| id_kpi (FK)      |   | id_caso (FK)    |   | descripcion      |
| periodo          |   | fecha_programada|   | fecha            |
| valor            |   | estado          |   | id_auditor (FK)  |
+----------------+     +----------------+     +----------------+

+----------------+
| Payment_Conditions |
+----------------+
| id_payment (PK)    |
| id_condicion (FK)  |
| id_auditor (FK)    |
| fecha_acuerdo      |
| monto              |
+----------------+


-- Creación de Tablas
CREATE TABLE Empresa (
    id_empresa INT AUTO_INCREMENT PRIMARY KEY,
    razon_social VARCHAR(255) NOT NULL,
    direccion VARCHAR(255) NOT NULL,
    localidad VARCHAR(255) NOT NULL,
    telefono VARCHAR(20) NOT NULL,
    correo VARCHAR(255) NOT NULL,
    id_condicion INT NOT NULL,
    FOREIGN KEY (id_condicion) REFERENCES Condicion(id_condicion)
);

CREATE TABLE Auditor (
    id_auditor INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(255) NOT NULL,
    apellido VARCHAR(255) NOT NULL,
    telefono VARCHAR(20) NOT NULL,
    correo VARCHAR(255) NOT NULL,
    id_empresa INT NOT NULL,
    id_condicion INT NOT NULL,
    FOREIGN KEY (id_empresa) REFERENCES Empresa(id_empresa),
    FOREIGN KEY (id_condicion) REFERENCES Condicion(id_condicion)
);

CREATE TABLE Caso (
    id_caso INT AUTO_INCREMENT PRIMARY KEY,
    id_auditor INT NOT NULL,
    fecha_inicio DATE NOT NULL,
    fecha_fin DATE NOT NULL,
    descripcion TEXT NOT NULL,
    id_condicion INT NOT NULL,
    FOREIGN KEY (id_auditor) REFERENCES Auditor(id_auditor),
    FOREIGN KEY (id_condicion) REFERENCES Condicion(id_condicion)
);

CREATE TABLE KPI (
    id_kpi INT AUTO_INCREMENT PRIMARY KEY,
    descripcion VARCHAR(255) NOT NULL,
    tipo VARCHAR(50) NOT NULL
);

CREATE TABLE CasoKPI (
    id_caso INT NOT NULL,
    id_kpi INT NOT NULL,
    valor DECIMAL(10, 2) NOT NULL,
    PRIMARY KEY (id_caso, id_kpi),
    FOREIGN KEY (id_caso) REFERENCES Caso(id_caso),
    FOREIGN KEY (id_kpi) REFERENCES KPI(id_kpi)
);

CREATE TABLE Condicion (
    id_condicion INT AUTO_INCREMENT PRIMARY KEY,
    descripcion VARCHAR(255) NOT NULL,
    precio DECIMAL(10, 2) NOT NULL DEFAULT 5000.00,
    plazo INT NOT NULL DEFAULT 0
);

CREATE TABLE AuditorProyecto (
    id_auditor INT NOT NULL,
    id_proyecto INT NOT NULL,
    descripcion TEXT NOT NULL,
    PRIMARY KEY (id_auditor, id_proyecto),
    FOREIGN KEY (id_auditor) REFERENCES Auditor(id_auditor)
);

CREATE TABLE Training_Sessions (
    id_session INT AUTO_INCREMENT PRIMARY KEY,
    id_auditor INT NOT NULL,
    fecha DATE NOT NULL,
    tema VARCHAR(255) NOT NULL,
    duracion TIME NOT NULL,
    feedback TEXT,
    FOREIGN KEY (id_auditor) REFERENCES Auditor(id_auditor)
);

CREATE TABLE Case_Escalations (
    id_escalation INT AUTO_INCREMENT PRIMARY KEY,
    id_caso INT NOT NULL,
    razon VARCHAR(255) NOT NULL,
    fecha_escalacion DATE NOT NULL,
    id_auditor_responsable INT NOT NULL,
    FOREIGN KEY (id_caso) REFERENCES Caso(id_caso),
    FOREIGN KEY (id_auditor_responsable) REFERENCES Auditor(id_auditor)
);

CREATE TABLE Customer_Satisfaction (
    id_satisfaction INT AUTO_INCREMENT PRIMARY KEY,
    id_caso INT NOT NULL,
    rating INT NOT NULL,
    comentarios TEXT,
    fecha DATE NOT NULL,
    FOREIGN KEY (id_caso) REFERENCES Caso(id_caso)
);

CREATE TABLE Performance_Metrics (
    id_metric INT AUTO_INCREMENT PRIMARY KEY,
    id_auditor INT NOT NULL,
    id_kpi INT NOT NULL,
    periodo VARCHAR(50) NOT NULL,
    valor DECIMAL NOT NULL,
    FOREIGN KEY (id_auditor) REFERENCES Auditor(id_auditor),
    FOREIGN KEY (id_kpi) REFERENCES KPI(id_kpi)
);

CREATE TABLE Audit_Schedules (
    id_schedule INT AUTO_INCREMENT PRIMARY KEY,
    id_auditor INT NOT NULL,
    id_caso INT NOT NULL,
    fecha_programada DATE NOT NULL,
    estado VARCHAR(50) NOT NULL,
    FOREIGN KEY (id_auditor) REFERENCES Auditor(id_auditor),
    FOREIGN KEY (id_caso) REFERENCES Caso(id_caso)
);

CREATE TABLE Data_Corrections (
    id_correction INT AUTO_INCREMENT PRIMARY KEY,
    id_caso INT NOT NULL,
    descripcion TEXT NOT NULL,
    fecha DATE NOT NULL,
    id_auditor INT NOT NULL,
    FOREIGN KEY (id_caso) REFERENCES Caso(id_caso),
    FOREIGN KEY (id_auditor) REFERENCES Auditor(id_auditor)
);

CREATE TABLE Audit_FollowUps (
    id_followup INT AUTO_INCREMENT PRIMARY KEY,
    id_caso INT NOT NULL,
    fecha DATE NOT NULL,
    observaciones TEXT,
    id_auditor INT NOT NULL,
    FOREIGN KEY (id_caso) REFERENCES Caso(id_caso),
    FOREIGN KEY (id_auditor) REFERENCES Auditor(id_auditor)
);

CREATE TABLE Payment_Conditions (
    id_payment INT AUTO_INCREMENT PRIMARY KEY,
    id_condicion INT NOT NULL,
    id_auditor INT NOT NULL,
    fecha_acuerdo DATE NOT NULL,
    monto DECIMAL NOT NULL,
    FOREIGN KEY (id_condicion) REFERENCES Condicion(id_condicion),
    FOREIGN KEY (id_auditor) REFERENCES Auditor(id_auditor)
);

-- Creación de Vistas
CREATE VIEW AuditoriasPorEmpresa AS
SELECT 
    e.id_empresa,
    e.razon_social,
    a.id_auditor,
    a.nombre AS auditor_nombre,
    c.id_caso,
    c.descripcion AS caso_descripcion
FROM 
    Empresa e
JOIN 
    Auditor a ON e.id_empresa = a.id_empresa
JOIN 
    Caso c ON a.id_auditor = c.id_auditor;

CREATE VIEW KPIResumen AS
SELECT 
    c.id_caso,
    ck.id_kpi,
    k.descripcion AS descripcion_kpi,
    ck.valor
FROM 
    Caso c
JOIN 
    CasoKPI ck ON c.id_caso = ck.id_caso
JOIN 
    KPI k ON ck.id_kpi = k.id_kpi;

CREATE VIEW AuditorDesempeno AS
SELECT 
    a.id_auditor,
    a.nombre,
    a.apellido,
    pm.periodo,
    pm.valor AS valor_kpi,
    k.descripcion AS descripcion_kpi
FROM 
    Auditor a
JOIN 
    Performance_Metrics pm ON a.id_auditor = pm.id_auditor
JOIN 
    KPI k ON pm.id_kpi = k.id_kpi;

CREATE VIEW EscalacionesPorAuditor AS
SELECT 
    ce.id_escalation,
    a.id_auditor,
    a.nombre,
    a.apellido,
    ce.razon,
    ce.fecha_escalacion,
    c.descripcion AS descripcion_caso
FROM 
    Case_Escalations ce
JOIN 
    Auditor a ON ce.id_auditor_responsable = a.id_auditor
JOIN 
    Caso c ON ce.id_caso = c.id_caso;

CREATE VIEW SatisfaccionPorCaso AS
SELECT 
    cs.id_satisfaction,
    c.id_caso,
    cs.rating,
    cs.comentarios,
    cs.fecha,
    c.descripcion AS descripcion_caso
FROM 
    Customer_Satisfaction cs
JOIN 
    Caso c ON cs.id_caso = c.id_caso;

-- Creación de Funciones
CREATE FUNCTION CalculateAverageKPI(
    auditor_id INT,
    kpi_id INT,
    periodo_evaluacion VARCHAR(50)
)
RETURNS DECIMAL(10, 2)
DETERMINISTIC
BEGIN
    DECLARE average_kpi DECIMAL(10, 2);
    
    SELECT AVG(valor) INTO average_kpi
    FROM Performance_Metrics
    WHERE id_auditor = auditor_id
      AND id_kpi = kpi_id
      AND periodo = periodo_evaluacion;
    
    RETURN average_kpi;
END;

CREATE FUNCTION GetCaseDuration(
    case_id INT
)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE duration_days INT;

    SELECT DATEDIFF(fecha_fin, fecha_inicio) INTO duration_days
    FROM Caso
    WHERE id_caso = case_id;

    RETURN duration_days;
END;

-- Creación de Triggers
CREATE TRIGGER AuditUpdateTimestamp
AFTER UPDATE ON Caso
FOR EACH ROW
BEGIN
    UPDATE Caso
    SET fecha_modificacion = CURRENT_TIMESTAMP
    WHERE id_caso = NEW.id_caso;
END;

CREATE TRIGGER CheckSatisfactionRating
BEFORE INSERT ON Customer_Satisfaction
FOR EACH ROW
BEGIN
    IF NEW.rating < 1 OR NEW.rating > 5 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'La calificación de satisfacción debe estar entre 1 y 5.';
    END IF;
END;

-- Inserción de Datos Utilizando LOAD DATA INFILE (Importante: Las rutas en los comandos deben ser ajustadas según la ubicación para ejecución)

LOAD DATA INFILE '/path/to/empresa.csv'
INTO TABLE Empresa
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/path/to/auditor.csv'
INTO TABLE Auditor
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

LOAD DATA INFILE '/path/to/caso.csv'
INTO TABLE Caso
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

-- Generación de Informes

-- Informe 1: Auditorías por Empresa
SELECT * FROM AuditoriasPorEmpresa;

-- Informe 2: Resumen de KPIs
SELECT * FROM KPIResumen;

-- Informe 3: Desempeño de Auditores
SELECT * FROM AuditorDesempeno;

-- Informe 4: Escalaciones por Auditor
SELECT * FROM EscalacionesPorAuditor;

-- Informe 5: Satisfacción por Caso
SELECT * FROM SatisfaccionPorCaso;

