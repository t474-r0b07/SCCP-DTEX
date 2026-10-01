# SCCP-DTEX — Guía de instalación

## Estado del repositorio

Este repositorio se publica como **prototipo funcional / laboratorio en desarrollo y testing**. La configuración de backend y las políticas de seguridad dependen del proyecto Supabase utilizado para la instalación.

## Requisitos

- Flutter SDK compatible con Dart `>=3.0.0 <4.0.0`.
- Android SDK para las superficies móviles.
- Una instancia de Supabase para backend, autenticación y datos.
- Chrome para ejecutar la superficie web durante desarrollo.

Comprueba el entorno con:

```bash
flutter doctor
flutter --version
```

## 1. Obtener el proyecto

```bash
git clone https://github.com/t474-r0b07/SCCP-DTEX.git
cd SCCP-DTEX
flutter pub get
```

## 2. Configurar Supabase

El proyecto recibe las credenciales mediante variables de compilación. No las escribas directamente en el código fuente.

### Web

```bash
flutter run -d chrome \
  --dart-define=SUPABASE_URL=https://TU-PROYECTO.supabase.co \
  --dart-define=SUPABASE_ANON_KEY=TU-CLAVE-ANON
```

### Android

Aplica las mismas variables `--dart-define` al comando de ejecución o compilación correspondiente.

## 3. Ejecutar la superficie web

```bash
flutter run -d chrome \
  --dart-define=SUPABASE_URL=https://TU-PROYECTO.supabase.co \
  --dart-define=SUPABASE_ANON_KEY=TU-CLAVE-ANON
```

La entrada principal es `lib/main.dart`.

## 4. Ejecutar DTEX Custodio

```bash
flutter run \
  --flavor dtex_custodio \
  --target lib/main_custodio.dart \
  --dart-define=SUPABASE_URL=https://TU-PROYECTO.supabase.co \
  --dart-define=SUPABASE_ANON_KEY=TU-CLAVE-ANON
```

Esta superficie utiliza funciones de campo como ubicación, telemetría, alertas y notificaciones. Android puede solicitar permisos adicionales durante la ejecución.

## 5. Ejecutar DTEX Supervisor

```bash
flutter run \
  --flavor dtex_supervisor \
  --target lib/main_dtex_supervisor.dart \
  --dart-define=SUPABASE_URL=https://TU-PROYECTO.supabase.co \
  --dart-define=SUPABASE_ANON_KEY=TU-CLAVE-ANON
```

## 6. Builds

### Web

```bash
flutter build web --release \
  --dart-define=SUPABASE_URL=https://TU-PROYECTO.supabase.co \
  --dart-define=SUPABASE_ANON_KEY=TU-CLAVE-ANON
```

### Android Custodio

```bash
flutter build apk \
  --flavor dtex_custodio \
  --target lib/main_custodio.dart \
  --dart-define=SUPABASE_URL=https://TU-PROYECTO.supabase.co \
  --dart-define=SUPABASE_ANON_KEY=TU-CLAVE-ANON
```

### Android Supervisor

```bash
flutter build apk \
  --flavor dtex_supervisor \
  --target lib/main_dtex_supervisor.dart \
  --dart-define=SUPABASE_URL=https://TU-PROYECTO.supabase.co \
  --dart-define=SUPABASE_ANON_KEY=TU-CLAVE-ANON
```

> Los builds de release todavía utilizan la configuración de firma indicada en el proyecto. Esto no debe interpretarse como configuración de distribución de producción.

## 7. Backend esperado

El código actual utiliza Supabase para autenticación y acceso a datos. Entre las entidades y vistas referenciadas por la aplicación se encuentran:

- `allowed_admins`
- `login_logs`
- entidades DTEX de misiones, destinos, extensiones y tracking
- `radio_mensajes`
- `radio_llamadas`
- `inconsistencias`
- `partes_sorpresa`
- entidades de oficiales/custodios
- vistas operativas de inconsistencias, alertas y telemetría

La definición exacta del esquema y sus políticas debe mantenerse en el proyecto Supabase que acompañe a cada instalación. **No se deben copiar esquemas antiguos de esta guía como si fueran el contrato actual del backend.**

## 8. Permisos Android

La aplicación declara permisos relacionados con ubicación precisa y en segundo plano, cámara, servicio foreground, notificaciones, conectividad, wakelock, optimización de batería y overlay del sistema.

Concede únicamente los permisos necesarios para la superficie que estés probando y revisa el comportamiento de Android en el dispositivo real.

## 9. Problemas frecuentes

### Supabase no inicializa

Comprueba que `SUPABASE_URL` y `SUPABASE_ANON_KEY` hayan sido proporcionados con `--dart-define`.

### Login rechazado

Comprueba la autenticación de Supabase y la configuración de `allowed_admins`. El acceso administrativo no depende únicamente de que exista una sesión autenticada.

### El tracking no funciona

Revisa permisos de ubicación, estado del GPS, restricciones de batería y permisos de ejecución en segundo plano.

### El mapa no muestra datos

Comprueba conectividad, credenciales de Supabase y disponibilidad de los datos que consume la vista.

## 10. Antes de considerar una instalación de producción

Este repositorio no declara una configuración de producción certificada. Antes de cualquier despliegue real deben revisarse, como mínimo:

- políticas RLS efectivas;
- permisos y roles de Supabase;
- almacenamiento y exposición de archivos;
- gestión de secretos;
- firma Android;
- permisos de segundo plano;
- pruebas automatizadas;
- recuperación ante errores de red;
- logging y observabilidad;
- comportamiento en dispositivos Android reales.

## Referencias

- `documentation/PROJECT_MEMORY.md` — evolución técnica.
- `documentation/DEVLOG.md` — problemas y decisiones.
- `documentation/CHANGELOG_PUBLIC.md` — estado público.
- `MANUAL_USUARIO.md` — recorrido funcional.

**Versión declarada:** 2.0.0
