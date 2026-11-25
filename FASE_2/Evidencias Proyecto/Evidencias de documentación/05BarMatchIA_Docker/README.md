# Docker y despliegue — documentación académica

**Propósito:** explicar la estrategia de despliegue sin publicar código fuente, Dockerfiles operacionales, variables sensibles ni credenciales.

## Alcance

La solución emplea Docker y Docker Compose para la reproducibilidad de servicios propios y laboratorios de integración. PostgreSQL OLTP, autenticación y almacenamiento de imágenes se gestionan mediante Supabase; no se presentan como bases de producción alojadas dentro del mismo Compose.

| Servicio | Tratamiento |
|---|---|
| API NestJS | Servicio contenerizable y ensayado en laboratorios de integración |
| Web React/Vite | Aplicación compilable y servible como recursos estáticos |
| BI/ETL y ML | Procesos Python separados de la transacción operacional |
| DWH de pruebas | PostgreSQL separado para el análisis y la comprobación de consultas |
| Mobile Expo | Aplicación del dispositivo; no se entrega como contenedor de producción |
| Supabase OLTP/Auth/Storage | Servicios externos gestionados |

## Evidencias de validación

La auditoría del 15/11 documenta laboratorios con PostgreSQL desechable y Supabase Auth en Docker, y una CI local de 31 pasos, incluyendo SQL del motor, autenticación, BI e integración. Los resultados acreditan reproducibilidad técnica en entorno de pruebas, **no un despliegue comercial público**.

## Configuración y resguardo

Los servicios utilizan variables de entorno con permisos diferenciados. Los secretos, conexiones privadas y certificados no se publican aquí. El repositorio académico documenta la solución; no entrega acceso ni instrucciones para ejecutar el código privado.

## Limitaciones

Quedan como trabajo posterior el hosting productivo, las cabeceras y HTTPS definitivos, los secretos del entorno de destino y la certificación del despliegue público. Los Dockerfiles y Compose auténticos permanecen en el repositorio privado del equipo.

**Trazabilidad:** `00BarMatchIA_MarcoAgil/05_diseno/05_docker.md`, PR #169 y `docs/pruebas/auditoria-mvp1-2026-11-15.md` del repositorio privado.
