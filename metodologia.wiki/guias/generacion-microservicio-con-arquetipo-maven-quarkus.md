[[_TOC_]]

# Guia de Uso del Arquetipo Maven para Microservicios SprintBoot

## Pre-Requisitos

Se debe instalar el arquetipo en el repositorio local maven para poder utilizarla.Se debe descargar el jar del arquetipo desde la URL siguiente: https://project.comsatel.com.pe/comsatel/development/common/archetypes/microservice-springboot-archetype/-/packages/1. Para ello emitir el siguiente comando (prerrequisito: se necesita tener maven instalado localmente):

Para SO Linux: 

```sh
mvn install:install-file \
   -Dfile=microservice-springboot-archetype-1.0.jar \
   -DgroupId=pe.com.comsatel.archetypes \
   -DartifactId=microservice-springboot-archetype \
   -Dversion=1.0 \
   -Dpackaging=jar \
   -DgeneratePom=true
```

Para SO Windows (DOS):

```sh
mvn install:install-file -Dfile=microservice-springboot-archetype-1.0.jar \
   -DgroupId=pe.com.comsatel.archetypes \
   -DartifactId=microservice-springboot-archetype \
   -Dversion=1.0 \
   -Dpackaging=jar \
   -DgeneratePom=true
```

## Procedimiento

### Paso 1: Generar estructura del microservicio

Para realizar la generación de la estructura base para un microservicio se deberá utilizar la siguiente estructura standard.

```sh
mvn archetype:generate \
  -DarchetypeGroupId=pe.com.comsatel.archetypes \
  -DarchetypeArtifactId=microservice-springboot-archetype \
  -DarchetypeVersion=1.0 \
  -DgroupId=com.comsatel.DOMINIO \
  -DartifactId=NOMBRE-MICROSERVICIO \
  -Dversion=1.0.0-SNAPSHOT \
  -Dpackage=com.comsatel.DOMINIO \
  -Dmicroservice="DOMINIO" \
  -Ddomain=DOMINIO \
  -Daggregate=AGREGADORAIZ
```

Un caso ejemplo para la generación de un microservicio para fabricantes.

#### Caso de uso sobre entorno Linux

```sh
mvn archetype:generate \
  -DarchetypeGroupId=pe.com.comsatel.archetypes \
  -DarchetypeArtifactId=microservice-springboot-archetype \
  -DarchetypeVersion=1.0 \
  -DgroupId=pe.com.comsatel.fabricantes \
  -DartifactId=fabricantes \
  -Dversion=1.0.0-SNAPSHOT \
  -Dpackage=pe.com.comsatel.fabricantes \
  -Dmicroservice="Fabricantes" \
  -Ddomain=fabricantes \
  -Daggregate=Fabricante
```

#### Caso de uso sobre entorno Windows

```sh
mvn archetype:generate -DarchetypeGroupId=pe.com.comsatel.archetypes
 -DarchetypeArtifactId=microservice-springboot-archetype
 -DarchetypeVersion=1.0 
 -DgroupId=pe.com.comsatel.fabricantes
 -DartifactId=fabricantes
 -Dversion=1.0.0-SNAPSHOT -Dpackage=pe.com.comsatel.fabricantes 
 -Dmicroservice="Fabricantes" 
 -Ddomain=fabricantes -Daggregate=Fabricante
```

Un vez ejecutado el arquetipo le pedirá confirmar la generación del código base del microservicio. Si todos los parámetros son correctos simplente ingresar "Y".

## Adecuación de la estructura generada

Al finalizar el proceso se creará una carpeta con el nombre del proyecto. Dicha carpeta con la estructura del microservicio deberá ser publicada en GitLab.

Algunas consideraciones importantes a tomar en cuenta:

1. Se debe reemplazar el archivo `api/src/main/resources/openapi/api.yaml` con el contenido del archivo Swagger / OpenAPI donde se ha elaborado el Diseño del API del Microservicio.
2. Se debe compilar el microservicio para corregir los nombres de clases que correspondan según en Swagger del API diseñada.
3. Se debe ajustar las clases `<NOMBREAGREGDO>APIDelegateImpl.java` según corresponda a las definiciones de tipos del Swagger.
4. se debe asegurar tener instalado el JDK de acuerdo con lo indicado en el `pom.xml` del arquetipo (al momento de escribir esta Guía se requiere tener instalado JDK versión 15).

> [!WARNING] Las librerias de flyway configuradas son compatible con MySQL Server version 5.7 ó superior.