[[_TOC_]]

# Guía para Dynamic Analysis Security Testing 

## Requisitos

Se debe tener instalado y configurada la versión más actualizada de los siguientes componentes.

- [OWASP ZAP 2.15.0](https://www.zaproxy.org/download/) (al momento de la revisión de esta guía)
- Firefox v132.0.1 (al momento de la revisión de esta guía)

## Procedimiento

### Instalación

1. Se debe descargar el ejecutable de la aplicación desde el site www.ZAPROXY.org.

2. Iniciar la instalación del producto

![image](uploads/f68edc9b410bfdde992f9ed11135a999/image.png)

3. Establecer la ruta de la instalación

La ruta de la instalación puede ser modificada o simplemente se confirma la ruta sugerida.

![image](uploads/8a68aab33dd13cda1c79701086344599/image.png)

![image](uploads/b8640a75435be484fdcb0231b625d218/image.png)

4. Completar la instalación

![image](uploads/87acdcfd94513144fc084e91618c6d0e/image.png)

### Ejecución de Análisis de Vulnerabilidades

Para la ejecución del análisis dinámico de seguridad de aplicación (DAST) se deben realizar los siguientes pasos.

1. Ingresar a la aplicación Web

En este paso se deberá abrir el browser Mozilla Firefox e ingresar la URL de la aplicación web analizar en la instancia del navegador que se abrirá luego de seleccionar el ícono indicado en la siguiente figura.

![image](uploads/ba5e0bceb3ac03417c4f20649827cc07/image.png)

> Debe indicar seleccionar "Continue to your target" en la imagen siguiente.

![image](uploads/092be6e09ed074a4fd957a5080a4a328/image.png)

> Se debe ingresar con las credenciales de usuario para dejar abierta la sesión para el análisis de vulnerabilidades.

2. Iniciar el "Ataque"

En este punto, seleccione "Atacar" como se indica en la imagen siguiente y esperar a que el proceso complete.

![image](uploads/cab1fc32303cce35f17da516beda4c27/image.png)

La aplicación realizará la detección de las vulnerabilidades de acuerdo con la base de datos de vulnerabilidades vigente al momento de realizar el ataque.

![image](uploads/a831a6556f534b53906138391adc1ab0/image.png)

3. Analizar los resultados

Se deberá analizar los resultados obtenidos y generar los tickets (issue) en GitLab por cada tipo de vulnerabilidad identificada durante el análisis, estableciendo las prioridades de su atención.

A modo referencial, se presenta en la siguiente imagen algunos posibles resultados obtenidos.

![image](uploads/81bfbe944c286f1862a4176865d333d1/image.png)

4. Archivar el informe de resultados.

Se deberá registrar el informe del resultado del ataque como evidencia del proceso en las carpetas correspondientes al proyecto y entorno sobre el que se ha realizado el análisis de vulnerabilidades.

![image](uploads/a33244642db929b16f59e770709e8709/image.png)

Se debe elegir la ruta donde almacenar el informe generado como evidencia del resultado del ataque.

|Entorno|Ruta base|
|-|-|
|QA|\\192.168.1.254\02_DESPLIEGUE_QA|
|CERT|\\192.168.1.254\03_DESPLIEGUE_CERT|
|PRE|\\192.168.1.254\04_DESPLIEGUE_PRE|
|PROD|\\192.168.1.254\05_DESPLIEGUE_PROD|

![image](uploads/6258af3e7e8a1d8166096a40d804d034/image.png)

> Deben quedar registradas las evidencias en el producto y proyecto correspondiente bajo la ruta `DAST` incluyendo todos los archivos del análisis de vulnerabilidades.

## Referencias

1. [Portal OWASP ZAP](https://www.zaproxy.org/)
2. [Descarga de OWASP ZAP](https://www.zaproxy.org/download/)