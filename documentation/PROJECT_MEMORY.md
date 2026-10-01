# PROJECT MEMORY — SCCP-DTEX

## Propósito

SCCP-DTEX nació como un experimento para construir una superficie de coordinación operativa con Flutter y Supabase. El proyecto fue creciendo por capas: primero una interfaz web, después módulos de autenticación y operación, y finalmente superficies Android diferenciadas para custodio y supervisor.

## Evolución técnica

- **Etapa inicial:** interfaz web con estética de centro de comando y módulos de consulta/operación.
- **Integración backend:** Supabase/PostgreSQL y Realtime como fuente común de datos.
- **Seguridad:** autenticación, allowlist de administradores, roles y verificación adicional mediante PIN para funciones de director.
- **Expansión móvil:** separación de las superficies Android para custodio y supervisor.
- **Operación de campo:** seguimiento GPS, telemetría, alertas, batería, conectividad y notificaciones.
- **Estado actual:** prototipo funcional en desarrollo/testing, con deuda técnica heredada de las distintas etapas.

## Arquitectura actual

```
Flutter Web
   │
   ├── Command Center
   ├── autenticación / roles
   └── módulos operativos
          │
          ▼
      Supabase
   PostgreSQL + Realtime
          ▲
          │
   ┌──────┴────────┐
   │               │
Custodio       Supervisor
 Android          Android
   │
   └── GPS + telemetría + alertas
```

## Decisiones que se conservan

El repositorio conserva decisiones de varias etapas porque forman parte de la historia técnica del proyecto. No todas representan la arquitectura que se elegiría hoy. La documentación pública distingue entre funcionalidad existente y deuda técnica en lugar de presentar el prototipo como producto terminado.

## Lección principal

La experiencia más valiosa de SCCP-DTEX no fue solamente conseguir que las pantallas funcionaran. Fue descubrir cómo cambian las decisiones cuando un prototipo empieza a necesitar autenticación, datos compartidos, tiempo real, permisos móviles, seguimiento de ubicación y recuperación ante condiciones reales de operación.
