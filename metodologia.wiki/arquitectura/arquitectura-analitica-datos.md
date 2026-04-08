[[_TOC_]]

# Arquitectura para Analítica de Datos (BI) - Comsatel

## 1. Introducción y Arquitectura de Referencia

### 1.1 Propósito y Alcance

Este documento explica cómo funciona el sistema de Inteligencia de Negocios (BI) y Análisis de Datos en Comsatel. Define las reglas y estándares que nos ayudan a diseñar, construir y operar nuestros sistemas de análisis de manera ordenada y eficiente.

Nuestro sistema conecta diferentes fuentes de datos y las organiza para que puedas acceder a información confiable. Almacenamos datos organizados (como bases de datos tradicionales), datos semi-organizados (como archivos JSON) y datos no estructurados (como audio, video o información de dispositivos IoT). Todo esto nos permite crear reportes, gráficos y herramientas de inteligencia artificial.

### 1.2 ¿Qué es una Arquitectura de Referencia?

Una Arquitectura de Referencia es como un manual de mejores prácticas que nos guía para construir sistemas de tecnología. Nos ayuda a trabajar de manera consistente y aprender de experiencias anteriores en proyectos similares.

Beneficios principales:

- **Mejores prácticas**: Soluciones probadas en proyectos reales
- **Estandarización**: Todos trabajamos de la misma manera
- **Mejora continua**: Aprendemos y evolucionamos constantemente
- **Trabajo en equipo**: Un lenguaje común entre técnicos y áreas de negocio

Ejemplos conocidos: TOGAF, AWS, Java EE, SAP.

### 1.3 Diagrama de Contexto General

![Contexto Arquitectónico General](uploads/44a92c6737f03411a07149c8eca18a74/image.png)

---

## 2. Vistas Arquitectónicas

### 2.1 Cómo Organizamos los Datos - Modelo Medallion

Usamos un sistema de tres capas llamado **Medallion** (Bronce-Plata-Oro) para mantener nuestros datos organizados, limpios y fáciles de usar:

![Arquitectura Lógica Medallion](images/arquitectura-logica-bi-medallion.png)

**Las tres capas:**

1. **Capa Bronce (Datos Crudos)**: Aquí guardamos los datos exactamente como vienen de los sistemas originales, sin hacerles cambios. Es como una copia de seguridad que siempre podemos consultar si algo sale mal.
2. **Capa Plata (Datos Limpios)**: En esta capa corregimos errores, estandarizamos formatos de fecha, eliminamos duplicados y organizamos la información para que sea consistente.
3. **Capa Oro (Datos Listos para Usar)**: Esta es la capa final donde los datos están completamente preparados y optimizados para crear reportes y dashboards. Apache Superset se conecta exclusivamente a esta capa para mostrar la información a los usuarios.

### 2.2 Componentes Tecnológicos

![Arquitectura de Componentes Físicos](images/componentes-software-arquitectura-analitica.png)

**Principales herramientas que utilizamos:**

| Componente | Tecnología | ¿Para qué sirve? |
|------------|-----------|----------|
| **Fuentes de Datos** | ERP, CRM, APIs, IoT | Sistemas donde se originan los datos |
| **Almacenamiento** | Data Lake, Data Warehouse | Lugares donde guardamos la información |
| **Procesamiento** | Motores Batch/Streaming | Herramientas que transforman y preparan los datos |
| **Visualización** | **Apache Superset** | **Plataforma principal para crear dashboards y reportes** |
| **Inteligencia Artificial** | H2O | Herramienta para análisis predictivo y machine learning |

**Importante:** Apache Superset es la herramienta principal que usamos en Comsatel para Business Intelligence. Actúa como puente entre los datos preparados y los usuarios finales.

---

## 3. Apache Superset - Plataforma Central de BI

### 3.1 ¿Por qué elegimos Apache Superset?

**Apache Superset** es la plataforma oficial que Comsatel utiliza para Inteligencia de Negocios. La elegimos porque es una solución profesional, gratuita (open-source), escalable y con excelentes capacidades para crear visualizaciones de datos.

**Ventajas principales:**

| Característica | Beneficio para Comsatel |
|---------------|------------------------|
| **Gratuito (Open-Source)** | Sin costos de licencias, comunidad activa que lo mejora constantemente |
| **Conecta múltiples bases de datos** | Se conecta con MySQL, PostgreSQL, Oracle, SQL Server, BigQuery, etc. |
| **Traduce datos técnicos** | Convierte nombres complicados de bases de datos en etiquetas fáciles de entender |
| **Seguridad avanzada** | Control de accesos por usuario, integración con nuestro sistema corporativo |
| **Rápido y eficiente** | Respuestas rápidas incluso con grandes volúmenes de datos |
| **Auto-servicio** | Los usuarios pueden explorar datos sin depender siempre del equipo de TI |
| **Flexible** | Se puede personalizar y ampliar según nuestras necesidades |

**¿Dónde se ubica Superset en nuestro sistema?**

Superset actúa como el puente entre los datos preparados (Capa Oro) y tú como usuario final. Esto nos permite:
- Tener **una sola versión de la verdad** para todas las métricas del negocio
- **Controlar quién ve qué información** de manera centralizada
- **Mantener un estándar** en todos los dashboards y reportes
- **Soportar muchos usuarios** al mismo tiempo sin problemas

![Superset Analytics Solution](uploads/b9c947af129176f4acfb9c8dde575c6e/image.png)

### 3.2 Capacidades Core de Apache Superset

#### 3.2.1 SQL Lab - Tu Espacio para Consultas SQL

**¿Para qué sirve?** SQL Lab es como un editor de texto especializado donde puedes escribir y probar consultas SQL antes de crear dashboards definitivos.

**Características principales:**
- Editor inteligente que te ayuda a escribir código SQL
- Puedes ejecutar consultas contra cualquier base de datos conectada
- Ves los resultados rápidamente en formato de tabla
- Puedes guardar consultas como datasets temporales (uso limitado, ver sección 4.2.1)
- Exportar resultados a Excel o CSV

