Para alinear la configuración de la detección de fuerza bruta en Keycloak con los principios de seguridad de ISO 27001, el objetivo principal es proteger las cuentas de usuario contra ataques de adivinación de contraseñas y fuerza bruta. ISO 27001, si bien no especifica valores técnicos exactos, requiere la implementación de controles de acceso adecuados (Anexo A.9) para proteger la confidencialidad, integridad y disponibilidad de la información.

Basándonos en la configuración de Detección de Fuerza Bruta que se muestra en la imagen, aquí tienes una configuración recomendada y su justificación en el contexto de ISO 27001:

1. Enabled (Activado):
   - Recomendación: ON.
   - Justificación ISO 27001: Habilitar esta función es fundamental para implementar un control de seguridad básico contra ataques de fuerza bruta, que es un requisito implícito para proteger los sistemas de acceso no autorizado según el Anexo A.9.

2. Permanent Lockout (Bloqueo Permanente):
   - Recomendación: Considerar ON para entornos de alta seguridad donde el riesgo de ataques persistentes es elevado. Si se elige OFF, asegurarse de que los tiempos de bloqueo temporal sean suficientes.
   - Justificación ISO 27001: Un bloqueo permanente ofrece una defensa más robusta contra atacantes persistentes. Sin embargo, requiere un proceso definido para que los usuarios legítimos recuperen el acceso (por ejemplo, a través de un restablecimiento de contraseña gestionado por soporte o un proceso de auto-servicio seguro). Si se opta por bloqueo temporal (OFF), los siguientes parámetros de tiempo se vuelven cruciales.

3. Max Login Failures (Máximo de Fallos de Inicio de Sesión):
   - Recomendación: Un valor bajo, típicamente entre 3 y 5. El valor de 5 mostrado en la imagen es un buen punto de partida.
   - Justificación ISO 27001: Limitar el número de intentos fallidos reduce significativamente la ventana de oportunidad para que un atacante adivine una contraseña. Un número pequeño de intentos minimiza el riesgo en caso de que un atacante obtenga una lista de nombres de usuario.

4. Wait Increment (Incremento del Tiempo de Espera):
   - Recomendación: Un valor inicial razonable, por ejemplo, 1 minuto como se muestra. Este tiempo se incrementará con cada bloqueo temporal sucesivo.
   - Justificación ISO 27001: Aumentar el tiempo de espera después de cada bloqueo temporal hace que los ataques de fuerza bruta sean progresivamente más lentos y costosos para el atacante, desalentando los intentos continuos.

5. Quick Login Check Milliseconds (Milisegundos para Verificación Rápida de Inicio de Sesión):
   - Recomendación: Mantener el valor por defecto (generalmente 1000 milisegundos) a menos que haya una razón específica de rendimiento para cambiarlo.
   - Justificación ISO 27001: Este parámetro es más técnico y afecta la forma en que Keycloak maneja las peticiones rápidas. Asegurarse de que esté configurado de manera que no permita la elusión de los contadores de fallo es importante.

6. Minimum Quick Login Wait (Espera Mínima para Verificación Rápida de Inicio de Sesión):
   - Recomendación: Un valor bajo pero que impida reintentos instantáneos, por ejemplo, 1 minuto como se muestra.
   - Justificación ISO 27001: Similar al Wait Increment, introduce un retardo que dificulta los intentos rápidos y repetidos en un corto período de tiempo.

7. Max Wait (Tiempo Máximo de Espera):
   - Recomendación: Un límite superior razonable para el bloqueo temporal, por ejemplo, 15 minutos como se muestra, o incluso más tiempo dependiendo del perfil de riesgo.
   - Justificación ISO 27001: Define el tiempo máximo que una cuenta permanecerá bloqueada temporalmente. Un tiempo máximo adecuado equilibra la seguridad (ralentizando al atacante) con la usabilidad (permitiendo que los usuarios legítimos recuperen el acceso sin intervención manual en un tiempo razonable si no se usa bloqueo permanente).

8. Failure Reset Time (Tiempo para Restablecer Fallos):
   - Recomendación: Un período de tiempo suficiente para que no sea trivial esperar y volver a intentar. 12 horas como se muestra es una configuración común y razonable. Períodos más largos (24 horas o más) pueden ofrecer mayor seguridad.
   - Justificación ISO 27001: Este parámetro determina cuánto tiempo debe pasar sin intentos fallidos para que el contador de "Max Login Failures" se reinicie a cero. Un tiempo de restablecimiento más largo dificulta los ataques de "prueba y espera" donde un atacante intenta algunas contraseñas, espera a que se reinicie el contador y vuelve a intentarlo.

Consideraciones Adicionales para ISO 27001:
1. Procedimientos de Gestión de Incidentes: Tener procedimientos claros sobre cómo responder a bloqueos de cuenta (tanto temporales como permanentes), incluyendo cómo los usuarios pueden solicitar el desbloqueo y cómo el personal de soporte verifica la identidad. Esto se relaciona con el Anexo A.16 de ISO 27001 (Gestión de Incidentes de Seguridad de la Información).
2. Monitorización y Alertas: Configurar Keycloak para registrar los eventos de inicio de sesión fallido y bloqueo de cuenta, y establecer sistemas de monitorización y alerta para detectar patrones sospechosos que puedan indicar un ataque en curso. Esto apoya el Anexo A.12.4 (Registro y Monitorización).
3. Concienciación del Usuario: Educar a los usuarios sobre la política de bloqueo de cuentas y la importancia de utilizar contraseñas fuertes para minimizar la probabilidad de bloqueos legítimos. Esto se alinea con el Anexo A.7.2 (Concienciación, Educación y Formación en Seguridad de la Información).
4. Pruebas Regulares: Realizar pruebas periódicas de la configuración de detección de fuerza bruta para asegurarse de que funciona según lo esperado y es efectiva contra las amenazas actuales.

> Nota 1: Una configuración alineada con ISO 27001 implica habilitar la detección de fuerza bruta con un número bajo de fallos permitidos, configurar tiempos de espera que desalienten los intentos continuos y definir un tiempo de restablecimiento de fallos adecuado. La elección entre bloqueo temporal y permanente dependerá del perfil de riesgo específico de la organización y sus capacidades operativas para gestionar cuentas bloqueadas.

La configuración final recomendada en Redhat Keycloak es la siguiente:

![image](uploads/97d01e6744acd546e2f8e19dfc73b7fa/image.png)