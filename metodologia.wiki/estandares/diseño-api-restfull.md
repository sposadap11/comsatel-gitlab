# Diseño de API Restfull

[[_TOC_]]

## Introducción
Para generar una guía técnica completa sobre el diseño de APIs RESTful en una arquitectura de microservicios, es esencial cubrir los principios, estándares y buenas prácticas que aseguran la consistencia, la escalabilidad y el mantenimiento.

Nuestras APIs se basan en Servicios Restfull donde protocolos como HTTP definan la base de su concepción e implementación, aunque también basados en gRPC donde las características del servicio lo ameriten.

Los microservicios representan una colección altamente cohesiva de funcionalidades expuestas para su consumo a través de endpoints con URI claramente definida, y con un bajo acomplamiento con otros microservicios o componentes de la arquitectura (a través de llamadas remotas asíncronas/síncronas o intercambios de mensajes "EDA - Event-Driven Architecture)

## Principios Fundamentales del Diseño RESTful
El diseño de APIs RESTful se basa en principios arquitectónicos específicos enfocados a sistemas distribuidos con componentes con bajo acoplamiento y alta cohesión, como los microservicios.

- Identificación de Recursos: Todo en la API debe ser un `recurso`.
  * Un recurso es una entidad a la que se puede acceder, como usuarios, productos, citas, dispositivos, viajes, entregas entre otros. 
  * Los recursos deben tener identificadores únicos (URIs). Por ejemplo: /api/v1/usuarios.

- Manipulación con Verbos HTTP: Se deben utilizar los verbos estándar de HTTP para indicar la acción a realizar sobre un recurso.

  * GET: Recuperar un recurso o una colección de recursos.
  * POST: Crear un nuevo recurso.
  * PUT: Actualizar un recurso completo.
  * PATCH: Actualizar parcialmente un recurso.
  * DELETE: Eliminar un recurso.

- Comunicación sin Estado (Stateless): Cada solicitud del cliente al servidor debe contener toda la información necesaria para que el servidor la procese. El servidor no debe almacenar ningún estado de la sesión del cliente entre las solicitudes. Esto hace que la API sea más escalable y resistente a fallos.

- Interfase Uniforme: La API debe tener una estructura consistente y predecible. Esto incluye una convención de nomenclatura uniforme, el uso de códigos de estado HTTP para indicar resultados y un formato de datos estándar (generalmente JSON).

## Estructura y Convenciones de Nomenclatura
Una estructura clara y consistente es crucial para la usabilidad de la API.

### URIs
  * Utiliza sustantivos en plural para los nombres de los recursos (/usuarios, no /usuario).
  * Evita verbos en los nombres de los recursos (/get-usuarios es incorrecto).
  * Utiliza camelCase (de preferencia) o snake_case (alternativo: consultar Arquitecto de Aplicaciones) para nombres de campos en los payloads de JSON, pero kebab-case para los URIs (/api/v1/citas/127/logs).

### Versionado
  * Incluye la versión de la API en la URI (/api/v1/usuarios). Esto evita romper el código de los clientes cuando se realizan cambios en la API.
  * Alternativamente, puedes usar un encabezado personalizado (Accept: application/vnd.tuempresa.v1+json).

### Códigos de Estado HTTP

#### Operaciones exitosas
Para operaciones exitosas se debe utilizar:
  * 200 OK: Éxito general.
  * 201 Created: El recurso se creó con éxito (después de un POST).
  * 204 No Content: La solicitud fue exitosa, pero no hay contenido que devolver (después de un DELETE).

#### Operaciones fallidas por data del consumidor
Para operaciones fallidas originadas en los datos proporcionados por el consumidor del servicio se deben utilizar según corresponda:
  * 400 Bad Request: La solicitud del cliente es incorrecta.
  * 401 Unauthorized: El cliente no tiene credenciales válidas.
  * 403 Forbidden: El cliente tiene credenciales, pero no tiene permisos para el recurso.
  * 404 Not Found: El recurso no existe.
  * 409 Conflict: Conflicto en el estado del recurso (ej. un recurso ya existe).

 > Siempre debe contener, según el estándar de objeto de error, la identificación del código de error interno (valor establecido único dentro de cada microservicio o dominio) y datos que describan el error.

#### Operaciones fallidas del lado del servidor
Para operaciones fallidas por problemas del lado del servidor que ofrece el microservicio:

  * 500 Internal Server Error: Algo salió mal en el servidor.
 > Siempre debe contener, según el estándar de objeto de error, la identificación del código de error interno (valor establecido único dentro de cada microservicio o dominio) y datos que describan el error.

## Paginación en consultas

Se debe proporcionar capacidad para paginación, filtrado en los endpoints destinados para consultas (GET). Para colecciones de recursos grandes, implementa mecanismos para limitar los resultados.

- Paginación: Utiliza parámetros de consulta como ?page=2&size=50 o ?offset=100&limit=50.

- Filtrado: Permite filtrar con parámetros de consulta (?estado=activo&tipo=premium).

- Búsqueda: Ofrece un parámetro de búsqueda general (?q=palabra_clave).

## Autenticación y Autorización

- Utiliza JWT (JSON Web Tokens) o OAuth 2.0 para la autenticación. Los JWT son ideales porque son "sin estado" (stateless) y se pueden verificar sin una consulta a la base de datos, lo que es perfecto para microservicios.

- Usar HTTPS. Nunca expongas tu API sin cifrado.

 > Los requerimientos de seguridad deben estar siempre alineados con las Politicas de Seguridad y norma ISO 27001 adoptadas por Comsatel.