**Recomendaciones de uso:**
- Usa SQL Lab **solo** para pruebas rápidas y exploración
- No compartas datasets temporales directamente con otros usuarios
- Cuando una consulta funcione bien, conviértela en un dataset definitivo en la Capa Oro

#### 3.2.2 Datasets - Cómo Preparamos los Datos para Ti

**¿Qué son los Datasets?** Los Datasets son la forma en que organizamos y presentamos los datos de las bases de datos para que sean fáciles de entender y usar.

**Tipos de Datasets:**

| Tipo | ¿Qué es? | ¿Cuándo lo usamos? |
|------|-------------|------------------|
| **Physical Dataset** | Se conecta directamente a una tabla preparada en la base de datos | **Recomendado**. Es más rápido y eficiente |
| **Virtual Dataset** | Ejecuta una consulta SQL cada vez que lo usas | Solo para pruebas o filtros especiales |

**¿Qué podemos configurar en un Dataset?**

1. **Columnas:**
   - Nombre fácil de entender (traducimos los nombres técnicos)
   - Tipo de dato (texto, número, fecha, sí/no)
   - Si se muestra u oculta al usuario
   - Si se puede usar para agrupar información
   - Si se puede usar como filtro

2. **Métricas (Cálculos):**
   - Fórmulas predefinidas (SUMA, PROMEDIO, CONTEO)
   - Cálculos personalizados en SQL
   - Formato de visualización (moneda, porcentaje, decimales)
   - Explicación de qué significa cada métrica

3. **Filtros Avanzados:**
   - Rangos de fechas dinámicos
   - Filtros personalizados según el usuario
   - Condiciones especiales en SQL

**Ejemplo de Configuración de Métrica:**

```sql
-- Nombre: Ventas Netas Totales
-- Expresión: SUM(monto_neto)
-- Formateo: Currency ($)
-- Descripción: Suma total de ventas netas después de devoluciones e impuestos
```

#### 3.2.3 Charts - Tipos de Gráficos Disponibles

**Catálogo de Visualizaciones:**

| Categoría | Tipos Disponibles | ¿Para qué sirve? |
|-----------|------------------|-------------|
| **Indicadores Clave** | Número Grande, Número con Tendencia | Mostrar métricas importantes de un vistazo |
| **Barras** | Barras Verticales, Horizontales, Apiladas | Comparar categorías |
| **Líneas** | Línea Simple, Área, Escalonada | Ver tendencias en el tiempo |
| **Tortas** | Torta, Dona, Rosa | Mostrar distribuciones porcentuales |
| **Tablas** | Tabla Simple, Tabla Dinámica, Mapa de Calor | Ver detalles específicos |
| **Mapas** | Mapbox, Deck.gl, Mapa por País | Análisis por ubicación geográfica |
| **Avanzados** | Treemap, Sunburst, Sankey, Nube de Palabras | Mostrar composiciones complejas |

**Estándar para Gráficos en Comsatel:**

- **Títulos claros** en lenguaje cotidiano
- **Información emergente** (tooltips) que explica los datos
- **Leyendas legibles** que no tapen el gráfico
- **Colores corporativos** (verde/azul para positivo, rojo/naranja para alertas)
- **Interactividad** activada (puedes hacer clic para filtrar)

#### 3.2.4 Dashboards - Cómo Organizamos la Información

**¿Qué es un Dashboard?** Un Dashboard agrupa varios gráficos en una sola pantalla para que puedas ver toda la información relacionada de manera organizada.

**Diseño Estándar - Patrón en Z:**

![Dashboard Example](uploads/50b751fe09ba3f5bb6fa0fb220cd0313/image.png)

| Zona | Contenido | Objetivo |
|------|-----------|----------|
| **Parte Superior (20%)** | Números Grandes + Indicadores Clave | Ver el estado general rápidamente |
| **Centro (60%)** | Gráficos de análisis (barras, líneas) | Explorar tendencias y detalles |
| **Parte Inferior (20%)** | Tablas con información detallada | Descargar datos o ver detalles específicos |

**Funcionalidades Principales:**

1. **Filtros Cruzados (Cross-Filtering):**
   - Al hacer clic en cualquier parte de un gráfico, automáticamente se filtran todos los demás gráficos
   - La actualización es instantánea
   - **Obligatorio** en todos los dashboards que usamos

2. **Filtros Globales:**
   - Selectores de fecha, región o categoría que aplican a todo el dashboard
   - Puedes compartir el enlace con los filtros ya aplicados
   - Podemos configurar valores predeterminados

3. **Pestañas y Organización Flexible:**
   - Podemos organizar la información en pestañas temáticas
   - Arrastrar y soltar para reorganizar gráficos fácilmente
   - Se adapta a computadora, tablet y celular

4. **Exportar y Compartir:**
   - Descargar gráficos como imagen (PNG/SVG)
   - Exportar datos a Excel o CSV
   - Compartir enlaces con permisos específicos
   - Insertar dashboards en otras aplicaciones

5. **Alertas y Reportes Automáticos:**
   - Recibir correos cuando una métrica supere ciertos límites
   - Reportes programados (diarios, semanales, mensuales) en PDF
   - Suscribirte a dashboards específicos

---

## 4. Ciclo de Vida del Proyecto BI 

Todo proyecto analítico en Comsatel debe estructurarse obligatoriamente en los siguientes hitos, garantizando control temporal, asignación de responsables e identificación de ruta crítica.

### 4.1 Definición de Arquitectura Base y Lanzamiento

#### 4.1.1 Arquitectura de Documentación BI para Levantamiento de Proyectos

**Descripción:** Establecimiento de la documentación base que guiará todo el ciclo de vida del proyecto, incluyendo templates, checklists y estándares de nomenclatura.

**Entregables:**
- Templates estandarizados de Issues en GitLab
- Checklists de validación por fase
- Estándares de nomenclatura para artefactos

#### 4.1.2 Daily Meeting: Sincronización y Desbloqueo Técnico (Recurrente)

