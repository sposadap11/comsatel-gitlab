[[_TOC_]]

# Propósito

> Colocar en esta sección el propósito del procedimiento

Realizar el análisis estático de código seguro (SAST - Static Analysis Security Testing) de una aplicación web usando SonarQube.

# Pre-Requisitos

> Colocar aquí los requisitos previos para una ejecución exitosa del Análisis Estático de Seguridad de Código (SAST - Static Analysis Security Testing)

1. Tener instalado `docker`
2. Tener instalado `git`
3. Espacio suficiente en disco para descargas de librerías java (maven)

# Procedimiento

> Colocar en esta sección los pasos necesarios para alcanzar el propósito del procedimiento

## Paso 1: Obtener código fuente

En este paso se realiza la descarga del código fuente de la aplicación web a ser analizada. Para ello, se deberá ejecutar el siguiente código en shell del SO (linux).

```sh
RAMA=develop
PRODUCTO=clocator
PROYECTO=clocator
GITLAB_BASE_URL=https://project.comsatel.com.pe/comsatel/development/products/$PROYECTO
git clone -b $RAMA $GITLAB_BASE_URL/$PROYECTO
```
Se debe colocar valores específicos para las variables siguientes:

|Variable|Significado|Valor implícito|
|-|-|-|
|RAMA|Rama de código a ser analizada|develop|
|GITLAB_BASE_URL|Ruta URL base de los proyectos del Proyecto Smart Suite|https://project.comsatel.com.pe/comsatel/development/products/smart-suite|
|PROYECTO|Ruta específica del proyecto dentro del producto|i-tracing/st-back.git|

## Paso 2: Ejecutar análisis de código

```sh
SONARQUBE_URL=https://sonarqube.qa.comsatel.com.pe
USER=ssuser
PWD=
PROJECT_KEY=smart-tracing-back
docker run -it --rm --name sonarqube-analyzer \
  -v ~/.m2:/root/.m2 \
  -v "$(pwd):/app" \
  -v "/tmp:/certs" \
  -w /app maven:3.8.3-adoptopenjdk-15 \
  mvn compile org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar \
  -DskipTests=true \
  -Dsonar.host.url=$SONARQUBE_URL \
  -Dsonar.login=$USER \
  -Dsonar.password=$PWD \
  -Dsonar.projectKey=$PROJECT_KEY \
  -Dcom.sun.net.ssl.checkRevocation=false
```
Se debe colocar valores específicos para las variables siguientes:

|Variable|Significado|Valor implícito|
|-|-|-|
|SONARQUBE_URL|Direccion del servidor SonarQube|https://sonarqube.qa.comsatel.com.pe|
|USER|Usuario con privilegio para publicar en SonarQube|ssuser|
|PWD|Contraseña del usuario|`solicitar`|
|PROJECT_KEY|Identificador del proyecto en SonarQube|smart-tracing-back|

## Paso 3: Análisis de resultados

En este paso se debe revisar el resultado del análisis del código, cuando el resultado sea fallido. Se debe identificar las acciones a realizar para asegurar cumplimiento con el Perfil de Calidad establecido en SonarQube.

Ingresar a través de [SonarQube](https://sonarqube.qa.comsatel.com.pe/)

![image](uploads/cf3f5859e0a652d394b9e63523de71f2/image.png)

## Paso 4: Resolver no conformidad

En este paso se debe ejecutar todas las acciones que permitan resolver los hallazgos que generan no conformidad con el perfil de calidad establecido.

![image](uploads/1d6a031db28d6f36510cdcf2c3d09e28/image.png)

# Referencias

> Incluir toda referencia que ayuda a comprender el procedimiento.

1. [Clean as you code](https://sonarqube.qa.comsatel.com.pe/documentation/user-guide/clean-as-you-code/)
1. [Quality Gates](https://sonarqube.qa.comsatel.com.pe/documentation/user-guide/quality-gates/)
1. [Metrics Definition](https://sonarqube.qa.comsatel.com.pe/documentation/user-guide/metric-definitions/)