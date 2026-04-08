[[_TOC_]]

# Arquitectura para Analítica de Datos (BI) - Comsatel

## 1. Introducción y Arquitectura de Referencia

### 1.1 Propósito y Alcance

Este documento establece la arquitectura de referencia para soluciones de Inteligencia de Negocios (BI) y Analítica de Datos en Comsatel. Define los patrones, estándares y directrices que proporcionan un marco estructurado para el diseño, implementación y operación de sistemas analíticos.

La arquitectura integra plataformas de datos transaccionales y analíticas de forma fluida, asegurando una única fuente de confianza que almacena datos estructurados (BD SQL), semi-estructurados (JSON) y no estructurados (audio, video, IoT), generando soluciones de análisis, visualización e inteligencia artificial.

### 1.2 Definición de Arquitectura de Referencia

Una Arquitectura de Referencia es un conjunto de patrones, estándares y directrices que proporciona estructura y marco de trabajo para el diseño e implementación de sistemas en dominios específicos (TI, analítica, seguridad, cloud, IA). Proporciona:

- **Buenas prácticas**: Recopiladas de lecciones aprendidas en proyectos reales
- **Estandarización**: Homogenización de enfoques técnicos
- **Evolución continua**: Capacidad de mejora post-implementación
- **Colaboración eficaz**: Lenguaje común entre equipos técnicos y de negocio

Ejemplos reconocidos: TOGAF, AWS Reference Architecture, Java EE, SAP.

### 1.3 Diagrama de Contexto General

![Contexto Arquitectónico General](uploads/44a92c6737f03411a07149c8eca18a74/image.png)

---

## 2. Vistas Arquitectónicas

### 2.1 Arquitectura Lógica - Modelo Medallion

La arquitectura lógica implementa el patrón **Medallion** (Bronce-Plata-Oro) para garantizar trazabilidad, calidad y performance:

![Arquitectura Lógica Medallion](images/arquitectura-logica-bi-medallion.png)

**Capas del Modelo:**

1. **Capa Bronce (Raw)**: Volcado idéntico y crudo de datos origen. Prohibido aplicar transformaciones. Funciona como ledger inmutable ante desastres.
2. **Capa Plata (Clean)**: Resolución de deuda técnica. Estandarización ISO 8601 de fechas, deduplicación, casteo tipográfico forzado.
3. **Capa Oro (Business/Mart)**: Única Fuente de Verdad. Vistas materializadas hiper-optimizadas con pre-agregaciones dimensionales. Superset se conecta exclusivamente a esta capa.

### 2.2 Arquitectura Física - Componentes de Software

![Arquitectura de Componentes Físicos](images/componentes-software-arquitectura-analitica.png)

**Componentes Principales:**

| Componente | Tecnología | Función |
|------------|-----------|----------|
| **Data Sources** | ERP, CRM, APIs, IoT | Sistemas transaccionales origen |
| **Data Storage** | Data Lake, Data Warehouse | Almacenamiento intermedio y final |
| **Data Processing** | Batch/Streaming Engines | Motores de transformación |
| **Semantic Layer** | **Apache Superset** | **Plataforma central de modelado semántico y visualización** |
| **ML/AI Engine** | H2O | Machine learning e inteligencia artificial |

**Nota Estratégica:** Apache Superset es la **herramienta principal** de Comsatel para Business Intelligence, actuando como capa semántica unificada entre los datos procesados y los usuarios de negocio.

---

## 3. Apache Superset - Plataforma Central de BI

### 3.1 Visión Estratégica de Superset en Comsatel

**Apache Superset** es la **plataforma oficial y estratégica** de Inteligencia de Negocios en Comsatel. Su adopción responde a la necesidad de una solución enterprise-grade, open-source, escalable y con capacidades avanzadas de visualización.

**Ventajas Competitivas:**

| Característica | Beneficio para Comsatel |
|---------------|------------------------|
| **Open-Source** | Sin costos de licenciamiento, comunidad activa, transparencia total |
| **Multi-Database** | Conecta con MySQL, PostgreSQL, Oracle, SQL Server, BigQuery, etc. |
| **Semantic Layer** | Capa de abstracción que traduce datos técnicos a lenguaje de negocio |
| **Security Enterprise** | RLS (Row Level Security), integración LDAP/SSO, roles granulares |
| **Performance** | Caché inteligente, consultas optimizadas, soporte para grandes volúmenes |
| **Self-Service BI** | Usuarios de negocio pueden explorar datos sin dependencia de TI |
| **Extensibilidad** | Plugins personalizados, APIs REST, integración con ecosistema Python |

