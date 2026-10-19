# 3. Sprint Backlog — índice

> **Responsable del artefacto:** Josefa Morales (Scrum Master). Borrador preparado por Daniel Godfray a partir de Jira (`KAN`) al 17/10/2026. ⚠️ **Validar Scrum Master.**

## Calendario de sprints

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

1. En Jira las tareas **no están asignadas a objetos *Sprint*** (la búsqueda `sprint is not EMPTY` devuelve 0 resultados). La pertenencia a cada sprint de este documento se **reconstruyó** con el prefijo del ticket (`D-`, `R-`, `J-`), su fecha de creación/resolución y el plan del CONTEXTO_IA §12. Acción correctiva: **KAN-43 "J-07 Sprint 2 en Jira"** (pendiente) — crear los sprints en Jira y mover los tickets, para que el tablero sea evidencia directa.
2. Los tickets del Sprint 1 se registraron en Jira el 06/10 (después de la demo); el trabajo previo quedó en minutas y commits.
3. Los estados son los que tenía Jira al 17/10. **Si un integrante avanzó y no actualizó Jira, debe corregir el ticket antes de publicar este documento**, porque el docente evaluará el tablero y este archivo en paralelo.

## Plantilla para sprints siguientes

```markdown
# Sprint N — <nombre>
**Fechas:** dd/mm – dd/mm · **Sprint Goal:** <una frase verificable>
**Capacidad:** 3 personas × <n> noches × 4 h = <h> h

| Jira | ID | Ítem | HU | Responsable | SP | Estado al cierre |
|---|---|---|---|---|---|---|

**Resultado del Sprint Goal:** Cumplido / Parcial / No cumplido — <por qué>
**Incremento entregado:** <qué se puede demostrar>
**Arrastre al siguiente sprint:** <tickets>
**Retrospectiva:** ../06_retrospectivas/retro-sprint-N.md
```
