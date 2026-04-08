# Estándar Técnico de Gestión de Logs en Aplicaciones:

## 1. Introducción
Este documento define las reglas de observabilidad para los sistemas de Comsatel. El objetivo es permitir diagnósticos rápidos sin comprometer la seguridad de los datos de los clientes.

---

## 2. Reglas Técnicas Obligatorias
* **Placeholders:** Prohibido usar `+` para strings. Usar `{}`.
    * *Bien:* `log.info("Unidad {} conectada", id);`
    * *Mal:* `log.info("Unidad " + id + " conectada");`
* **Excepciones:** Siempre pasar el objeto `Exception` como último parámetro para capturar el stacktrace.

---

## 3. Tratamiento de Datos Sensibles (PII)
Queda prohibido el logueo de datos sensibles en texto claro. Aplicar las siguientes reglas:

| Dato | Regla de Enmascaramiento | Ejemplo |
| :--- | :--- | :--- |
| **Contraseñas** | **REDACTED** (Nunca loguear) | `pass=[REDACTED]` |
| **Latitud/Longitud** | Redondear a 2 decimales (en INFO) | `pos=-12.12, -77.02` |
| **DNI/Documento** | Solo últimos 4 dígitos | `dni=****5678` |
| **Nombres** | Solo inicial y primer apellido | `user=J. Perez` |
| **Celular** | Enmascarar dígitos centrales | `tel=98****32` |

---

## 4. Niveles de Log
1.  **ERROR:** Fallos críticos. 
2.  **WARN:** Situaciones anómalas. 
3.  **INFO:** Hitos de negocio.
4.  **DEBUG:** Diagnóstico técnico.

---

## 5. Formato de Salida (Pintado)
Se debe usar el patrón **Key-Value** para facilitar la indexación en herramientas como ELK o Splunk:

`EVENT=[Nombre] | ENTITY=[Tipo:ID] | [Detalles adicionales...]`

**Ejemplo de log estándar:**
`log.info("EVENT=UNIT_IGNITION_ON | ENTITY=UNIT:{} | VOLTAGE={}", imei, volt);`

---

## 6. Correlación (MDC)
Es obligatorio que cada log incluya un `traceId` en sistemas distribuidos para seguir una petición desde el API hasta el procesamiento.### Consideraciones de errores en caso se presenten caídas del servicio o caída del flujo:


---

## 7. Control automático (puntos 2 y 5)
Para evitar que se violen el **punto 2** (placeholders `{}` y excepciones como último parámetro) y el **punto 5** (formato `EVENT= | ENTITY=`), el proyecto incluye tests que se ejecutan en `mvn test`:

- **`LoggingStandardsTest`** (en `api/src/test/...`):
  - **Punto 2:** Falla si en algún `log.info/error/warn/debug` se usa concatenación con `+` o `String.format(...)` en el mensaje. Se debe usar siempre placeholders `{}`.
  - **Punto 5:** Falla si el mensaje del log (primer argumento string) no contiene `EVENT=`.

**Exclusión puntual:** Si en una línea concreta debes incumplir la regla de forma justificada, añade en esa misma línea el comentario:

`// logging-standards-exclude`
Así esa llamada a log no se considerará violación.
### Consideraciones de errores en caso se presenten caídas del servicio o caída del flujo: