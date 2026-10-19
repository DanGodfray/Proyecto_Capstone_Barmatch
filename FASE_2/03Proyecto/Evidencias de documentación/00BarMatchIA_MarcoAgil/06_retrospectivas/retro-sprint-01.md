# Retrospectiva Sprint 1 — Demo técnica (05/10/2026)

**Asistentes:** `<TBD:asistentes>` · **Facilitadora:** Josefa Morales · **Fecha:** `<TBD:fecha-retro>`

## Métricas del sprint (Jira al 17/10)

| Bloque | Planificado | Finalizado | En curso / no iniciado |
|---|---|---|---|
| Frontend + QA (D-01…D-09) | 9 | 8 | 1 (D-09) |
| Backend (R-01…R-07) | 7 | 0 | 7 |
| BI (J-02…J-05) | 4 | 0 | 4 |

**Sprint Goal:** ⚠️ Parcial `<TBD:confirmar-equipo>` — ciclo central demostrable sobre mocks; integración con API real y analítica en arrastre.

## Hechos observados (fuentes: Jira, minutas, repo)
- El monorepo quedó reproducible y la estrategia de contratos + mocks permitió que el frontend no se bloqueara.
- La demo del 05/10 requirió un triage posterior (KAN-20).
- Los tickets del sprint se registraron en Jira **después** de la demo (06/10); el tablero no reflejó el trabajo en tiempo real.
- El equipo acumula ~2 semanas de atraso respecto del cronograma inicial.
- La coordinación entre integrantes no ha sido óptima, con horarios disponibles solo de 20:00 a 00:00.

## Continue
- Trabajo paralelo por contratos (`packages/contracts`) y adaptadores mock.
- Un PR a la vez, verificado manualmente antes de integrar a `develop`.
- `<TBD:voz-equipo>`

## Stop
- Registrar tickets en Jira de forma retroactiva.
- `<TBD:voz-equipo>`

## Start
- Revisión diaria de PR en una ventana fija → **KAN-42 (R-18)**.
- Sprints creados en Jira con los tickets asignados → **KAN-43 (J-07)**.
- Uso controlado de Claude Code con `CLAUDE.md` y contexto normativo en el repo → **KAN-21 (D-11)**, **KAN-92**.
- `<TBD:voz-equipo>`

## Acciones

| Acción | Dueño | Ticket | Estado (17/10) |
|---|---|---|---|
| Ventana diaria de revisión de PR | Raúl | KAN-42 | ⏳ |
| Crear sprints en Jira y mover tickets | Josefa | KAN-43 | ⏳ |
| Setup de Claude Code + `CLAUDE.md` | Daniel | KAN-21 | ✅ |
| Triage y corrección de la demo | Daniel | KAN-20 | ✅ |