**Descripción:** Ceremonia diaria de máximo 15 minutos para sincronización del equipo.

**Objetivos:**
- Sincronizar avances del equipo
- Identificar bloqueos técnicos tempranos
- Reasignar recursos si hay desviaciones en la ruta crítica

**Duración:** 15 minutos máximo  
**Frecuencia:** Diaria (días hábiles)  
**Participantes:** Equipo técnico completo

#### 4.1.3 Implementación de Templates Globales de Issues en GitLab

**Descripción:** Estandarización de la creación de tareas técnicas en GitLab para garantizar uniformidad y trazabilidad.

**Template Obligatorio para Issues:**

```markdown
[[_TOC_]]

## ¿Qué hay que hacer?
- Describa las actividades necesarias para cumplir el objetivo de la tarea.

## Criterios de aceptación
> Son condiciones que tienen que cumplir la tarea para ser considerada como cerrada
- [ ] Describa el criterio de aceptación.

## Artefactos involucrados
> Información debe ser registrada por DEV

| Artefacto | Versión Anterior | Versión Nueva | 
| --------- | ---------------- | ------------- |
|  |  |  |

/label ~Task
```

**Componentes del Template:**

| Componente | Propósito | Ejemplo |
|------------|-----------|----------|
| `[[_TOC_]]` | Macro GitLab para tabla de contenidos automática | Genera índice navegable |
| `¿Qué hay que hacer?` | Contexto técnico detallado | "Crear vista materializada en capa Oro que agregue ventas diarias por sucursal, excluyendo estado 'Anulado'" |
| `Criterios de aceptación` | Definition of Done micro | "Query demora <3s", "Métrica coincide 100% con ERP" |
| `Artefactos involucrados` | Control de versiones y auditoría | "etl_viajes.py \| v1.2 → v1.3" |
| `/label ~Task` | Etiquetado automático para métricas Kanban | Clasifica como tarea operativa |

**Nomenclatura Estricta de Issues:**

**Formato:** `[Tipo] Breve descripción`

**Tipos Válidos:**
- `[Extract]`: Extracción de datos desde fuentes origen
- `[Transform]`: Transformación y limpieza de datos
- `[Superset-Model]`: Modelado semántico en Superset
- `[Superset-Viz]`: Creación de visualizaciones
- `[QA]`: Pruebas y validación de calidad

**Ejemplo:** `[Transform] Limpieza de fechas nulas en tabla de viajes`

---

### 4.2 Metodología de Trabajo e Ingeniería de Datos

#### 4.2.1 Protocolo de Perfilamiento y Limpieza de Datos (Data Profiling)

**Descripción:** Ejecución de consultas de perfilamiento mandatorias antes de codificar conectores o extraer información.

**Checklist de Análisis de Fuentes:**

**1. Volumetría y Escalabilidad**
- ¿Cuántos millones de registros existen?
- ¿Cuál es la tasa de ingesta diaria?
- Dictamina estrategia: Full Refresh vs Incremental (basado en timestamps)

**2. Anomalías y Tipado**
- Detectar formatos de fecha incompatibles
- IDs nulos o duplicados
- Conflictos de codificación (Latin-1 vs UTF-8) que corrompan caracteres españoles (tildes, eñes)

**3. Integridad Referencial**
- Mapear Primary Keys (PK) y Foreign Keys (FK)
- Asegurar que JOINs no generen productos cartesianos

![Data Source](uploads/e709aa952d1cefe7c4627658c1b9a1a6/image.png)

**Fuentes de Datos Contempladas:**

| Tipo | Descripción |
|------|-------------|
| **Data Source** | Ubicación inicial de todos los datos |
| **Data Storage** | Espacio de almacenamiento configurable con políticas de acceso |

---

### 4.3 Construcción de la Tubería y Plataforma

#### 4.3.1 Optimización de Infraestructura, Performance y Escalabilidad

**Descripción:** Diseño e implementación de la arquitectura de procesamiento y flujo de datos.

**Procesamiento y Transformación de Datos:**

Definir cómo se realizará el procesamiento para preparar datos para análisis:
- Limpieza y validación
- Agregación y enriquecimiento
- Integración multi-fuente

**Arquitectura de Flujo de Datos:**

![Data Flow](uploads/9ddc4afa3aac6b3901bf99d64f7b1257/image.png)

![Pipeline de Procesamiento](uploads/5e3634a3001ecf4fe149abfdad987f07/image.png)

**Definiciones:**

| Término | Definición |
|---------|------------|
| **Data Flow** | Conjunto de pasos que definen el procesamiento; los datos viajan desde origen hasta almacenamiento final analizable |
| **Data Processing** | Extracción desde data source con ingesta batch o streaming |

**Habilitación y Disponibilidad:**

**Requisitos Técnicos:**
- Utilizar únicamente credenciales **Read-Only** en bases transaccionales origen
- Orquestador (Airflow/Cron) con reintentos automáticos configurados ante fallos de red
- Garantizar cumplimiento de SLA de actualización matutina

**Ejemplo de SLA:** Datos listos a las 06:00 AM requiere procesos batch nocturnos terminados antes de las 04:00 AM

**Referencia Relacionada:** [Escenarios y Casuísticas para Arquitectura de Datos](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Escenarios-y-Casu%C3%ADsticas-para-Arquitectura-de-Datos)

---

### 4.4 Desarrollo de Dashboards Operativos en Superset

#### 4.4.1 Configuración de Capa Semántica y Estándares UI/UX en Superset

**Descripción:** Configuración de la capa semántica y aplicación de estándares de diseño en Apache Superset.

##### 4.4.1.1 Estándares para Datasets en Superset

**Importante:** Configurar correctamente los Datasets es **fundamental** para el éxito del proyecto. Un Dataset mal configurado puede causar:
- Consultas lentas que hacen esperar al usuario
- Métricas incorrectas que generan desconfianza
- Dificultad para mantener y mejorar el dashboard

**Physical Datasets vs Virtual Datasets:**