**Posicionamiento Arquitectónico:**

Superset se ubica como la **capa semántica única** entre la Capa Oro (datos preparados) y los usuarios finales, garantizando:
- **Una sola fuente de verdad** para métricas de negocio
- **Gobernanza centralizada** de accesos y permisos
- **Estandarización** de visualizaciones y dashboards
- **Escalabilidad** para cientos de usuarios concurrentes

![Superset Analytics Solution](uploads/b9c947af129176f4acfb9c8dde575c6e/image.png)

### 3.2 Capacidades Core de Apache Superset

#### 3.2.1 SQL Lab - Entorno de Desarrollo SQL

**Propósito:** IDE SQL integrado para exploración, prototipado y creación de datasets virtuales.

**Características Clave:**
- Editor SQL con autocompletado y syntax highlighting
- Ejecución de queries ad-hoc contra cualquier base conectada
- Visualización rápida de resultados en tablas
- Guardado de queries como **Virtual Datasets** (uso restringido, ver sección 4.2.1)
- Exportación de resultados a CSV/Excel

**Mejores Prácticas:**
- Usar SQL Lab **exclusivamente** para prototipado rápido
- No exponer Virtual Datasets directamente a usuarios de negocio
- Migrar queries validados a Physical Datasets en Capa Oro

#### 3.2.2 Datasets - Modelado Semántico

**Definición:** Los Datasets son la representación semántica de tablas/vistas de la base de datos, con metadatos enriquecidos para usuarios de negocio.

**Tipos de Datasets:**

| Tipo | Descripción | Uso Recomendado |
|------|-------------|------------------|
| **Physical Dataset** | Apunta directamente a tabla/vista materializada en BD | **Prioritario**. Máximo performance, caché eficiente |
| **Virtual Dataset** | Query SQL dinámica ejecutada en tiempo real | Prototipado, filtros Jinja paramétricos |

**Elementos Configurables en Datasets:**

1. **Columnas (Columns):**
   - Label descriptivo (traducción de nombres técnicos)
   - Tipo de dato (string, number, datetime, boolean)
   - Visibilidad (mostrar/ocultar al usuario)
   - Agrupable (GROUP BY habilitado)
   - Filtrable (WHERE clause habilitado)

2. **Métricas (Metrics):**
   - Fórmulas predefinidas (SUM, AVG, COUNT, DISTINCT COUNT)
   - Expresiones SQL personalizadas
   - Formateo (moneda, porcentaje, decimales)
   - Descripción documentada para usuarios

3. **Filtros Avanzados:**
   - Temporal Time Range (rangos de fecha dinámicos)
   - Jinja Templating (filtros paramétricos con variables de sesión)
   - Custom SQL WHERE clauses

**Ejemplo de Configuración de Métrica:**

```sql
-- Nombre: Ventas Netas Totales
-- Expresión: SUM(monto_neto)
-- Formateo: Currency ($)
-- Descripción: Suma total de ventas netas después de devoluciones e impuestos
```

#### 3.2.3 Charts - Visualizaciones Interactivas

**Catálogo de Visualizaciones Soportadas:**

| Categoría | Tipos Disponibles | Caso de Uso |
|-----------|------------------|-------------|
| **KPIs** | Big Number, Big Number with Trendline | Métricas consolidadas |
| **Barras** | Bar Chart, Horizontal Bar, Stacked Bar | Comparativas categóricas |
| **Líneas** | Line Chart, Area Chart, Step Line | Tendencias temporales |
| **Tortas** | Pie Chart, Donut Chart, Rose Chart | Distribución porcentual |
| **Tablas** | Table, Pivot Table, Heatmap | Detalles granulares |
| **Geográficas** | Mapbox, Deck.gl, Country Map | Análisis por ubicación |
| **Avanzadas** | Treemap, Sunburst, Sankey, Word Cloud | Composición jerárquica |

**Configuración Estándar de Charts en Comsatel:**

- **Título descriptivo** en lenguaje de negocio
- **Tooltips informativos** con contexto adicional
- **Leyendas claras** sin invadir el gráfico
- **Colores corporativos** (verde/azul positivo, rojo/naranja alerta)
- **Interactividad habilitada** (cross-filtering obligatorio)

#### 3.2.4 Dashboards - Consolidación de Insights

**Definición:** Los Dashboards agrupan múltiples charts en una vista unificada con filtros globales y cross-filtering.

**Estándar de Diseño - Patrón en Z:**

