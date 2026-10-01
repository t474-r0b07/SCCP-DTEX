# CHANGELOG PÚBLICO — SCCP-DTEX

## 2.0.0 — Estado público actual

### Arquitectura

- Web Command Center en Flutter.
- Superficies Android separadas para Custodio y Supervisor.
- Supabase/PostgreSQL como backend.
- Realtime para sincronización de información.

### Seguridad

- Supabase Auth.
- Allowlist de administradores.
- Roles de supervisor/director.
- Verificación adicional mediante PIN para funciones de director.
- Registro de actividad de autenticación.

### Operación

- Gestión de misiones DTEX.
- OTP asociado a misiones.
- Tracking GPS.
- Telemetría de batería y conectividad.
- Alertas operativas.
- Notificaciones locales y servicio de seguimiento en foreground.
- Lógica para precisión GPS, saltos de coordenadas, velocidad y desviaciones.

### Limpieza de repositorio

- Eliminada la dependencia local inexistente `sccp_shared`.
- Añadida configuración pública mediante `--dart-define`.
- Unificada la versión declarada en `2.0.0`.
- README actualizado para reflejar desarrollo/testing en lugar de estados `LIVE` no verificables.

## Antes de 2.0.0

El proyecto evolucionó como prototipo y acumuló decisiones de diferentes etapas. Parte de esa deuda permanece deliberadamente identificada para futuras iteraciones.

## Próximas revisiones

- Verificación completa de políticas RLS frente a cada operación del cliente.
- Auditoría de permisos Android y requisitos de ejecución en segundo plano.
- Eliminación de código legado que no tenga valor histórico o funcional.
- Pruebas automatizadas de los flujos críticos.
- Revisión de arquitectura y separación de responsabilidades.