| Tipo | Uso Recomendado | Ventajas |
|------|----------------|----------|
| **Physical Datasets** | Maximizar uso. Apuntar a tablas materializadas de Capa Oro | Delega carga computacional a la BD, permite caché eficiente |
| **Virtual Datasets (SQL Lab)** | Restringir. Solo para filtros paramétricos dinámicos (Jinja templating) o prototipado rápido | Flexibilidad temporal antes de consolidar en Capa Oro |

**Traducción de Nombres Técnicos (OBLIGATORIO):**

**Regla de Oro:** Los usuarios NUNCA deben ver nombres técnicos complicados. Cada campo debe tener un nombre fácil de entender.

**Cómo traducir nombres en Superset:**

1. Ve a **Datasets → [Nombre del Dataset] → Editar**
2. Pestaña **Columns** (Columnas)
3. Para cada columna, completa:
   - **Verbose Name**: Nombre amigable (ej: "Monto Total de Ventas")
   - **Description**: Explicación de qué significa (aparece como ayuda)
   - **Filterable**: Sí/No (¿se puede usar como filtro?)
   - **Groupable**: Sí/No (¿se puede agrupar información?)
   - **Visible**: Sí/No (¿se muestra al usuario?)

**Ejemplos de Traducción:**

| Columna Técnica | Label Requerido | Descripción (Tooltip) |
|----------------|----------------|----------------------|
| `mnt_tot_vta_01` | Monto Total de Ventas | Suma de ventas brutas antes de descuentos |
| `flg_act` | Estado Activo | Indicador binario: 1=Activo, 0=Inactivo |
| `fec_reg_ts` | Fecha de Registro | Timestamp de creación del registro (UTC) |
| `cod_cli_fk` | Código de Cliente | ID único del cliente en sistema CRM |

##### 4.4.1.2 Cómo Crear Métricas y Cálculos en Superset

**Importante:** Las métricas que definimos en Superset son la **fuente oficial** para el negocio. Debemos validarlas cuidadosamente antes de publicarlas.

**Pasos para Crear una Métrica:**

1. Ve a **Datasets → [Nombre del Dataset] → Editar → Metrics**
2. Haz clic en **+ Metric** (Agregar Métrica)
3. Configura:
   - **Metric Name**: Nombre técnico (sin espacios, ej: `ventas_netas_total`)
   - **Verbose Name**: Nombre amigable (ej: "Ventas Netas Totales")
   - **Expression**: Fórmula SQL (ej: `SUM(monto_neto)`)
   - **Metric Type**: Simple (suma, promedio) o Python (lógica compleja)
   - **D3 Format**: Formato visual (ej: `$,.2f` para moneda con dos decimales)
   - **Description**: Explicación de la fórmula y reglas de negocio
   - **Extra**: Configuraciones avanzadas en JSON (opcional)

**Requisitos Obligatorios:**
- Crear métricas nativas guardadas en la pestaña Metrics del Dataset (ej: `SUM(monto_neto)`)
- Dar formato adecuado (añadir símbolo "$" para moneda)
- Ocultar campos técnicos o IDs que solo causan confusión
**Ejemplos de Métricas Estándar:**

| Métrica | Expresión SQL | D3 Format | Descripción |
|---------|--------------|-----------|-------------|
| Ventas Brutas | `SUM(monto_bruto)` | `$,.2f` | Total ventas antes de descuentos |
| Ventas Netas | `SUM(monto_neto)` | `$,.2f` | Ventas después de descuentos e impuestos |
| Ticket Promedio | `AVG(monto_neto)` | `$,.2f` | Valor promedio por transacción |
| Tasa Conversión | `COUNT(DISTINCT id_cliente) / COUNT(*) * 100` | `.2%` | Porcentaje de visitantes que compran |
| Clientes Activos | `COUNT(DISTINCT CASE WHEN flg_act = 1 THEN id_cliente END)` | `,` | Clientes con estado activo |

##### 4.4.1.3 Diseño Visual de Dashboards en Superset

**Guía de Diseño:**

**Jerarquía Visual (Patrón en Z):**

| Zona | Contenido | Propósito |
|------|-----------|-----------|
| **Franja Superior** | Big Numbers y KPIs totales consolidados | Lectura rápida del estado general |
| **Cuerpo Central** | Gráficos de tendencias y composición (barras, líneas, treemaps) | Análisis detallado |
| **Franja Inferior** | Tablas de detalles granulares | Exportación y escrutinio contable |

**Interactividad Sin Esfuerzo:**

**Filtros Cruzados (OBLIGATORIO):**
- **Configuración:** En cada gráfico, activa la opción "Emitir filtro cruzado"
- **Cómo funciona:** Al hacer clic en una barra, segmento o punto, automáticamente se filtran todos los gráficos del dashboard
- **Beneficio:** Exploración intuitiva sin necesidad de aplicar filtros manualmente
- **Validación:** Verificar que todos los gráficos respondan a los clics

**Colores Corporativos:**
- Usar la paleta oficial de Comsatel
- **Contraste lógico:** Verde/Azul para métricas positivas; Rojo/Naranja para alertas o disminuciones

**Visualización y Presentación de Resultados:**

![Dashboard Example](uploads/50b751fe09ba3f5bb6fa0fb220cd0313/image.png)

**Cómo Configurar Colores en Superset:**

1. Ve a **Settings → Feature Flags** (Configuración → Banderas de Funciones)
2. Habilita `ENABLE_TEMPLATE_PROCESSING`
3. Define la paleta corporativa en CSS personalizado:
   ```css
   :root {
     --color-positivo: #28a745;  /* Verde */
     --color-negativo: #dc3545;  /* Rojo */
     --color-alerta: #fd7e14;    /* Naranja */
     --color-info: #007bff;      /* Azul */
   }
   ```

##### 4.4.1.4 Cómo Mejorar el Rendimiento en Superset

**Estrategias para que todo funcione más rápido:**

