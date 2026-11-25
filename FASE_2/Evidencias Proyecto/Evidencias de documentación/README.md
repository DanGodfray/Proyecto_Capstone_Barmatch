# BarMatchIA — Evidencias de documentación

**Ruta:** `FASE_2/Evidencias Proyecto/Evidencias de documentación/`. **Cierre técnico MVP1:** 15/11/2026. **Revisión documental:** 25/11/2026.

Este índice conecta el marco ágil existente con las evidencias técnicas de cierre. La integración funcional, el Motor, los flujos operacionales y BI alcanzaron validación académica en laboratorio y demostración. Los documentos históricos conservan su corte; sus estados no se confunden con los del cierre.

## Fuentes y alcance

Contraste con `develop` del repositorio privado, consultado el 25/11/2026 en `99056d0e0dd1e364257385e72e92b017e46dbb31`. Las fuentes requieren acceso del equipo; se citan sin publicar código, credenciales ni configuración sensible.

| Fuente privada | Qué respalda |
|---|---|
| [Auditoría MVP1 del 15/11](https://github.com/barmatchia-team/barmatchia-mvp/blob/99056d0e0dd1e364257385e72e92b017e46dbb31/docs/pruebas/auditoria-mvp1-2026-11-15.md) | Entornos, resultados, correcciones, FAIL no bloqueantes y NO EJ |
| [Historial de integración](https://github.com/barmatchia-team/barmatchia-mvp/commits/99056d0e0dd1e364257385e72e92b017e46dbb31/) | #159 integración; #163 KPI; #169 cierre; #175 fixture; #176 ADR del DWH |
| [Estado de Docker](https://github.com/barmatchia-team/barmatchia-mvp/blob/99056d0e0dd1e364257385e72e92b017e46dbb31/docs/pruebas/docker-estado.md) | Imagen web y límites del Compose integral |
| [Contrato DWH](https://github.com/barmatchia-team/barmatchia-mvp/blob/99056d0e0dd1e364257385e72e92b017e46dbb31/apps/bi/dwh/CONTRATO_DWH.md) y [ADR-001](https://github.com/barmatchia-team/barmatchia-mvp/blob/99056d0e0dd1e364257385e72e92b017e46dbb31/apps/bi/docs/adr/ADR-001-ubicacion-dwh.md) | Ubicación provisional, privacidad y permisos KPI |
| [Acta BI-03](https://github.com/barmatchia-team/barmatchia-mvp/blob/99056d0e0dd1e364257385e72e92b017e46dbb31/docs/pruebas/acta-ventana-bi03-2026-11-13.md) y [acta 006](https://github.com/barmatchia-team/barmatchia-mvp/blob/99056d0e0dd1e364257385e72e92b017e46dbb31/docs/pruebas/acta-instalacion-006-dwh-2026-11-14.md) | Histórico Parquet sintético y paridad ETL/DWH |
| [ML-01/ML-04](https://github.com/barmatchia-team/barmatchia-mvp/blob/99056d0e0dd1e364257385e72e92b017e46dbb31/docs/pruebas/e10-pronostico-prospectivo-2026-11-12.md) y [ML-03](https://github.com/barmatchia-team/barmatchia-mvp/blob/99056d0e0dd1e364257385e72e92b017e46dbb31/docs/pruebas/e10-ml03-experimental.md) | Horizonte y alcance experimental de Inteligencia |
| [Integración del Motor](https://github.com/barmatchia-team/barmatchia-mvp/blob/99056d0e0dd1e364257385e72e92b017e46dbb31/docs/pruebas/motor-integracion-develop.md) y [ADR del Motor](https://github.com/barmatchia-team/barmatchia-mvp/blob/99056d0e0dd1e364257385e72e92b017e46dbb31/docs/arquitectura/adr-motor-reservas.md) | Evolución de controles transaccionales y despliegue; distinguir fechas de cada acta |

La Matriz, RN 21.11 y briefs son fuentes de requisitos; los contratos y actas verifican implementación y resultados. Un PR integrado no prueba automáticamente el cierre administrativo de Jira. No se certifican estados actuales del tablero ni ceremonias no respaldadas.

## Alcance cumplido y validado

| Área | Logro del MVP1 |
|---|---|
| Integración | App Expo/React Native, web React/Vite y API NestJS; 15 slices móviles y 8 web reales en ensayo sin mocks |
| Motor | PostgreSQL como fuente de verdad, factibilidad física, bloqueo transaccional e idempotencia; cero sobreventas en laboratorio |
| Operación | Reserva, cancelación, check-in, asignación, atención y walk-in integrados |
| Identidad | Cuatro perfiles, autorización por local y MFA administrativo probado |
| BI | ETL, histórico Parquet y DWH de pruebas, seis KPI/seis gráficos y paridad 15/15 locales |
| QA | CI local 31/31, ensayo general 10/10 y barrido final 19/19, con alcance y entornos identificados |

Raúl Guerra: Backend/PO. Josefa Morales: BI/Scrum Master. Daniel Godfray: Frontend/QA/integración. Las evidencias registran el trabajo desarrollado; no se reconstruyen reuniones ni asignaciones nuevas.

## Marco ágil existente

| Artefacto | Archivo |
|---|---|
| Visión y alcance | [Product Vision](00BarMatchIA_MarcoAgil/01_product_vision.md) |
| Avance técnico y referencias Jira históricas | [Product Backlog](00BarMatchIA_MarcoAgil/02_product_backlog.md) |
| Planificación y evolución verificable | [Sprint Backlogs](00BarMatchIA_MarcoAgil/03_sprint_backlog/README.md) |
| Criterios de aceptación | [Definition of Done](00BarMatchIA_MarcoAgil/04_definition_of_done.md) |
| Arquitectura, datos, UML, RNF y Docker | [Diseño](00BarMatchIA_MarcoAgil/05_diseno/README.md) |
| Registros históricos conservados | [Retrospectivas](00BarMatchIA_MarcoAgil/06_retrospectivas/README.md) |
| Estrategia y resultados | [Pruebas](00BarMatchIA_MarcoAgil/07_pruebas/README.md) |
| Operación y alcance documentado | [Manual técnico](00BarMatchIA_MarcoAgil/08_manual_tecnico.md) |
| Propuesta de valor | [Innovación](00BarMatchIA_MarcoAgil/09_innovacion.md) |
| Relación con entregables Word | [Mapa de informes](00BarMatchIA_MarcoAgil/10_mapa_informes_apt.md) |

## Evidencias complementarias

| Carpeta | Evidencia |
|---|---|
| 01 Arquitectura | [Síntesis](01BarMatchIA_Arquitectura/Arquitectura_BarMatchIA.md) y [diagrama de presentación anterior](01BarMatchIA_Arquitectura/ArquitecturaBarMatchIA_MVP.png), sin certificación de arquitectura final |
| 02 Modelo de datos | [ER PDF del 19/10](02BarMatchIA_ModeloDeDatos/BarMatchIA_OLTP_ER_20261019.pdf) y [ER interactivo](02BarMatchIA_ModeloDeDatos/BarMatchIA_OLTP_ER_Interactivo20261019.html), modelos históricos conservados |
| 03 Mockups | [Mockup HTML histórico](03BarMatchIA_Mockups/) |
| 04 RNF | [Requisitos no funcionales](04BarMatchIA_RNF/Requisitos_No_Funcionales.md) |
| 05 Docker | [Despliegue y reproducibilidad](05BarMatchIA_Docker/Despliegue_Docker.md) |
| 06 Pruebas | [Plan y evidencias](06BarMatchIA_Pruebas/Plan_y_Evidencias_de_Pruebas.md) |
| 07 Innovación | [Propuesta de valor y alcance](07BarMatchIA_Innovacion/Innovacion_BarMatchIA.md) |
| 08 Reglas de negocio | [RN, actores y briefs](08BarMatchIA_ReglasDeNegocio/) |

Se conservan los [videos del 18/10 y 15/11](../Evidencias%20de%20sistema%20aplicacion/) y la [presentación existente de Fase 2](../Presentacion%20Proyecto.pptx). Los [diarios](../../Evidencias%20Individuales/) y [Word 2.4/2.6 y planillas](../../Evidencias%20Grupales/) están en las categorías oficiales.

## Limitaciones y evolución, según impacto

- **Limitaciones no bloqueantes del MVP1:** hallazgos de endurecimiento, permisos/minimización y dependencias, además de casos físicos NO EJ. Se conservan en el [plan de pruebas](06BarMatchIA_Pruebas/Plan_y_Evidencias_de_Pruebas.md), sin presentarlos como fallos del incremento funcional ni como PASS.
- **Capacidades experimentales:** ML-01/ML-04 BETA y ML-03 informativo, sin inferencia individual validada. La comprobación manual «Mañana» del checklist auditado no se cierra con otra prueba de servicio.
- **Evolución y producción:** galería editable, BI-06 incremental, BI-08 supresión, hosting y DWH productivo; requieren decisiones y validaciones propias, no son condiciones pendientes para reconocer el MVP1 demostrado.

**Estados:** implementado = integrado; validado = resultado con entorno; pendiente = falta evidencia o decisión específica; MVP2 = evolución posterior. Las metas del plan original no son SLA productivos. Las retrospectivas y referencias Jira conservan su valor histórico sin atribuirles resultados posteriores.
