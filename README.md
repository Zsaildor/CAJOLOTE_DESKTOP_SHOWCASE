# 🥑 Cajolote Desktop — Showcase del Proyecto

> **Sistema Integral de Punto de Venta (POS), Inventario, Control de Créditos, Analítica Comercial y Mantenimiento Inteligente para Windows con Arquitectura Offline-First, Sincronización Cloud y Actualizaciones Automáticas.**

---

## 📌 Tabla de Contenidos

1. [Visión General del Proyecto](#-visión-general-del-proyecto)
2. [Propuesta de Valor y Principios de Diseño](#-propuesta-de-valor-y-principios-de-diseño)
3. [Stack Tecnológico y Dependencias](#-stack-tecnológico-y-dependencias)
4. [Arquitectura de Software](#-arquitectura-de-software)
   - [Patrón Arquitectónico MVVM](#patrón-arquitectónico-mvvm)
   - [Diagrama de Arquitectura de Capas](#diagrama-de-arquitectura-de-capas)
   - [Flujo del Ciclo de Vida y Arranque](#flujo-del-ciclo-de-vida-y-arranque)
5. [Módulos y Catálogo Exhaustivo de Funcionalidades](#-módulos-y-catálogo-exhaustivo-de-funcionalidades)
   - [1. Autenticación y Control de Acceso](#1-autenticación-y-control-de-acceso-loginwindow)
   - [2. Ventana Maestra y Navegación](#2-ventana-maestra-y-navegación-mainwindow)
   - [3. Punto de Venta Ágil (POS)](#3-punto-de-venta-ágil-pos-posview)
   - [4. Consultador Rápido de Precios](#4-consultador-rápido-de-precios-revisarprecioview)
   - [5. Gestión de Catálogo e Inventario](#5-gestión-de-catálogo-e-inventario-productosview)
   - [6. Ventas Recientes y Modificaciones](#6-ventas-recientes-y-modificaciones-recientesview)
   - [7. Historial y Registros Consolidados](#7-historial-y-registros-consolidados-registrosview)
   - [8. Control de Créditos y Cuentas Fiadas (Notas)](#8-control-de-créditos-y-cuentas-fiadas-notas-notasview)
   - [9. Dashboards y Analítica Comercial](#9-dashboards-y-analítica-comercial)
   - [10. Identidad del Negocio y Recorte de Logotipo](#10-identidad-del-negocio-y-recorte-de-logotipo-perfilview)
   - [11. Configuración, Mantenimiento y Actualizaciones](#11-configuración-mantenimiento-y-actualizaciones-configuracionview)
6. [Integración con Hardware Periférico y Almacenamiento](#-integración-con-hardware-periférico-y-almacenamiento)
7. [Modelo y Diccionario de Datos](#-modelo-y-diccionario-de-datos)
   - [Diagrama Entidad-Relación (ER)](#diagrama-entidad-relación-er)
   - [Esquema de Tablas SQLite](#esquema-de-tablas-sqlite)
   - [Seguridad y Almacenamiento Cifrado (DPAPI)](#seguridad-y-almacenamiento-cifrado-dpapi)
8. [Estrategias de Rendimiento, Seguridad y Resiliencia](#-estrategias-de-rendimiento-seguridad-y-resiliencia)
9. [Estructura del Código Fuente](#-estructura-del-código-fuente)
10. [Requisitos del Sistema, Instalación, Empaquetado y Despliegue](#-requisitos-del-sistema-instalación-empaquetado-y-despliegue)

---

## 📖 Visión General del Proyecto

**Cajolote Desktop** es una aplicación de escritorio de alto rendimiento diseñada específicamente para satisfacer las necesidades operativas de pequeños y medianos comercios minoristas, abarrotes, cremerías y tiendas de conveniencia.

Desarrollada sobre la plataforma **.NET 10.0** y **Windows Presentation Foundation (WPF)**, la aplicación combina una estética visual moderna basada en **Material Design** con un motor de datos local transaccional **SQLite** operando bajo el paradigma **Offline-First**. Esto garantiza que el cajero pueda procesar cobros, consultar precios y gestionar transacciones a latencia prácticamente nula, con total independencia del estado de la conexión a internet. Además, integra servicios cloud en segundo plano con **Google Firebase** (Auth, Firestore y Storage), respaldos de base de datos comprimidos sin bloqueo y un sistema integral de **actualizaciones automáticas (In-App Auto-Update)** con verificación criptográfica SHA256.

```mermaid
mindmap
  root((Cajolote Desktop))
    Operación en Caja
      Punto de Venta POS Ultrarrápido
      Consultador de Precios
      Ventas a Granel / Peso
      Productos Rápidos sin Código
      Cobro Mixto y Abono Inmediato
      Calculadora de Efectivo y Cambio
      Guardias contra Doble Venta
    Gestión Comercial
      Inventario de Productos
      Categorías Jerárquicas
      Cuentas Fiadas y Abonos Notas
      Reimpresión y Ajuste de Ventas
    Analítica y Reportes
      Dashboard Diario en Tiempo Real
      Gráficos Interactivos SkiaSharp
      Estadísticas Corto y Largo Plazo
      Generación de Tickets y Reportes PDF
    Hardware e Infraestructura
      Lectores de Código Global Hook
      Impresoras Térmicas y Diálogo Selectivo
      Cifrado Local Windows DPAPI
      SQLite WAL + Sales Archiver
      Sincronización Cloud Firestore
      Respaldos VACUUM INTO + GZip en Firebase Storage
      Actualizador Automático In-App con Hash SHA256
      Almacenamiento Centralizado y Limpieza de PDFs
```

---

## 🎯 Propuesta de Valor y Principios de Diseño

* **Operatividad Ininterrumpida (100% Offline-First):** La base de datos local SQLite con registro de escritura por anticipado (`WAL mode`) permite que el comercio nunca detenga sus ventas ante fallas de red o caídas de internet.
* **Velocidad Extrema en el Mostrador:** Integración de un gancho de teclado a nivel de sistema operativo para capturar códigos de barras en milisegundos sin requerir que el cursor esté situado en un campo de texto específico.
* **Flexibilidad Comercial y Cobro Mixto:** Soporte nativo para artículos a granel (peso decimal en kilogramos o gramos), acceso rápido para artículos sin código y liquidación dividida en mostrador (pago de una porción en efectivo y envío del remanente a crédito en la misma operación).
* **Gestión de Confianza ("Fiado"):** Módulo integral de cuentas por cobrar para registrar clientes, asignarles compras fiadas, asentar cargos manuales y registrar abonos parciales con comprobantes impresos al momento.
* **Optimización Continua y Respaldos en la Nube:** Mantenimiento automático del tamaño y rendimiento de la base de datos local mediante archivado periódico de ventas históricas, aunado a respaldos mensuales automáticos en la nube mediante instantáneas seguras SQLite `VACUUM INTO`, compresión GZip y subida a Firebase Storage.
* **Ciclo de Vida de Software Moderno:** Sistema desatendido de actualización de versiones con manifiesto remoto, descarga en segundo plano con barra de progreso, verificación de integridad SHA256 y ejecutable instalador profesional generado con Inno Setup.

---

## 🛠️ Stack Tecnológico y Dependencias

| Capa / Componente | Tecnología | Versión | Rol en el Sistema |
| :--- | :--- | :--- | :--- |
| **Framework Base** | [.NET SDK](https://dotnet.microsoft.com/) | `10.0-windows` | Núcleo de ejecución, compilación C# y runtime nativo de alto rendimiento. |
| **Motor Gráfico (UI)** | [WPF](https://learn.microsoft.com/dotnet/desktop/wpf/) | Windows Desktop | Interfaz gráfica nativa con aceleración por hardware DirectX. |
| **Patrón MVVM** | `CommunityToolkit.Mvvm` | `8.4.2` | Implementación de `ObservableObject`, `RelayCommand` y `WeakReferenceMessenger`. |
| **Diseño Visual** | `MaterialDesignThemes` | `5.3.2` | Componentes visuales modernos, sombras elevadas, tipografía y paletas temáticas. |
| **Visualización de Datos** | `LiveChartsCore.SkiaSharpView.WPF` | `2.0.2` | Gráficas interactivas fluidas (líneas, pastel/donas y barras) renderizadas con SkiaSharp. |
| **Animaciones UI** | `LottieSharp` | `2.4.3` | Renderizado vectorial de animaciones fluidas (pantallas de espera, confirmaciones). |
| **Base de Datos Local** | [SQLite](https://www.sqlite.org/) | 3.x | Base de datos embebida transaccional, ultraligera y sin configuración de servidor. |
| **Mapeador ORM** | `Microsoft.EntityFrameworkCore.Sqlite` | `10.0.0` | Abstracción de acceso a datos, migraciones automáticas y consultas LINQ tipadas. |
| **Inyección de Dependencias** | `Microsoft.Extensions.DependencyInjection` | `10.0.8` | Contenedor de inversión de control (IoC) y ciclo de vida de servicios singleton/transient. |
| **Generación de Documentos** | [QuestPDF](https://www.questpdf.com/) | `2026.5.0` | Maquetación fluida y generación vectorial de comprobantes, tickets y reportes PDF. |
| **Gestión de Comprobantes** | `PdfStorageService` | Nativo | Organización centralizada en el escritorio (`Comprobantes Cajolote`), métricas y limpieza de disco. |
| **Comunicación Serial** | `System.IO.Ports` | `10.0.8` | Control directo de puertos COM físicos/virtuales para impresoras térmicas ESC/POS. |
| **Autenticación en la Nube** | `FirebaseAuthentication.net` | `4.1.0` | Autenticación de usuarios vía tokens JWT contra Firebase Auth con verificación de correo. |
| **Sincronización Cloud** | Cloud Firestore REST Client | REST API | Sincronización híbrida segura de transacciones, catálogo y perfil en segundo plano. |
| **Almacenamiento Cloud & Backups** | Firebase Storage REST API | REST API | Respaldos mensuales automáticos de SQLite (GZip) y alojamiento de manifiestos/instaladores de actualización. |
| **Compresión de Datos** | `System.IO.Compression.GZipStream` | Nativo .NET | Compresión óptima de bases de datos antes de transmitirse a la nube. |
| **Actualizaciones Automáticas** | `UpdateService` & `UpdateManifest` | Custom Engine | Detección remota de versiones, descarga asistida/silenciosa y validación SHA256. |
| **Empaquetado de Instalador** | [Inno Setup](https://jrsoftware.org/isinfo.php) | 6.x | Compilación de instaladores `.exe` de producción para Windows x64. |
| **Seguridad Local** | Windows DPAPI | Nativo Windows | Cifrado a nivel de usuario en disco para llaves de sesión y tokens de acceso. |

---

## 🏗️ Arquitectura de Software

### Patrón Arquitectónico MVVM

Cajolote implementa una estricta separación de responsabilidades a través del patrón **Model-View-ViewModel (MVVM)**, orquestado mediante el framework oficial **CommunityToolkit.Mvvm**:

* **Views (Vistas):** Archivos `.xaml` con mínimo code-behind, dedicados exclusivamente al binding de controles, plantillas visuales, plantillas modales y transiciones de interfaz.
* **ViewModels:** Clases que heredan de `ObservableObject`. Exponen propiedades reactivas `[ObservableProperty]`, encapsulan la lógica de presentación con `[RelayCommand]` y se comunican de forma desacoplada con el bus de eventos `WeakReferenceMessenger`.
* **Models (Dominio):** Entidades POCO que representan las tablas de SQLite y las estructuras de negocio (`Product`, `Sale`, `Note`, `UpdateManifest`, etc.).
* **Services (Lógica de Negocio):** Componentes registrados como Singletons en el contenedor de dependencias que resuelven tareas especializadas (impresión, escaneo, respaldo seguro, actualización in-app, almacenamiento de PDFs, cifrado y sincronización).

---

### Diagrama de Arquitectura de Capas

```mermaid
graph TD
    subgraph UI ["Capa de Presentación (WPF & XAML)"]
        LoginWin["LoginWindow.xaml"]
        MainWin["MainWindow.xaml"]
        UserViews["Vistas Modulares (POS, Productos, Notas, Reportes, Config)"]
        Converters["IValueConverters (Booleanos, Visibilidad, Progreso, Iconos)"]
    end

    subgraph MVVM ["Lógica de Presentación (MVVM Toolkit)"]
        VMs["ViewModels (PosVM, ProductosVM, NotasVM, ConfiguracionVM, etc.)"]
        Messenger["WeakReferenceMessenger (Bus de Mensajes Desacoplado)"]
    end

    subgraph BusinessLogic ["Servicios Centrales de Negocio (Singletons)"]
        AuthSvc["FirebaseAuthService"]
        SyncSvc["SyncService"]
        NetSvc["NetworkService"]
        CacheSvc["CatalogCacheService (In-Memory)"]
        BarcodeSvc["BarcodeScannerService (Global Hook)"]
        PrinterSvc["ThermalPrinterService (ESC/POS & PrintDialog)"]
        PDFSvc["Generadores QuestPDF (Ticket, Recibo, Ventas)"]
        PdfStorageSvc["PdfStorageService (Gestor & Limpieza Centralizada)"]
        BackupSvc["DatabaseBackupService (VACUUM INTO + GZip)"]
        UpdateSvc["UpdateService (Auto-Update Engine + SHA256)"]
        ArchiverSvc["SalesArchiver (Archivado Mensual)"]
        SpellSvc["SpellCheckHelper (Corrección Ortográfica Global)"]
        DPAPISvc["SecureStorageHelper (DPAPI)"]
    end

    subgraph DataLayer ["Capa de Persistencia Local & Almacenamiento"]
        DbContext["CajoloteDbContext (EF Core 10)"]
        Migrations["Migraciones EF Core"]
        Seeder["DbSeeder (Catálogo Inicial)"]
        SQLiteDB[("cajolote.db (SQLite - WAL Mode)")]
        SessionDat[("session.dat (DPAPI Cifrado)")]
        UserSettings[("UserSettings.json (Ajustes & Versión)")]
        DesktopFolder[("Desktop/Comprobantes Cajolote/ (Tickets y Deudas)")]
    end

    subgraph CloudHW ["Servicios Externos & Nube (Google Firebase)"]
        HardwareDevices["Hardware: Lector Códigos + Impresoras Térmicas"]
        FirebaseAuth["Firebase Auth (Tokens JWT & Verificación Correo)"]
        FirebaseFirestore["Cloud Firestore (Sync de Catálogo y Transacciones)"]
        FirebaseStorage["Firebase Storage (Respaldos .db.gz & version.json/Instaladores)"]
    end

    %% Conexiones
    LoginWin --> VMs
    MainWin --> UserViews
    UserViews --> VMs
    Converters --> UserViews
    VMs -.-> Messenger
    VMs --> BusinessLogic
    VMs --> DbContext

    BusinessLogic --> DbContext
    BusinessLogic --> HardwareDevices
    BusinessLogic --> DesktopFolder
    AuthSvc --> FirebaseAuth
    SyncSvc --> FirebaseFirestore
    BackupSvc --> FirebaseStorage
    UpdateSvc --> FirebaseStorage
    DPAPISvc --> SessionDat

    DbContext --> SQLiteDB
    CacheSvc -.-> DbContext
```

---

### Flujo del Ciclo de Vida y Arranque

Al inicializarse la aplicación ([`App.xaml.cs`](file:///c:/CAJOLOTE/Cajolote_desktop/App.xaml.cs)), se ejecuta una secuencia controlada de arranque que garantiza la integridad estructural y la seguridad de los datos:

```mermaid
sequenceDiagram
    autonumber
    actor User as Usuario / SO
    participant App as App.xaml.cs
    participant Env as EnvLoader
    participant DI as ServiceCollection
    participant DB as CajoloteDbContext & SQLite
    participant DPAPI as SecureStorageHelper
    participant Auth as FirebaseAuthService
    participant Cache as CatalogCacheService
    participant Main as MainWindow & Servicios Background

    User->>App: Ejecución de la aplicación
    App->>Env: Load(): Cargar variables de entorno (.env)
    App->>DI: ConfigureServices(): Registrar DbContext, ViewModels y Servicios
    App->>DB: Migración retrocompatible (shopsharp.db -> cajolote.db)
    App->>DB: Database.Migrate(): Crear/Actualizar esquema SQLite
    App->>DB: PRAGMA journal_mode=WAL: Habilitar modo WAL de alta velocidad
    App->>DB: DbSeeder.Seed(): Sembrar categorías/datos base si está vacía
    App->>DPAPI: RestoreSessionAsync(): Leer session.dat cifrado
    alt Sesión Válida y Token Vigente
        DPAPI-->>Auth: Sesión cargada (IsUserAuthenticated = true)
        App->>Cache: InitializeAsync() en segundo plano (Precalentamiento)
        App->>Main: Inicializar y mostrar MainWindow
        Note over Main: Tareas en segundo plano al arrancar:
        Main->>Main: 1. SyncStoreProfileAsync() con Firestore
        Main->>Main: 2. SalesArchiver.ArchiveOldSalesIfNeeded()
        Main->>Main: 3. DatabaseBackupService.CheckAndPerformMonthlyBackupAsync() (si >= 30 días)
        Main->>Main: 4. UpdateService.CheckForUpdatesAsync() (a los 5s de arrancar)
    else Sin Sesión o Token Vencido
        App->>Main: Inicializar y mostrar LoginWindow
    end
```

---

## 💻 Módulos y Catálogo Exhaustivo de Funcionalidades

### 1. Autenticación y Control de Acceso (`LoginWindow`)
* **Validación de Identidad Cloud:** Comprobación de credenciales (correo electrónico y contraseña) conectándose con el servicio de Firebase Authentication.
* **Registro de Nuevos Comercios:** Creación inmediata de nuevas cuentas de propietario en la nube desde la propia interfaz de escritorio.
* **Recuperación Automática de Contraseña:** Generación y despacho de correos de restablecimiento para usuarios con contraseñas olvidadas.
* **Persistencia Cifrada de Sesión:** Almacenamiento seguro del token de acceso JWT y datos de cuenta en disco local mediante la API nativa de protección de datos de Windows (DPAPI), evitando tener que iniciar sesión en cada apertura.

---

### 2. Ventana Maestra y Navegación (`MainWindow`)
* **Barra Lateral Reactiva:** Menú de navegación persistente con iconos claros y resaltado dinámico de la vista activa.
* **Escaneo Global de Código de Barras:** Intercepta ráfagas de entrada del escáner en cualquier parte de la ventana mediante un gancho global (`Attach(this)`), dirigiéndolas automáticamente al Punto de Venta o al Verificador de Precios según corresponda.
* **Monitor de Conectividad en Tiempo Real:** Detección de conectividad a internet (`NetworkService`) con alertas visuales y cambio cromático dinámico de indicadores de red.
* **Sincronización Automática en el Arranque:** Respalda la base de datos en segundo plano al iniciar y sincroniza el perfil comercial con Firestore.
* **Mantenimiento y Desfragmentación Silenciosa:** Invoca a `SalesArchiver` al inicio para mover ventas de meses previos a tablas históricas mientras muestra una pantalla de carga sutil con Lottie.
* **Verificación de Respaldo Mensual Desatendida:** Ejecuta en segundo plano la evaluación de los 30 días transcurridos para disparar el respaldo comprimido de base de datos a Firebase Storage sin interrumpir la operación.
* **Detección Desatendida de Actualizaciones:** Comprueba de forma pasiva a los 5 segundos del arranque si existe una nueva versión en Firebase Storage, notificando al usuario mediante un cuadro informativo no intrusivo.
* **Atajos de Teclado Globales:** Navegación acelerada mediante combinaciones de teclas:
  * `Alt + 1`: Dashboard
  * `Alt + 2`: Punto de Venta (POS)
  * `Alt + 3`: Revisar Precio
  * `Alt + 4`: Catálogo de Productos
  * `Alt + 5`: Ventas Recientes
  * `Alt + 6`: Notas y Créditos
  * `Alt + 7`: Registro Histórico
  * `Alt + 8`: Estadísticas
  * `Alt + 9`: Perfil de la Tienda
  * `Alt + 0`: Configuración

---

### 3. Punto de Venta Ágil (POS) (`PosView`)
* **Entrada de Productos Multimodal:**
  * **Lector de Códigos de Barras:** Agregado instantáneo de artículos al carrito.
  * **Acceso a Productos Rápidos:** Panel con cuadrícula personalizable de artículos sin código físico (ej. bolillo, tortilla, huevo, verduras), organizable con o sin divisiones de categoría.
* **Gestión de Artículos a Granel (Peso):** Detección automática de artículos marcados como `IsBulk`, abriendo un teclado numérico interactivo para digitar la cantidad exacta o decimal en kilogramos (`kg`) o gramos (`g`) según la preferencia del usuario.
* **Carrito de Ventas Reactivo:**
  * Modificación de cantidades unitarias mediante botones `+` y `-` o ingreso directo.
  * Edición rápida de precio unitario en el propio renglón de la venta para aplicar cambios de precio, descuentos o promociones en el acto.
  * Eliminación selectiva de renglones o vaciado integral de la orden.
* **Modal de Cobro en Efectivo con Calculadora de Cambio:**
  * Diálogo modal especializado con teclado numérico para registrar el efectivo entregado por el cliente.
  * Cálculo dinámico y visual del cambio exacto a devolver.
  * Indicador de dinero insuficiente con cálculo del importe faltante.
  * Botones de atajo rápido para montos comunes de billetes.
* **Modalidad de Cobro Mixto / Abono Inmediato:**
  * Opción de liquidar parte de la compra en efectivo y enviar el remanente a la libreta de cuentas fiadas (`Note`) en una única operación fluida.
  * Selector para vincular la deuda remanente a una cuenta existente o dar de alta al nuevo cliente deudor en el momento.
* **Guardias de Procesamiento Antidistorsión:**
  * Bloqueo contra doble clic accidental (`IsProcessingSale`) con animaciones de espera para evitar registrar ventas duplicadas o saturar el hilo UI.
  * Desprendimiento explícito de entidades en Entity Framework Core (`EntityState.Detached`) para prevenir colisiones de claves foráneas entre transacciones sucesivas.
* **Modal Post-Venta y Flujo de Impresión:**
  * Tras confirmar una venta, la app despliega un modal de confirmación con opciones para imprimir ticket térmico directo, abrir el selector de impresoras de Windows o finalizar sin comprobante físico.
  * Generación y almacenamiento automático de copia digital en PDF maquetada con QuestPDF y archivada centralizadamente mediante `PdfStorageService`.

---

### 4. Consultador Rápido de Precios (`RevisarPrecioView`)
* **Pantalla de Verificación Inmediata:** Interfaz de tipografía prominente pensada para uso ágil por parte del cajero o como terminal de consulta para el cliente.
* **Información Desplegada:** Muestra el precio actual, nombre completo, código de barras, categoría asociada e icono gráfico del producto escaneado.
* **Acceso Directo a Edición:** Botón de un solo clic para saltar al formulario de modificación del catálogo si se detecta un precio desactualizado.

---

### 5. Gestión de Catálogo e Inventario (`ProductosView`)
* **Búsqueda Avanzada y Paginación Virtual:** Filtrado rápido por nombre, código de barras, rango de precio o categoría, con límites configurables para evitar congelamientos de interfaz con catálogos masivos.
* **Administración Completa de Productos (CRUD):**
  * Alta y edición de nombre, código de barras, precio unitario.
  * Vinculación a categorías existentes.
  * Activación de bandera `IsQuickProduct` (aparición en mostrador rápido).
  * Activación de bandera `IsBulk` (producto vendido por peso decimal).
  * Asignador visual de iconos temáticos para identificación gráfica rápida.
  * Eliminación segura de productos con confirmación de diálogo.
* **Gestión Jerárquica de Categorías (CRUD):**
  * Creación y renombrado rápido de categorías.
  * Eliminación protegida: validación de integridad referencial que previene el borrado accidental de categorías que tengan productos asociados.
* **Corrección Ortográfica Integrada:** Asistencia en vivo mediante `SpellCheckHelper` que subraya y asiste ortográficamente los nombres y descripciones digitados.

---

### 6. Ventas Recientes y Modificaciones (`RecientesView`)
* **Auditoría de Ventas del Turno/Día:** Listado cronológico de las ventas efectuadas en la jornada actual, con detalles de estado de pago (liquidada o fiada) y bandera de sincronización.
* **Reimpresión de Comprobantes:** Capacidad de volver a imprimir tickets físicos o regenerar sus PDFs en cualquier momento.
* **Ajuste de Transacciones:** Herramienta para corregir renglones de una venta cerrada por error en mostrador, recalculando el total y dejando trazabilidad con la marca `IsEdited` y `EditedAt`.
* **Liquidación de Cuentas:** Marcado y cobro de ventas fiadas directamente desde la lista de ventas del día.

---

### 7. Historial y Registros Consolidados (`RegistrosView`)
* **Búsqueda Cronológica Extensa:** Filtros por rangos de fecha personalizados (desde un día particular hasta periodos bimestrales o anuales).
* **Unificación Transparente de Datos:** Consulta simultánea y sin fisuras tanto de la tabla de ventas activas (`Sales`) como del repositorio de ventas archivadas (`HistoricalSales`).
* **Generador de Informes de Ventas en PDF:** Despacho de reportes ejecutivos consolidados con sumatorias de ingresos, desglose de artículos vendidos y promedios operativos maquetados con QuestPDF.

---

### 8. Control de Créditos y Cuentas Fiadas ("Notas") (`NotasView`)
* **Libreta Digital de Cuentas por Cobrar:** Gestión de deudas de confianza organizadas por cliente.
* **Cuentas Activas y Saldadas:** Pestañas para distinguir deudores con saldo pendiente de clientes con notas liquidadas.
* **Desglose Exhaustivo de la Deuda:**
  * Relación de ventas cargadas a la nota con detalle de artículos consumidos.
  * Registro de cargos manuales (`ManualDebts`) para asentar préstamos directos de dinero en efectivo o ajustes de cuenta con su concepto y fecha.
* **Registro de Abonos y Cobro Mixto:** Entrada de pagos parciales o totales (originados en mostrador o en el panel de notas) que recalculan al instante el saldo restante de la nota.
* **Recibo Térmico y Digital de Abono:** Generación inmediata de recibos de liquidación/abono (`NoteReceiptGenerator`), imprimiendo en ticket térmico y guardando el PDF en la carpeta centralizada `Desktop/Comprobantes Cajolote/Liquidacion_Deudas/`.

---

### 9. Dashboards y Analítica Comercial
* **Tablero Ejecutivo Diario (`DashboardView`):**
  * **Tarjetas KPI:** Total de ventas acumuladas hoy ($), número de tickets cerrados, unidades de producto vendidas y monto promedio por transacción.
  * **Gráfica Horaria:** Curva de ingresos a lo largo de las horas del día para identificar las horas pico de tráfico.
  * **Distribución por Categorías:** Gráfica de pastel interactiva; al interactuar con una porción o su leyenda, el centro despliega en grande el nombre de la categoría, ingresos exactos y porcentaje relativo.
  * **Top de Artículos:** Gráfica de barras horizontales con los 5 productos con mayor demanda del día.
* **Estadísticas de Corto Plazo (`EstadisticasView`):** Análisis comparativo por días de la semana y semanas del mes.
* **Estadísticas Históricas y Macrotendencias (`EstadisticasTotalesView`):** Consolidación de ingresos acumulados a lo largo de meses y años, fusionando datos vigentes y archivados.

---

### 10. Identidad del Negocio y Recorte de Logotipo (`PerfilView`)
* **Personalización del Establecimiento:** Configuración de nombre comercial, RFC, domicilio y número de teléfono que aparece en los tickets y comprobantes.
* **Editor y Recortador Visual de Logotipo (`ImageCropWindow`):**
  * Ventana dedicada para cargar imágenes para el logo del negocio en formatos PNG o JPG (no se imprime en tickets físicos, se refleja en la barra principal).
  * Lienzo de recorte y escalado interactivo para ajustar el encuadre exacto del logotipo.
* **Indicador Dinámico de Versión:** Muestra de forma reactiva la versión del ejecutable en ejecución (`CurrentAppVersion`).

---

### 11. Configuración, Mantenimiento y Actualizaciones (`ConfiguracionView`)

Este módulo centraliza el control operativo, preferencias de usuario, almacenamiento y mantenimiento en la nube:

1. **Sincronización Cloud y Verificación de Identidad:**
   * **Monitoreo de Enlace Firebase:** Indicador reactivo del estado de sincronización y fecha/hora de la última renovación del token de sesión JWT.
   * **Verificación de Correo Electrónico:** Visualización del estatus del correo en Firebase Auth (`Correo verificado ✓` o `Correo sin verificar ⚠`), con acciones directas para **Revisar Estado** en línea o **Reenviar Correo** de verificación.
2. **Copia de Seguridad Automática a Firebase Storage:**
   * Respaldo mensual inteligente de la base de datos local SQLite.
   * Registro y muestra de la fecha y hora de la última copia de seguridad exitosa.
3. **Políticas de Privacidad y Protección de Datos:**
   * Acordeón visual desplegable con términos de confidencialidad, uso de datos restringido a diagnóstico interno del software, garantía de no cesión a terceros y transporte cifrado TLS/HTTPS.
4. **Preferencias Operativas del Mostrador:**
   * **Medición a Granel:** Alternar entre unidades en Kilogramos (`kg`) o Gramos (`g`) como valor base en caja (atajo `Ctrl + G`).
   * **Visualización de Tarjetas Rápidas:** Selector para mostrar productos sin código con o sin división visual por categorías.
5. **Corrección Ortográfica Avanzada (`SpellCheckHelper`):**
   * Interruptor global para activar o desactivar la revisión ortográfica de Windows en todos los campos de texto (`TextBox`).
   * Selector dinámico de idioma de corrección: Español (`es-MX`) o Inglés (`en-US`), con propagación reactiva instantánea mediante `WeakReferenceMessenger`.
6. **Preferencias de Impresión:**
   * **Modelo de Impresión:** Elegir entre enviar tickets directamente a una única impresora fija o abrir el selector de destino en cada venta (atajo `Ctrl + M`).
   * **Impresora Destino:** Detección automática de todas las colas de impresión conectadas en Windows (`LocalPrintServer`) y selección directa (atajo `Ctrl + P`).
7. **Almacenamiento Centralizado y Limpieza de Comprobantes PDF (`PdfStorageService`):**
   * **Métricas en Vivo:** Indicador en tiempo real del número total de tickets de venta y recibos de deuda almacenados, junto con el tamaño total consumido en disco (en B, KB o MB).
   * **Acceso Directo:** Botón **Abrir Carpeta** para desplegar en el Explorador de Windows el directorio centralizado de comprobantes.
   * **Motor de Limpieza:** Botón **Limpiar Comprobantes** que ejecuta la purga de archivos PDF en segundo plano con diálogo de confirmación, informando cuántos archivos se eliminaron y cuántos megabytes se liberaron.
8. **Centro de Actualizaciones de Software In-App (`UpdateService`):**
   * **Comprobación Remota:** Consulta manual o automática contra el archivo `version.json` en Firebase Storage.
   * **Detección de Nuevas Versiones:** Despliegue de notas de la versión (`ReleaseNotes`) y número de versión disponible.
   * **Descargador Integrado con Progreso:** Descarga del instalador ejecutable reportando el porcentaje en tiempo real.
   * **Validación Criptográfica:** Verificación del hash **SHA256** del instalador descargado para garantizar que no fue alterado durante la transferencia.
   * **Instalación Desatendida o Asistida:** Lanzamiento del instalador oficial con cierre ordenado y seguro de la aplicación.
   * **Seguimiento Persistente de Versión:** Registro automático en `UserSettings.json` para garantizar que la interfaz reporte la versión recién aplicada aun antes de un reinicio completo.

---

## 🔌 Integración con Hardware Periférico y Almacenamiento

### 1. Escáner de Código de Barras Global (`BarcodeScannerService`)
A diferencia de sistemas convencionales donde el cursor debe estar ubicado obligatoriamente en un `TextBox`, Cajolote implementa un gancho de eventos de teclado en la ventana principal (`PreviewTextInput` y temporizador de cadencia de caracteres).
* **Detección por Cadencia:** Identifica entradas que ocurren a una velocidad típica de un escáner láser/óptico (inferior a 35 milisegundos entre dígitos) finalizadas con un delimitador de retorno de carro (`Enter`).
* **Enrutamiento Inteligente:** Si la ventana activa es el Consultador de Precios, actualiza la tarjeta de consulta; si es el Punto de Venta, agrega el producto al carrito; si el usuario está en un campo de búsqueda común, respeta la digitación manual sin interferir.

### 2. Impresora Térmica de Recibos (`ThermalPrinterService` & `TicketGenerator`)
* **Protocolo ESC/POS Nativo:** Envío directo de bytes de control estándar mediante `System.IO.Ports.SerialPort`.
* **Comandos Implementados:**
  * Inicialización de cabezal (`ESC @`).
  * Alineación de texto: izquierda, centro y derecha (`ESC a n`).
  * Estilos tipográficos: negrita activada/desactivada (`ESC E n`), doble ancho y doble alto.
  * Avance de líneas (`LF` / `ESC d n`).
  * Comando de corte parcial o total de papel (`GS V 66 n`).
* **Modo Diálogo y Ticket Digital:** Selección de cola de impresión configurable mediante las API de impresión de Windows (`PrintDialog`) y respaldo en comprobante vectorial PDF mediante `TicketGenerator`.

### 3. Almacenamiento Centralizado de Documentos PDF (`PdfStorageService`)
Para evitar la dispersión de comprobantes en directorios temporales, el sistema agrupa todos los documentos en una ruta predecible y accesible en el Escritorio del usuario:
```text
%USERPROFILE%\Desktop\Comprobantes Cajolote\
├── Tickets_Venta\           # Tickets de venta individuales (Ticket_123.pdf)
└── Liquidacion_Deudas\      # Recibos de abono/liquidación de notas (ComprobanteDeuda_45.pdf)
```

---

## 🗄️ Modelo y Diccionario de Datos

### Diagrama Entidad-Relación (ER)

```mermaid
erDiagram
    Category ||--o{ Product : "contiene"
    Product ||--o{ SaleDetail : "se incluye en"
    Sale ||--|{ SaleDetail : "se compone de"
    Note ||--o{ Sale : "acumula"
    Note ||--o{ ManualDebt : "registra cargos"
    HistoricalSale ||--|{ HistoricalSaleDetail : "se compone de"
    Product ||--o{ HistoricalSaleDetail : "se incluye en"

    Category {
        int Id PK
        string Name
    }

    Product {
        int Id PK
        string Barcode UK
        string Name
        decimal Price
        int CategoryId FK
        bool IsQuickProduct
        bool IsBulk
        string IconKey
    }

    Sale {
        int Id PK
        datetime Date
        decimal Total
        bool IsSynced
        bool IsPaid
        int NoteId FK
        bool IsEdited
        datetime EditedAt
    }

    SaleDetail {
        int Id PK
        int SaleId FK
        int ProductId FK
        decimal Quantity
        decimal UnitPrice
        decimal Subtotal
    }

    HistoricalSale {
        int Id PK
        datetime Date
        decimal Total
        bool IsSynced
        bool IsPaid
        int NoteId
        bool IsEdited
        datetime EditedAt
        datetime ArchivedAt
    }

    HistoricalSaleDetail {
        int Id PK
        int HistoricalSaleId FK
        int ProductId FK
        decimal Quantity
        decimal UnitPrice
        decimal Subtotal
    }

    Note {
        int Id PK
        string CustomerName
        datetime CreatedAt
        bool IsPaid
        decimal Amount
        decimal PaidAmount
        datetime PaidAt
    }

    ManualDebt {
        int Id PK
        int NoteId FK
        string Concept
        decimal Amount
        datetime CreatedAt
    }

    StoreProfile {
        int Id PK
        string StoreName
        string Rfc
        string Address
        string Phone
        string CustomMessage
        string ImagePath
        datetime LastUpdated
        bool IsDirty
    }
```

---

### Esquema de Tablas SQLite

La base de datos se aloja en `%LocalAppData%\cajolote.db` y se gestiona íntegramente mediante **Entity Framework Core 10**:

1. **`Categories`**: Clasificación jerárquica del catálogo de productos.
2. **`Products`**: Catálogo de artículos disponibles para la venta. Dispone de un índice único sobre `Barcode` para búsquedas en tiempo $O(1)$.
3. **`Sales`**: Cabeceras de transacciones de venta activas (del mes corriente o deudas no liquidadas).
4. **`SaleDetails`**: Renglones detallados de productos vendidos por transacción con cantidades enteras o fraccionarias (`decimal(18,3)`).
5. **`HistoricalSales` & `HistoricalSaleDetails`**: Repositorio de ventas finalizadas de meses anteriores. Conservan los identificadores originales (`Id`) sin auto-incrementos que alteren la correlación.
6. **`Notes`**: Directorio de cuentas por cobrar o libretas de crédito por cliente, controlando el total acumulado (`Amount`) y los abonos realizados (`PaidAmount`).
7. **`ManualDebts`**: Ajustes de saldo y préstamos monetarios registrados manualmente sobre la cuenta de un cliente.
8. **`StoreProfiles`**: Datos de personalización comercial e imagen institucional del establecimiento.

---

### Seguridad y Almacenamiento Cifrado (DPAPI)

Para desacoplar las credenciales confidenciales de la base de datos SQLite y evitar fugas de información si el archivo `.db` es copiado:
* **Archivo:** `%LocalAppData%\session.dat`
* **Mecanismo:** `System.Security.Cryptography.ProtectedData` (Windows Data Protection API - DPAPI).
* **Alcance:** `DataProtectionScope.CurrentUser`. Los datos solo pueden ser descifrados por la misma cuenta de usuario de Windows en la misma máquina.
* **Datos Protegidos:** Tokens de autenticación JWT de Firebase, Refresh Tokens, UID de usuario y marcas de expiración.

---

## ⚡ Estrategias de Rendimiento, Seguridad y Resiliencia

1. **Modo WAL (Write-Ahead Logging):**
   Al arrancar, el sistema ejecuta `PRAGMA journal_mode=WAL;`. Esto permite lecturas y escrituras simultáneas en SQLite sin bloqueos de archivo, multiplicando la velocidad de registro de tickets.
2. **Caché en Memoria Desconectada (`CatalogCacheService`):**
   El catálogo de productos se precalienta de manera asíncrona al arrancar la aplicación utilizando consultas desconectadas (`AsNoTracking()`). Al realizar búsquedas o escanear en el POS, las consultas no tocan el disco, brindando una experiencia instantánea.
3. **Archivado Mensual Inteligente (`SalesArchiver`):**
   Para evitar que la tabla `Sales` crezca indefinidamente y ralentice los índices de búsqueda, un proceso en segundo plano transfiere las ventas pagadas de meses concluidos a `HistoricalSales`. Las consultas históricas combinan ambas tablas mediante `Concat` o `UnionAll` solo cuando el usuario solicita reportes analíticos globales.
4. **Respaldos Atómicos sin Bloqueo (`VACUUM INTO` + GZip):**
   Para respaldar la base de datos sin detener la operación de la tienda ni causar bloqueos de SQLite (`database is locked`), `DatabaseBackupService` ejecuta la instrucción nativa `VACUUM INTO '<temp_file>'`. Esto genera una instantánea limpia y consistente en disco que posteriormente se comprime al vuelo con `GZipStream` y se envía mediante HTTPS con Bearer Token a Firebase Storage.
5. **Prevención de Condiciones de Carrera en POS (`IsProcessingSale`):**
   Para prevenir transacciones duplicadas por doble pulsación en pantalla táctil o teclado, el motor de ventas implementa guardias de estado reactivas (`IsProcessingSale`), desacoplamiento forzado de entidades EF Core (`EntityState.Detached`) y temporizadores de transición visual.
6. **Consultas Asíncronas No Bloqueantes:**
   Toda interacción de entrada/salida (I/O) hacia SQLite, Firebase o puertos de impresión se ejecuta mediante llamadas asíncronas (`async/await`), asegurando que la interfaz gráfica permanezca fluida a 60 FPS sin congelamientos.

---

## 📂 Estructura del Código Fuente

```text
Cajolote_desktop/
├── Assets/                               # Recursos gráficos, logotipos y animaciones vectoriales
│   ├── Animations/                       # Archivos Lottie (waiting.json, Accepted.json)
│   └── Icons/                            # Definiciones de vectores e iconos XAML
├── Converters/                           # Convertidores IValueConverter para la vista WPF
│   ├── IconKeyToFriendlyNameConverter.cs # Nombres descriptivos para iconos
│   ├── InverseBooleanConverter.cs        # Inversión de banderas booleanas en bindings
│   ├── ProgressToBrushConverter.cs       # Color dinámico según porcentaje de descarga
│   ├── ResourceKeyToIconConverter.cs     # Mapeador de vectores de recursos
│   ├── StringNotEmptyToVisibilityConverter.cs # Visibilidad basada en cadenas de texto
│   └── ZeroToEmptyStringConverter.cs     # Formateo numérico limpio
├── Data/                                 # Persistencia y contexto de Entity Framework Core
│   ├── CajoloteDbContext.cs              # Mapeo de modelos, restricciones e índices SQLite
│   └── DbSeeder.cs                       # Poblado inicial de catálogo para primer arranque
├── documentation/                        # Documentación técnica interna
│   ├── ARCHITECTURE.md                   # Arquitectura técnica y patrones
│   ├── DICCIONARIO_DATOS.md              # Especificación de tablas, columnas y tipos
│   └── FUNCIONALIDADES.md               # Detalle minucioso de cada módulo
├── installer/                            # Empaquetado oficial con Inno Setup
│   ├── cajolote-installer.iss            # Script maestro de compilación de Inno Setup
│   ├── README.md                         # Guía paso a paso de empaquetado de producción
│   ├── publish/                          # Salida de dotnet publish (ignorado por Git)
│   └── output/                           # Instaladores .exe compilados (ignorado por Git)
├── Messages/                             # Mensajería desacoplada (WeakReferenceMessenger)
│   ├── ProfileSyncedMessage.cs           # Notificación de refresco de perfil comercial
│   ├── ShowModalMessage.cs               # Petición de cuadros de diálogo asíncronos
│   └── SpellCheckSettingsChangedMessage.cs # Difusión de cambios en corrección ortográfica
├── Migrations/                           # Migraciones de esquema autogeneradas por EF Core
├── Models/                               # Modelos de dominio y entidades de base de datos
│   ├── Category.cs                       # Categoría de agrupación
│   ├── HistoricalSale.cs                 # Venta archivada
│   ├── HistoricalSaleDetail.cs           # Renglón de venta archivada
│   ├── LocalSession.cs                   # Objeto de sesión cifrada
│   ├── ManualDebt.cs                     # Cargo manual a nota de cliente
│   ├── Note.cs                           # Libreta de crédito / cuenta fiada
│   ├── Product.cs                        # Catálogo de producto
│   ├── Sale.cs                           # Encabezado de venta activa
│   ├── SaleDetail.cs                     # Detalle de producto vendido
│   ├── StoreProfile.cs                   # Perfil comercial de la tienda
│   └── UpdateManifest.cs                 # Esquema del manifiesto de actualización remota
├── Repositories/                         # Abstracciones de persistencia
│   ├── EncryptedUserRepository.cs        # Repositorio de credenciales
│   └── IRepository.cs                    # Interfaz de repositorio genérico
├── Services/                             # Servicios de negocio especializados (Singletons)
│   ├── BarcodeScannerService.cs          # Gancho de captura global de código de barras
│   ├── CatalogCacheService.cs            # Caché en memoria del catálogo
│   ├── DatabaseBackupService.cs          # Respaldo automático a Firebase Storage (GZip)
│   ├── EnvLoader.cs                      # Parser de archivo .env
│   ├── FirebaseAuthService.cs            # Conexión con Firebase Auth y verificación
│   ├── ICatalogCacheService.cs           # Contrato de interfaz de caché
│   ├── NetworkService.cs                 # Monitor de conectividad a Internet
│   ├── NoteReceiptGenerator.cs           # Generación de comprobantes de abono PDF
│   ├── PdfStorageService.cs              # Gestor y limpiador de PDFs en el escritorio
│   ├── SalesArchiver.cs                  # Archivador mensual de ventas
│   ├── SalesReportGenerator.cs           # Generador de reportes consolidados PDF
│   ├── SecureStorageHelper.cs            # Cifrado con Windows DPAPI
│   ├── SettingsService.cs                # Ajustes persistentes (UserSettings.json)
│   ├── SpellCheckHelper.cs               # Corrector ortográfico reactivo global
│   ├── SyncService.cs                    # Sincronizador en la nube con Firestore
│   ├── ThermalPrinterService.cs          # Controlador serie ESC/POS para tickets
│   ├── TicketGenerator.cs                # Generador de tickets de compra PDF
│   └── UpdateService.cs                  # Motor de auto-actualización con hash SHA256
├── ViewModels/                           # Lógica de presentación reactiva (MVVM)
│   ├── ConfiguracionViewModel.cs
│   ├── DashboardViewModel.cs
│   ├── EstadisticasTotalesViewModel.cs
│   ├── EstadisticasViewModel.cs
│   ├── LoginViewModel.cs
│   ├── MainViewModel.cs
│   ├── NotasViewModel.cs
│   ├── PerfilViewModel.cs
│   ├── PosViewModel.cs
│   ├── ProductosViewModel.cs
│   ├── RecientesViewModel.cs
│   ├── RegistrosViewModel.cs
│   └── RevisarPrecioViewModel.cs
├── Views/                                # Interfaces gráficas XAML
│   ├── ConfiguracionView.xaml
│   ├── DashboardView.xaml
│   ├── EstadisticasTotalesView.xaml
│   ├── EstadisticasView.xaml
│   ├── ImageCropWindow.xaml              # Recortador de logotipo comercial
│   ├── NotasView.xaml
│   ├── PerfilView.xaml
│   ├── PosView.xaml                      # POS con calculadora y cobro mixto
│   ├── ProductosView.xaml
│   ├── RecientesView.xaml
│   ├── RegistrosView.xaml
│   └── RevisarPrecioView.xaml
├── AGENTS.md                             # Protocolo estandarizado de empaquetado para agentes
├── App.xaml / App.xaml.cs                # Punto de entrada, DI y ciclo de vida de la app
├── LoginWindow.xaml / .cs                # Ventana de autenticación
├── MainWindow.xaml / .cs                 # Ventana contenedora principal
├── Cajolote.csproj                       # Archivo de proyecto .NET 10 y dependencias NuGet
└── Cajolote.sln                          # Solución de Visual Studio
```

---

## 🚀 Requisitos del Sistema, Instalación, Empaquetado y Despliegue

### Requisitos de Hardware y Software

* **Sistema Operativo:** Windows 10 (versión 1809 o superior) o Windows 11 (64 bits).
* **Entorno de Ejecución:** [.NET 10.0 Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/10.0) o superior (en modo Framework-Dependent).
* **Procesador:** CPU x64 de 1.6 GHz o superior (compatible con arquitectura moderna).
* **Memoria RAM:** Mínimo 2 GB (Recomendado: 4 GB o más).
* **Almacenamiento:** 150 MB de espacio libre para la aplicación y base de datos local SQLite.
* **Periféricos Opcionales:**
  * Lector de código de barras USB (emulación de teclado / Keyboard Wedge).
  * Impresora térmica de tickets de 58 mm o 80 mm conectada por puerto serie COM o emulador USB-a-Serie / cola de impresión de Windows.

---

### Pasos para Compilación y Ejecución en Desarrollo

1. **Clonar el repositorio:**
   ```powershell
   git clone <URL_DEL_REPOSITORIO>
   cd Cajolote_desktop
   ```

2. **Configuración de Entorno (Opcional):**
   Si se desea enlazar un proyecto propio de Firebase para la sincronización en la nube y respaldos, crear un archivo `.env` en la raíz con las credenciales requeridas:
   ```env
   FIREBASE_API_KEY=tu_api_key
   FIREBASE_AUTH_DOMAIN=tu_proyecto.firebaseapp.com
   FIREBASE_STORAGE_BUCKET=tu_proyecto.firebasestorage.app
   ```
   *Nota: Si no se configuran, la aplicación arrancará con las variables locales predeterminadas.*

3. **Restaurar dependencias y compilar:**
   ```powershell
   dotnet restore
   dotnet build -c Release
   ```

4. **Ejecutar la aplicación:**
   ```powershell
   dotnet run --project Cajolote.csproj
   ```

---

### Flujo Estandarizado de Empaquetado para Producción (Inno Setup)

Conforme a las directrices de [`AGENTS.md`](file:///c:/CAJOLOTE/Cajolote_desktop/AGENTS.md) y [`installer/README.md`](file:///c:/CAJOLOTE/Cajolote_desktop/installer/README.md), el empaquetado oficial del instalador `.exe` se realiza mediante **Inno Setup 6+**:

1. **Verificar y sincronizar versiones:**
   Asegurar que la versión deseada (ej. `1.0.6`) coincida tanto en [`Cajolote.csproj`](file:///c:/CAJOLOTE/Cajolote_desktop/Cajolote.csproj) (`<Version>X.Y.Z</Version>`) como en [`installer/cajolote-installer.iss`](file:///c:/CAJOLOTE/Cajolote_desktop/installer/cajolote-installer.iss) (`#define MyAppVersion "X.Y.Z"`).

2. **Generar la publicación binaria Release:**
   ```powershell
   dotnet publish Cajolote.csproj -c Release -r win-x64 --self-contained false -o installer/publish
   ```

3. **Compilar el instalador con Inno Setup:**
   ```powershell
   & "C:\Program Files (x86)\Inno Setup 6\ISCC.exe" installer\cajolote-installer.iss
   ```

4. **Resultado del instalador generado:**
   El archivo final autónomo se ubicará en:
   ```text
   installer/output/Cajolote_Setup_vX.Y.Z.exe
   ```
   *Las carpetas `installer/publish` e `installer/output` se mantienen excluidas del control de versiones mediante `.gitignore`.*

---

<p align="center">
  <b>Cajolote Desktop</b> — Diseñado con ❤️ para empoderar al comercio local y de proximidad.
</p>
