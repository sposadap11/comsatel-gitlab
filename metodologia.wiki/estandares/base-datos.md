[[_TOC_]]
# Estándard para Base de Datos

Este documento presenta los estándares a aplicar en la gestión de bases de datos con el fin de asegurar la inteligibilidad de los modelos de datos, facilidad de portabilidad entre los diferentes motores de bases de datos y proponer una mejora en el ciclo de vida del desarrollo de software.

## Estándares para Objetos de Bases de Datos

------

### Bases de Datos

Se detallará una serie de normas respecto a la definición de los diferentes objetos que contienen las bases de datos: esquemas, tablas, vistas, índices, restricciones, disparadores, paquetes, procedimientos y funciones con el objetivo de tener una clara definición en todos los objetos.

### Esquemas

Un esquema es un contenedor con nombre para objetos de base de datos, que permite agrupar objetos en espacios de nombres independientes.

#### Sintaxis

```
CREATE DATABASE `NombreEsquema` DEFAULT CHARACTER SET latin1;
```

#### Estándar

1. Los esquemas deberán ser definidos en minúscula y en lenguaje natural lo más descriptivo posible.
2. Los esquemas serán la representación de cada sistema o conjunto de sistemas.
3. Se escribirá en singular y sólo deberá contener una palabra.

Los esquemas propuestos para los sistemas *inhouse* de Comsatel serán los siguientes:

- maestro: Almacenará los objetos y datos maestros de los diferentes sistemas de Comsatel.
- sigo: Almacenará los objetos y datos del sistema de gestión y operaciones (SIGO).
- clocator: Almacenará los objetos y datos del sistema de localización CL, se irá migrando las nuevas tablas a este esquema de forma progresiva.
- integracion: Almacenará los objetos y datos de los sistemas de integración entre las aplicaciones internas.
- app: Almacenará los objetos y datos de todas las aplicaciones móviles.
- web: Almacenará los objetos y datos de la página web de Comsatel.

### Tablas

Una tabla se refiere al tipo de modelado de datos, donde se guardan los datos recogidos por un sistema. Las tablas se componen de dos estructuras:

- Campo: corresponde al nombre de la columna. Debe ser único y además de tener un tipo de dato asociado.
- Registro: corresponde a cada columna que compone la tabla. Allí se componen los datos y los registros. Eventualmente pueden ser nulos en su almacenamiento.

En la definición de cada campo, debe existir un nombre único, con su tipo de dato correspondiente. A los campos se les puede asignar, además, propiedades especiales que afectan a los registros insertados. El campo puede ser definido como índice o autoincremental, lo cual permite que los datos de ese campo cambien solos o sean el principal índice a la hora de ordenar los datos contenidos. Cada tabla creada debe tener un nombre único en la base de datos.

#### Sintaxis

```
CREATE TABLE `tb_nombretabla` (
  `campoid` bigint(20) NOT NULL AUTO_INCREMENT,
  `campo1` varchar(100) NOT NULL,
  `campo2` varchar(100) DEFAULT NULL,
  `campo3` char(1) NOT NULL,
  `campo4` bigint(20) NOT NULL,
  `campo5` double DEFAULT NULL,
  `campo6` double DEFAULT NULL,
  `campo7` bigint(20) NOT NULL,
  PRIMARY KEY (`Id`),
  KEY `idx_nombretabla_nombrecampo` (`campo4`),
  CONSTRAINT `fk_nombretablaorigen_nombretabladestino` FOREIGN KEY (`campo7`) REFERENCES `nombretabladestino ` (`campoforaneodestino`)
) ENGINE=InnoDB AUTO_INCREMENT=0 DEFAULT CHARSET=utf8;
```

#### Estándar

1. Las tablas deberán ser definidas en minúscula y en lenguaje natural lo más descriptivo posible.
2. Las tablas tendrán el prefijo de tb_.
3. Se escribirá en singular y si contienen varias palabras se separarán con subrayado (*underline*).
4. Por defecto se usará el motor de base de datos *InnoDB*.
5. Por defecto las tablas transaccionales deberán ser *autoincrementales*.
6. Por defecto se usará el charset *utf8*.

### Vistas

Una vista es una consulta que se presenta como una tabla virtual a partir de un conjunto de tablas en una base de datos relacional. Las vistas tienen la misma estructura que una tabla: filas y columnas. La única diferencia es que sólo se almacena de ellas la definición, no los datos. Los datos que se recuperan mediante una consulta a una vista se presentarán igual que los de una tabla. De hecho, si no se sabe que se está trabajando con una vista, nada hace suponer que es así. Al igual que sucede con una tabla, se pueden insertar, actualizar, borrar y seleccionar datos en una vista. Aunque siempre es posible seleccionar datos de una vista, en algunas condiciones existen restricciones para realizar el resto de las operaciones sobre vistas.