![Dashboard Example](uploads/50b751fe09ba3f5bb6fa0fb220cd0313/image.png)

| Zona | Contenido | Objetivo |
|------|-----------|----------|
| **Superior (20%)** | Big Numbers + KPIs críticos | Lectura inmediata del estado |
| **Central (60%)** | Gráficos de análisis (barras, líneas, treemaps) | Exploración de tendencias |
| **Inferior (20%)** | Tablas detalladas | Drill-down y exportación |

**Funcionalidades Avanzadas de Dashboards:**

1. **Cross-Filtering (Filtros Cruzados):**
   - Clic en un elemento de un chart filtra TODOS los demás charts
   - Propagación instantánea en cascada
   - **Obligatorio** en todos los dashboards productivos

2. **Filtros Globales (Native Filters):**
   - Selectores de fecha, región, categoría aplicables a todo el dashboard
   - Persistencia de filtros en URL (compartible)
   - Valores por defecto configurables

3. **Tabs y Layout Dinámico:**
   - Organización en pestañas temáticas
   - Drag-and-drop para reorganizar charts
   - Responsive design (desktop, tablet, mobile)

4. **Exportación y Compartir:**
   - Download charts como PNG/SVG
   - Export data a CSV/Excel
   - Share dashboard URL con permisos específicos
   - Embed dashboards en aplicaciones externas (iframe)

5. **Alertas y Reportes Programados:**
   - Alertas por email cuando métricas superan umbrales
   - Reportes PDF programados (diario, semanal, mensual)
   - Suscripción de usuarios a dashboards específicos

---

## 4. Ciclo de Vida del Proyecto BI 

Todo proyecto analítico en Comsatel debe estructurarse obligatoriamente en los siguientes hitos, garantizando control temporal, asignación de responsables e identificación de ruta crítica.

### 3.1 Definición de Arquitectura Base y Lanzamiento

#### 3.1.1 Arquitectura de Documentación BI para Levantamiento de Proyectos

**Descripción:** Establecimiento de la documentación base que guiará todo el ciclo de vida del proyecto, incluyendo templates, checklists y estándares de nomenclatura.

**Entregables:**
- Templates estandarizados de Issues en GitLab
- Checklists de validación por fase
- Estándares de nomenclatura para artefactos

#### 3.1.2 Daily Meeting: Sincronización y Desbloqueo Técnico (Recurrente)

**Descripción:** Ceremonia diaria de máximo 15 minutos para sincronización del equipo.

**Objetivos:**
- Sincronizar avances del equipo
- Identificar bloqueos técnicos tempranos
- Reasignar recursos si hay desviaciones en la ruta crítica

**Duración:** 15 minutos máximo  
**Frecuencia:** Diaria (días hábiles)  
**Participantes:** Equipo técnico completo

#### 3.1.3 Implementación de Templates Globales de Issues en GitLab

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

### 3.2 Metodología de Trabajo e Ingeniería de Datos

#### 3.2.1 Protocolo de Perfilamiento y Limpieza de Datos (Data Profiling)

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

### 3.3 Construcción de la Tubería y Plataforma

#### 3.3.1 Optimización de Infraestructura, Performance y Escalabilidad

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

**Importancia Estratégica:** La correcta configuración de Datasets en Superset es **crítica** para el éxito del proyecto. Un Dataset mal configurado genera:
- Consultas lentas que degradan la experiencia de usuario
- Métricas inconsistentes que generan desconfianza en el negocio
- Dificultad de mantenimiento y evolución del dashboard

**Physical Datasets vs Virtual Datasets:**

| Tipo | Uso Recomendado | Ventajas |
|------|----------------|----------|
| **Physical Datasets** | Maximizar uso. Apuntar a tablas materializadas de Capa Oro | Delega carga computacional a la BD, permite caché eficiente |
| **Virtual Datasets (SQL Lab)** | Restringir. Solo para filtros paramétricos dinámicos (Jinja templating) o prototipado rápido | Flexibilidad temporal antes de consolidar en Capa Oro |

**Traducción Semántica (MANDATORIO):**

**Regla de Oro:** Los usuarios de negocio NUNCA deben ver nombres técnicos de columnas. Todo campo debe tener una etiqueta descriptiva en lenguaje de negocio.

**Proceso de Traducción Semántica en Superset:**

1. Ir a **Datasets → [Nombre Dataset] → Edit**
2. Pestaña **Columns**
3. Para cada columna técnica, completar:
   - **Verbose Name**: Label amigable (ej: "Monto Total de Ventas")
   - **Description**: Explicación del significado (tooltip)
   - **Filterable**: Yes/No (¿puede usarse en filtros?)
   - **Groupable**: Yes/No (¿puede agruparse en charts?)
   - **Visible**: Yes/No (¿mostrar al usuario final?)

