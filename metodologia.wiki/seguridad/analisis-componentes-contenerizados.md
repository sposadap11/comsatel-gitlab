[[_TOC_]]

# Análisis de Vulnerabilidad de Componentes Contenerizados

## Requisitos

Se necesita tener instalado:

- trivy: como paquete instalado al nivel de sistema operativo (https://aquasecurity.github.io/trivy/v0.18.3/installation/)
- docker / docker-compose: para ejecutar trivy desde contenedor

## Procedimiento (Manual)

Para realizar el análisis de vulnerabilidades de contenedores se realizarán los siguientes pasos:

1. **Paso 1**: ejecutar trivy sobre la imagen de contenedor

1.1 Alternativa 1 (recomendada)

```sh
IMAGEN_DOCKER=comsatel/telemetria:1.0.0
ARCHIVO_REPORTE=report.html
docker run --rm -v $(pwd)/cache:/root/.cache/ \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $(pwd):/data aquasec/trivy \
  image --format template --template "@contrib/html.tpl" \
  -o /data/$ARCHIVO_REPORTE \
  $IMAGEN_DOCKER
```

En la carpeta local desde donde se ejecuta `trivy` se recibe el resultado del análisis en el archivo `result.txt`

1.2 Alternativa 2 (`no recomendada`)

En esta alternativa se ejecuta `trivy` desde paquete instalado en sistema operativo.

```sh
trivy image -o /data/result.txt aquasec/trivy image $IMAGEN_DOCKER
```

En la carpeta local desde donde se ejecuta `trivy` se recibe el resultado del análisis en el archivo `result.txt`

2. **Paso 2**: Análisis de resultados

Se debe revisar el resultado de las pruebas y ejecutar las acciones para resolver los casos de criticidades siguientes:

**Perfil de Seguridad**
|Nivel de Criticidad|Máximas Ocurrencias|
|-|-|
|CRITICAL|0|
|HIGH|0|
|MEDIUM|>1|
|LOW|>1|

> Nota: el pase a producción solo se puede realizar de cumplirse con el Perfil de Seguridad

3. **Paso 3**: Registro de Resultados

El informe final debe ser archivado como parte de las evidencias para el pase a producción.

4. **Paso 4**: Resumen de Vulnerabilidades

```sh
IMAGEN_DOCKER=comsatel/citas:1.2.1
ARCHIVO_REPORTE=report-citas-resumen.txt
docker run --rm -v $(pwd)/cache:/root/.cache/ \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $(pwd):/data aquasec/trivy image \
  --format template \
  --template '{{- $critical := 0 }}{{- $high := 0 }}{{- range . }}{{- range .Vulnerabilities }}{{- if  eq .Severity "CRITICAL" }}{{- $critical = add $critical 1 }}{{- end }}{{- if  eq .Severity "HIGH" }}{{- $high = add $high 1 }}{{- end }}{{- end }}{{- end }}Critical: {{ $critical }}, High: {{ $high }}' \
  -o /data/ $ARCHIVO_REPORTE $IMAGEN_DOCKER
```

## Ejemplos

1. CL2: [Telemetría Report.html](uploads/99fe68baeb73a8ebfc177afd59027c35/report.html)

## Referencias

1. [Trivy Reporting](https://aquasecurity.github.io/trivy/v0.41/docs/configuration/reporting/#html)