| Estrategia | ¿Cómo se implementa? | Resultado |
|-----------|---------------|----------|
| **Usar Physical Datasets** | Conectar a tablas ya preparadas en la Capa Oro | Reduce la carga en la base de datos original |
| **Caché de Consultas** | Configurar Redis/Memcached para guardar resultados | Respuestas en menos de 2 segundos para consultas repetidas |
| **Pre-agregar datos** | Crear vistas resumidas en Capa Oro (diarias, mensuales) | Elimina cálculos en tiempo real |
| **Índices en Base de Datos** | Asegurar índices en columnas de filtro/agrupación | Acelera las búsquedas |
| **Limitar Datos** | Aplicar límite de filas en gráficos (ej: top 50 categorías) | Evita cargar demasiada información |
| **Evitar Virtual Datasets** | Migrar a Physical Datasets siempre que sea posible | Rendimiento más consistente |

**Tiempos de Respuesta Esperados:**
- **Dashboard completo (primera vez):** Menos de 5 segundos
- **Gráfico individual (con caché):** Menos de 2 segundos
- **Filtros cruzados:** Menos de 1 segundo
- **Exportar a Excel (menos de 10,000 filas):** Menos de 3 segundos

**Cómo Monitorear el Rendimiento:**
- Revisar registros de Superset en `/var/log/superset/`
- Identificar consultas lentas (más de 3 segundos) en **SQL Lab → Query Search**
- Optimizar gradualmente basándose en el uso real

---

### 4.5 Gobernanza, Seguridad y QA en Superset

#### 4.5.1 Pruebas de Calidad y Validación de Datos en Superset

**Importante:** Publicar un dashboard sin pruebas documentadas es una falta grave a nuestra metodología de trabajo.

**Herramientas de Prueba en Superset:**

**1. Validación de Métricas (Comparación con Origen):**
**Cómo hacerlo en Superset:**
   - Abre el gráfico en modo **Editar**
   - Revisa la consulta SQL generada en la pestaña **Query**
   - Copia la consulta y ejecútala en SQL Lab contra la base de datos original
   - Compara que el resultado numérico sea exactamente igual
   - **Tolerancia:** 0% de diferencia
   - **Si hay diferencia:** Revisa las uniones (JOINs) y filtros en el Dataset

**2. Pruebas de Velocidad y Rendimiento:**
**Metodología de Prueba:**
   - Limpia el caché: **Settings → Clear Cache** (Configuración → Limpiar Caché)
   - Abre el dashboard en una ventana privada (sin sesión previa)
   - Mide el tiempo hasta que se muestra completo
   - **Meta:** Dashboard completo en menos de 5 segundos
   - **Si tarda más:** Optimiza el Dataset (ver sección 4.4.1.4)

**3. Pruebas de Interfaz Visual:**
**Lista de Verificación:**
   - La información emergente (tooltips) aparece al pasar el mouse
   - Los ejes son legibles (rotación automática si hay muchas categorías)
   - Las leyendas no tapan el gráfico (máximo 15% del espacio)
   - El dashboard se ve bien en resolución estándar (1366x768)
   - Los filtros cruzados funcionan en todos los gráficos interactivos
   - Los colores cumplen con el estándar corporativo

#### 4.5.2 Seguridad: Control de Accesos y Roles en Superset

**Marco de Seguridad en Comsatel:**

Apache Superset proporciona herramientas de seguridad profesionales que debemos configurar cuidadosamente para cumplir con las políticas de protección de datos de Comsatel.

##### 4.5.2.1 Gestión de Roles Personalizados

**Roles Predeterminados de Superset (NO USAR directamente):****

| Rol Predeterminado | Permisos | Estado en Comsatel |
|-----------|----------|--------------------|
| **Admin** | Acceso total, gestión de usuarios | ❌ Solo arquitectos de plataforma |
| **Alpha** | Crear/editar dashboards, SQL Lab sin restricciones | ❌ **PROHIBIDO** para usuarios de negocio |
| **Gamma** | Solo visualizar dashboards publicados | ⚠️ Base para rol Visualizador personalizado |
| **sql_lab** | Acceso a SQL Lab | ⚠️ Asignar solo a rol Creador |

**Roles Personalizados de Comsatel (CREAR OBLIGATORIAMENTE):**

**Cómo Crear un Rol Personalizado:**

1. Ve a **Settings → List Roles → + Add Role** (Configuración → Lista de Roles → Agregar Rol)
2. Nombre: `Creador_Operaciones` o `Visualizador_Ventas`
3. Selecciona permisos específicos:
   - **Para Creador:** `can_edit on Dashboard`, `can_write on Dataset`, `can_sql_json on SqlLab`
   - **Para Visualizador:** `can_view on Dashboard`, `can_read on Dataset`
4. Asigna usuarios desde **Settings → List Users** (Configuración → Lista de Usuarios)
5. Valida los permisos con una cuenta de prueba

##### 4.5.2.2 Cómo Implementar Seguridad por Fila (RLS) en Superset

**¿Qué es RLS?** Row Level Security (Seguridad por Fila) filtra los datos según el usuario que está conectado, asegurando que cada persona vea SOLO la información autorizada para su perfil.

**Casos de Uso Típicos en Comsatel:**
- Gerentes regionales ven solo datos de su región
- Vendedores ven solo sus propias transacciones
- Proveedores externos ven solo datos de sus contratos

**Pasos Detallados para Configurar RLS:**

**Paso 1: Crear Tabla de Mapeo de Accesos en la Base de Datos**

```sql
-- Crear tabla dim_accesos en esquema de BI
CREATE TABLE dim_accesos (
    email_usuario VARCHAR(255) PRIMARY KEY,
    id_region INT,
    id_departamento INT,
    rol_bi VARCHAR(50)
);

-- Insertar mapeos
INSERT INTO dim_accesos VALUES 
('juan.perez@comsatel.com', 1, 10, 'Gerente Regional'),
('maria.gomez@comsatel.com', 2, 20, 'Gerente Regional');
```

**Paso 2: Configurar RLS en Superset**

