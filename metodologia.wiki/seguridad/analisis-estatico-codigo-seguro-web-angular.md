[[_TOC_]]

# Propósito

> Colocar en esta sección el propósito del procedimiento

Realizar el análisis estático de código seguro (SAST - Static Analysis Security Testing) de una aplicación web usando SonarQube.

# Pre-Requisitos

> Colocar aquí los requisitos previos para una ejecución exitosa del Análisis Estático de Seguridad de Código (SAST - Static Analysis Security Testing)

1. Tener instalado `docker`
2. Tener instalado `git`

# Procedimiento

> Colocar en esta sección los pasos necesarios para alcanzar el propósito del procedimiento

## Paso 1: Obtener código fuente

En este paso se realiza la descarga del código fuente de la aplicación web a ser analizada. Para ello, se deberá ejecutar el siguiente código en shell del SO (linux).

```sh
RAMA=develop
GITLAB_BASE_URL=https://project.comsatel.com.pe/comsatel/development/products/smart-suite
git clone -b $RAMA $GITLAB_BASE_URL/$PROYECTO
```
Se debe colocar valores específicos para las variables siguientes:

|Variable|Significado|Valor implícito|
|-|-|-|
|RAMA|Rama de código a ser analizada|develop|
|GITLAB_BASE_URL|Ruta URL base de los proyectos del Proyecto Smart Suite|https://project.comsatel.com.pe/comsatel/development/products/smart-suite|
|PROYECTO|Ruta específica del proyecto dentro del producto|i-tracing/st-web.git|

## Paso 2: Ejecutar análisis de código

```sh
SONARQUBE_URL=https://sonarqube.qa.comsatel.com.pe
USER=ssuser
PWD=
PROJECT_KEY=smart-suite_i-tracing_st-web
docker run --rm   -e SONAR_HOST_URL=$SONARQUBE_URL \
  -e SONAR_LOGIN="$USER" \
  -e SONAR_PASSWORD="$PWD" \
  -v "$(pwd):/usr/src" \
  sonarsource/sonar-scanner-cli \
  sonar-scanner \
  -Dsonar.projectKey=$PROJECT_KEY \
  -Dsonar.sources=.
```
Se debe colocar valores específicos para las variables siguientes:

|Variable|Significado|Valor implícito|
|-|-|-|
|SONARQUBE_URL|Direccion del servidor SonarQube|https://sonarqube.qa.comsatel.com.pe|
|USER|Usuario con privilegio para publicar en SonarQube|ssuser|
|PWD|Contraseña del usuario|`solicitar`|
|PROJECT_KEY|Identificador del proyecto en SonarQube|smart-suite_i-tracing_st-web|

## Paso 3: Análisis de resultados

En este paso se debe revisar el resultado del análisis del código, cuando el resultado sea fallido. Se debe identificar las acciones a realizar para asegurar cumplimiento con el Perfil de Calidad establecido en SonarQube.

Ingresar a través de [SonarQube](https://sonarqube.qa.comsatel.com.pe/)

![image](/uploads/b16dc26989aa3df7cb7d35ad5c415769/image.png)

## Paso 4: Resolver no conformidad

En este paso se debe ejecutar todas las acciones que permitan resolver los hallazgos que generan no conformidad con el perfil de calidad establecido.

![image](/uploads/2bf0275134cd9af77c39a72baa871b6b/image.png)

# Referencias

> Incluir toda referencia que ayuda a comprender el procedimiento.

1. [Sonar Scanner](https://sonarqube.qa.comsatel.com.pe/documentation/analysis/scan/sonarscanner/)
2. [Clean as you code](https://sonarqube.qa.comsatel.com.pe/documentation/user-guide/clean-as-you-code/)
3. [Quality Gates](https://sonarqube.qa.comsatel.com.pe/documentation/user-guide/quality-gates/)
4. [Metrics Definition](https://sonarqube.qa.comsatel.com.pe/documentation/user-guide/metric-definitions/)