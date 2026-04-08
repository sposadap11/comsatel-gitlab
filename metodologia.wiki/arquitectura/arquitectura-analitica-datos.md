[[_TOC_]]

# Arquitectura para Analítica de Datos (BI)

## Arquitectura de Referencia

Son un conjunto de patrones, estándares y directrices que proporciona una estructura y un marco de trabajo para el diseño e implementación de sistemas y soluciones en un dominio específico. Estos dominios pueden variar desde la tecnología de la información y las comunicaciones hasta la analítica, la seguridad, la nube, la inteligencia artificial, entre otros.

La Arquitectura de Referencia proporciona buenas prácticas, estandarización y homogenización debido a que estas se han recopilado de lecciones aprendidas en proyectos y tienen la capacidad de evolucionar luego de haber sido construidos. 

La arquitectura general de la(s) Plataformas de Datos transaccionales y para analítica deben integrarse de forma fluida. En el siguiente diagrama se incluye el contexto general.

![image](uploads/44a92c6737f03411a07149c8eca18a74/image.png)


Las Arquitecturas de Referencia, sean para Analitica de Datos u otro contexto, pueden darse en lenguajes de programación, en base datos, entre otros. Ejemplos de arquitectura de referencia: TOGAF, AWS, JAVA, SAP, entre otros, ya que permite que la colaboración y comunicación de manera eficaz en torno a un proyecto.


### Arquitectura Lógica

![Arquitectura Medallion](images/arquitectura-logica-bi-medallion.png)

### Arquitectura Física

![Arquitectura de Componentes](images/componentes-software-arquitectura-analitica.png)


### Principales Componentes

Es un marco o modelo que proporciona una guía estructurada y estándar para el diseño e implementación de soluciones de analítica de datos. Debe abordar todos los aspectos clave involucrados en la implementación efectiva de soluciones de analítica de datos, asegurando que se alinee con los objetivos comerciales y las necesidades de la organización.

Se deben tener en cuenta los siguientes elementos clave:

**1. Objetivos y requerimientos de negocio:** Es esencial comprender los objetivos comerciales y las necesidades específicas que se pretenden abordar con la analítica. Esto permitirá diseñar una arquitectura que se alinee con los objetivos y ofrezca valor real a la organización.

Con el aumento de datos a gran escala en las organizaciones y por supuesto en Comsatel, se busca una única fuente de confianza que pueda almacenar datos estructurados como BD SQL, semi estructurados como json y no estructurados como audio, videos, entre otros, generados por los equipos, personas e internet de las cosas (IoT); y crear grandes soluciones de análisis como reportes y visualizaciones; e inteligencia artificial.

**2. Fuentes de datos y almacenamiento:** Identificar las fuentes de datos relevantes para el análisis y decidir cómo se almacenarán estos datos. Pueden incluir bases de datos, data lakes, data warehouses, fuentes en la nube, entre otros.

Actualmente se contemplan los siguientes:

* Data source:

  Ubicación incicial de todos los datos.

  ![image](uploads/e709aa952d1cefe7c4627658c1b9a1a6/image.png)
* Data Storage:

  Es el espacio donde se almacenan los datos que llegan de dispositivos, computadoras, terminales. Los usuarios pueden realizar configuraciones de acceso y almacenamiento de estos datos.

**3. Procesamiento y transformación de datos:** Definir cómo se realizará el procesamiento y la transformación de los datos para prepararlos para el análisis. Esto puede implicar la limpieza de datos, agregación, enriquecimiento, integración de datos de diferentes fuentes, entre otros procesos.

* Data Flow:

Conjunto de pasos que definen un procesamiento de datos; es decir estos datos viajan desde un origen hasta un almacenamiento final en donde pueden ser analizados, todo ello usando estrategias y metologías.

* Data Processing

  En este punto se extrae data del data source y la ingesta puede ser por lotes o streaming.

  ![image](uploads/9ddc4afa3aac6b3901bf99d64f7b1257/image.png)

**4. Tecnologías y herramientas de analítica:** Seleccionar las herramientas y tecnologías adecuadas para llevar a cabo la analítica. Esto puede incluir lenguajes de programación (Python, R), herramientas de visualización (Tableau, Power BI), frameworks de machine learning, entre otros.

**5. Modelado y algoritmos:** Definir cómo se desarrollarán los modelos de análisis y los algoritmos que se utilizarán para extraer información y conocimientos de los datos. Esto incluye técnicas de machine learning, estadísticas, minería de datos, etc.

