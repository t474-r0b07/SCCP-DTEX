# MANUAL DE USUARIO — SCCP-DTEX

## 1. Alcance

SCCP-DTEX reúne una superficie web de comando y dos superficies Android especializadas:

- **Web Command Center** — supervisión y operación desde navegador.
- **DTEX Custodio** — operación de campo.
- **DTEX Supervisor** — supervisión móvil y coordinación.

El repositorio está en **desarrollo/testing**. Este manual describe la funcionalidad representada actualmente por el código; no constituye una especificación de despliegue operativo.

## 2. Acceso y roles

La aplicación utiliza Supabase Auth y una capa adicional de autorización para usuarios administrativos.

El controlador de autenticación contempla:

- restauración de sesión;
- inicio y cierre de sesión;
- validación contra la allowlist de administradores;
- roles de supervisor y director;
- verificación adicional mediante PIN para funciones de director;
- registro de eventos de autenticación.

Los permisos efectivos dependen de la configuración del backend.

## 3. Command Center Web

La entrada web es `lib/main.dart`.

La navegación incluye superficies para dashboard, inconsistencias, partes, oficiales y funciones DTEX.

La interfaz utiliza mapas, indicadores, tarjetas operativas y componentes de actualización de datos.

## 4. DTEX Custodio

La entrada Android es `lib/main_custodio.dart`.

La superficie está orientada a operaciones de campo y contiene componentes relacionados con:

- misiones;
- ubicación;
- seguimiento GPS;
- radio operativa;
- alertas;
- reportes;
- telemetría;
- cámara;
- notificaciones.

El servicio `DtexAndroidTrackingService` gestiona el ciclo de seguimiento y aplica reglas para precisión, saltos de posición, velocidad y estado de la ubicación.

## 5. DTEX Supervisor

La entrada Android es `lib/main_dtex_supervisor.dart`.

Esta superficie está orientada a supervisión y coordinación. Comparte backend con la superficie de custodio, pero mantiene una interfaz y un punto de entrada independientes.

## 6. Misiones

Las operaciones DTEX trabajan alrededor de misiones y destinos.

Dependiendo del flujo, el sistema puede:

1. crear o consultar una misión;
2. asociar un custodio;
3. generar o validar un mecanismo OTP;
4. iniciar seguimiento;
5. recibir posiciones;
6. generar alertas ante determinadas condiciones;
7. actualizar el estado de la misión;
8. cerrar la operación.

La disponibilidad exacta de cada acción depende del rol y del estado de la misión.

## 7. Seguimiento GPS

El seguimiento considera, entre otros factores:

- precisión reportada;
- saltos de coordenadas;
- velocidad;
- desviación respecto al destino;
- estado del servicio de ubicación;
- batería;
- conectividad.

El comportamiento real también depende de Android, permisos concedidos y condiciones del dispositivo.

## 8. Alertas y telemetría

El sistema contempla alertas operativas y datos de telemetría asociados al seguimiento.

Una alerta GPS representa una condición detectada por las reglas implementadas; no debe interpretarse automáticamente como prueba de manipulación.

## 9. Radio

La aplicación utiliza entidades de radio para comunicación entre superficies.

La implementación contempla mensajes, canales, llamadas y componentes WebRTC/señalización presentes en el proyecto.

La disponibilidad de comunicación depende de la configuración del backend y de la conectividad.

## 10. Cámara y reportes

La superficie móvil declara permisos de cámara y contiene flujos de captura asociados a reportes.

En un dispositivo real, Android puede solicitar permisos antes de permitir la captura.

## 11. Solución de problemas

### No hay sesión

Comprueba las credenciales de Supabase y el estado del usuario administrativo.

### No aparece una misión

Comprueba conectividad, sesión, rol y datos disponibles en Supabase.

### No se actualiza la ubicación

Comprueba GPS activo, permisos de ubicación, permiso de segundo plano cuando corresponda, restricciones de batería y conectividad.

### No llegan notificaciones

Comprueba el permiso de notificaciones y el estado del servicio Android.

### Una operación devuelve un error de Supabase

Revisa primero la respuesta del backend y las políticas de acceso configuradas. El cliente no puede compensar una política RLS incorrecta.

## 12. Estado del manual

Este documento sustituye al manual histórico de SCCP Command Center v1.0. El código actual conserva elementos de aquella etapa, pero la arquitectura pública ahora incluye las superficies DTEX y los servicios móviles correspondientes.

Para evolución técnica, consultar:

- `documentation/PROJECT_MEMORY.md`
- `documentation/DEVLOG.md`
- `documentation/CHANGELOG_PUBLIC.md`
