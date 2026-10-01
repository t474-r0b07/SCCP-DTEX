# Security Policy

## Estado del proyecto

SCCP-DTEX es un proyecto público en **desarrollo/testing**. El repositorio no debe interpretarse como evidencia de que un despliegue operativo cumple por sí solo todos los requisitos de seguridad.

## Reportar una vulnerabilidad

Si encuentras una vulnerabilidad que pueda comprometer autenticación, autorización, exposición de datos, secretos, ubicación, almacenamiento o ejecución remota, evita publicar los detalles técnicos en un issue público.

Incluye, cuando sea posible:

- archivo o componente afectado;
- condición necesaria para reproducirlo;
- impacto observable;
- pasos mínimos de reproducción;
- evidencia que no exponga información sensible.

## No publicar

No incluyas en un reporte:

- contraseñas;
- claves de Supabase;
- tokens;
- credenciales de servicios;
- datos personales;
- coordenadas o información operacional real;
- archivos de configuración privados.

## Alcance

Son relevantes para este proyecto los problemas relacionados con dependencias, autenticación, autorización, almacenamiento, exposición accidental de secretos y validación de datos.

Las configuraciones específicas del proyecto Supabase utilizado para una instalación pueden introducir riesgos que no estén presentes en el código público y deben auditarse por separado.

## Correcciones

Las correcciones de seguridad se incorporarán al historial público cuando hacerlo no exponga información sensible ni facilite la explotación de una vulnerabilidad aún abierta.