1. Ve a **Security → Row Level Security Filters → + Add Filter** (Seguridad → Filtros de Seguridad por Fila → Agregar Filtro)
2. Completa los campos:
   - **Name**: `rls_region_por_usuario`
   - **Description**: "Filtra datos por región basada en email de usuario"
   - **Tables**: Selecciona el/los Dataset(s) afectados
   - **Clause** (Condición SQL):
   ```sql
   region_id = (
       SELECT id_region 
       FROM dim_accesos 
       WHERE email_usuario = '{{ current_username() }}'
   )
   ```
   - **Roles**: Asigna a los roles que requieren este filtro (ej: `Visualizador_Ventas`)
   - **Enabled**: ✅ Yes (Activado)

3. Haz clic en **Save** (Guardar)

**Paso 3: Validar que RLS Funciona Correctamente**

1. Inicia sesión con un usuario de prueba (ej: juan.perez@comsatel.com)
2. Abre el dashboard afectado
3. Verifica que solo muestra datos de región_id = 1
4. Repite con un usuario de otra región para confirmar que no ven datos de otras regiones

##### 4.5.2.3 Integración con Sistema Corporativo y Auditoría

**Autenticación Empresarial:**

Superset se conecta automáticamente con el directorio corporativo de Comsatel mediante LDAP/SSO, eliminando la necesidad de recordar contraseñas adicionales.

**Configuración LDAP (superset_config.py):****

```python
AUTH_TYPE = AUTH_LDAP
AUTH_LDAP_SERVER = 'ldap://ldap.comsatel.com:389'
AUTH_LDAP_USE_TLS = True
AUTH_LDAP_SEARCH = 'DC=comsatel,DC=com,DC=pe'
AUTH_LDAP_BIND_USER = 'CN=svc_superset,OU=Service Accounts,DC=comsatel,DC=com,DC=pe'
AUTH_LDAP_BIND_PASSWORD = os.environ.get('LDAP_BIND_PASSWORD')
AUTH_LDAP_UID_FIELD = 'sAMAccountName'
AUTH_LDAP_FIRSTNAME_FIELD = 'givenName'
AUTH_LDAP_LASTNAME_FIELD = 'sn'
AUTH_LDAP_EMAIL_FIELD = 'mail'
```

**Cómo Funciona el Inicio de Sesión:**
1. El usuario accede a `https://superset.qa.comsatel.com.pe/superset/welcome/`
2. Redirección al login corporativo (SSO)
3. Validación contra Active Directory
4. Superset crea/actualiza el usuario automáticamente
5. Asignación de roles basada en grupos de AD (configurable)

**Auditoría y Cumplimiento:**

**Registros de Actividad en Superset:**

Ubicación: `/var/log/superset/superset.log`

**Eventos que se Registran:**
- Inicio/cierre de sesión de usuarios
- Creación/edición/eliminación de dashboards
- Ejecución de consultas en SQL Lab
- Cambios en configuración de Datasets
- Modificaciones en roles y permisos
- Exportaciones de datos (CSV/Excel)

**Proceso de Revisión Trimestral:**

1. Extraer registros de acceso de los últimos 90 días:
   ```bash
   grep "INFO:werkzeug:" /var/log/superset/superset.log | \
   awk '{print $1, $2, $7}' | sort | uniq -c | sort -rn
   ```

2. Identificar usuarios inactivos (más de 90 días sin iniciar sesión)

3. Revocar permisos automáticamente:
   ```sql
   -- Consulta para encontrar usuarios inactivos
   SELECT username, email, last_login 
   FROM ab_user 
   WHERE last_login < NOW() - INTERVAL '90 days';
   ```

4. Generar reporte de auditoría para Compliance

5. Documentar acciones correctivas en GitLab Issue

**Seguridad y Privacidad de Datos:**

![Data Governance](uploads/7f9de016e6ebf50eea40cae80fa6e6e5/image.png)

**Medidas de Protección Implementadas:****

| Capa | Medida | Implementación |
|------|--------|----------------|
| **Acceso** | Autenticación LDAP/SSO | Directorio corporativo centralizado |
| **Autorización** | Roles personalizados + RLS | Mínimo privilegio por diseño |
| **Encriptación** | TLS 1.3 en tránsito | Certificado SSL corporativo |
| **Encriptación** | AES-256 en reposo | Base de datos metadata encriptada |
| **Auditoría** | Logs completos | Retención 1 año, revisión trimestral |
| **Backup** | Snapshots diarios | Retención 30 días, offsite weekly |
| **Compliance** | GDPR/LFPDPPP | Derecho al olvido implementado |

---

### 4.6 QA, Testing y Documentación Final

#### 4.6.1 Documentación Técnica que Debemos Entregar

**Descripción:** Generación de documentación completa para cerrar el proyecto.

**Documentación Específica de Superset que Debemos Entregar:****

| Documento | Formato | Contenido Específico de Superset |
|-----------|---------|----------------------------------|
| **Manual de Arquitectura** | Markdown | Arquitectura de Superset + procedimientos |
| **Diccionario de Datasets** | Excel | Metadatos de todos los Datasets, métricas, filtros RLS |
| **Guía de Usuario de Superset** | PDF de 1 página | Cómo navegar, filtrar, exportar en Superset |
| **Catálogo de Dashboards** | Wiki GitLab | Lista de dashboards productivos con responsables y tiempos de respuesta |
| **Registro de Configuraciones** | YAML/JSON | Respaldo de configuraciones de roles, RLS, opciones especiales |

**Gobierno de Datasets en Superset:**

Establecer políticas para garantizar calidad y consistencia de los Datasets durante todo su ciclo de vida.

**Requisitos para Datasets:**
- **Nombres claros**: Etiquetas descriptivas, explicaciones documentadas
- **Métricas validadas**: Fórmulas aprobadas por el responsable del área de negocio
- **Seguridad aplicada**: RLS configurado según política de accesos
- **Rendimiento óptimo**: Consultas en menos de 3 segundos (95% de las veces)
- **Versionado**: Cambios registrados en Issues de GitLab