#### Sintaxis

```
CREATE VIEW `vws_nombrevista`
    AS select_statement;
```

#### Estándar

1. Las vistas deberán ser definidas en minúscula y en lenguaje natural lo más descriptivo posible.
2. Las tablas tendrán el prefijo de **vws_**.
3. Se escribirá en singular y si contienen varias palabras se separarán con subrayado (*underline*).
4. Los nombres de las vistas siguen las mismas convenciones que el de las tablas.

### Campos

#### Estándar

1. Los nombres de campos deberán ser únicos dentro de la tabla a la que corresponden.
2. Los nombres de campos deberán derivarse de acuerdo al identificador utilizando durante la fase de análisis de negocio.
3. Se escribirá en singular y si contienen varias palabras se separarán con subrayado (*underline*).
4. No se deberán usar palabras reservadas como nombres de campos.

#### Palabras reservadas

| **ADD**              | **ALL**            | **ALTER**               |
| -------------------- | ------------------ | ----------------------- |
| **ANALYZE**          | **AND**            | **AS**                  |
| **ASC**              | **ASENSITIVE**     | **BEFORE**              |
| **BETWEEN**          | **BIGINT**         | **BINARY**              |
| **BLOB**             | **BOTH**           | **BY**                  |
| **CALL**             | **CASCADE**        | **CASE**                |
| **CHANGE**           | **CHAR**           | **CHARACTER**           |
| **CHECK**            | **COLLATE**        | **COLUMN**              |
| **CONDITION**        | **CONSTRAINT**     | **CONTINUE**            |
| **CONVERT**          | **CREATE**         | **CROSS**               |
| **CURRENT_DATE**     | **CURRENT_TIME**   | **CURRENT_TIMESTAMP**   |
| **CURRENT_USER**     | **CURSOR**         | **DATABASE**            |
| **DATABASES**        | **DAY_HOUR**       | **DAY_MICROSECOND**     |
| **DAY_MINUTE**       | **DAY_SECOND**     | **DEC**                 |
| **DECIMAL**          | **DECLARE**        | **DEFAULT**             |
| **DELAYED**          | **DELETE**         | **DESC**                |
| **DESCRIBE**         | **DETERMINISTIC**  | **DISTINCT**            |
| **DISTINCTROW**      | **DIV**            | **DOUBLE**              |
| **DROP**             | **DUAL**           | **EACH**                |
| **ELSE**             | **ELSEIF**         | **ENCLOSED**            |
| **ESCAPED**          | **EXISTS**         | **EXIT**                |
| **EXPLAIN**          | **FALSE**          | **FETCH**               |
| **FLOAT**            | **FLOAT4**         | **FLOAT8**              |
| **FOR**              | **FORCE**          | **FOREIGN**             |
| **FROM**             | **FULLTEXT**       | **GRANT**               |
| **GROUP**            | **HAVING**         | **HIGH_PRIORITY**       |
| **HOUR_MICROSECOND** | **HOUR_MINUTE**    | **HOUR_SECOND**         |
| **IF**               | **IGNORE**         | **IN**                  |
| **INDEX**            | **INFILE**         | **INNER**               |
| **INOUT**            | **INSENSITIVE**    | **INSERT**              |
| **INT**              | **INT1**           | **INT2**                |
| **INT3**             | **INT4**           | **INT8**                |
| **INTEGER**          | **INTERVAL**       | **INTO**                |
| **IS**               | **ITERATE**        | **JOIN**                |
| **KEY**              | **KEYS**           | **KILL**                |
| **LEADING**          | **LEAVE**          | **LEFT**                |
| **LIKE**             | **LIMIT**          | **LINES**               |
| **LOAD**             | **LOCALTIME**      | **LOCALTIMESTAMP**      |
| **LOCK**             | **LONG**           | **LONGBLOB**            |
| **LONGTEXT**         | **LOOP**           | **LOW_PRIORITY**        |
| **MATCH**            | **MEDIUMBLOB**     | **MEDIUMINT**           |
| **MEDIUMTEXT**       | **MIDDLEINT**      | **MINUTE_MICROSECOND**  |
| **MINUTE_SECOND**    | **MOD**            | **MODIFIES**            |
| **NATURAL**          | **NOT**            | **NO_WRITE_TO_BINLOG**  |
| **NULL**             | **NUMERIC**        | **ON**                  |
| **OPTIMIZE**         | **OPTION**         | **OPTIONALLY**          |
| **OR**               | **ORDER**          | **OUT**                 |
| **OUTER**            | **OUTFILE**        | **PRECISION**           |
| **PRIMARY**          | **PROCEDURE**      | **PURGE**               |
| **READ**             | **READS**          | **REAL**                |
| **REFERENCES**       | **REGEXP**         | **RELEASE**             |
| **RENAME**           | **REPEAT**         | **REPLACE**             |
| **REQUIRE**          | **RESTRICT**       | **RETURN**              |
| **REVOKE**           | **RIGHT**          | **RLIKE**               |
| **SCHEMA**           | **SCHEMAS**        | **SECOND_MICROSECOND**  |
| **SELECT**           | **SENSITIVE**      | **SEPARATOR**           |
| **SET**              | **SHOW**           | **SMALLINT**            |
| **SONAME**           | **SPATIAL**        | **SPECIFIC**            |
| **SQL**              | **SQLEXCEPTION**   | **SQLSTATE**            |
| **SQLWARNING**       | **SQL_BIG_RESULT** | **SQL_CALC_FOUND_ROWS** |
| **SQL_SMALL_RESULT** | **SSL**            | **STARTING**            |
| **STRAIGHT_JOIN**    | **TABLE**          | **TERMINATED**          |
| **THEN**             | **TINYBLOB**       | **TINYINT**             |
| **TINYTEXT**         | **TO**             | **TRAILING**            |
| **TRIGGER**          | **TRUE**           | **UNDO**                |
| **UNION**            | **UNIQUE**         | **UNLOCK**              |
| **UNSIGNED**         | **UPDATE**         | **USAGE**               |
| **USE**              | **USING**          | **UTC_DATE**            |
| **UTC_TIME**         | **UTC_TIMESTAMP**  | **VALUES**              |
| **VARBINARY**        | **VARCHAR**        | **VARCHARACTER**        |
| **VARYING**          | **WHEN**           | **WHERE**               |
| **WHILE**            | **WITH**           | **WRITE**               |
| **XOR**              | **YEAR_MONTH**     | **ZEROFILL**            |
| **ASENSITIVE**       | **CALL**           | **CONDITION**           |
| **CONTINUE**         | **CURSOR**         | **DECLARE**             |
| **DETERMINISTIC**    | **EACH**           | **ELSEIF**              |
| **EXIT**             | **FETCH**          | **INOUT**               |
| **INSENSITIVE**      | **ITERATE**        | **LEAVE**               |
| **LOOP**             | **MODIFIES**       | **OUT**                 |
| **READS**            | **RELEASE**        | **REPEAT**              |
| **RETURN**           | **SCHEMA**         | **SCHEMAS**             |
| **SENSITIVE**        | **SPECIFIC**       | **SQL**                 |
| **SQLEXCEPTION**     | **SQLSTATE**       | **SQLWARNING**          |
| **TRIGGER**          | **UNDO**           | **WHILE**               |

