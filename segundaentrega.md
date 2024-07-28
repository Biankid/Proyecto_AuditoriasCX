# Proyecto_AuditoriasCX
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

# Listado de Funciones

## 1.Función ObtenerDuracionAuditoria

*Descripción:* 
Calcula la duración en días de una auditoría.

*Objetivo:* 
Obtener la duración de una auditoría en días.

*Datos/Tablas:* 
- `Caso`

## 2.Función ObtenerPromedioKPI

*Descripción:* 
Calcula el promedio de un KPI para un caso específico.

*Objetivo:* 
Evaluar el rendimiento promedio de un KPI en un caso.

*Datos/Tablas:* 
- `CasoKPI`

# Listado de Stored Procedures

## 1.Stored Procedure InsertarAuditor

*Descripción:* 
Inserta un nuevo auditor en la base de datos.

*Objetivo:*
Facilitar la inserción de nuevos auditores.

*Tablas:*
- `Auditor`

## 2.Stored Procedure ActualizarKPI

*Descripción:*
Actualiza el valor de un KPI para un caso específico.

*Objetivo:* 
Mantener actualizados los valores de los KPIs.

*Tablas:*
- `CasoKPI`

## 3.Stored Procedure EliminarAuditor

*Descripción:* 
Elimina un auditor y todas sus asignaciones de la base de datos.

*Objetivo:* 
Gestionar la eliminación completa de datos de un auditor.

*Tablas:*
- `Auditor`
- `Caso`
- `AuditorProyecto`

  ## Paso a Paso para la Inserción de Datos mediante Importación

- Crear los archivos CSV: Crear archivos CSV con los datos para cada una de las tablas.
- Guardar los archivos CSV: Guardar los archivos CSV en la ruta adecuada.
- Utilizar los comandos COPY: Usar los comandos COPY para importar los datos desde los archivos CSV a las tablas correspondientes.

  ## Script de Creación de Vistas, Funciones, Stored Procedures y Triggers e inserción de datos:

```sql
-- Creación de Vistas
CREATE VIEW AuditoriasPorEmpresa AS
SELECT 
    e.razon_social AS Empresa,
    COUNT(c.id_caso) AS TotalAuditorias
FROM 
    Empresa e
JOIN 
    Auditor a ON e.id_empresa = a.id_empresa
JOIN 
    Caso c ON a.id_auditor = c.id_auditor
GROUP BY 
    e.razon_social;

CREATE VIEW KPIResumen AS
SELECT 
    c.descripcion AS CasoDescripcion,
    k.descripcion AS KPIDescripcion,
    ck.valor AS ValorKPI
FROM 
    Caso c
JOIN 
    CasoKPI ck ON c.id_caso = ck.id_caso
JOIN 
    KPI k ON ck.id_kpi = k.id_kpi;

-- Creación de Funciones
CREATE FUNCTION ObtenerDuracionAuditoria(id_caso INT) RETURNS INT
BEGIN
    DECLARE duracion INT;
    SELECT DATEDIFF(fecha_fin, fecha_inicio) INTO duracion
    FROM Caso
    WHERE id_caso = id_caso;
    RETURN duracion;
END;

CREATE FUNCTION ObtenerPromedioKPI(id_caso INT) RETURNS DECIMAL(10, 2)
BEGIN
    DECLARE promedio DECIMAL(10, 2);
    SELECT AVG(valor) INTO promedio
    FROM CasoKPI
    WHERE id_caso = id_caso;
    RETURN promedio;
END;

-- Creación de Stored Procedures
CREATE PROCEDURE InsertarAuditor(
    IN p_nombre VARCHAR(255),
    IN p_apellido VARCHAR(255),
    IN p_telefono VARCHAR(20),
    IN p_correo VARCHAR(255),
    IN p_id_empresa INT,
    IN p_id_condicion INT
)
BEGIN
    INSERT INTO Auditor (nombre, apellido, telefono, correo, id_empresa, id_condicion)
    VALUES (p_nombre, p_apellido, p_telefono, p_correo, p_id_empresa, p_id_condicion);
END;

CREATE PROCEDURE ActualizarKPI(
    IN p_id_caso INT,
    IN p_id_kpi INT,
    IN p_valor DECIMAL(10, 2)
)
BEGIN
    UPDATE CasoKPI
    SET valor = p_valor
    WHERE id_caso = p_id_caso AND id_kpi = p_id_kpi;
END;

CREATE PROCEDURE EliminarAuditor(IN p_id_auditor INT)
BEGIN
    DELETE FROM AuditorProyecto WHERE id_auditor = p_id_auditor;
    DELETE FROM Caso WHERE id_auditor = p_id_auditor;
    DELETE FROM Auditor WHERE id_auditor = p_id_auditor;
END;

-- En la primer entrega se realizó ingreso de datos con insert to, se detalla la importacion con LOAD DATA INFILE

-- Importar datos a la tabla Condicion

LOAD DATA INFILE '/path/to/condicion.csv'
INTO TABLE Condicion
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(id_condicion, descripcion, precio, plazo);

-- Importar datos a la tabla Empresa

LOAD DATA INFILE '/path/to/empresa.csv'
INTO TABLE Empresa
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(id_empresa, razon_social, direccion, localidad, telefono, correo, id_condicion);

-- Importar datos a la tabla Auditor

LOAD DATA INFILE '/path/to/auditor.csv'
INTO TABLE Auditor
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(id_auditor, nombre, apellido, telefono, correo, id_empresa, id_condicion);

-- Importar datos a la tabla Caso
LOAD DATA INFILE '/path/to/caso.csv'
INTO TABLE Caso
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(id_caso, id_auditor, fecha_inicio, fecha_fin, descripcion, id_condicion);

-- Importar datos a la tabla KPI
LOAD DATA INFILE '/path/to/kpi.csv'
INTO TABLE KPI
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(id_kpi, descripcion, tipo);

-- Importar datos a la tabla CasoKPI

LOAD DATA INFILE '/path/to/caso_kpi.csv'
INTO TABLE CasoKPI
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(id_caso, id_kpi, valor);

-- Importar datos a la tabla AuditorProyecto

LOAD DATA INFILE '/path/to/auditor_proyecto.csv'
INTO TABLE AuditorProyecto
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(id_auditor, id_proyecto, descripcion);