---

### 4.7 Entrega Final, Demostración y Cierre del Proyecto

#### 4.7.1 Despliegue Oficial, Demostración Práctica y Cierre

##### 4.7.1.1 Cómo Hacer la Demostración "Manos a la Obra"

**Metodología "Manos a la Obra" Específica para Superset:**

**Regla Crítica:** El Analista NUNCA opera Superset durante la demostración. El cliente debe interactuar directamente con la plataforma.

**Guión de Demostración:**

1. **Transferir control:** Compartir pantalla y pasar el control del mouse al cliente
2. **Escenario real:** "Filtra el mes de marzo y dime cuál sucursal tuvo más incidencias"
3. **Observar comportamiento:**
   - ¿El cliente encuentra fácilmente los filtros?
   - ¿Los gráficos responden correctamente a los filtros cruzados?
   - ¿La información emergente es comprensible?
4. **Documentar comentarios:** Capturar ajustes visuales solicitados
5. **Crear tareas pequeñas:** Issues en GitLab para cambios menores (menos de 2 días)

**Beneficio:** Elimina el miedo a la tecnología y acelera la adopción

##### 4.7.1.2 Pase a Producción en Superset

**Lista de Verificación Pre-Producción:**

- [ ] Estado del Dashboard cambiado de **Draft** (Borrador) → **Published** (Publicado)
- [ ] RLS configurado y validado con usuarios de prueba
- [ ] Roles personalizados asignados correctamente
- [ ] Caché configurado (Redis/Memcached)
- [ ] Opciones especiales revisadas (ENABLE_TEMPLATE_PROCESSING, etc.)
- [ ] URLs oficiales compartidas vía correo corporativo
- [ ] Marcadores creados para accesos rápidos
- [ ] Alertas/reportes programados configurados (si aplica)

##### 4.7.1.3 Capacitación y Cierre en Superset

**Materiales de Capacitación:**

1. **Guía Rápida de Superset:**
   - URL de acceso: `https://superset.qa.comsatel.com.pe/superset/welcome/`
   - Cómo aplicar filtros nativos (barra lateral izquierda)
   - Cómo usar filtros cruzados (clic en gráficos)
   - Cómo exportar datos (botón download en gráficos)
   - Cómo guardar marcadores personales
   - Contacto soporte: sebastian.posada@globalr.net


2. **Cierre en GitLab:**
   - Épicas marcadas como **Done** (Completado)
   - Registro de tiempo completo (comandos `/spend` validados)
   - Carta Gantt cerrada
   - Lecciones aprendidas documentadas

#### 4.7.2 Seguimiento Continuo y Revisión de Adopción

##### 4.7.2.1 Monitoreo de Uso y Retroalimentación

**Programación:** Reunión de revisión a los **30 días después del lanzamiento**

**Métricas de Adopción en Superset:**

Superset proporciona herramientas nativas para medir el uso real:

1. **Ve a:** **Settings → Activity Log** (Configuración → Registro de Actividad)
2. **Filtra por:** Dashboard específico, rango de fechas
3. **Métricas clave:**
   - Número de usuarios únicos (últimos 30 días)
   - Frecuencia de acceso (usuarios activos diarios/semanales)
   - Gráficos más consultados
   - Exportaciones realizadas (CSV/Excel)
   - Consultas lentas identificadas

**Objetivos de la Reunión de Revisión:**
- Presentar métricas de adopción reales
- Identificar dashboards poco utilizados (menos de 5 usuarios/mes)
- Levantar nuevos requerimientos
- Validar satisfacción del usuario (encuesta NPS)

**Regla para Nuevos Requerimientos:**

Cualquier solicitud de:
- Nueva dimensión de análisis no contemplada inicialmente
- Métrica adicional con lógica compleja
- Integración con nueva fuente de datos
- Dashboard completamente nuevo

...constituye un **nuevo proyecto BI**, iniciando el ciclo desde la Fase 1 (sección 4.1).

**Cambios menores** (ajustes visuales, corrección de nombres) se gestionan como correcciones en GitLab con prioridad baja.

---

## 5. Gestión Ágil y Control de Proyecto

### 5.1 Estructura de Épicas e Historias de Usuario en GitLab

**Definiciones:**

| Elemento | Descripción | Ejemplo |
|----------|-------------|----------|
| **Épicas** | Entregables funcionales de alto valor. Agrupan múltiples tareas técnicas | `[BI-OPS] Dashboard de Rendimiento de Flota V1.0` |
| **Issues** | Tareas atómicas, técnicas y ejecutables conectadas a la Épica | `[Transform] Limpieza de fechas nulas` |

### 5.2 Gestión del Tablero Kanban y WIP

**Flujo de Estados (Estricto y Unidireccional):**

| Estado | Descripción | Reglas |
|--------|-------------|--------|
| **Backlog** | Requerimientos sin Definition of Ready (DoR) | No pueden moverse a To Do hasta cumplir DoR |
| **To Do** | Issues priorizados, listos para Sprint | Asignación pendiente |
| **In Progress** | Tarea en ejecución activa | **WIP Limit:** Máximo 2 issues simultáneos por analista |
| **Review (QA)** | Desarrollo finalizado, esperando Code Review o pruebas de integridad | Requiere aprobación de par |
| **Done** | Código fusionado, desplegado en producción y aprobado por usuario | Cierre definitivo |

**Regla WIP (Work in Progress):** Máximo 2 issues simultáneos por analista para evitar parálisis por análisis y context-switching.

### 5.3 Registro de Esfuerzo (Time Tracking)

**Requisitos de Seguimiento:**

| Actividad | Comando GitLab | Frecuencia | Propósito |
|-----------|---------------|------------|-----------|
| **Checkpoint Diario** | Comentario en Issue | Antes 10:00 AM | Actualizar progreso y bloqueos |
| **Estimación** | `/estimate 1d` | Al iniciar tarea | Planificación de esfuerzo |
| **Registro de Tiempo** | `/spend 4h` | Al finalizar jornada | Alimentar gráficos Burndown |