### Indices

El índice de una base de datos es una estructura de datos que mejora la velocidad de las operaciones, por medio de identificador único de cada fila de una tabla, permitiendo un rápido acceso a los registros de una tabla en una base de datos.

#### Estándar

1. No se debe usar palabras reservadas como nombres de índices.

2. Los índices deberán ser definidas en minúscula y en lenguaje natural lo más descriptivo posible.

3. Las claves primarias utilizan el prefijo de **PRIMARY**. 

   ```
   PRIMARY KEY (`usuarioid`)
   ``` 

4. Las claves foráneas utilizan el prefijo de **fk_**. 

   ```
   CONSTRAINT `fk_tablaorigen_tabladestino` FOREIGN KEY (`usuarioid`) REFERENCES `tabladestino` (`usuarioid`)
   ```

5. Los índices agrupados utilizan el prefijo **idx_**. 

   ```
   KEY `idx_nombretabla_columnaindice` (`columnaindice`) USING BTREE
   ```

   

### Procedimientos Almacenados, Funciones y Disparadores

### Estándar

1. No se debe usar palabras reservadas como nombres de procedimientos almacenados, funciones y/o disparadores.
2. Los procedimientos almacenados, funciones y disparadores deberán ser definidos en minúscula y en lenguaje natural lo más descriptivo posible.
3. Los procedimientos almacenados usarán el prefijo de **sp_.** Seguido de la tabla principal que afecte y la acción que se realice. 

**sp_evento_fuel_insertar**

1. Las funciones usarán el prefijo de **fn_.** Seguido de la tabla principal que afecte y la acción que se realice. 

**fn_alarma_obtener**

1. Los disparadores usarán el prefijo de **tr_.** Seguido de la tabla principal que afecte y la acción que se realice. 

**tr_usuario_actualizar**

1. Si la acción a realizar de los objetos contiene más de una palabra se recomienda la notación Camel Case. No se debe utilizar espacios en el nombre de los objetos.

## Especificaciones y Restricciones

------

El presente documento es un estándar definido para el desarrollo interno de Comsatel, por lo tanto, es posible adaptar nuevos estándares y/o modificar alguno. 

