# 7. Plan de pruebas + evidencias

> **Responsable:** Daniel Godfray (QA). Las pruebas se ejecutan **en cada sprint** y sus evidencias se registran en `evidencias-sprint-N.md`.
> **Regla de evidencia:** un caso solo se marca ✅ con una evidencia verificable (salida de CI, captura, video o reporte). Sin evidencia → `<TBD:resultado>`.

## 7.1 Estrategia

| Nivel | Qué valida | Herramienta | Dónde corre |
|---|---|---|---|
| Unitarias | Funciones, hooks y reglas aisladas (p. ej., `diaSemanaDesdeJs`, cálculo de cancelación tardía en UI) | Jest (jest-expo en móvil), pytest (BI) | Local + CI |
| De contrato | Que los adaptadores `mock` y `http` cumplan el mismo contrato (`packages/contracts`) | Jest (misma suite, dos adaptadores) | Local + CI |
| De componente / pantalla | Una prueba por pantalla crítica (render, estados de carga/error) | Jest + Testing Library | CI |
| De integración | API real + PostgreSQL: reserva, check-in, cancelar, eventos | Jest (supertest en NestJS) `<TBD:confirmar-Raúl>` | CI con base de prueba |
| De concurrencia | Sin sobreventa ante confirmaciones simultáneas | Script Node con `Promise.all` / k6 | Manual + reporte |
| De rendimiento | Tiempos de respuesta bajo carga | k6 ⚠️ propuesta | Manual + reporte |
| De seguridad | 401/403/404, IDOR, RLS, secretos | Jest/supertest + `curl` con *anon key* + `grep` de secretos | CI + manual |
| De BI | ETL de solo lectura, ausencia de PII, reproducibilidad del generador | pytest + ruff | CI (job BI, KAN-90) |
| Exploratorias / físicas | Flujo real en teléfono y tablet (cámara, luz, red) | Checklist QA + video | Antes de cada demo |

**Automatización:** Husky ejecuta `lint-staged` en cada commit; CI ejecuta lint + `contracts:build` + tests de cada app + job de BI (ruff + pytest).

## 7.2 Catálogo de casos de prueba

### Unitarias y de contrato (PU)

| ID | Caso | HU / RN | Resultado esperado | Estado |
|---|---|---|---|---|
| PU-01 | `diaSemanaDesdeJs(0)` devuelve 7 (domingo) | Matriz #10 | 7 | `<TBD:resultado>` |
| PU-02 | Suite de contrato v2 pasa con adaptador `mock` | T-02 | 100 % verde | ✅ KAN-28 — adjuntar salida de CI |
| PU-03 | Suite de contrato v2 pasa con adaptador `http` contra la API | T-04 | 100 % verde | ⏳ (depende de KAN-93) |
| PU-04 | Una prueba por pantalla crítica (explorar, disponibilidad, confirmación, mis reservas, panel Personal, check-in, tabla Admin) | HU-02…HU-18 | Render y estados OK | ✅ KAN-28 — adjuntar listado |
| PU-05 | Formulario de registro rechaza menores de 18 | RNF-PRI-05 | Error de validación | `<TBD:resultado>` |

### Integración (PI)

| ID | Caso | Resultado esperado | Estado |
|---|---|---|---|
| PI-01 | `POST /reservas` crea `reserva` + `evento_reserva` (CREADA/CONFIRMADA) | 201 + 2 filas | ⏳ (KAN-80) |
| PI-02 | Cancelar a > 60 min | `CANCELADA`, `cancelacion_tardia=false` | ⏳ (KAN-40) |
| PI-03 | Cancelar a < 60 min | `CANCELADA`, `cancelacion_tardia=true`, sin error | ⏳ (KAN-40) |
| PI-04 | Reserva nueva aparece en el panel del Personal | ≤ 30 s | ✅ mock / ⏳ API |
| PI-05 | Check-in fuera de ventana (> 20 min antes) | 409 `FUERA_DE_VENTANA_CHECKIN` | ⏳ |
| PI-06 | Cada cambio de estado deja un `evento_reserva` con actor | 1 evento por transición | ⏳ |
| PI-07 | No-show automático a los 15 min sin check-in | `NO_SHOW` + evento | ⏳ (KAN-37) |