**Ejemplo de Checkpoint:** "Progreso: 50%. Bloqueo: Esperando aprobación de TI..."

### 5.4 Control de Cronograma: Gantt y Burndown

**Importancia:** Cumplimiento de tiempos es crítico para credibilidad del área de BI.

**Herramientas Mandatorias:** Dos herramientas de control alojadas en plantillas oficiales de Comsatel (Código Formato: **CP-FOR-008**).

#### 5.4.1 Carta Gantt de Proyecto (Visión Macro)

**Características:**

| Aspecto | Descripción |
|---------|-------------|
| **Estructura por Hitos** | Todo proyecto debe estructurarse en los 7 Hitos definidos en este documento (sección 3) |
| **Asignación de Responsables** | Cada actividad declara explícitamente si recae en Proveedor (tercero) o Equipo Interno |
| **Trazabilidad Temporal** | Fecha Inicio/Fin parametrizada, marcando días de ejecución en calendario |
| **Ruta Crítica** | Retrasos en "Construcción de Tubería" (Hito 3) impactan cascada abajo en "Desarrollo de Dashboards" (Hito 4). Revisar en ceremonias semanales para reasignar recursos |

#### 5.4.2 Tabla de Burndown (Control Micro y Desviaciones)

**Responsable:** Project Manager  
**Frecuencia de Actualización:** Diaria (final de jornada)

**Métricas Clave:**

| Métrica | Descripción | Interpretación |
|---------|-------------|----------------|
| **Balance Estimado vs Real** | Compara tareas/puntos planificados Día N vs ejecutados (valores extraídos de GitLab con `/spend`) | Indica ritmo de consumo de esfuerzo |
| **Variación (%)** | Diferencia porcentual entre avance teórico y real | Positiva: entrega acelerada. Negativa: retraso |

**Límites de Tolerancia:** Banda paramétrica entre Mínimo (-10%) y Máximo (+10%)

**Estados del Proyecto:**

| Estado | Rango de Variación | Acción Requerida |
|--------|-------------------|------------------|
| **Normal** | -10% a +10% | Proyecto saludable. Continuar monitoreo |
| **Retraso** | < -10% (ej: -11%, -15%, -25%) | Celda cambia a "Retraso" (semáforo naranja/rojo). PM tiene 24 horas para documentar plan de mitigación |

**Protocolo de Acción ante Retrasos:**

El Project Manager tiene **24 horas** para documentar plan de mitigación en GitLab:
- Reducir alcance no crítico
- Aprobar horas extras
- Escalar impedimento técnico a gerencia de TI

---

## 6. Capacidades y Roles

Identificar habilidades necesarias en el equipo de analítica y definir roles/responsabilidades:

**Ver documentación complementaria:**
- [Responsabilidades y Competencias](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Roles-(Responsabilidades-&-Competencias))
- [Dirección de Gestión](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Direcci%C3%B3n-de-Gesti%C3%B3n)
- [Proceso de Desarrollo (SCRUM)](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Proceso-de-Desarrollo-(SCRUM))

---

## 7. Referencias y Fuentes

### 7.1 Referencias Arquitectónicas Internas

- [Escenarios y Casuísticas para Arquitectura de Datos](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Escenarios-y-Casu%C3%ADsticas-para-Arquitectura-de-Datos)

### 7.2 Fuentes Externas de Referencia

1. [Azure Databricks - Data Science & Machine Learning](https://learn.microsoft.com/es-es/azure/architecture/solution-ideas/articles/azure-databricks-data-science-machine-learning)
2. [AWS Modern Data Architecture](https://aws.amazon.com/es/big-data/datalakes-and-analytics/modern-data-architecture/)
3. [Azure Databricks Modern Analytics Architecture](https://learn.microsoft.com/es-es/azure/architecture/solution-ideas/articles/azure-databricks-modern-analytics-architecture)
4. [Azure Data Guide](https://learn.microsoft.com/es-es/azure/architecture/data-guide/)
5. [IBM Data Architecture](https://www.ibm.com/es-es/topics/data-architecture)
6. [Google Cloud Data Science](https://cloud.google.com/data-science?hl=es)
7. [Stitch Data Pipeline Architecture](https://www.stitchdata.com/resources/data-pipeline-architecture/)
8. [Oracle AI Overview](https://www.oracle.com/pe/artificial-intelligence/what-is-ai/)
9. [Qué es una Arquitectura de Referencia en TI](https://edgarjayo.wordpress.com/2021/09/15/que-es-una-arquitectura-de-referencia-en-ti-2/)
10. [Reference Architecture Definition](https://conexiam.com/es/what-is-a-reference-architecture/)
11. [Google Cloud - Data Pipeline Architecture](https://cloud.google.com/blog/topics/developers-practitioners/what-data-pipeline-architecture-should-i-use)
12. [IBM Data Storage](https://www.ibm.com/topics/data-storage)
13. [Pipeline de Datos - Aprender Big Data](https://aprenderbigdata.com/pipeline-de-datos/)
14. [Google Cloud - Data Governance](https://cloud.google.com/learn/what-is-data-governance?hl=es)

---

## 8. Historial de Versiones

| Versión | Fecha | Descripción de Cambios | Autor |
|---------|-------|------------------------|-------|
| 1.0 | [Fecha Original] | Documento base de arquitectura | Equipo Arquitectura |
| 1.1 | 2026-04-08 | Consolidación con Manual de Procedimientos SOP v1.2. Integración de hitos, templates GitLab, protocolos QA, seguridad RLS y controles de proyecto | Lingma AI Assistant |
| 1.2 | Pendiente | Revisiones futuras según evolución de plataforma | Por definir |

---

**Clasificación**: Confidencial / Uso Interno Comsatel  
**Área**: Inteligencia de Negocios / Arquitectura de Datos  
**Rol Objetivo**: Analista de Datos / Ingeniero de Datos / Project Manager / Arquitecto de Soluciones