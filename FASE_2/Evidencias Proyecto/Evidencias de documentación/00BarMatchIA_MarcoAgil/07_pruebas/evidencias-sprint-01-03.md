# Evidencias de pruebas — Sprints 1 a 3

> Registro histórico de octubre. Sus marcadores se conservan cuando no hay salida individual verificable; la auditoría posterior no se retrofecha como ejecución de estos sprints.

| Caso | Fecha | Evidencia | Resultado | Observación |
|---|---|---|---|---|
| Base Jest (jest-expo) pasa en monorepo limpio (`git clone → pnpm install → pnpm --filter mobile test`) | 06/10 | KAN-58 · `<TBD:link-CI>` | `<TBD:resultado>` | Criterio de cierre de D-01 |
| PU-02 / PU-04 — contrato v2 + pantallas críticas | 09/10 | KAN-28 · `<TBD:link-CI>` | `<TBD:N tests / N verdes>` | |
| PC-01 / PC-04 — concurrencia R1/R4, guion v2 y video | 13/10 | KAN-29 · `<TBD:link-video>` | `<TBD:resultado>` | Indicar si fue contra mock o API |
| PB-04 — job BI ruff + pytest | `<TBD>` | KAN-90 · `<TBD:link-CI>` | `<TBD:resultado>` | |
| Checklist físico demo 05/10 | 05/10 | KAN-70 · `<TBD:link-video>` | `<TBD:resultado>` | D-09 aún en curso |


## Evidencia posterior del cierre

Las suites y flujos dejaron de estar solo sobre mocks: la auditoría del 15/11 respalda integración real, CI local 31/31, API 284/284 y ensayo general 10/10. El [plan de pruebas actualizado](README.md#75-resultados-ejecutados-del-cierre-mvp1) consolida resultados, entornos y límites sin inventar enlaces a Actions ni cifras de cobertura de octubre.

Los [videos publicados del 18/10 y 15/11](../../../Evidencias%20de%20sistema%20aplicacion/) son evidencias históricas complementarias; no se asignan automáticamente a KAN-29/KAN-70 ni cierran los casos físicos NO EJ. Las instrucciones de cobertura del borrador se retiran: este repositorio no distribuye el código ejecutable y la revisión no ejecuta nuevas baterías.
