# Docker y despliegue — documentación académica

**Propósito:** explicar la estrategia de despliegue sin publicar código fuente, Dockerfiles operacionales, variables sensibles ni credenciales.

## Alcance

Docker respalda laboratorios y pruebas de integración reproducibles; el Compose general permanece como artefacto incompleto. PostgreSQL OLTP, autenticación y almacenamiento de imágenes se gestionan mediante Supabase; no se presentan como bases de producción alojadas dentro del mismo Compose.

| Servicio | Tratamiento |
|---|---|
| API NestJS | API validada con laboratorios PostgreSQL/Auth; Dockerfile propio no acreditado, Compose integral incompleto |
| Web React/Vite | Imagen multi-etapa corregida el 12/11 y prueba local registrada; Compose pendiente de alineación y hosting real posterior |
| BI/ETL y ML | Procesos Python separados de la transacción operacional |
| DWH de pruebas | PostgreSQL separado para el análisis y la comprobación de consultas |
| Mobile Expo | Aplicación del dispositivo; no se entrega como contenedor de producción |
| Supabase OLTP/Auth/Storage | Servicios externos gestionados |

## Evidencias de validación

La auditoría del 15/11 documenta laboratorios con PostgreSQL desechable y Supabase Auth en Docker, y una CI local de 31 pasos, incluyendo SQL del motor, autenticación, BI e integración. Los resultados acreditan reproducibilidad técnica en entorno de pruebas, **no un despliegue comercial público**.

## Configuración y resguardo

Los servicios utilizan variables de entorno con permisos diferenciados. Los secretos, conexiones privadas y certificados no se publican aquí. El repositorio académico documenta la solución; no entrega acceso ni instrucciones para ejecutar el código privado.

## Limitaciones

El Compose general aún conserva configuración incompatible para web y referencia a un Dockerfile de API inexistente. No se debe afirmar que la solución completa arranca con Compose. Hosting productivo, HTTPS definitivo y gestión de secretos son trabajo posterior; no invalidan los laboratorios ni el MVP1 integrado.

**Trazabilidad:** [diseño Docker](../00BarMatchIA_MarcoAgil/05_diseno/05_docker.md), [estado privado de Docker y auditoría](../README.md#fuentes-y-alcance).

El [diseño Docker actualizado](../00BarMatchIA_MarcoAgil/05_diseno/05_docker.md) sustituye las plantillas provisionales por el estado efectivamente documentado. [Fuente privada y auditoría](../README.md#fuentes-y-alcance).
