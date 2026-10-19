# Evidencias de pruebas — Sprints 1 a 3

> Completar cada fila con un **enlace verificable** (ejecución de GitHub Actions, PR, captura en `/evidencias/img`, video). No pegar resultados sin respaldo.

| Caso | Fecha | Evidencia | Resultado | Observación |
|---|---|---|---|---|
| Base Jest (jest-expo) pasa en monorepo limpio (`git clone → pnpm install → pnpm --filter mobile test`) | 06/10 | KAN-58 · `<TBD:link-CI>` | `<TBD:resultado>` | Criterio de cierre de D-01 |
| PU-02 / PU-04 — contrato v2 + pantallas críticas | 09/10 | KAN-28 · `<TBD:link-CI>` | `<TBD:N tests / N verdes>` | |
| PC-01 / PC-04 — concurrencia R1/R4, guion v2 y video | 13/10 | KAN-29 · `<TBD:link-video>` | `<TBD:resultado>` | Indicar si fue contra mock o API |
| PB-04 — job BI ruff + pytest | `<TBD>` | KAN-90 · `<TBD:link-CI>` | `<TBD:resultado>` | |
| Checklist físico demo 05/10 | 05/10 | KAN-70 · `<TBD:link-video>` | `<TBD:resultado>` | D-09 aún en curso |

## Cómo generar evidencia reproducible

```bash
# Cobertura de pruebas de la app móvil (genera carpeta coverage/ con reporte HTML)
pnpm --filter mobile test -- --coverage

# Cobertura de la web
pnpm --filter web-admin test -- --coverage

# BI: lint + pruebas
cd apps/bi && ruff check . && pytest -q
```
Adjuntar el resumen de cobertura (tabla final de Jest) como texto o captura; enlazar la ejecución de CI del commit correspondiente.