### Concurrencia (PC)

| ID | Caso | Resultado esperado | Estado |
|---|---|---|---|
| PC-01 (R1) | 20 confirmaciones simultáneas sobre un bloque con capacidad para 1 grupo | 1 × 201, 19 × 409 `SIN_CAPACIDAD`; 0 sobreventa | ✅ escenario y guion (KAN-29) / ⏳ contra API real (KAN-80) |
| PC-02 | Dos asignaciones simultáneas de la misma mesa | Una sola asignación | ⏳ (KAN-38) |
| PC-03 (R2) | Walk-in y reserva compiten por el último cupo | Capacidad nunca negativa | ⏳ (KAN-41) |
| PC-04 (R4) | `<TBD:definir-R4 según KAN-29>` | — | ✅ KAN-29 — adjuntar evidencia |

### Seguridad (PS)

| ID | Caso | Resultado esperado | Estado |
|---|---|---|---|
| PS-01 | Ruta protegida sin token | 401 `NO_AUTENTICADO` | ⏳ |
| PS-02 | Token expirado o con firma alterada | 401 | ⏳ |
| PS-03 | Cliente llama `GET /operacion/reservas` | 403 `SIN_PERMISO` | ⏳ (guion de demo §9.4) |
| PS-04 | Admin Local consulta un local que no administra | 403 | ⏳ |
| PS-05 | Consulta directa a una tabla con la *anon key* | Rechazada (RLS) | ⏳ (KAN-75) |
| PS-06 | `grep` de `service_role` / `DATABASE_URL` en `apps/mobile` y `apps/web-admin` | 0 coincidencias | `<TBD:resultado>` |
| PS-07 | Cliente pide `GET /reservas/:codigo` de otra persona (IDOR) | 404 `NO_ENCONTRADO` | ⏳ |
| PS-08 | Respuestas de API no contienen `id_reserva`; `qr_token` solo en vista del dueño | Inspección de JSON | ✅ contrato / ⏳ API |
| PS-09 | Admin sin MFA no accede al panel | Bloqueado | ✅ mock (KAN-89) / ⏳ real |

### Rendimiento (PR) y BI (PB)

| ID | Caso | Meta | Estado |
|---|---|---|---|
| PR-01 | Disponibilidad con 50 usuarios virtuales | ⚠️ p95 < 800 ms | ⏳ Sprint 6 |
| PR-02 | Creación de reservas en ráfaga | ⚠️ p95 < 1,5 s, 0 errores 5xx | ⏳ Sprint 6 |
| PB-01 | ETL usa credencial de solo lectura (intento de escritura falla) | Error de permisos | ⏳ (KAN-47) |
| PB-02 | El DWH no tiene columnas de PII (email, nombre, apellido, `qr_token`) | 0 columnas | ⏳ |
| PB-03 | El generador sintético con la misma semilla produce los mismos datos | Hash idéntico | ⏳ (KAN-83) |
| PB-04 | Job de BI en CI (ruff + pytest) | Verde | ✅ KAN-90 — adjuntar salida |

## 7.3 Pruebas exploratorias físicas (checklist QA antes de cada demo)

- [ ] App en teléfono Android y en tablet (Personal) en la misma red que la API.
- [ ] Escaneo de QR con poca luz y con brillo bajo de pantalla; si falla, búsqueda manual.
- [ ] Cambio de cuenta Cliente ↔ Personal sin cerrar la app.
- [ ] Pérdida de red durante una reserva → mensaje comprensible, sin reserva duplicada.
- [ ] Etiqueta "Datos sintéticos" visible en analítica.
- [ ] Guion de demo completo cronometrado y video de respaldo grabado.

## 7.4 Evidencias por sprint

| Sprint | Archivo |
|---|---|
| 1–3 | [evidencias-sprint-01-03.md](evidencias-sprint-01-03.md) |
