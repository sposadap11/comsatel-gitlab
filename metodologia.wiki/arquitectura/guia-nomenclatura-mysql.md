[[_TOC_]]

# Guía de nomenclatura en MySQL

## Objectivos
El objetivo de este documento es deifinir los estándares de diseño, nomenclatura y mantenimiento para la base de datos MySQL, con el fin de asegurar consistencia, escalabilidad y facilidad de mantenimiento entre diferentes proyectos.

## Estructura general
- Cada sistema o microservicio debe tener su propia base de datos independiente, siguiendo el prinicio de desacomplamiento.
- Las base de datos no deben de compartir tablas ni relación directa entre microservicios.
- Las relaciones entre datos de distintos dominios deben de gestionarse a través de servicios o APIs, no mediante FOREIGN KEY.
- El control de auditoría (creación, modificación, usuario responsable, etc) se manejan a nivel de base de datos, como también de un microservicio de auditoría centralizado.
- Cada base de datos debe enforcarse únicamente en los datos del dominio que le corresponde.

## Convenciones de nombres
### Bases de datos
- Usar **snake_case** en minúsculas (`db_unidad`).
### Tablas
- Nombres en plural y utilizando **snake_case**. (`tb_notificaciones`).
### Columnas
- Nombres en minúsculas y utilizando **snake_case**
- Evitar abreviaciones y prefijos innecesarios (`usuario_id`).
### Llaves primarias y foráneas
- Llave primaria: **id** (tipo `INT AUTO_INCREMENT` o `BIGINT AUTO_INCREMENT`).
- Llave foránea: `<tabla_referenciada>_id`.


## Tipos de datos
| Tipo de datos     | Uso recomendado                           |
|-------------------|-------------------------------------------|
| INT / BIGINIT     | Identificadores y contadores              |
| VARCHAR(n)        | Cadenas cortas y no mayores a 255         |
| TEXT              | Descripción o comentarios largos          |
| BOOLEAN / BIT     | Valores lógicos                           |
| DATE / DATETIME   | Fechas                                    |
| DECIMAL(p, s)     | Valores monetarios o con precisión        |

## Ingregridad y relaciones
- Las relaciones entre tablas del mismo microservicio deben definirse mediante claves foráneas explícitas **FOREIGN KEY**, garantizando la integridad referencial.
- Cuando la relación involucre entidades que pertenecen a otro microservicio o base de datos, no se debe crear una clave foránea física, si no mas bien la refencia a través de un campo identificador `<entidad>_id`.
- En estos casos, la validación y consistencia de los datos se debe de manejar a nivel de aplicación o mediante APIs entre microservicios.
- Crear índices en los campos que se utilicen frecuentemente para búsquedas o uniones (`JOIN`, `WHERE`, `ORDER BY`).

## Versionamiento de scripts
Se recomienda utilizar **Flyway** para gestionar migraciones de forma controlada.
- Todos los scripts deben almacenarse en `resources/db/migration/`.
- Nomenclatura recomendada:
```
V1__XXXXXXX.sql
V2__XXXXXXX.sql
```

## Buenas prácticas Flyway
```sql
-- Script: V1__create_user_table.sql
-- Autor: Nombres y Apellidos
-- Fecha: 31/10/2025
-- Descripción: Crea la tabla usuarios con sus campos principales.
```