# DEVLOG — SCCP-DTEX

Este documento resume problemas y decisiones que marcaron la construcción del proyecto.

## 1. De interfaz a sistema

El proyecto empezó con una interfaz de estilo centro de comando. Con el crecimiento aparecieron necesidades que no podían resolverse solamente desde la UI: identidad, roles, persistencia, sincronización y trazabilidad.

**Decisión:** usar Supabase como backend compartido y mantener Flutter como superficie principal.

## 2. Separación de superficies móviles

La operación de campo y la supervisión no tienen las mismas responsabilidades.

**Decisión:** mantener dos entrypoints Android (`main_custodio.dart` y `main_dtex_supervisor.dart`) para representar superficies distintas sin convertirlas en una sola aplicación monolítica.

## 3. Autenticación y autorización

El login no debía equivaler automáticamente a acceso administrativo.

**Decisión:** combinar Supabase Auth con un registro `allowed_admins`, roles y una segunda verificación mediante PIN para las funciones de director.

## 4. Seguimiento GPS

El tracking introdujo problemas diferentes a los de una aplicación convencional: permisos de segundo plano, precisión, saltos de posición, velocidad, batería, conectividad y continuidad de la sesión.

**Decisión:** encapsular la lógica en `DtexAndroidTrackingService`, con reportes periódicos y reglas explícitas para aceptar puntos, detectar desviaciones y generar alertas.

## 5. Configuración pública

Una dependencia local que apuntaba a un paquete `sccp_shared` inexistente impedía que el repositorio fuera autocontenido.

**Solución:** eliminar esa dependencia del proyecto público y conservar las constantes necesarias localmente.

También existía una referencia a `env.dart` que no estaba presente.

**Solución:** incorporar una configuración mediante `String.fromEnvironment` para que las credenciales se proporcionen durante la compilación y no formen parte del repositorio.

## 6. Deuda técnica

El proyecto conserva código y decisiones de distintas etapas. Esto no se trata como un defecto que deba ocultarse: forma parte del objeto de estudio.

La limpieza pública se hace con tres criterios:

- código que representa funcionalidad real;
- código heredado que necesita revisión;
- referencias o claims que ya no corresponden al estado real.

## 7. Estado de esta etapa

El repositorio se presenta como **desarrollo/testing** y **prototipo funcional/laboratorio**. No se afirma que sea un sistema certificado o listo para producción.
