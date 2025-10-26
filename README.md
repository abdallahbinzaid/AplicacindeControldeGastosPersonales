# 💰 Control de Gastos Personales.

Aplicación Android nativa para administrar tus finanzas personales de forma fácil y segura.

![Android](https://img.shields.io/badge/Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![Kotlin](https://img.shields.io/badge/Kotlin-0095D5?style=for-the-badge&logo=kotlin&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)

## 📱 Características

### ✅ Funcionalidades Principales

- **Autenticación Segura**
  - 📧 Registro e inicio de sesión con email y contraseña
  - 🔐 Integración con Google Sign-In
  - 🛡️ Protección de datos con Firebase Authentication

- **Gestión de Gastos (CRUD Completo)**
  - ➕ Agregar nuevos gastos con nombre, categoría, monto y notas
  - ✏️ Editar gastos existentes
  - 🗑️ Eliminar gastos con confirmación
  - 👁️ Visualizar historial completo

- **Análisis Financiero**
  - 💵 Cálculo automático del total mensual
  - 📊 Estadísticas por categoría con porcentajes
  - 🔍 Filtrado por categoría
  - 📅 Navegación entre meses (anterior/siguiente)

- **Categorías Predefinidas**
  - 🍔 Alimentación
  - 🚗 Transporte
  - 🎮 Entretenimiento
  - 🏥 Salud
  - 📚 Educación
  - 💡 Servicios
  - 🏠 Hogar
  - 👕 Ropa
  - 💻 Tecnología
  - 📦 Otros

- **Experiencia de Usuario**
  - 🎨 Diseño moderno con Material Design 3
  - ⚡ Actualizaciones en tiempo real
  - 📱 Interfaz intuitiva y responsive
  - 🌙 Soporte para tema del sistema
  - 💾 Cache local para uso offline

## 🛠️ Tecnologías Utilizadas

### Stack Principal
- **Lenguaje:** Kotlin 2.1.0
- **UI:** Jetpack Compose + Material 3
- **Arquitectura:** MVVM (Model-View-ViewModel)
- **Backend:** Firebase (Authentication + Firestore)
- **Asíncronia:** Kotlin Coroutines + Flow

### Bibliotecas y Dependencias

```kotlin
// Android Core
androidx.core:core-ktx:1.15.0
androidx.lifecycle:lifecycle-runtime-ktx:2.8.7
androidx.activity:activity-compose:1.9.3

// Compose
androidx.compose.bom:2024.12.01
androidx.compose.ui
androidx.compose.material3

// Firebase
firebase-bom:33.7.0
firebase-auth-ktx
firebase-firestore-ktx

// Google Sign-In
play-services-auth:21.2.0

// Coroutines
kotlinx-coroutines-play-services:1.9.0
```

## 📋 Requisitos

- **Android Studio:** Hedgehog (2023.1.1) o superior
- **SDK Mínimo:** API 24 (Android 7.0)
- **SDK Objetivo:** API 36 (Android 14+)
- **JDK:** Java 11
- **Gradle:** 8.7.3
- **Cuenta de Firebase** (gratuita)

## 🚀 Instalación y Configuración

### 1. Clonar el Repositorio

```bash
git clone https://github.com/tu-usuario/control-gastos.git
cd control-gastos
```

### 2. Configurar Firebase

#### 2.1 Crear Proyecto en Firebase

1. Ve a [Firebase Console](https://console.firebase.google.com/)
2. Haz clic en "Agregar proyecto"
3. Sigue los pasos para crear tu proyecto

#### 2.2 Agregar App Android

1. En la consola de Firebase, selecciona tu proyecto
2. Haz clic en el ícono de Android
3. **Package name:** `com.example.controlgastos`
4. **App nickname:** Control Gastos (opcional)
5. Descarga el archivo `google-services.json`
6. Coloca `google-services.json` en `app/google-services.json`

#### 2.3 Habilitar Authentication

1. En Firebase Console, ve a **Authentication** > **Sign-in method**
2. Habilita **Email/Password**
3. Habilita **Google**

#### 2.4 Configurar Google Sign-In

1. Abre la terminal en Android Studio
2. Ejecuta para obtener el SHA-1:

```bash
./gradlew signingReport
```

O con keytool:

```bash
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

3. Copia el SHA-1 que aparece
4. En Firebase Console: **Project Settings** > **Your apps** > **Add fingerprint**
5. Pega el SHA-1

#### 2.5 Obtener Web Client ID

1. En Firebase Console: **Authentication** > **Sign-in method** > **Google**
2. Copia el **Web client ID**
3. Pégalo en `app/src/main/res/values/strings.xml`:

```xml
<string name="default_web_client_id">TU_WEB_CLIENT_ID.apps.googleusercontent.com</string>
```

#### 2.6 Configurar Firestore

1. En Firebase Console, ve a **Firestore Database**
2. Haz clic en **Create database**
3. Selecciona **Start in production mode**
4. Elige la región más cercana
5. Ve a la pestaña **Rules** y pega:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
      
      match /gastos/{gastoId} {
        allow read, write: if request.auth != null && request.auth.uid == userId;
        
        allow create: if request.auth != null 
                      && request.auth.uid == userId
                      && request.resource.data.keys().hasAll(['nombre', 'categoria', 'monto', 'fecha', 'nota', 'createdAt'])
                      && request.resource.data.nombre is string
                      && request.resource.data.categoria is string
                      && request.resource.data.monto is number
                      && request.resource.data.fecha is timestamp
                      && request.resource.data.nota is string
                      && request.resource.data.createdAt is timestamp;
      }
    }
  }
}
```

6. Haz clic en **Publish**

#### 2.7 Crear Índices (Opcional pero recomendado)

1. Ve a **Firestore Database** > **Indexes** > **Composite**
2. Haz clic en **Create index**
3. Configura:
   - **Collection ID:** `gastos`
   - **Fields to index:**
     - `fecha` - Descending
   - **Query scope:** Collection

### 3. Compilar y Ejecutar

```bash
# Sincronizar proyecto
./gradlew build

# Ejecutar en emulador o dispositivo
# Presiona el botón Run en Android Studio o usa:
./gradlew installDebug
```

## 📁 Estructura del Proyecto

```
app/
├── src/main/
│   ├── java/com/example/controlgastos/
│   │   ├── MainActivity.kt           # Activity principal con UI
│   │   ├── ExpensesViewModel.kt      # Lógica de negocio y estados
│   │   └── ui/theme/                 # Tema de la aplicación
│   ├── res/
│   │   ├── values/
│   │   │   └── strings.xml           # Strings y Web Client ID
│   │   └── ...
│   └── AndroidManifest.xml           # Configuración de la app
├── build.gradle.kts                   # Dependencias del módulo
└── google-services.json              # Configuración de Firebase
```

## 🎯 Uso de la Aplicación

### Primera Vez

1. **Crear Cuenta:**
   - Abre la aplicación
   - Ingresa email y contraseña
   - Haz clic en "Crear cuenta"
   
   O usa "Continuar con Google"

2. **Agregar tu Primer Gasto:**
   - Haz clic en el botón flotante "Agregar Gasto"
   - Completa el formulario
   - Presiona "Agregar"

### Gestión Diaria

- **Ver gastos del mes actual:** Aparecen automáticamente al iniciar
- **Cambiar de mes:** Usa las flechas ← → en el selector de mes
- **Filtrar por categoría:** Haz clic en los chips de categoría
- **Ver estadísticas:** Haz clic en el ícono de información (ℹ️)
- **Editar un gasto:** Toca el ícono de lápiz en el gasto
- **Eliminar un gasto:** Toca el ícono de basura y confirma

## ⚙️ Configuración Avanzada

### Optimizar Rendimiento

En `gradle.properties`:

```properties
org.gradle.caching=true
org.gradle.daemon=true
org.gradle.jvmargs=-Xmx4096m -XX:MaxMetaspaceSize=512m
org.gradle.parallel=true
kotlin.incremental=true
android.enableR8.fullMode=true
```

### Build Release

Para generar una APK optimizada:

```bash
./gradlew assembleRelease
```

La APK estará en: `app/build/outputs/apk/release/`

## 🐛 Solución de Problemas

### Error: "Default FirebaseApp is not initialized"

**Solución:** Verifica que `google-services.json` esté en `app/google-services.json`

### Error: "Unable to resolve dependency for ':app@debug/compileClasspath'"

**Solución:** 
```bash
./gradlew --refresh-dependencies
File > Invalidate Caches / Restart
```

### Google Sign-In no funciona

**Solución:** 
1. Verifica que el SHA-1 esté agregado en Firebase
2. Confirma que el Web Client ID sea correcto en `strings.xml`
3. Asegúrate de que Google Sign-In esté habilitado en Firebase Console

### La app es muy lenta

**Solución:**
1. Prueba en modo Release, no Debug
2. Usa un dispositivo físico en lugar del emulador
3. Verifica tu conexión a internet
4. Revisa que los índices de Firestore estén creados

## 📊 Base de Datos (Firestore)

### Estructura

```
users/
└── {userId}/
    └── gastos/
        └── {gastoId}/
            ├── nombre: String
            ├── categoria: String
            ├── monto: Number
            ├── fecha: Timestamp
            ├── nota: String
            └── createdAt: Timestamp
```

### Consultas Optimizadas

- Cache local habilitado
- Límite de 50 gastos por consulta
- Índice compuesto en campo `fecha`

## 🔐 Seguridad

- ✅ Autenticación obligatoria para todos los endpoints
- ✅ Reglas de
