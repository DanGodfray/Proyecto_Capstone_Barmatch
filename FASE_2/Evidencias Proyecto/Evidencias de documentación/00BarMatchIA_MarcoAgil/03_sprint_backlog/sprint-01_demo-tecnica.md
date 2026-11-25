# Sprint 1 — Demo técnica

**Fechas:** 29/09 – 05/10/2026 · **Sprint Goal:** demostrar el ciclo central (Cliente reserva → Personal valida con QR → Admin Local consulta) sobre contratos compartidos, con la API en modo memoria o BD.

| Jira | ID | Ítem | Responsable | Estado (Jira 17/10) |
|---|---|---|---|---|
| KAN-52 | D-01 | Higiene del monorepo (subtareas KAN-53…KAN-60) | Daniel | ✅ |
| KAN-61 | D-02 | `packages/contracts` v1 + `establecimientos.demo.json` | Daniel | ✅ |
| KAN-62 | D-03 | PR `frontend/scafolding` → `develop` | Daniel | ✅ |
| KAN-63 | D-04 | Cliente HTTP + adaptadores http/mock + proveedor de inyección | Daniel | ✅ |
| KAN-64 | D-05 | Login Supabase móvil + `GET /me` + rutas por perfil | Daniel | ✅ |
| KAN-65 | D-06 | Flujo Cliente completo (+ KAN-66 mockup, KAN-67 navegación) | Daniel | ✅ |
| KAN-68 | D-07 | Flujo Personal: panel con polling, QR, check-in, búsqueda manual | Daniel | ✅ |
| KAN-69 | D-08 | Web: login, guard por perfil, layout, tabla, `/analitica` | Daniel | ✅ |
| KAN-70 | D-09 | QA: suite de contrato, test por pantalla, checklist del guion, video | Daniel | 🔄 |
| KAN-71 | R-01 | Confirmar o ajustar los `<TBD>` de su dominio | Raúl | 🔄 |
| KAN-72 | R-02 | Supabase: semilla, RLS, export de bloques (KAN-73 ✅, KAN-74/75/76 ⏳) | Raúl | 🔄 |
| KAN-77 | R-03 | Esqueleto NestJS por slices, CORS, filtro de errores, Swagger | Raúl | 🔄 |
| KAN-78 | R-04 | Guard JWT Supabase + `/me` + decorador de perfil | Raúl | 🔄 |
| KAN-79 | R-05 | Endpoints §7.1 en modo memoria | Raúl | ⏳ |
| KAN-80 | R-06 | Modo BD: lecturas → reserva transaccional → check-in → cancelar | Raúl | 🔄 |
| KAN-81 | R-07 | `/admin/analitica/kpis` sirviendo el JSON de BI | Raúl | ⏳ |
| KAN-82 | J-02 | Confirmar `KpisDTO` + publicar `kpis.ejemplo.json` | Josefa | ⏳ |
| KAN-83 | J-03 | Generador sintético reproducible | Josefa | ⏳ |
| KAN-84 | J-04 | ETL mínimo → tablas estrella → KPIs JSON | Josefa | ⏳ |
| KAN-85 | J-05 | `features/analitica`: 3 gráficos + aviso "Datos sintéticos" | Josefa | ⏳ |

**Resultado del Sprint Goal:** ⚠️ **Parcial** `<TBD:confirmar-equipo>`. El frontend completo sobre adaptadores mock quedó listo; la integración con la API real y la analítica quedaron como arrastre.
**Incremento:** app móvil (Cliente + Personal) y web Admin Local navegables de punta a punta sobre mocks que respetan el contrato; monorepo reproducible (`git clone → pnpm install → pnpm --filter mobile test`).
**Arrastre al Sprint 2:** R-01 a R-07, J-02 a J-05, D-09.
**Retrospectiva:** [retro-sprint-01](../06_retrospectivas/retro-sprint-01.md) — triage de la demo en KAN-20.