**6. Arquitectura de procesamiento y flujo de datos:** Diseñar la arquitectura que permitirá el flujo de datos desde las fuentes hasta los sistemas de analítica y visualización. Esto puede incluir el uso de pipelines, sistemas de mensajería, y tecnologías de big data. Ver: [Escenarios y Casuísticas para Arquitectura de Datos](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Escenarios-y-Casu%C3%ADsticas-para-Arquitectura-de-Datos)

![image](uploads/5e3634a3001ecf4fe149abfdad987f07/image.png)

**7. Seguridad y privacidad:** Considerar medidas de seguridad para proteger los datos sensibles y garantizar la privacidad de los usuarios. Esto implica el acceso seguro a los datos, el cumplimiento de regulaciones, y la encriptación, entre otros aspectos.

**8. Visualización y presentación de resultados:** Definir cómo se presentarán los resultados del análisis para que sean comprensibles y útiles para los usuarios finales. Esto incluye la creación de paneles de control, informes, gráficos interactivos, entre otros.

![image](uploads/50b751fe09ba3f5bb6fa0fb220cd0313/image.png)

8\.1. Superset

Software de código abierto para visualización de datos. Apache Superset admite varias bases de datos para el almacenamiento.

Solución analítica:

![image](uploads/b9c947af129176f4acfb9c8dde575c6e/image.png)

8\.2. H2o

Software de código abierto para el aprendizaje automático e inteligencia artificial.

**9. Gobierno de datos y calidad:** Establecer políticas y procedimientos para garantizar la calidad de los datos y la gestión adecuada de los mismos a lo largo del ciclo de vida del proyecto. Los datos deben ser privados, seguros, disponibles y se debe regular su ciclo de vida (procesamiento, almacenamiento, recopilación y eliminación)

![image](uploads/7f9de016e6ebf50eea40cae80fa6e6e5/image.png)

**10. Capacidades y roles:** Identificar las habilidades necesarias en el equipo de analítica y definir los roles y responsabilidades de cada miembro del equipo.
 Ver: 
- [Responsabilidades y Competencias](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Roles-(Responsabilidades-&-Competencias))
- [Dirección de Gestión\](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Direcci%C3%B3n-de-Gesti%C3%B3n)
- [Proceso de Desarrollo (SCRUM)](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Proceso-de-Desarrollo-(SCRUM))

# Fuentes

 1. [https://learn.microsoft.com/es-es/azure/architecture/solution-ideas/articles/azure-databricks-data-science-machine-learning](https://learn.microsoft.com/es-es/azure/architecture/solution-ideas/articles/azure-databricks-data-science-machine-learning)
 2. [https://aws.amazon.com/es/big-data/datalakes-and-analytics/modern-data-architecture/](https://aws.amazon.com/es/big-data/datalakes-and-analytics/modern-data-architecture/)
 3. [https://learn.microsoft.com/es-es/azure/architecture/solution-ideas/articles/azure-databricks-modern-analytics-architecture](https://learn.microsoft.com/es-es/azure/architecture/solution-ideas/articles/azure-databricks-modern-analytics-architecture)
 4. [https://learn.microsoft.com/es-es/azure/architecture/data-guide/](https://learn.microsoft.com/es-es/azure/architecture/data-guide/)
 5. [https://www.ibm.com/es-es/topics/data-architecture](https://www.ibm.com/es-es/topics/data-architecture)
 6. [https://cloud.google.com/data-science?hl=es](https://cloud.google.com/data-science?hl=es)
 7. [https://www.stitchdata.com/resources/data-pipeline-architecture/](https://www.stitchdata.com/resources/data-pipeline-architecture/)
 8. [https://www.oracle.com/pe/artificial-intelligence/what-is-ai/](https://www.oracle.com/pe/artificial-intelligence/what-is-ai/)
 9. [https://edgarjayo.wordpress.com/2021/09/15/que-es-una-arquitectura-de-referencia-en-ti-2/](https://edgarjayo.wordpress.com/2021/09/15/que-es-una-arquitectura-de-referencia-en-ti-2/)
10. [https://conexiam.com/es/what-is-a-reference-architecture/](https://conexiam.com/es/what-is-a-reference-architecture/)
11. [https://cloud.google.com/blog/topics/developers-practitioners/what-data-pipeline-architecture-should-i-use](https://cloud.google.com/blog/topics/developers-practitioners/what-data-pipeline-architecture-should-i-use)
12. [https://www.ibm.com/topics/data-storage](https://www.ibm.com/topics/data-storage)
13. [https://aprenderbigdata.com/pipeline-de-datos/](https://aprenderbigdata.com/pipeline-de-datos/)
14. [https://cloud.google.com/learn/what-is-data-governance?hl=es](https://cloud.google.com/learn/what-is-data-governance?hl=es)