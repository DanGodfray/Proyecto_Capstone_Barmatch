# 8. Manual técnico académico — cierre MVP1

**Corte técnico:** 15/11/2026. **Revisión:** 25/11/2026 contra `develop` (`99056d0`). Se documenta lo que el equipo integró y validó; este repositorio no distribuye el aplicativo ni habilita acceso al entorno privado.

## 8.1 Componentes y responsabilidades

| Componente | Responsabilidad |
|---|---|
| Móvil Expo/React Native | Cliente y Personal: descubrimiento, reserva, QR y operación |
| Web React/Vite | Admin Local y Admin Platform, configuración, consultas y analítica |
| API NestJS | Autorización por perfil/local, MFA administrativo y reglas transaccionales |
| Motor y PostgreSQL | Consistencia, factibilidad física, idempotencia y eventos |
| Supabase Auth/Storage | Identidad y archivos del entorno de demostración |
| BI y DWH de pruebas | ETL, histórico sintético Parquet, modelo estrella y KPI |
| Servicio de Inteligencia | Python privado; ML-01/ML-04 BETA y ML-03 experimental, separado del flujo de reserva |

El monorepo privado conserva estas áreas y los contratos compartidos. No se copian implementaciones al repositorio académico.

## 8.2 Reproducibilidad verificada

El repositorio fija Node 24 y pnpm 12.4.2. Las versiones y dependencias Python pertenecen a cada componente; no se impone una versión genérica a BI y ML. El lockfile congelado, la compilación de contratos y las herramientas de pruebas por área forman parte del registro auditado.

La auditoría reporta contratos 25/25, API 284/284, web 866 PASS + 1 omitido, móvil 1.352 PASS + 1 pendiente y CI local 31/31. BI registra 299 PASS + 21 omitidos en Chile/UTC, además de sus laboratorios específicos. Los resultados corresponden a las puntas y entornos del acta, no a una ejecución nueva hecha por esta revisión documental.

## 8.3 Condiciones de operación de la demostración

El ensayo general utilizó API real y DWH de pruebas con datos sintéticos declarados; verificó 15 slices móviles y 8 web sin pantallas simuladas. El acceso administrativo requirió MFA y pertenencia al local. No basta interpretar una variable global `DATA_SOURCE`: el proyecto habilita adaptadores por slice y configuración de entorno.

Los adaptadores mock permanecen como recursos de desarrollo y QA; no sustituyen la evidencia de integración real. Jobs, correo, entregas y continuidad automática estaban apagados en el ensayo. Su implementación y validación en laboratorio no acreditan activación operacional.

## 8.4 Configuración y resguardo

Las plantillas y procedimientos vigentes permanecen en el repositorio privado. La configuración pública del cliente se distingue de las credenciales de servidor, que nunca deben exponerse en el bundle ni en este repositorio. No se publican valores, hosts privados, cadenas de conexión, tokens, material de modelos ni instrucciones de migración o activación.

Los roles del OLTP, BI y lectura KPI tienen responsabilidades separadas. El DWH utiliza una ubicación provisional y permisos específicos según C1 v1.6 y ADR-001. Esta síntesis no prescribe cambios a bases ni habilita accesos.

## 8.5 Docker y despliegue

Docker se utilizó en laboratorios desechables de PostgreSQL/Auth. La imagen web tiene una corrección registrada, pero el Compose integral sigue incompleto y no hay un Dockerfile de API que permita acreditar el despliegue de toda la solución. Se eliminan los comandos y plantillas provisionales del manual anterior; no eran instrucciones verificadas.

Ver [Docker y reproducibilidad](05_diseno/05_docker.md) y [evidencia complementaria](../05BarMatchIA_Docker/Despliegue_Docker.md). Hosting productivo, HTTPS definitivo, observabilidad y DWH productivo son evolución posterior.

## 8.6 Calidad, trazabilidad y cierre

| Aspecto | Evidencia y alcance |
|---|---|
| Integración | PR #159; API, app y web conectadas |
| BI real de pruebas | BI-03, migración 006 y PR #163; paridad ETL/DWH en 15/15 locales |
| Cierre técnico | PR #169 y auditoría del 15/11; ensayo 10/10 y barrido 19/19 |
| Mantenimiento documental BI | PR #175 y #176: fixture y enmienda del ADR ya integrados |
| Gestión | Jira conserva referencias históricas; esta revisión no inventa cierres, aprobaciones ni ceremonias |

Las limitaciones identificadas no invalidan el MVP1 funcional: la auditoría las clasifica como no bloqueantes o NO EJ. Antes de producción se requiere atender el endurecimiento y privacidad pendientes y completar las verificaciones físicas de Android/iOS. La galería editable, BI-06 incremental y BI-08 supresión se distinguen como evolución.

Fuentes privadas fijadas y enlaces académicos: [índice documental](../README.md#fuentes-y-alcance). Resultados y casos pendientes: [plan de pruebas](07_pruebas/README.md). No se ejecutaron pruebas del aplicativo ni cambios de infraestructura durante esta actualización de Markdown.
