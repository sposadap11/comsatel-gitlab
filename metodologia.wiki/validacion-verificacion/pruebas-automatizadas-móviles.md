* Guía de Instalación y Configuración - Framework Android

  \[\[_TOC_\]\]

  ---

  ## **Introducción**

  El **Framework de Automatización**  es una solución completa para pruebas automatizadas de aplicaciones móviles Android, desarrollado específicamente para las aplicaciones **Comsatel**

  Este framework implementa las mejores prácticas de la industria combinando:
  * **Page Object Model (POM)** para mantenibilidad
  * **Behavior Driven Development (BDD)** con Cucumber para legibilidad
  * **Integración CI/CD** con GitLab para ejecución continua

  ---

  ## **Objetivos del Framework**
  * **Automatizar** pruebas funcionales de las aplicaciones móviles Comsatel
  * **Reducir** tiempo de ejecución de pruebas de regresión
  * **Aumentar** cobertura de pruebas
  * **Generar** reportes detallados con evidencia visual

  ---

  ## **Stack Tecnológico**

  ### Componentes Principales
  | Componente | Tecnología | Versión | Propósito |
  |------------|------------|---------|-----------|
  | **Lenguaje** | Java (OpenJDK) | 25 (LTS) | Lenguaje base del framework |
  | **Build Tool** | Apache Maven | 3.9.x | Gestión del ciclo de vida y dependencias |
  | **Automation Engine** | Appium Server | 3.1.1 | Servidor de interacción con el dispositivo |
  | **Appium Client** | Appium Java Client | 9.3.0 | Librería cliente para comunicación con Appium |
  | **BDD Framework** | Cucumber JVM | 7.20.1 | Definición de escenarios en lenguaje Gherkin |
  | **Test Runner** | JUnit | 4.13.2 | Ejecutor de pruebas y aserciones |
  | **Reporting** | ExtentReports | 1.14.0 | Generación de reportes HTML detallados |
  | **Logging** | Log4j2 | 2.24.3 | Registro de eventos y depuración |

  ---

  ## **Componentes del Framework**

  ### 1. Base Package (com.comsatel.base)

  **DriverManager.java**
  * Gestión del ciclo de vida del Appium Driver
  * Inicialización de capabilities desde `capabilities.properties`
  * Singleton pattern para una única instancia del driver
  * Métodos para iniciar/detener el driver

  **BasePage.java**
  * Métodos comunes para todas las páginas
  * Waits
  * Acciones genéricas (click, sendKeys, scroll, swipe)
  * Captura de screenshots
  * Manejo de elementos móviles (tap, long press)

  **ConfigReader.java**
  * Lectura de archivos de configuración (.properties)
  * Gestión de variables de entorno
  * Configuración de capabilities dinámicas

  ### 2. Page Objects (com.comsatel.pageobjects)

  Representación de las pantallas de la aplicación siguiendo el patrón POM:
  * **WelcomePage.java** - Pantalla de bienvenida
  * **LoginPage.java** - Pantalla de autenticación
  * **HomePage.java** - Pantalla principal
  * **ViajesPage.java** - Pantalla de gestión de viajes

  ### 3. Step Definitions (com.comsatel.stepdefs)

  Implementación de escenarios Gherkin:
  * **LoginSteps.java** - Pasos para autenticación
  * **ViajesSteps.java** - Pasos para gestión de viajes

  ### 4. Hooks (com.comsatel.hooks)

  **TestHooks.java**
  * Configuración previa a cada escenario (@Before)
  * Limpieza posterior a cada escenario (@After)
  * Captura de screenshots en caso de fallo

  ### 5. Estructura de Directorios

  ```
  android-automation-framework/
  │
  ├── src/
  │   └── test/
  │       ├── java/
  │       │   └── com/
  │       │       └── automation/
  │       │           │
  │       │           ├── base/                     # Capa base - Factory y gestión del Driver
  │       │           │   ├── BasePage.java         # Clase base con métodos comunes para POM
  │       │           │   ├── ConfigReader.java     # Lectura de archivos de configuración
  │       │           │   └── DriverManager.java    # Gestión del ciclo de vida del Appium Driver
  │       │           │
  │       │           ├── hooks/                    # Hooks de Cucumber - Gestión del ciclo de vida
  │       │           │   └── TestHooks.java        # Before/After hooks para inicializar/limpiar
  │       │           │
  │       │           ├── pageobjects/              # Page Object Model (POM) - Elementos y acciones
  │       │           │   ├── LoginPage.java        # POM: Ejemplo - Pantalla de autenticación
  │       │           │   ├── HomePage.java         # POM: Ejemplo - Pantalla principal
  │       │           │   └── [PageName].java       # POM: Agregar más páginas según sea necesario
  │       │           │
  │       │           ├── runner/                   # Test Runner - Configuración de ejecución
  │       │           │   └── TestRunner.java       # Configuración de Cucumber con JUnit
  │       │           │
  │       │           └── stepdefs/                 # Step Definitions - Implementación de pasos Gherkin
  │       │               ├── LoginSteps.java       # Steps: Ejemplo - Escenarios de login
  │       │               └── [ModuleName]Steps.java # Steps: Agregar más módulos según sea necesario
  │       │
  │       └── resources/
  │           ├── config/                           # Archivos de configuración
  │           │   ├── capabilities.properties       # Configuración de Appium (host, port, app)
  │           │   └── test-data.properties          # Datos de prueba (usuarios, credenciales)
  │           │
  │           ├── features/                         # Feature Files - Escenarios en Gherkin
  │           │   ├── login/
  │           │   │   └── login.feature             # Escenarios: Ejemplo - Autenticación
  │           │   │
  │           │   └── [module_name]/
  │           │       └── [module_name].feature     # Escenarios: Agregar más módulos según sea necesario
  │           │
  │           └── logging/
  │               └── log4j2.xml                    # Configuración de logs (consola y archivo)
  │
  ├── reports/                                      # Reportes generados automáticamente
  │   ├── screenshots/                              # Capturas de pantalla en caso de fallo
  │   └── logs/
  │       └── automation.log                        # Logs de ejecución
  │
  ├── target/                                       # Generado por Maven (compilación y ejecución)
  │   ├── classes/                                  # Clases compiladas
  │   ├── test-classes/                             # Clases de test compiladas
  │   ├── cucumber-json/                            # Reportes Cucumber JSON
  │   ├── cucumber-reports/                         # Reportes Cucumber HTML
  │   ├── generated-sources/                        # Fuentes generadas
  │   ├── generated-test-sources/                   # Fuentes de test generadas
  │   ├── maven-status/                             # Estado de Maven
  │   ├── surefire-reports/                         # Reportes de ejecución
  │   └── test-classes/                             # Clases de test compiladas
  │
  ├── .vscode/                                      # Configuración de Visual Studio Code
  │   └── settings.json                             # Configuración del IDE
  │
  ├── logs/                                         # Logs de ejecución
  │   └── automation.log                            # Archivo de logs
  │
  ├── screenshots/                                  # Capturas de pantalla
  │
  ├── test-output/                                  # Salida de tests
  │
  ├── pom.xml                                       # Descriptor del proyecto Maven
  ├── README.md                                     # Documentación principal del proyecto
  ├── Dockerfile                                    # Configuración Docker (opcional)
  ├── .gitignore                                    # Archivos ignorados por Git
  └── CHANGELOG.md                                  # Historial de cambios
  
  ```

  ---

  # Guía de Instalación Paso a Paso

  ## Paso 1: Requisitos Previos

  ### 1.1 Instalar Java JDK 25

  **Descargar:**
  1. Ir a: [https://www.oracle.com/java/technologies/downloads/#java25](https://www.oracle.com/java/technologies/downloads/#java25)
  2. Descargar JDK 25 para Windows (x64 Installer)

  **Instalar:**
  1. Ejecutar el instalador descargado
  2. Seguir el asistente de instalación
  3. Ruta de instalación recomendada: `C:\Program Files\Java\jdk-25`

  **Configurar Variable de Entorno:**

  Abrir PowerShell como Administrador y ejecutar:

  ```
  # Establecer JAVA_HOME
  [System.Environment]::SetEnvironmentVariable('JAVA_HOME', 'C:\Program Files\Java\jdk-25', 'Machine')
  
  # Agregar al PATH
  $path = [System.Environment]::GetEnvironmentVariable('Path', 'Machine')
  $newPath = $path + ';C:\Program Files\Java\jdk-25\bin'
  [System.Environment]::SetEnvironmentVariable('Path', $newPath, 'Machine')
  
  # Cerrar y abrir una nueva terminal para aplicar cambios
  
  ```

  **Verificar Instalación:**

  ```
  java -version
  ```

  Salida esperada:

  ```
  java version "25.0.1" 2025-10-21 LTS
  Java(TM) SE Runtime Environment (build 25.0.1+8-LTS-27)
  Java HotSpot(TM) 64-Bit Server VM (build 25.0.1+8-LTS-27, mixed mode, sharing)
  ```

  ---

  ### 1.2 Instalar Apache Maven 3.9.11

  **Descargar:**
  1. Ir a: [https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)
  2. Descargar `apache-maven-3.9.11-bin.zip`

  **Instalar:**
  1. Extraer el archivo ZIP
  2. Mover a: `C:\Program Files\Maven\apache-maven-3.9.11`

  **Configurar Variable de Entorno:**

  Abrir PowerShell como Administrador y ejecutar:

  ```
  # Establecer MAVEN_HOME
  [System.Environment]::SetEnvironmentVariable('MAVEN_HOME', 'C:\Program Files\Maven\apache-maven-3.9.11', 'Machine')
  
  # Agregar al PATH
  $path = [System.Environment]::GetEnvironmentVariable('Path', 'Machine')
  $newPath = $path + ';C:\Program Files\Maven\apache-maven-3.9.11\bin'
  [System.Environment]::SetEnvironmentVariable('Path', $newPath, 'Machine')
  ```

  **Verificar Instalación:**

  ```
  mvn -version
  ```

  Salida esperada:

  ```
  Apache Maven 3.9.11 (3e54c93a704957b63ee3494413a2b544fd3d825b)
  Maven home: C:\Program Files\Maven\apache-maven-3.9.11
  Java version: 25.0.1, vendor: Oracle Corporation
  ```

  ---

  ### 1.3 Instalar Node.js

  **Descargar:**
  1. Ir a: [https://nodejs.org/](https://nodejs.org/)
  2. Descargar LTS (v20.x o superior)

  **Instalar:**
  1. Ejecutar el instalador
  2. Seguir el asistente (aceptar valores por defecto)
  3. Ruta de instalación: `C:\Program Files\nodejs\`

  **Verificar Instalación:**

  ```
  node -v
  npm -v
  ```

  Salida esperada:

  ```
  v24.11.1
  10.9.2
  ```

  ---

  ### 1.4 Instalar Android SDK

  **Opción A: Android Studio (Recomendado)**
  1. Descargar Android Studio: [https://developer.android.com/studio](https://developer.android.com/studio)
  2. Instalar Android Studio
  3. Abrir Android Studio → Tools → SDK Manager
  4. Instalar los siguientes componentes dentro de Android Studio:
     * Android SDK Platform 34 (API Level 34)
     * Android SDK Build-Tools (últimas versiones)
     * Android SDK Platform-Tools
     * Android SDK Command-line Tools
     * Android Emulator
     * Intel x86 Emulator Accelerator (HAXM)

  **Opción B: Command Line Tools Only**
  1. Descargar: [https://developer.android.com/studio#command-line-tools-only](https://developer.android.com/studio#command-line-tools-only)
  2. Extraer a: `C:\Users\{USUARIO}\AppData\Local\Android\Sdk\cmdline-tools\latest`

  **Configurar Variable de Entorno:**

  Abrir PowerShell como Administrador y ejecutar:

  ```
  # Establecer ANDROID_HOME
  [System.Environment]::SetEnvironmentVariable('ANDROID_HOME', 'C:\Users\TOMAS\AppData\Local\Android\Sdk', 'Machine')
  
  # Agregar al PATH
  $path = [System.Environment]::GetEnvironmentVariable('Path', 'Machine')
  $newPath = $path + ';C:\Users\TOMAS\AppData\Local\Android\Sdk\cmdline-tools\latest\bin;C:\Users\TOMAS\AppData\Local\Android\Sdk\platform-tools;C:\Users\TOMAS\AppData\Local\Android\Sdk\emulator'
  [System.Environment]::SetEnvironmentVariable('Path', $newPath, 'Machine')
  
  ```

  **Verificar Instalación:**

  ```
  adb --version
  ```

  Salida esperada:

  ```
  Android Debug Bridge version 1.0.41
  Version 36.0.0-13206524
  ```

  ---

  ### 1.5 Instalar Git

  **Descargar:**
  1. Ir a: [https://git-scm.com/download/win](https://git-scm.com/download/win)
  2. Descargar Git para Windows

  **Instalar:**
  1. Ejecutar el instalador
  2. Seguir el asistente (aceptar valores por defecto)

  **Verificar Instalación:**

  ```
  git --version
  ```

  Salida esperada:

  ```
  git version 2.43.0.windows.1
  ```

  ---

  ## Paso 2: Instalar Appium

  ### 2.1 Instalar Appium CLI Globalmente

  Abrir PowerShell como Administrador y ejecutar:

  ```
  npm install -g appium@3.1.1
  ```

  **Verificar Instalación:**

  ```
  appium -v
  ```

  Salida esperada:

  ```
  3.1.1
  ```

  ---

  ### 2.2 Instalar Appium Drivers

  Instalar el driver UIAutomator2 para Android:

  ```
  appium driver install uiautomator2
  ```

  **Verificar Instalación:**

  ```
  appium driver list --installed
  ```

  Salida esperada:

  ```
  ✔ Listing installed drivers
  - uiautomator2@2.x.x [installed (npm)]
  ```

  ---

  ## Paso 3: Herramientas Opcionales (Recomendadas)

  ### 3.1 Instalar Visual Studio Code

  **Descargar:**
  1. Ir a: [https://code.visualstudio.com/](https://code.visualstudio.com/)
  2. Descargar Visual Studio Code para Windows

  **Instalar:**
  1. Ejecutar el instalador
  2. Seguir el asistente

  **Extensiones Recomendadas:**

  Abrir VS Code y acceder a Extensions (Ctrl+Shift+X):
  1. **Cucumber (Gherkin) Full Support**
     * Autor: Alexander Krechik
     * Propósito: Soporte para archivos .feature
  2. **Extension Pack for Java**
     * Autor: Microsoft
     * Propósito: Soporte completo para Java (IntelliSense, debugging, testing)
  3. **Debugger for Java**
     * Autor: Microsoft
     * Propósito: Depuración de código Java
  4. **Maven for Java**
     * Autor: Microsoft
     * Propósito: Integración con Maven
  5. **GitLens - Git supercharged**
     * Autor: GitKraken
     * Propósito: Visualización avanzada de Git
  6. **GitHub Copilot Chat**
     * Autor: GitHub
     * Propósito: Asistencia de IA para codificación

  ---

  ### 3.2 Instalar Appium Inspector

  **Descargar:**
  1. Ir a: [https://github.com/appium/appium-inspector/releases](https://github.com/appium/appium-inspector/releases)
  2. Descargar la versión más reciente para Windows (.exe)

  **Instalar:**
  1. Ejecutar el instalador descargado
  2. Seguir el asistente

  **Uso:**
  * Herramienta visual para inspeccionar elementos de la aplicación
  * Permite identificar locators (ID, XPath, Accessibility ID, etc.)
  * Facilita la creación de Page Objects

    ![image](uploads/7cbb959692f03c0ed481f95f93cc51af/image.png)

  ---

  ### 3.3 Instalar Appium Doctor

  **Instalar Globalmente:**

  ```
  npm install -g appium-doctor
  ```

  **Verificar Instalación:**

  ```
  appium-doctor
  ```

  **Uso:**
  * Valida que todas las dependencias estén correctamente instaladas
  * Identifica problemas de configuración
  * Proporciona recomendaciones de corrección

  ![image](uploads/c4e34257420bcf59f4ad3c5ed0db8a6c/image.png)

  **Ejecutar Validación:**

  ```
  appium-doctor --android
  ```

  Salida esperada (sin errores):

  ```
  ✔ ANDROID_HOME is set
  ✔ JAVA_HOME is set
  ✔ Android SDK is installed
  ✔ adb is available
  ✔ emulator is available
  ✔ Android SDK Build-tools are installed
  
  ```

  ---

  ## Paso 4: Configuración de Appium

  ### 4.1 Iniciar Appium Server

  **Opción 1: Línea de Comandos (Recomendado para desarrollo)**

  Abrir PowerShell y ejecutar:

  ```
  appium --address 127.0.0.1 --port 4723
  ```

  Salida esperada:

  ```
  [Appium] Welcome to Appium v3.1.1
  [Appium] Appium REST http interface listener started on 127.0.0.1:4723
  
  ```

  **Opción 2: Con Configuración Avanzada**

  ```
  appium --address 127.0.0.1 --port 4723 --log-level debug
  ```

  Parámetros útiles:
  * `--address` : Dirección IP del servidor (127.0.0.1 para local)
  * `--port` : Puerto de escucha (4723 es el estándar)
  * `--log-level` : Nivel de logging (debug, info, warn, error)
  * `--allow-insecure` : Comandos inseguros permitidos
  * `--relaxed-security` : Seguridad relajada (solo desarrollo)

  **Opción 3: Detener el Servidor**

  Presionar `Ctrl+C` en la terminal donde se ejecuta Appium

  ---

  ### 4.2 Configurar Capabilities

  Las capabilities definen cómo Appium debe conectarse al dispositivo. Se configuran en `src/test/resources/capabilities.properties`:

  ```
  # Configuración de Appium Server
  appium.server.host=127.0.0.1
  appium.server.port=4723
  
  # Configuración del Dispositivo
  platformName=Android
  appium:automationName=UiAutomator2
  appium:appPackage=com.comsatel.cgo
  appium:appActivity=com.comsatel.MainActivity
  
  # Configuración Avanzada
  appium:noReset=false
  appium:autoGrantPermissions=true
  appium:newCommandTimeout=300
  appium:connectHardwareKeyboard=false
  appium:unlockType=pin
  appium:unlockKey=1234
  ```

  **Parámetros Principales:**
  | Parámetro | Descripción | Valor Ejemplo |
  |-----------|-------------|---------------|
  | `platformName` | Sistema operativo | Android, iOS |
  | `appium:automationName` | Motor de automatización | UiAutomator2  |
  | `appium:appPackage` | Paquete de la aplicación | com.comsatel.cgo |
  | `appium:appActivity` | Actividad principal | com.comsatel.MainActivity |
  | `appium:noReset` | No resetear app entre tests | true, false |
  | `appium:autoGrantPermissions` | Otorgar permisos automáticamente | true, false |
  | `appium:newCommandTimeout` | Timeout entre comandos (segundos) | 300 |
  | `appium:deviceName` | Nombre del dispositivo | (automático si no se especifica) |
  | `appium:udid` | ID único del dispositivo | (para dispositivos físicos) |

  **Para Dispositivo Físico:**

  ```
  platformName=Android
  appium:automationName=UiAutomator2
  appium:appPackage=com.comsatel.cgo
  appium:appActivity=com.comsatel.MainActivity
  appium:udid=adb-xxxxxxxxxxx-16co6E._adb-tls-connect._tcp
  appium:noReset=false
  appium:autoGrantPermissions=true
  
  ```

  **Para Emulador:**

  ```
  platformName=Android
  appium:automationName=UiAutomator2
  appium:appPackage=com.comsatel.cgo
  appium:appActivity=com.comsatel.MainActivity
  appium:deviceName=emulator-####
  appium:noReset=false
  appium:autoGrantPermissions=true
  ```

  ---

  ### 4.3 Usar Appium Inspector

  **Pasos para Inspeccionar Elementos:**
  1. Iniciar Appium Server (ver sección 4.1)
  2. Abrir Appium Inspector
  3. Configurar Capabilities:
     * Remote Host: 127.0.0.1
     * Remote Port: 4723
     * Capabilities JSON:

  ```
  {
    "platformName": "Android",
    "appium:automationName": "UiAutomator2",
    "appium:appPackage": "com.comsatel.cgo",
    "appium:appActivity": "com.comsatel.MainActivity",
    "appium:noReset": false,
    "appium:autoGrantPermissions": true
  }
  ```
  1. Hacer clic en "Start Session"
  2. Usar el árbol de elementos para identificar locators
  3. Copiar los atributos necesarios para crear locators

     ![image](uploads/355a1a8a44e8e8b186eaeb4c581e5915/image.png)

  **Tipos de Locators Disponibles:**

  ```
  // ID de accesibilidad (Recomendado)
  AppiumBy.accessibilityId("Mis viajes, Diario")
  
  // ID de recurso
  AppiumBy.id("com.comsatel.cgo:id/btn_login")
  
  // Clase
  AppiumBy.className("android.widget.EditText")
  
  // Texto
  AppiumBy.xpath("//*[@text='Ingresar']")
  
  // XPath (Menos recomendado)
  AppiumBy.xpath("//android.widget.Button[@text='Ingresar']")
  
  ```

  ---

  ### 4.4 Validar Configuración con Appium Doctor

  Ejecutar validación completa:

  ```
  appium-doctor --android
  ```

  ![image](uploads/2ce09abdd8ba9108abe7fae2214caf37/image.png)

  Errores comunes y soluciones:
  | Error | Causa | Solución |
  |-------|-------|----------|
  | ANDROID_HOME not set | Variable no configurada | Ejecutar: `[System.Environment]::SetEnvironmentVariable('ANDROID_HOME', 'C:\Users\USER\AppData\Local\Android\Sdk', 'Machine')` |
  | JAVA_HOME not set | Variable no configurada | Ejecutar: `[System.Environment]::SetEnvironmentVariable('JAVA_HOME', 'C:\Program Files\Java\jdk-25', 'Machine')` |
  | adb not found | ADB no en PATH | Agregar `%ANDROID_HOME%\platform-tools` al PATH |
  | emulator not found | Emulador no en PATH | Agregar `%ANDROID_HOME%\emulator` al PATH |

  ---

  ## Paso 5: Clonar y Configurar el Proyecto

  ### 5.1 Clonar el Repositorio

  ```
  # Clonar el repositorio
  git clone https://project.comsatel.com.pe/comsatel/development/products/clocator2/apps/automatizacion/c-go_app.git
  # Entrar al directorio del proyecto
  cd cgo-android-automation
  o
  https://project.comsatel.com.pe/comsatel/development/products/clocator2/apps/automatizacion/c-go_app/-/tree/main
  ```

  ---

  ### 5.2 Configurar Archivos de Propiedades

  **capabilities.properties:**

  ```
  # Appium Server Configuration
  appium.server.host=127.0.0.1
  appium.server.port=4723
  
  # Device Configuration
  platformName=Android
  appium:automationName=UiAutomator2
  appium:appPackage=com.comsatel.cgo
  appium:appActivity=com.comsatel.MainActivity
  appium:noReset=false
  appium:autoGrantPermissions=true
  appium:newCommandTimeout=300
  
  ```

  **test-data.properties:**

  ```
  # Credenciales de prueba
  username=qacomsatel@gmail.com
  password=xxxxxx
  
  # Credenciales inválidas para pruebas negativas
  invalid.username=usuario_invalido@test.com
  invalid.password=password_incorrecto
  ```

  ---

  ### 5.3 Descargar Dependencias Maven

  ```
  # Ejecutar en consola
  # Descargar todas las dependencias
  mvn clean install
  
  # Solo descargar sin ejecutar tests
  mvn clean install -DskipTests
  ```

  ![image](uploads/f4ecae656309530e9e2d12d6693ce814/image.png)

  ---

  ## Paso 6: Verificación Final del Entorno

  ### 6.1 Script de Validación Completa

  Crear archivo `validate-environment.ps1 (tambien se puede copiar todas las líneas y pegarlas en PowerShell)`:

  ```
  Write-Host "========================================" -ForegroundColor Cyan
  Write-Host "VALIDACION DE ENTORNO - C-GO AUTOMATION" -ForegroundColor Cyan
  Write-Host "========================================" -ForegroundColor Cyan
  Write-Host ""
  
  # Validar Java
  Write-Host "Validando Java..." -ForegroundColor Yellow
  $javaVersion = java -version 2>&1 | Select-Object -First 1
  if ($javaVersion -match "25") {
      Write-Host "✓ Java 25 instalado correctamente" -ForegroundColor Green
  } else {
      Write-Host "✗ Java no está correctamente configurado" -ForegroundColor Red
  }
  Write-Host ""
  
  # Validar Maven
  Write-Host "Validando Maven..." -ForegroundColor Yellow
  $mavenVersion = mvn -version 2>&1 | Select-Object -First 1
  if ($mavenVersion -match "3.9") {
      Write-Host "✓ Maven 3.9.11 instalado correctamente" -ForegroundColor Green
  } else {
      Write-Host "✗ Maven no está correctamente configurado" -ForegroundColor Red
  }
  Write-Host ""
  
  # Validar Node.js
  Write-Host "Validando Node.js..." -ForegroundColor Yellow
  $nodeVersion = node -v
  Write-Host "✓ Node.js $nodeVersion instalado" -ForegroundColor Green
  Write-Host ""
  
  # Validar Appium
  Write-Host "Validando Appium..." -ForegroundColor Yellow
  $appiumVersion = appium -v
  Write-Host "✓ Appium $appiumVersion instalado" -ForegroundColor Green
  Write-Host ""
  
  # Validar ADB
  Write-Host "Validando Android SDK..." -ForegroundColor Yellow
  $adbVersion = adb --version 2>&1 | Select-Object -First 1
  Write-Host "✓ $adbVersion" -ForegroundColor Green
  Write-Host ""
  
  # Validar Variables de Entorno
  Write-Host "Validando Variables de Entorno..." -ForegroundColor Yellow
  Write-Host "JAVA_HOME: $env:JAVA_HOME" -ForegroundColor Green
  Write-Host "MAVEN_HOME: $env:MAVEN_HOME" -ForegroundColor Green
  Write-Host "ANDROID_HOME: $env:ANDROID_HOME" -ForegroundColor Green
  Write-Host ""
  
  # Validar Dispositivos
  Write-Host "Dispositivos Conectados:" -ForegroundColor Yellow
  adb devices -l
  Write-Host ""
  
  Write-Host "========================================" -ForegroundColor Cyan
  Write-Host "VALIDACION COMPLETADA" -ForegroundColor Cyan
  Write-Host "========================================" -ForegroundColor Cyan
  
  ```

  Ejecutar el script:

  ```
  .\validate-environment.ps1
  ```

  ![image](uploads/1653bf10a45b02a1c15a699e0fa1925c/image.png)

  ---

  ## Paso 7: Primeros Pasos con el Framework

  ### 7.1 Ejecutar un Test Simple

  ```
  # Ejecutar todos los tests
  mvn clean test
  
  # Ejecutar solo tests de login
  mvn clean test '-Dcucumber.filter.tags=@login'
  
  # Ejecutar todos los tests críticos
  mvn clean test '-Dcucumber.filter.tags=@smoke'
  
  # Ejecutar solo tests en desarrollo
  mvn clean test '-Dcucumber.filter.tags=@wip'
  
  # Ejecutar todos EXCEPTO los en desarrollo
  mvn clean test '-Dcucumber.filter.tags=not @wip'
  
  # Ejecutar todos los tests negativos
  mvn clean test '-Dcucumber.filter.tags=@negative'
  
  # Ejecutar todos los tests de validación
  mvn clean test '-Dcucumber.filter.tags=@validation'
  
  # Ejecutar con más detalles
  mvn clean test -X
  ```
  | Operador | Descripción | Ejemplo |
  |----------|-------------|---------|
  | `@tag1` | Ejecutar escenarios con tag1 | `@smoke` |
  | `@tag1 and @tag2` | Ejecutar escenarios con AMBOS tags | `@smoke and @login` |
  | `@tag1 or @tag2` | Ejecutar escenarios con CUALQUIERA de los tags | `@smoke or @negative` |
  | `not @tag1` | Ejecutar escenarios SIN el tag | `not @wip` |
  | `@tag1 and not @tag2` | Ejecutar con tag1 pero SIN tag2 | `@smoke and not @wip` |

  ## 7.2 Organización de Tags

  ```
  # Tipo de test
  @smoke          # Tests críticos
  @functional     # Tests funcionales
  @negative       # Tests negativos
  @validation     # Tests de validación
  
  # Área de la aplicación
  @login          # Tests de login
  @module         # Tests del módulo
  
  # Características
  @ui             # Tests de UI
  @google         # Tests con Google
  
  # Estado del test
  @wip            # Work In Progress
  @skip           # Saltar ejecución
  @flaky          # Tests inestables
  
  # Identificadores únicos
  @1, @2, @3...   # Números para identificar escenarios
  ```

  ---

  ### 7.3 Generar Reportes

  Después de ejecutar los tests, los reportes se generan automáticamente:

  ![image](uploads/c547afe72dce5b2fd3736e12b0a43674/image.png)

  **Reportes Cucumber:**

  ```
  target/cucumber-reports/
  ```

  **Reportes ExtentReports:**

  ```
  target/test-output/
  ```

  ---

  ## Paso 8: Troubleshooting

  ### Problema: "appium command not found"

  **Solución:**

  ```
  # Verificar instalación global
  npm list -g appium
  
  # Reinstalar si es necesario
  npm install -g appium@3.1.1
  ```

  ---

  ### Problema: "ANDROID_HOME not set"

  **Solución:**

  ```
  # Verificar variable
  $env:ANDROID_HOME
  
  # Si está vacía, establecer:
  [System.Environment]::SetEnvironmentVariable('ANDROID_HOME', 'C:\Users\USER\AppData\Local\Android\Sdk', 'Machine')
  ```

  ---

  ### Problema: "adb devices" no muestra dispositivos

  **Solución:**

  ```
  # Reiniciar el servidor ADB
  adb kill-server
  adb start-server
  
  # Verificar conexión
  adb devices
  
  # Para dispositivos físicos, habilitar depuración USB en el dispositivo
  # Configuración > Acerca del teléfono > Tocar 7 veces en "Número de compilación"
  # Configuración > Opciones de desarrollador > Depuración USB
  ```

  ---

  ### Problema: "Appium Server no inicia"

  **Solución:**

  ```
  # Verificar que el puerto 4723 no esté en uso
  netstat -ano | findstr :4723
  
  # Si está en uso, matar el proceso
  taskkill /PID <PID> /F
  
  # O usar un puerto diferente
  appium server --host 127.0.0.1 --port 4724
  ```

  ---

  ## Resumen de Comandos Importantes

  ```
  # Iniciar Appium Server
  appium --address 127.0.0.1 --port 4723
  
  # Validar entorno
  appium-doctor --android
  
  # Listar dispositivos
  adb devices -l
  
  # Ejecutar todos los tests
  mvn clean test
  
  # Ejecutar tests específicos
  mvn clean test -Dcucumber.filter.tags="@login"
  
  # Ejecutar con logs detallados
  mvn clean test -X
  
  # Limpiar proyecto
  mvn clean
  
  # Instalar dependencias
  mvn install
  
  # Compilar proyecto
  mvn compile
  ```

  ---

  ## Conclusión

  Con esta guía completamente configurada, tu entorno está listo para:
  1. Desarrollar nuevos tests automatizados
  2. Ejecutar pruebas en dispositivos físicos y emuladores
  3. Generar reportes detallados
  4. Integrar con CI/CD

  Para cualquier duda o problema adicional, consulta la documentación oficial:
  * Appium: [https://appium.io/docs/](https://appium.io/docs/)
  * Cucumber: [https://cucumber.io/docs/cucumber/](https://cucumber.io/docs/cucumber/)
  * Maven: [https://maven.apache.org/](https://maven.apache.org/)
  * Selenium: [https://www.selenium.dev/documentation/](https://www.selenium.dev/documentation/)