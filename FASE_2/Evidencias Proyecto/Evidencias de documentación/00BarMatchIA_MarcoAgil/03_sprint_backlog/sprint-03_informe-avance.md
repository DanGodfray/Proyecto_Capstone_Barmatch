# Sprint 3 — Informe de avance

**Fechas:** 12/10 – 16/10/2026 · **Sprint Goal:** congelar evidencias para el informe de avance (16/10) y empezar a reemplazar mocks por la API real.

| Jira | ID | Ítem | Responsable | Estado (Jira 17/10) |
|---|---|---|---|---|
| KAN-29 | D-21 | Pruebas de concurrencia R1/R4, guion v2, video (resuelto 13/10) | Daniel | ✅ |
| KAN-70 | D-09 | QA: suite de contrato, checklist del guion, video (arrastre) | Daniel | 🔄 |
| KAN-93 | — | Alinear `packages/contracts` con la API real (#60, BACK-04) — borrador en rama propia, **no entra al corte** | Daniel | 🔄 |
| — | — | Minuta 13/10 (§2.4: el backend es la referencia de los contratos) | Equipo | ✅ |
| `<TBD>` | — | Configuración inicial 21.6.9; supervisión 21.6.11; consulta administrativa + timeline 21.6.18 (plan §12.2) | Raúl | `<TBD:registrar-en-jira>` |
| `<TBD>` | — | Dataset de entrenamiento; modelo base (plan §12.2) | Josefa | `<TBD:registrar-en-jira>` |

**Resultado histórico del Sprint Goal:** `<TBD:confirmar-equipo>`; no hay acta de aceptación adicional en esta revisión.
**Decisión registrada (Minuta 13/10):** los contratos se alinean a las respuestas reales del backend (nomenclatura `camelCase`, IDs como texto, error `{ statusCode, codigo, message }`), pendiente de confirmación de Raúl (BACK-01/02). Esto reemplaza el valor provisional `<TBD:naming> = snake_case`.

## Contraste posterior: cierre técnico del 15/11/2026

Esta sección actualiza la trazabilidad sin atribuir el trabajo posterior a las fechas del sprint ni cambiar sus estados Jira del 17/10. La integración real (#159), KPI/DWH (#163) y auditoría de cierre (#169) respaldan la evolución del incremento. El ensayo sin mocks dio 10/10 y el barrido final 19/19; el [backlog vigente](../02_product_backlog.md) distingue implementado, validado, pendiente y MVP2.

Los mocks describen el incremento histórico, no el estado actual del MVP1. BI-03 y el dashboard con DWH de pruebas están acreditados; jobs y correo no se consideran activados, y las pruebas físicas siguen NO EJ. No se declara una aceptación del Sprint Goal, una retrospectiva adicional ni un cierre de tickets sin acta o evidencia administrativa.

Fuentes y commits: [índice documental](../../README.md#fuentes-y-alcance).
