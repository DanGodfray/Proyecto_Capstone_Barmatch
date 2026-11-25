# Sprint 0 — Definición del producto

**Fechas:** 07/09 – 28/09/2026 · **Sprint Goal:** cerrar la definición funcional y el modelo de datos que desbloquean el desarrollo paralelo.

| Jira | Ítem | Responsable | Estado |
|---|---|---|---|
| KAN-4 | Épica EA1 — Primera presentación del proyecto | Daniel (coordinación) | ✅ Finalizado |
| KAN-1 / KAN-2 | Evidencias individuales y grupales (Fase 1) | Daniel | ✅ |
| KAN-3 / KAN-5 | PPT y pitch del proyecto | Josefa | ✅ |
| KAN-6 | Arquitectura y tecnología | Raúl | ✅ |
| KAN-7 | Riesgos internos y externos | Equipo | ✅ |
| KAN-8 | Mockup inicial | Daniel | ✅ |
| KAN-9 | Contexto y monetización | Equipo | ✅ |
| KAN-10 / KAN-11 | Épicas de inicio y avance de desarrollo | Equipo | ✅ |
| KAN-12 | Reunión semanal de avance (minuta en la descripción del ticket) | Josefa | 🔄 Épica viva |
| — | Mapa de Actores 21.1, Documento Maestro v0.2.1, Checkpoint 21.5, 19 briefs 21.6.x | Raúl / equipo | ✅ |
| — | **RN 21.11 (62 reglas, versión oficial)** | Raúl | ✅ |
| — | DDL PostgreSQL OLTP v1 + diccionario de datos | Raúl | ✅ |
| — | Propuesta preliminar de Data Mart | Josefa / Raúl | ✅ |
| — | Matriz final de alineamiento (02/10) | Raúl (PO) | ✅ |

**Resultado registrado en el documento histórico:** Cumplido. La definición quedó cerrada con jerarquía de fuentes explícita, lo que permitió iniciar el Sprint 1 con contratos de API.
**Ajuste relevante:** el stack de la presentación EA1 (FastAPI, SvelteKit, Redis, VPS) fue **reemplazado** por la Matriz de alineamiento (NestJS + Supabase, Expo, React; PostgreSQL transaccional contra sobreventa en lugar de Redis). Ver [arquitectura](../05_diseno/01_arquitectura.md) §5.1.8.

## Contraste posterior: cierre técnico del 15/11/2026

Esta sección actualiza la trazabilidad sin atribuir el trabajo posterior a las fechas del sprint ni cambiar sus estados Jira del 17/10. La integración real (#159), KPI/DWH (#163) y auditoría de cierre (#169) respaldan la evolución del incremento. El ensayo sin mocks dio 10/10 y el barrido final 19/19; el [backlog vigente](../02_product_backlog.md) distingue implementado, validado, pendiente y MVP2.

Los mocks describen el incremento histórico, no el estado actual del MVP1. BI-03 y el dashboard con DWH de pruebas están acreditados; jobs y correo no se consideran activados, y las pruebas físicas siguen NO EJ. No se declara una aceptación del Sprint Goal, una retrospectiva adicional ni un cierre de tickets sin acta o evidencia administrativa.

Fuentes y commits: [índice documental](../../README.md#fuentes-y-alcance).