**Ejemplos de Traducción:

| Columna Técnica | Label Requerido | Descripción (Tooltip) |
|----------------|----------------|----------------------|
| `mnt_tot_vta_01` | Monto Total de Ventas | Suma de ventas brutas antes de descuentos |
| `flg_act` | Estado Activo | Indicador binario: 1=Activo, 0=Inactivo |
| `fec_reg_ts` | Fecha de Registro | Timestamp de creación del registro (UTC) |
| `cod_cli_fk` | Código de Cliente | ID único del cliente en sistema CRM |

##### 4.4.1.2 Definición de Métricas y Dimensiones en Superset

**Importancia:** Las métricas definidas en Superset son la **única fuente de verdad** para el negocio. Deben ser validadas rigurosamente antes de publicación.

**Procedimiento para Crear Métricas:**

1. Ir a **Datasets → [Nombre Dataset] → Edit → Metrics**
2. Click en **+ Metric**
3. Configurar:
   - **Metric Name**: Nombre técnico (sin espacios, ej: `ventas_netas_total`)
   - **Verbose Name**: Label amigable (ej: "Ventas Netas Totales")
   - **Expression**: Fórmula SQL (ej: `SUM(monto_neto)`)
   - **Metric Type**: Simple (agregación) o Python (lógica compleja)
   - **D3 Format**: Formateo visual (ej: `$,.2f` para moneda)
   - **Description**: Documentación de la fórmula y reglas de negocio
   - **Extra**: JSON con configuraciones avanzadas (opcional)

**Requisitos Obligatorios:**
**Ejemplos de Métricas Estándar:**

| Métrica | Expresión SQL | D3 Format | Descripción |
|---------|--------------|-----------|-------------|
| Ventas Brutas | `SUM(monto_bruto)` | `$,.2f` | Total ventas antes de descuentos |
| Ventas Netas | `SUM(monto_neto)` | `$,.2f` | Ventas después de descuentos e impuestos |
| Ticket Promedio | `AVG(monto_neto)` | `$,.2f` | Valor promedio por transacción |
| Tasa Conversión | `COUNT(DISTINCT id_cliente) / COUNT(*) * 100` | `.2%` | Porcentaje de visitantes que compran |
| Clientes Activos | `COUNT(DISTINCT CASE WHEN flg_act = 1 THEN id_cliente END)` | `,` | Clientes con estado activo |

##### 4.4.1.3 UI/UX y Diseño de Dashboards en Superset

**Guía de Diseño Visual en Superset:**

**Jerarquía Visual (Patrón en Z):**

| Zona | Contenido | Propósito |
|------|-----------|-----------|
| **Franja Superior** | Big Numbers y KPIs totales consolidados | Lectura rápida del estado general |
| **Cuerpo Central** | Gráficos de tendencias y composición (barras, líneas, treemaps) | Análisis detallado |
| **Franja Inferior** | Tablas de detalles granulares | Exportación y escrutinio contable |

**Interactividad Cero-Fricción en Superset:**

**Cross-Filtering (MANDATORIO):**
- **Configuración:** En cada chart, activar opción "Emitir filtro cruzado"
- **Comportamiento:** Clic en barra/segmento/punto filtra automáticamente todos los charts del dashboard
- **Beneficio:** Exploración intuitiva sin necesidad de filtros manuales
- **Validación QA:** Probar que todos los charts responden a interacciones

**Estándar Cromático Corporativo:**
- Utilizar paleta oficial de Comsatel
- **Contraste lógico:** Verde/Azul para métricas positivas; Rojo/Naranja para mermas/alertas

**Visualización y Presentación de Resultados:**

![Dashboard Example](uploads/50b751fe09ba3f5bb6fa0fb220cd0313/image.png)

**Configuración de Colores en Superset:**

1. Ir a **Settings → Feature Flags**
2. Habilitar `ENABLE_TEMPLATE_PROCESSING`
3. Definir paleta corporativa en CSS personalizado:
   ```css
   :root {
     --color-positivo: #28a745;  /* Verde */
     --color-negativo: #dc3545;  /* Rojo */
     --color-alerta: #fd7e14;    /* Naranja */
     --color-info: #007bff;      /* Azul */
   }
   ```

##### 4.4.1.4 Optimización de Performance en Superset

**Estrategias de Optimización:**

