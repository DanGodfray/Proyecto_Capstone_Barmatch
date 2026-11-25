# Sprint 2 — Operación del local

**Fechas:** 06/10 – 11/10/2026 · **Sprint Goal:** cubrir la operación del local después del check-in (asignación, atención, liberación, walk-in, no-show) y los estados nuevos en Cliente, Personal y Web.

| Jira | ID | Ítem | Responsable | Estado (Jira 17/10) |
|---|---|---|---|---|
| KAN-20 | D-10 | Triage y corrección de la demo 5/10 | Daniel | ✅ |
| KAN-21 | D-11 | Setup Claude Code + revisión de `CLAUDE.md` | Daniel | ✅ |
| KAN-22 | D-12 | Contratos v2 + fixtures de unidades | Daniel | ✅ |
| KAN-23 | D-19 | Adaptadores mock de `AtencionRepository` | Daniel | ✅ |
| KAN-24 | D-14 | Check-in → sugerencias → asignar | Daniel | ✅ |
| KAN-25 | D-17 | Panel Personal con estados nuevos | Daniel | ✅ |
| KAN-26 | D-13 | Atenciones activas + liberar con confirmación | Daniel | ✅ |
| KAN-27 | D-16 | Cliente: aviso de cancelación tardía + estados | Daniel | ✅ |
| KAN-28 | D-20 | Tests de contrato v2 + uno por pantalla crítica | Daniel | ✅ |
| KAN-30 | D-15 | Walk-in (reutiliza `SelectorUbicacion`) | Daniel | ✅ |
| KAN-31 | D-18 | Web: estados nuevos + filtro en tabla Admin | Daniel | ✅ |
| KAN-86 | — | Selector de local del Admin Local (ADM-05, RN-045) | Daniel | ✅ |
| KAN-87 | — | Mocks de `/admin/reservas` validan estado, fechas y local (ADM-03) | Daniel | ✅ |
| KAN-88 | — | Consola Admin Platform: scaffolding sobre mocks | Daniel | ✅ |
| KAN-89 | — | Admin Local: configuración y MFA sobre mocks | Daniel | ✅ |
| KAN-90 | — | Job de BI con ruff y pytest en CI | Daniel | ✅ |
| KAN-91 | — | Mockups de Personal, Admin Local y Admin Platform | Daniel | ✅ |
| KAN-92 | — | Contexto normativo del PO en el repo | Daniel | ✅ |
| KAN-32 | R-17 | Aprobar recortes y simplificación R3 | Raúl | ⏳ |
| KAN-33 | R-01B | Cerrar `<TBD:scheduler>` y error `UBICACION_NO_DISPONIBLE` | Raúl | ⏳ |
| KAN-34 | R-08 | Arrastre modo BD de la demo | Raúl | ⏳ |
| KAN-35 | R-16 | ORM publicado | Raúl | 🔄 |
| KAN-36 | R-11 | Endpoints operación v2 en memoria | Raúl | ⏳ |
| KAN-37 | R-09 | Scheduler no-show + actualización condicional | Raúl | ⏳ |
| KAN-38 | R-12 | Asignación transaccional con lock (R1) | Raúl | ⏳ |
| KAN-39 | R-14 | Finalizar atención | Raúl | ⏳ |
| KAN-40 | R-10 | `cancelacion_tardia` derivada | Raúl | 🔄 |
| KAN-41 | R-13 | Walk-in con lock + resta de disponibilidad (R2) | Raúl | ⏳ |
| KAN-42 | R-18 | Ventana diaria de revisión de PR | Raúl | ⏳ |
| KAN-43 | J-07 | Sprint 2 en Jira | Josefa | ⏳ |
| KAN-44 | J-08 | Decidir ubicación del DWH + esquema estrella | Josefa | ⏳ |
| KAN-45 | J-12 | Semilla SQL sectores/unidades/combinaciones | Josefa | ⏳ |
| KAN-46 | J-09 | Generador sintético: atenciones, no-show, walk-in | Josefa | ⏳ |
| KAN-47 | J-10 | ETL OLTP → DWH (solo lectura) | Josefa | ⏳ |
| KAN-48 | J-11 | KPIs v2 desde el DWH + gráficos web | Josefa | ⏳ |
| KAN-49 | J-13 | Propuesta de problema ML (1 página) | Josefa | ⏳ |
| KAN-50 | J-14 | Redactar ADRs con decisiones de Raúl | Josefa | ⏳ |
| KAN-51 | J-06 | Atributos de cliente del Data Mart (Matriz #25) | Josefa | ⏳ |

**Resultado del Sprint Goal:** ⚠️ **Parcial** `<TBD:confirmar-equipo>`. La capa de presentación de toda la operación del local está lista sobre mocks; la lógica transaccional (asignación con lock, walk-in con lock, scheduler de no-show) y el pipeline BI siguen pendientes.
**Riesgo detectado:** el frontend avanza más rápido que backend y BI; la brecha aumenta el riesgo de integración tardía. Ver retrospectiva.
