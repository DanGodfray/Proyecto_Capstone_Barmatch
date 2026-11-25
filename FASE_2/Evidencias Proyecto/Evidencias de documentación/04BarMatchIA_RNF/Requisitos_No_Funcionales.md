# Requisitos no funcionales — estado al cierre del MVP1

**Corte:** 25/11/2026. Los requisitos originales están descritos en `00BarMatchIA_MarcoAgil/05_diseno/04_requisitos_no_funcionales.md`. Esta síntesis distingue evidencia ejecutada de mejoras pendientes.

| Dimensión | Requisito y evidencia | Estado de cierre |
|---|---|---|
| Seguridad de acceso | Autenticación con Supabase Auth, verificación de JWT y permisos por perfil/local; MFA en administración. Auditoría: 90 rutas, 285 intentos de acceso indebido, sin autorización incorrecta. | Validado en pruebas |
| Integridad | Reserva transaccional e idempotencia. En pruebas con 50, 100 y 200 solicitudes simultáneas sobre el último cupo hubo una sola confirmación, sin sobreventa. | Validado en laboratorio |
| Rendimiento | Medición de carga en PC de desarrollo: aproximadamente 1.000 solicitudes/s sostenidas; degradación según criterio de la auditoría a 700 usuarios virtuales. | Medido en laboratorio; sin SLA productivo |
| Calidad | Auditoría de API, frontend, base de datos, BI y seguridad; CI local final 31/31 pasos y API 284/284 pruebas. | Validado para el corte auditado |
| Protección de datos | Datos de demostración ficticios/sintéticos; separación OLTP/DWH, seudonimización y minimización; políticas y evaluaciones pendientes para producción. | Parcial; requiere cierre preproductivo |
| Disponibilidad | Manejo de errores y mensajes de desconexión; servicios desacoplados; no existe certificación de disponibilidad comercial. | Validación parcial |
| Portabilidad | Monorepo con configuración por entorno y Docker para servicios propios; no se incluye código ni configuración sensible en el repositorio académico. | Diseño y validaciones locales |
| Accesibilidad | Correcciones de contraste, lectura de interfaz y revisión de web con herramientas automatizadas. | Validación web; pendientes en dispositivo físico |
| Escalabilidad | Bloqueo transaccional por establecimiento, relectura y verificación de factibilidad física; pruebas de concurrencia e idempotencia. | Validado en laboratorio; sin capacidad de producción comprometida |

## Riesgos y pendientes explícitos

La auditoría identifica, entre otros, rate limiting, cabeceras HTTP de la API, revocación efectiva de sesiones, registro de auditoría, sanitización EXIF, alertas de dependencias, decisiones de retención, revisión de DPIA y pruebas físicas de Android/iOS. No son requisitos que puedan presentarse como certificados en producción.

**Trazabilidad:** [RNF del marco ágil](../00BarMatchIA_MarcoAgil/05_diseno/04_requisitos_no_funcionales.md) y [auditoría y fuentes de cierre](../README.md#fuentes-y-alcance).

## Interpretación académica del resultado

La integración, consistencia y autorización probadas constituyen logros del MVP1. Los hallazgos de endurecimiento son no bloqueantes según la auditoría; las metas productivas, retención y despliegue requieren su propia aceptación posterior. La ubicación provisional del DWH no invalida el BI de pruebas.

El catálogo detallado y su correspondencia con evidencia están en [RNF del marco ágil](../00BarMatchIA_MarcoAgil/05_diseno/04_requisitos_no_funcionales.md). La enmienda de ADR-001 y el fixture KPI ya se integraron en #176/#175; no se mantienen como tareas documentales abiertas. [Fuentes del contraste](../README.md#fuentes-y-alcance).