| Estrategia | Implementación | Impacto |
|-----------|---------------|----------|
| **Physical Datasets** | Usar tablas materializadas de Capa Oro | Reduce carga en BD transaccional |
| **Caché de Queries** | Configurar Redis/Memcached como backend de caché | Respuestas <2s para queries repetidas |
| **Pre-agregaciones** | Crear vistas resumidas en Capa Oro (daily, monthly) | Elimina cálculos en tiempo real |
| **Índices en BD** | Asegurar índices en columnas de filtro/agrupación | Acelera execution plan |
| **Limitar Datos** | Aplicar row limit en charts (ej: top 50 categorías) | Evita renderizado excesivo |
| **Evitar Virtual Datasets** | Migrar a Physical Datasets siempre que sea posible | Performance consistente |

**SLA de Performance:**
- **Dashboard completo (cold cache):** <5 segundos
- **Chart individual (con caché):** <2 segundos
- **Filtros cruzados:** <1 segundo
- **Exportación CSV (<10k rows):** <3 segundos

**Monitoreo de Performance:**
- Revisar logs de Superset en `/var/log/superset/`
- Identificar queries lentas (>3s) en **SQL Lab → Query Search**
- Optimizar gradualmente basándose en métricas reales de uso

---

### 4.5 Gobernanza, Seguridad y QA en Superset

#### 4.5.1 Protocolo de QA y Matriz de Cuadratura de Datos en Superset

**Descripción:** El despliegue a Producción sin evidencia documentada de pruebas en GitLab es falta grave a la metodología operativa.

**Herramientas de QA en Superset:**

**1. Validación de Métricas (Cross-Validation):**
**Procedimiento en Superset:**
   - Abrir chart en modo **Edit**
   - Ver expresión SQL generada en pestaña **Query**
   - Copiar query y ejecutar en SQL Lab contra BD origen
   - Comparar resultado numérico exacto
   - **Tolerancia:** 0% de discrepancia
   - **Acción correctiva:** Si difiere, revisar JOINs y filtros en Dataset

**2. Pruebas de Estrés y Rendimiento (Performance en Superset):**
**Metodología de Testing:**
   - Limpiar caché: **Settings → Clear Cache**
   - Abrir dashboard en ventana incógnito (sin sesión previa)
   - Cronometrar tiempo hasta renderizado completo
   - **SLA:** Dashboard completo en frío <5 segundos
   - **Acción correctiva:** Si excede 5s, optimizar Dataset (ver sección 4.4.1.4)

**3. Pruebas de Interfaz (UI/UX en Superset):**
**Checklist de Validación Visual:**
   - Tooltips aparecen al hover con información contextual
   - Ejes legibles (rotación automática si hay muchas categorías)
   - Leyendas no invaden área de gráfico (máximo 15% del espacio)
   - Responsive: dashboard usable en resolución 1366x768 (estándar corporativo)
   - Cross-filtering funcional en todos los charts interactivos
   - Paleta de colores cumple estándar corporativo

#### 4.5.2 Implementación de Seguridad RLS y Roles en Superset

**Marco de Seguridad de Superset en Comsatel:**

Apache Superset proporciona un framework de seguridad enterprise-grade que debe configurarse rigurosamente para cumplir con políticas de gobierno de datos de Comsatel.

##### 4.5.2.1 Gestión de Roles Personalizados en Superset

**Roles Nativos de Superset (NO USAR directamente):**

| Rol Nativo | Permisos | Estado en Comsatel |
|-----------|----------|--------------------|
| **Admin** | Acceso total, gestión de usuarios | ❌ Solo arquitectos de plataforma |
| **Alpha** | Crear/editar dashboards, SQL Lab unrestricted | ❌ **PROHIBIDO** para usuarios de negocio |
| **Gamma** | Solo visualizar dashboards publicados | ⚠️ Base para rol Visualizador personalizado |
| **sql_lab** | Acceso a SQL Lab | ⚠️ Asignar solo a rol Creador |

**Roles Personalizados de Comsatel (CREAR OBLIGATORIAMENTE):**

**Procedimiento para Crear Rol Personalizado:**

1. Ir a **Settings → List Roles → + Add Role**
2. Nombre: `Creador_Operaciones` o `Visualizador_Ventas`
3. Seleccionar permisos granulares:
   - **Para Creador:** `can_edit on Dashboard`, `can_write on Dataset`, `can_sql_json on SqlLab`
   - **Para Visualizador:** `can_view on Dashboard`, `can_read on Dataset`
4. Asignar usuarios desde **Settings → List Users**
5. Validar permisos con cuenta de test

