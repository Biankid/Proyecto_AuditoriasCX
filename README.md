# Proyecto_AuditoriasCX
# Proyecto de Base de Datos para el Equipo de Auditores de CX

## Descripción de la temática de la base de datos

El objetivo de esta base de datos es gestionar y evaluar la información del equipo de auditores que realizan auditorías de casos de customer experience en diversas empresas. La base de datos almacenará detalles sobre las empresas, los auditores, los casos de auditoría, los indicadores clave de desempeño (KPIs), las condiciones de pago y trabajo, y la asignación de auditores a proyectos específicos.

## Diagramas de entidad relación de la base de datos

### Diagrama de Entidad-Relación

![Diagrama E-R](diagrama_ER.png)

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

## Scripts SQL para la creación de la base de datos y las tablas + inserción de datos en las tablas

```sql
-- Creación de la base de datos
CREATE DATABASE IF NOT EXISTS auditoria;
USE auditoria;

-- Creación de la tabla Condicion
CREATE TABLE IF NOT EXISTS Condicion (
    id_condicion INT AUTO_INCREMENT PRIMARY KEY,
    descripcion VARCHAR(255) NOT NULL,
    precio DECIMAL(10, 2) NOT NULL DEFAULT 5000.00,
    plazo INT NOT NULL DEFAULT 0
);

-- Creación de la tabla Empresa
CREATE TABLE IF NOT EXISTS Empresa (
    id_empresa INT AUTO_INCREMENT PRIMARY KEY,
    razon_social VARCHAR(255) NOT NULL,
    direccion VARCHAR(255) NOT NULL,
    localidad VARCHAR(255) NOT NULL,
    telefono VARCHAR(20) NOT NULL,
    correo VARCHAR(255) NOT NULL,
    id_condicion INT NOT NULL,
    FOREIGN KEY (id_condicion) REFERENCES Condicion(id_condicion)
);

-- Creación de la tabla Auditor
CREATE TABLE IF NOT EXISTS Auditor (
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

-- Creación de la tabla Caso
CREATE TABLE IF NOT EXISTS Caso (
    id_caso INT AUTO_INCREMENT PRIMARY KEY,
    id_auditor INT NOT NULL,
    fecha_inicio DATE NOT NULL,
    fecha_fin DATE NOT NULL,
    descripcion TEXT NOT NULL,
    id_condicion INT NOT NULL,
    FOREIGN KEY (id_auditor) REFERENCES Auditor(id_auditor),
    FOREIGN KEY (id_condicion) REFERENCES Condicion(id_condicion)
);

-- Creación de la tabla KPI
CREATE TABLE IF NOT EXISTS KPI (
    id_kpi INT AUTO_INCREMENT PRIMARY KEY,
    descripcion VARCHAR(255) NOT NULL,
    tipo VARCHAR(50) NOT NULL
);

-- Creación de la tabla CasoKPI
CREATE TABLE IF NOT EXISTS CasoKPI (
    id_caso INT NOT NULL,
    id_kpi INT NOT NULL,
    valor DECIMAL(10, 2) NOT NULL,
    PRIMARY KEY (id_caso, id_kpi),
    FOREIGN KEY (id_caso) REFERENCES Caso(id_caso),
    FOREIGN KEY (id_kpi) REFERENCES KPI(id_kpi)
);

-- Creación de la tabla AuditorProyecto
CREATE TABLE IF NOT EXISTS AuditorProyecto (
    id_auditor INT NOT NULL,
    id_proyecto INT NOT NULL,
    descripcion TEXT NOT NULL,
    PRIMARY KEY (id_auditor, id_proyecto),
    FOREIGN KEY (id_auditor) REFERENCES Auditor(id_auditor)
);

-- Scripts SQL para la inserción de datos en las tablas

-- Inserción de datos en la tabla Condicion
INSERT INTO Condicion (descripcion, precio, plazo) VALUES
('Pago inmediato', 1000.00, 0),
('Pago a 30 días', 950.00, 30);

-- Inserción de datos en la tabla Empresa
INSERT INTO Empresa (razon_social, direccion, localidad, telefono, correo, id_condicion) VALUES
('Bodega BIA', 'Calle San Martin 1234', 'Buenos Aires', '01112345678', 'contacto@bodegabia.com', 1),
('Bodega Schifrin', 'Avenida Cordoba 5678', 'Mendoza', '02611234567', 'info@bodegaschifrin.com', 2);

-- Inserción de datos en la tabla Auditor
INSERT INTO Auditor (nombre, apellido, telefono, correo, id_empresa, id_condicion) VALUES
('Juan', 'Perez', '01198765432', 'juan.perez@bodegabia.com', 1, 1),
('Ana', 'Gomez', '02619876543', 'ana.gomez@bodegaschifrin.com', 2, 2),
('Carlos', 'Lopez', '01187654321', 'carlos.lopez@bodegabia.com', 1, 1),
('María', 'Fernandez', '02618765432', 'maria.fernandez@bodegaschifrin.com', 2, 2);

-- Inserción de datos en la tabla Caso
INSERT INTO Caso (id_auditor, fecha_inicio, fecha_fin, descripcion, id_condicion) VALUES
(1, '2024-01-01', '2024-01-15', 'Auditoría de procesos en Bodega BIA', 1),
(2, '2024-02-01', '2024-02-10', 'Auditoría de calidad en Bodega Schifrin', 2),
(3, '2024-03-01', '2024-03-20', 'Revisión de cumplimiento en Bodega BIA', 1),
(4, '2024-04-01', '2024-04-15', 'Evaluación de seguridad en Bodega Schifrin', 2);

-- Inserción de datos en la tabla KPI
INSERT INTO KPI (descripcion, tipo) VALUES
('Tiempo de resolución de casos', 'Cuantitativo'),
('Satisfacción del cliente', 'Cualitativo'),
('Cumplimiento de normas', 'Cuantitativo'),
('Eficiencia operativa', 'Cuantitativo');

-- Inserción de datos en la tabla CasoKPI
INSERT INTO CasoKPI (id_caso, id_kpi, valor) VALUES
(1, 1, 10.5),
(1, 2, 8.2),
(2, 1, 7.0),
(2, 2, 9.0),
(3, 3, 95.0),
(3, 4, 87.5),
(4, 3, 92.0),
(4, 4, 85.0);

-- Inserción de datos en la tabla AuditorProyecto
INSERT INTO AuditorProyecto (id_auditor, id_proyecto, descripcion) VALUES
(1, 1, 'Proyecto de mejora continua en Bodega BIA'),
(2, 2, 'Proyecto de auditoría de calidad en Bodega Schifrin'),
(3, 3, 'Proyecto de revisión de cumplimiento en Bodega BIA'),
(4, 4, 'Proyecto de evaluación de seguridad en Bodega Schifrin');


