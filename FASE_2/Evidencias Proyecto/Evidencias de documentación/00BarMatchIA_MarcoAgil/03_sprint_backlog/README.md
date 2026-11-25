# 3. Sprint Backlog — índice

> **Responsable del artefacto:** Josefa Morales (Scrum Master). Borrador preparado por Daniel Godfray a partir de Jira (`KAN`) al 17/10/2026. ⚠️ **Validar Scrum Master.**

## Calendario de sprints (plan histórico)

| Sprint | Fechas | Objetivo (Sprint Goal) | Archivo |
|---|---|---|---|
| 0 | 07/09 – 28/09 | Definición funcional, reglas de negocio, modelo OLTP y presentación EA1 | [sprint-00](sprint-00_definicion.md) |
| 1 | 29/09 – 05/10 | Demo técnica: ciclo central de reserva de punta a punta sobre contratos | [sprint-01](sprint-01_demo-tecnica.md) |
| 2 | 06/10 – 11/10 | Operación del local: asignación, atención, walk-in, no-show, estados nuevos | [sprint-02](sprint-02_operacion-local.md) |
| 3 | 12/10 – 16/10 | Cierre de evidencias para el informe de avance; alineación de contratos con la API real | [sprint-03](sprint-03_informe-avance.md) |
| 4 | 19/10 – 25/10 | Lista de espera + correo; indisponibilidades; entrenamiento y evaluación ML | `<pendiente>` |
| 5 | 26/10 – 01/11 | Admin Platform; historial; exploración completa; recomendaciones | `<pendiente>` |
| 6 | 02/11 – 08/11 | Endurecimiento: RLS fina, pruebas 403/IDOR, sesión web, E2E | `<pendiente>` |
| 7 | 09/11 – 15/11 | Congelamiento de código (15/11); solo correcciones | `<pendiente>` |
| — | 16/11 – 20/11 | **Informe final 20/11** | — |
| — | 23/11 – 04/12 | Ensayos. **Presentación final 04/12** | — |

## Nota de método (transparencia)

1. En Jira las tareas **no están asignadas a objetos *Sprint*** (la búsqueda `sprint is not EMPTY` devuelve 0 resultados). La pertenencia a cada sprint de este documento se **reconstruyó** con el prefijo del ticket (`D-`, `R-`, `J-`), su fecha de creación/resolución y el plan del CONTEXTO_IA §12. Acción correctiva histórica: **KAN-43 "J-07 Sprint 2 en Jira"** (pendiente al 17/10; sin nuevo estado verificado) — crear los sprints en Jira y mover los tickets, para que el tablero sea evidencia directa.
2. Los tickets del Sprint 1 se registraron en Jira el 06/10 (después de la demo); el trabajo previo quedó en minutas y commits.
3. Los estados son los que tenía Jira al 17/10. No se reinterpretan como estados actuales: el avance técnico posterior se contrasta por PR y auditoría, sin inventar cierres en el tablero.


## Evolución verificable hasta el cierre

| Evidencia posterior al plan inicial | Resultado y límite |
|---|---|
| PR #159, integrado el 13/11 | Integración de operación e Inteligencia; no constituye acta de sprint |
| PR #165 y acta BI-03 del 13/11 | Histórico Parquet sintético cargado; segunda corrida idempotente real P5 NO EJ, con ensayo previo |
| PR #163 y acta de la 006 del 14/11 | KPI desde DWH; paridad ETL/DWH en 15/15 locales |
| PR #169, integrado el 14/11; auditoría titulada 15/11 | CI local final 31/31; ensayo general 10/10 y barrido final 19/19, con FAIL no bloqueantes y NO EJ |
| PR #175/#176, integrados el 14/11 | Fixture KPI y enmienda ADR-001 documentados |

Fechas de integración tomadas del historial Git, distintas de las fechas internas de las actas. Los sprints 4–7 conservan su planificación histórica; no hay artefactos o reuniones que puedan reconstruirse sin respaldo. No se crean nuevos backlogs paralelos. Los archivos 0–3 incorporan una nota posterior con estos límites.

Detalle de estados: [Product Backlog](../02_product_backlog.md). Resultados: [pruebas](../07_pruebas/README.md). Fuentes privadas fijadas al commit revisado: [índice](../../README.md#fuentes-y-alcance).