##### 4.5.2.2 Implementación de Row Level Security (RLS) en Superset

**Qué es RLS:** Row Level Security filtra datos a nivel de fila basado en el usuario autenticado, garantizando que cada usuario vea SOLO los datos autorizados para su perfil.

**Casos de Uso Típicos en Comsatel:**
- Gerentes regionales ven solo datos de su región
- Vendedores ven solo sus propias transacciones
- Proveedores externos ven solo datos de sus contratos

**Procedimiento Detallado para Configurar RLS:**

**Paso 1: Crear Tabla de Mapeo de Accesos en BD**

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

1. Ir a **Security → Row Level Security Filters → + Add Filter**
2. Completar campos:
   - **Name**: `rls_region_por_usuario`
   - **Description**: "Filtra datos por región basada en email de usuario"
   - **Tables**: Seleccionar Dataset(s) afectados
   - **Clause** (SQL):
   ```sql
   region_id = (
       SELECT id_region 
       FROM dim_accesos 
       WHERE email_usuario = '{{ current_username() }}'
   )
   ```
   - **Roles**: Asignar a roles que requieren este filtro (ej: `Visualizador_Ventas`)
   - **Enabled**: ✅ Yes

3. Click en **Save**

**Paso 3: Validar RLS**

1. Login con usuario de test (ej: juan.perez@comsatel.com)
2. Abrir dashboard afectado
3. Verificar que solo muestra datos de región_id = 1
4. Repetir con usuario de otra región para confirmar aislamiento

##### 4.5.2.3 Integración LDAP/SSO y Auditoría en Superset

**Autenticación Enterprise:**

Superset se integra nativamente con el directorio corporativo de Comsatel mediante LDAP/SSO, eliminando la necesidad de credenciales separadas.

**Configuración LDAP (superset_config.py):**

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

**Flujo de Autenticación:**
1. Usuario accede a `https://bi.comsatel.com`
2. Redirección a login corporativo (SSO)
3. Validación contra Active Directory
4. Superset crea/actualiza usuario automáticamente
5. Asignación de roles basada en grupos AD (mapeo configurable)

**Auditoría y Compliance:**

**Logs de Superset:**

Ubicación: `/var/log/superset/superset.log`

**Eventos Auditados:**
- Login/logout de usuarios
- Creación/edición/eliminación de dashboards
- Ejecución de queries en SQL Lab
- Cambios en configuración de Datasets
- Modificaciones en roles y permisos
- Exportaciones de datos (CSV/Excel)

**Proceso de Auditoría Trimestral:**

1. Extraer logs de acceso últimos 90 días:
   ```bash
   grep "INFO:werkzeug:" /var/log/superset/superset.log | \
   awk '{print $1, $2, $7}' | sort | uniq -c | sort -rn
   ```

2. Identificar usuarios inactivos (>90 días sin login)

3. Revocar permisos automáticamente:
   ```sql
   -- Query para encontrar usuarios inactivos
   SELECT username, email, last_login 
   FROM ab_user 
   WHERE last_login < NOW() - INTERVAL '90 days';
   ```

4. Generar reporte de auditoría para Compliance

5. Documentar acciones correctivas en GitLab Issue

**Seguridad y Privacidad de Datos en Superset:**

![Data Governance](uploads/7f9de016e6ebf50eea40cae80fa6e6e5/image.png)

**Medidas de Protección Implementadas:**

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

#### 4.6.1 Consolidación de Documentación Técnica de Superset

**Descripción:** Generación de documentación entregable completa para cierre de proyecto.

**Documentación Específica de Superset Entregable:**

| Documento | Formato | Contenido Específico de Superset |
|-----------|---------|----------------------------------|
| **Manual de Arquitectura** | Markdown | Arquitectura Superset + procedimientos |
| **Diccionario de Datasets** | Excel | Metadatos de todos los Datasets, métricas, filtros RLS |
| **Guía de Usuario Superset** | PDF 1-pager | Cómo navegar, filtrar, exportar en Superset |
| **Catálogo de Dashboards** | Wiki GitLab | Lista de dashboards productivos con owners y SLA |
| **Registro de Configuraciones** | YAML/JSON | Backup de configs de roles, RLS, feature flags |

**Gobierno de Datasets en Superset:**

Establecer políticas para garantizar calidad y consistencia de Datasets durante todo su ciclo de vida.

