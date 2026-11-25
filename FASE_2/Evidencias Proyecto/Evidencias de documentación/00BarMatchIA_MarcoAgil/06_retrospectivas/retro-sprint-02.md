# Retrospectiva Sprint 2 — Operación del local (11–13/10/2026)

**Asistentes:** `<TBD:asistentes>` · **Facilitadora:** Josefa Morales · **Fecha:** `<TBD:fecha-retro>` (minuta 13/10)

## Métricas del sprint (Jira al 17/10)

| Bloque | Planificado | Finalizado | En curso / no iniciado |
|---|---|---|---|
| Frontend + QA (D-10…D-20 y KAN-86…92) | 18 | 18 | 0 |
| Backend (R-08…R-18) | 11 | 0 | 11 (2 en curso) |
| BI (J-06…J-14) | 9 | 0 | 9 |

## Hechos observados
- La capa de presentación de toda la operación del local quedó lista sobre mocks.
- La brecha entre frontend y backend/BI creció: aumenta el riesgo de **integración tardía** (descubrir incompatibilidades cerca del informe final).
- Decisión de la minuta 13/10 (§2.4): **el backend es la referencia de los contratos**, con nomenclatura uniforme → KAN-93.

## Continue
- Pruebas de contrato que corren contra mock y http (KAN-28).
- `<TBD:voz-equipo>`

## Stop
- Que el frontend defina contratos que el backend después no respeta (origen de KAN-93).
- `<TBD:voz-equipo>`

## Start
- Integración temprana por slice: cada endpoint real se conecta apenas existe, en vez de esperar a tener todos.
- Redistribución de carga: una vez cerrado el MVP de frontend, el rol QA/Frontend apoya integración de backend y BI.
- `<TBD:voz-equipo>`

## Acciones

| Acción | Dueño | Ticket | Estado (17/10) |
|---|---|---|---|
| Alinear `packages/contracts` con la API real | Daniel (+ aprobación Raúl) | KAN-93 | 🔄 |
| Publicar ORM | Raúl | KAN-35 | 🔄 |
| Decidir ubicación del DWH | Josefa | KAN-44 | ⏳ |
| Aprobar recortes de alcance (R3) | Raúl (PO) | KAN-32 | ⏳ |
