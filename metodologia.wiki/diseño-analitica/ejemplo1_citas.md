[[_TOC_]]

# Breve Resumen

Para gestionar eficazmente las operaciones de campo (Instalación, Mantenimiento y Retiro de equipos GPS), el enfoque metodológico debe alejarse de la simple medición de volumen y centrarse en la Eficiencia Operativa y la Satisfacción del Cliente.

A continuación, se presenta un resumen de la Ficha Metodológica de KPI, diseñada para ser el insumo principal antes de cualquier configuración en Apache Superset.

# Ficha Metodológica de KPI

## 1. El Eje Central: Índice de Cumplimiento de Citas (ICC)

Metodológicamente, este KPI es el "corazón" del proceso, ya que mide la capacidad de la organización para cumplir sus promesas de servicio en el vehículo.Ficha Técnica del KPI.

* **Nombre del KPI**: Índice de Cumplimiento de Citas de Campo (ICC).
* **Definición**: Porcentaje de servicios (instalación, mantenimiento o retiro) ejecutados exitosamente en la fecha y hora pactada, sin re-trabajos.
* **Fórmula**:

> ICC = (Citas Ejecutadas Exitosamente / Total de Citas Programadas) * 100


* **Unidad de Medida**: Porcentaje (%).
* **Frecuencia de Revisión**: Semanal (operativo) / Mensual (estratégico).

## 2. Aspectos Metodológicos Clave (Dimensiones de Análisis)

Para que el analista de BI pueda construir el Modelo Dimensional, debe considerar estas variables que explican el comportamiento del ICC:

- A. Clasificación del Servicio (Atributos)

  - *Tipo de Atención*: Instalación (Alta), Mantenimiento (Correctivo/Preventivo), Retiro (Baja).
  - *Tipo de Servicio*: Básica o Flota.

- B. Dimensiones de Tiempo y Lugar
  - **Ventana Horaria**: Mañana vs. Tarde (identificar bloqueos por tráfico o disponibilidad del vehículo).
  - **Ubicación**: Taller propio vs. Domicilio del cliente (identificar eficiencia de cuadrillas móviles).

- C. Los "Umbrales" (Contextualización)

Metodológicamente, el KPI necesita un semáforo para la toma de decisiones:

  - **Verde** (> 92%): Operación óptima.
  - **Amarillo** (85% - 91%): Revisar disponibilidad de técnicos o stock de accesorios.
  - **Rojo** (< 85%): Acción inmediata (posible crisis de servicio o problemas logísticos graves).

## 3. Flujo de Trazabilidad: Del Negocio al Dato
Siguiendo la arquitectura que hemos analizado, así se despliega este KPI en el sistema:

Capa de Requerimiento: El Gerente de Operaciones define que el ICC es vital para reducir costos de desplazamiento.

1. **Capa de Modelo (Dataset)**: Se unen las tablas de Programación_Citas con Reportes_Técnicos_Campo en el bloque metadata/ODS.

2. Capa de Visualización (Dashboard):

3. **KPI Principal**: Número grande en pantalla con el % de ICC actual.

4. **Gráfico de Tendencia**: Línea temporal para ver si el cumplimiento mejora o empeora.

5. **Gráfico de Causa Raíz**: Gráfico de barras que muestre por qué fallaron las citas (ej. "Vehículo no disponible", "Técnico tarde", "Falta de stock").

6. Capa de Entrega (Worker/Alertas): Si el ICC cae por debajo del 85% en una ciudad específica, el Worker detecta la anomalía y dispara una Alerta inmediata al Supervisor regional vía email/Telegram.

# Interpretación de resultados

Si el ICC es bajo, impacta directamente en:
- **Costos**: Cada viaje fallido de una cuadrilla es dinero perdido.
- **Ingresos**: Si no se instala el GPS, no se puede empezar a facturar el servicio de monitoreo.
- **Churn**: Un cliente frustrado por citas incumplidas cancelará el contrato.