**Requisitos de Datasets:**
- **Semántica clara**: Labels descriptivos, descripciones documentadas
- **Métricas validadas**: Fórmulas aprobadas por business owner
- **Seguridad aplicada**: RLS configurado según política de accesos
- **Performance óptima**: Queries <3s en p95
- **Versionado**: Changes trackeados en GitLab Issues

---

### 4.7 Entrega Final, Demo y Cierre de Proyecto Superset

#### 4.7.1 Despliegue Oficial, Demo "Hands-on" en Superset y Cierre

##### 4.7.1.1 Protocolo de Demostración "Hands-on" en Superset

**Metodología "Hands-on" Específica para Superset:**

**Regla Crítica:** El Analista JAMÁS opera Superset durante la demo. El cliente debe interactuar directamente con la plataforma.

**Script de Demo Guiada:**

1. **Transferir control:** Compartir pantalla y pasar mouse al cliente
2. **Escenario real:** "Filtra el mes de marzo y dime cuál sucursal tuvo más incidencias"
3. **Observar comportamiento:**
   - ¿El cliente encuentra fácilmente los filtros?
   - ¿Los charts responden correctamente al cross-filtering?
   - ¿Los tooltips son comprensibles?
4. **Documentar feedback:** Capturar ajustes visuales solicitados
5. **Crear micro-sprint:** Issues en GitLab para cambios menores (<2 días)

**Beneficio:** Destruye barrera del miedo tecnológico y acelera adopción

##### 4.7.1.2 Pase a Producción en Superset

**Checklist Pre-Producción en Superset:**

- [ ] Dashboard status cambiado de **Draft** → **Published**
- [ ] RLS configurado y validado con usuarios de test
- [ ] Roles personalizados asignados correctamente
- [ ] Caché configurado (Redis/Memcached)
- [ ] Feature flags revisados (ENABLE_TEMPLATE_PROCESSING, etc.)
- [ ] URLs oficiales compartidas vía email corporativo
- [ ] Bookmarks creados para accesos rápidos
- [ ] Alertas/reportes programados configurados (si aplica)

##### 4.7.1.3 Capacitación y Cierre en Superset

**Entregables de Capacitación:**

1. **Guía Rápida "1-Pager" de Superset:**
   - URL de acceso: `https://bi.comsatel.com`
   - Cómo aplicar filtros nativos (sidebar izquierdo)
   - Cómo usar cross-filtering (clic en charts)
   - Cómo exportar datos (botón download en charts)
   - Cómo guardar bookmarks personales
   - Contacto soporte: bi-soporte@comsatel.com

2. **Video Tutorial (5 min):** Grabación de navegación básica

3. **GitLab Closure:**
   - Épicas marcadas como **Done**
   - Time tracking completo (`/spend` commands validados)
   - Carta Gantt cerrada
   - Lecciones aprendidas documentadas

#### 4.7.2 Ciclo de Feedback Continuo y Auditoría de Adopción de Superset

##### 4.7.2.1 Monitoreo de Adopción y Feedback en Superset

**Programación:** Reunión de calibración a los **30 días post-lanzamiento**

**Métricas de Adopción en Superset:**

Superset proporciona analytics nativos para medir uso real:

1. **Ir a:** **Settings → Activity Log**
2. **Filtrar por:** Dashboard específico, rango de fechas
3. **Métricas clave:**
   - Número de usuarios únicos (last 30 days)
   - Frecuencia de acceso (daily/weekly active users)
   - Charts más consultados
   - Exportaciones realizadas (CSV/Excel)
   - Queries lentas identificadas

**Objetivos de la Reunión de Calibración:**
- Presentar métricas de adopción reales
- Identificar dashboards subutilizados (<5 usuarios/mes)
- Levantar requerimientos evolutivos
- Validar satisfacción del usuario (NPS survey)

**Regla de Nuevo Requerimiento:**

Cualquier solicitud de:
- Nueva dimensión analítica no contemplada inicialmente
- Métrica adicional con lógica compleja
- Integración con nueva fuente de datos
- Dashboard completamente nuevo

...constituye **nuevo proyecto BI**, iniciando ciclo ágil desde Fase 1 (sección 4.1).

**Cambios menores** (ajustes visuales, corrección de labels) se gestionan como bugs en GitLab con prioridad baja.

---

## 4. Gestión Ágil y Control de Proyecto

### 4.1 Estructura de Épicas e Historias de Usuario en GitLab

**Definiciones:**

| Elemento | Descripción | Ejemplo |
|----------|-------------|----------|
| **Épicas** | Entregables funcionales de alto valor. Agrupan múltiples tareas técnicas | `[BI-OPS] Dashboard de Rendimiento de Flota V1.0` |
| **Issues** | Tareas atómicas, técnicas y ejecutables conectadas a la Épica | `[Transform] Limpieza de fechas nulas` |

