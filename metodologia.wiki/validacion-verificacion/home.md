[[_TOC_]]

# Validación y Verificación de Software

De acuerdo con las buenas prácticas de industria el aseguramiento de calidad de productos software se realizan bajo dos tipos de actividades principales cuyos objetivos son diferentes. Estas son, de acuerdo con la Norma Técnica Peruana (NTP) ISO/IEC 12207, las siguientes:

1. Las Validaciones cuyo objetivo es _"...confirmación, mediante la aportación de evidencia objetiva de que se han cumplido los requisitos para una utilización o aplicación específica prevista"_. Es decir, según Bohem _¿se ha desarrollado el producto correcto?_. Según Roger Pressman, esta _...se refiere al conjunto de tareas que garantizan que el software implementa correctamente una función específica_.
2. Las verificaciones cuyo objetivo es _"...confirmación mediante la aportación de evidencia objetiva de que se han cumplido los requisitos especificados"_. Es decir, según Bohem _¿se ha desarrollado el producto de la forma correcta?_. Según Roger Pressman, esta incluye _un conjunto diferente de tareas que aseguran que el software que se construye sigue los requerimientos del cliente._

Desde la perspectiva de la metodología en COMSATEL nos enfocamos entonces en:

1. Validación: Comprobar que el producto software hace lo que el usuario espera de él.
2. Verificación: Evaluar si el producto satisface las condiciones establecidas al inicio del proyecto.

# Pruebas del Software

## Pruebas Funcionales

El objetivo principal de las Pruebas Funcionales es _comprobar si el producto o componente bajo pruebas satisface todos los requerimientos funcionales comprometidos en el alcance, proyecto y sprint_. 

Tomar en consideración que las funcionalidades del producto o componente bajo prueba surgen y evolucionan en base a las historias de usuario. 

Algunas consideraciones importantes a tomar en cuenta:

1. Se debe contar con al menos `un caso de prueba` asociado a cada requerimiento funcional.
   ![Esquema Conceptual](../requerimientos/images/esquema-conceptual-issues.png)
2. Se debe considerar que todas las pruebas funcionales automatizadas deben quedar dentro del [Proyecto Postman](https://project.comsatel.com.pe/comsatel/development/common/archetypes/microservice-springboot-archetype/-/blob/develop/src/main/resources/archetype-resources/test/functional/postman/Test.postman_collection) como parte integral de las fuentes del microservicio.
3. Se debe considerar que los resultados de las pruebas deben quedar completamente evidenciados asegurando objetivamente el resultado de las pruebas. En las pruebas automatizadas las evidencias deben quedar registradas en la ejecución del Pipeline.

## Pruebas de Carga

El objetivo principal de las Pruebas de Carga es _comprobar si el producto o componente bajo pruebas cumple con el nivel de carga requerido por cliente o negocio para el peor de los escenarios (picos de transaccionalidad) sin que se degrade el desempeño_

Algunas consideraciones importantes a tomar en cuenta:

1. Se debe contar con al menos `un caso de prueba` asociado a cada requerimiento no funcional de carga requerida
   ![Esquema Conceptual](../requerimientos/images/esquema-conceptual-issues.png)
2. Se debe realizar sobre funcionalidad sobre funcionalidades que ya hayan sido verificadas y los resultados de esas verificaciones confirmen que operan de forma exitosa.

## Pruebas de Seguridad
 - [Vulnerabilidades](https://project.comsatel.com.pe/comsatel/development/metodologia/-/wikis/gestion-configuracion/vulnerabilidad-contenedor)