### 4.2 Gestión del Tablero Kanban y WIP

**Flujo de Estados (Estricto y Unidireccional):**

| Estado | Descripción | Reglas |
|--------|-------------|--------|
| **Backlog** | Requerimientos sin Definition of Ready (DoR) | No pueden moverse a To Do hasta cumplir DoR |
| **To Do** | Issues priorizados, listos para Sprint | Asignación pendiente |
| **In Progress** | Tarea en ejecución activa | **WIP Limit:** Máximo 2 issues simultáneos por analista |
| **Review (QA)** | Desarrollo finalizado, esperando Code Review o pruebas de integridad | Requiere aprobación de par |
| **Done** | Código fusionado, desplegado en producción y aprobado por usuario | Cierre definitivo |

**Regla WIP (Work in Progress):** Máximo 2 issues simultáneos por analista para evitar parálisis por análisis y context-switching.

### 4.3 Registro de Esfuerzo (Time Tracking)

**Requisitos de Seguimiento:**

| Actividad | Comando GitLab | Frecuencia | Propósito |
|-----------|---------------|------------|-----------|
| **Checkpoint Diario** | Comentario en Issue | Antes 10:00 AM | Actualizar progreso y bloqueos |
| **Estimación** | `/estimate 1d` | Al iniciar tarea | Planificación de esfuerzo |
| **Registro de Tiempo** | `/spend 4h` | Al finalizar jornada | Alimentar gráficos Burndown |

**Ejemplo de Checkpoint:** "Progreso: 50%. Bloqueo: Esperando aprobación de TI..."

### 4.4 Control de Cronograma: Gantt y Burndown

**Importancia:** Cumplimiento de tiempos es crítico para credibilidad del área de BI.

**Herramientas Mandatorias:** Dos herramientas de control alojadas en plantillas oficiales de Comsatel (Código Formato: **CP-FOR-008**).

#### 4.4.1 Carta Gantt de Proyecto (Visión Macro)

**Características:**

| Aspecto | Descripción |
|---------|-------------|
| **Estructura por Hitos** | Todo proyecto debe estructurarse en los 7 Hitos definidos en este documento (sección 3) |
| **Asignación de Responsables** | Cada actividad declara explícitamente si recae en Proveedor (tercero) o Equipo Interno |
| **Trazabilidad Temporal** | Fecha Inicio/Fin parametrizada, marcando días de ejecución en calendario |
| **Ruta Crítica** | Retrasos en "Construcción de Tubería" (Hito 3) impactan cascada abajo en "Desarrollo de Dashboards" (Hito 4). Revisar en ceremonias semanales para reasignar recursos |

#### 4.4.2 Tabla de Burndown (Control Micro y Desviaciones)

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

## 5. Capacidades y Roles

Identificar habilidades necesarias en el equipo de analítica y definir roles/responsabilidades:

**Ver documentación complementaria:**
- [Responsabilidades y Competencias](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Roles-(Responsabilidades-&-Competencias))
- [Dirección de Gestión](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Direcci%C3%B3n-de-Gesti%C3%B3n)
- [Proceso de Desarrollo (SCRUM)](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Proceso-de-Desarrollo-(SCRUM))

---

## 6. Referencias y Fuentes

### 6.1 Referencias Arquitectónicas Internas

- [Escenarios y Casuísticas para Arquitectura de Datos](https://project.comsatel.com.pe/comsatel/development/products/clocator2/collaboration/-/wikis/Escenarios-y-Casu%C3%ADsticas-para-Arquitectura-de-Datos)

### 6.2 Fuentes Externas de Referencia

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

## 7. Historial de Versiones

| Versión | Fecha | Descripción de Cambios | Autor |
|---------|-------|------------------------|-------|
| 1.0 | [Fecha Original] | Documento base de arquitectura | Equipo Arquitectura |
| 1.1 | 2026-04-08 | Consolidación con Manual de Procedimientos SOP v1.2. Integración de hitos, templates GitLab, protocolos QA, seguridad RLS y controles de proyecto | Lingma AI Assistant |
| 1.2 | Pendiente | Revisiones futuras según evolución de plataforma | Por definir |

---

**Clasificación**: Confidencial / Uso Interno Comsatel  
**Área**: Inteligencia de Negocios / Arquitectura de Datos  
**Rol Objetivo**: Analista de Datos / Ingeniero de Datos / Project Manager / Arquitecto de Soluciones