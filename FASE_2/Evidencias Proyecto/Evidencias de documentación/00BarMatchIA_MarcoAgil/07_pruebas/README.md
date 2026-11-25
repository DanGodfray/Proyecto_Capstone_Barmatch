# 7. Plan de pruebas + evidencias

> **Responsable:** Daniel Godfray (QA). El plan conserva el catálogo original; los resultados de cierre se incorporan en este mismo documento, sin generar otra serie de actas.
> **Regla de evidencia:** un caso solo se declara validado con fuente y entorno. Las cifras agregadas no prueban cada ID del catálogo. Se distinguen PASS, FAIL no bloqueantes y NO EJ de la auditoría del 15/11.

## 7.1 Estrategia

| Nivel | Qué valida | Herramienta | Dónde corre |
|---|---|---|---|
| Unitarias | Funciones, hooks y reglas aisladas (p. ej., `diaSemanaDesdeJs`, cálculo de cancelación tardía en UI) | Jest móvil, Node test runner API/contratos, Vitest web, pytest BI | Local + CI |
| De contrato | Que los adaptadores `mock` y `http` cumplan el mismo contrato (`packages/contracts`) | Pruebas de contratos y adaptadores por área | Local + CI |
| De componente / pantalla | Una prueba por pantalla crítica (render, estados de carga/error) | Jest/Vitest + Testing Library | CI |
| De integración | API real + PostgreSQL: reserva, check-in, cancelar, eventos | Node test runner y laboratorios SQL/HTTP | CI con base de prueba |
| De concurrencia | Sin sobreventa ante confirmaciones simultáneas | Script Node con `Promise.all` / k6 | Manual + reporte |
| De rendimiento | Tiempos de respuesta bajo carga | Batería de carga documentada en auditoría; k6 no se certifica como herramienta ejecutada | Manual + reporte |
| De seguridad | 401/403/404, IDOR, RLS, secretos | Batería por OpenAPI, JWT, labs SQL y revisión de bundles/secretos | CI + manual |
| De BI | ETL de solo lectura, ausencia de PII, reproducibilidad del generador | pytest + ruff | CI (job BI, KAN-90) |
| Exploratorias / físicas | Flujo real en teléfono y tablet (cámara, luz, red) | Checklist QA + video | Casos físicos NO EJ salvo evidencia específica |

**Automatización:** Husky ejecuta `lint-staged` en cada commit; CI ejecuta lint + `contracts:build` + tests de cada app + job de BI (ruff + pytest).

## 7.2 Catálogo de casos de prueba

### Unitarias y de contrato (PU)

| ID | Caso | HU / RN | Resultado esperado | Estado |
|---|---|---|---|---|
| PU-01 | `diaSemanaDesdeJs(0)` devuelve 7 (domingo) | Matriz #10 | 7 | Caso del catálogo original sin salida individual trazada; las suites agregadas se informan en §7.5. |
| PU-02 | Suite de contrato v2 pasa con adaptador `mock` | T-02 | 100 % verde | Histórico KAN-28; cierre: contratos 25/25. La corrida histórica conserva su propia fecha y fuente. |
| PU-03 | Suite de contrato v2 pasa con adaptador `http` contra la API | T-04 | 100 % verde | Contratos e integración real validados en cierre; el agregado del cierre no se reasigna a una corrida histórica. |
| PU-04 | Una prueba por pantalla crítica (explorar, disponibilidad, confirmación, mis reservas, panel Personal, check-in, tabla Admin) | HU-02…HU-18 | Render y estados OK | Frontend integrado y suites auditadas; listado histórico individual no reconstruido. |
| PU-05 | Formulario de registro rechaza menores de 18 | RNF-PRI-05 | Error de validación | Registro corregido en PR #153; sin nuevo resultado individual agregado aquí. |

### Integración (PI)

| ID | Caso | Resultado esperado | Estado |
|---|---|---|---|
| PI-01 | `POST /reservas` crea `reserva` + `evento_reserva` (CREADA/CONFIRMADA) | Reserva y evento según operación/contrato; no se presupone conteo fijo de filas | Ciclo real de reserva validado en ensayo/barrido; el conteo exacto de filas exige su caso específico. |
| PI-02 | Cancelar a > 60 min | `CANCELADA`, `cancelacion_tardia=false` | Cancelación integrada y laboratorios; sin atribuir ejecución individual a este ID. |
| PI-03 | Cancelar a < 60 min | `CANCELADA`, `cancelacion_tardia=true`, sin error | Clasificación tardía implementada; validar el borde exacto por contrato/acta. |
| PI-04 | Reserva nueva aparece en el panel del Personal | ≤ 30 s | Panel con API real; el SLA de 30 s no se certifica con evidencia agregada. |
| PI-05 | Check-in fuera de ventana (> 20 min antes) | 409 `FUERA_DE_VENTANA_CHECKIN` | Regla implementada; el borde temporal requiere su registro específico. |
| PI-06 | Cada cambio de estado deja un `evento_reserva` con actor | 1 evento por transición | Eventos integrados; no equivale a registro global de incidentes (H-6). |
| PI-07 | No-show automático a los 15 min sin check-in | `NO_SHOW` + evento | Laboratorios de jobs; apagados en ensayo, sin validación de activación automática en demo. |

### Concurrencia (PC)

| ID | Caso | Resultado esperado | Estado |
|---|---|---|---|
| PC-01 (R1) | 20 confirmaciones simultáneas sobre un bloque con capacidad para 1 grupo | 1 × 201, 19 × 409 `SIN_CAPACIDAD`; 0 sobreventa | Cierre: 50/100/200 concurrentes al último cupo, una confirmada y cero sobreventas (auditoría §3.2). |
| PC-02 | Dos asignaciones simultáneas de la misma mesa | Una sola asignación | Motor/asignación con labs SQL y E2E PASS; el caso de dos actores físicos requiere su registro específico. |
| PC-03 (R2) | Walk-in y reserva compiten por el último cupo | Capacidad nunca negativa | Operación transaccional del Motor validada; el caso específico debe rastrearse en sus labs. |
| PC-04 (R4) | `<TBD:definir-R4 según KAN-29>` | — | Referencia histórica KAN-29; la definición y resultado específicos conservan su fuente histórica. |

### Seguridad (PS)

| ID | Caso | Resultado esperado | Estado |
|---|---|---|---|
| PS-01 | Ruta protegida sin token | 401 `NO_AUTENTICADO` | Validado en matriz OpenAPI de control de acceso; cero accesos indebidos. |
| PS-02 | Token expirado o con firma alterada | 401 | Nueve JWT manipulados rechazados; JWT vencido tras esperar una hora: NO EJ según auditoría. |
| PS-03 | Cliente llama `GET /operacion/reservas` | 403 `SIN_PERMISO` | Validado dentro de 285 llamadas indebidas sobre 90 rutas. |
| PS-04 | Admin Local consulta un local que no administra | 403 | Validado por matriz de roles/locales y laboratorios SQL. |
| PS-05 | Consulta directa a una tabla con la *anon key* | Rechazada (RLS) | Labs de permisos validados; no supone certificación genérica de todas las vistas/privilegios. |
| PS-06 | `grep` de `service_role` / `DATABASE_URL` en `apps/mobile` y `apps/web-admin` | 0 coincidencias | Bundles y verificador de secretos revisados en auditoría; no se interpreta grep de nombres de variables como fuga. |
| PS-07 | Cliente pide `GET /reservas/:codigo` de otra persona (IDOR) | 404 `NO_ENCONTRADO` | Aislamiento por perfil/local validado; resultado específico requiere su registro. |
| PS-08 | Objetivo de minimizar IDs y limitar QR a su dueño; contrastar con H-4 | Inspección de JSON | Limitación conocida H-4: IDs internos aún expuestos en determinadas respuestas; mejora no bloqueante. |
| PS-09 | Admin sin MFA no accede al panel | Bloqueado | Validado: MFA real; administrador aal1 rechazado con MFA_REQUERIDO. |

### Rendimiento (PR) y BI (PB)

| ID | Caso | Meta | Estado |
|---|---|---|---|
| PR-01 | Disponibilidad con 50 usuarios virtuales | ⚠️ p95 < 800 ms | Carga documentada (~1.000 solicitudes/s, quiebre a 700 VU); no acredita esta meta p95. |
| PR-02 | Creación de reservas en ráfaga | ⚠️ p95 < 1,5 s, 0 errores 5xx | Cero sobreventas en prueba concurrente; no se certifica la meta p95 propuesta. |
| PB-01 | ETL usa credencial de solo lectura (intento de escritura falla) | Error de permisos | Separación/roles documentados; incremental OLTP vivo fuera de BI-03 y del cierre acreditado. |
| PB-02 | El DWH no tiene columnas de PII (email, nombre, apellido, `qr_token`) | 0 columnas | Validado: smoke sin columnas personales y KPI sin PII (auditoría N3-01/N3-06). |
| PB-03 | El generador sintético con la misma semilla produce los mismos datos | Hash idéntico | Histórico Parquet verificado por manifiesto; no equivale a repetir el generador de este caso. |
| PB-04 | Job de BI en CI (ruff + pytest) | Verde | CI local de cierre: ruff y pytest BI 299 PASS + 21 omitidos; ejecución local, distinta de un run remoto de Actions. |

## 7.3 Pruebas físicas y alcance del checklist

- [ ] App en teléfono Android y en tablet (Personal) en la misma red que la API.
- [ ] Escaneo de QR con poca luz y con brillo bajo de pantalla; si falla, búsqueda manual.
- [ ] Cambio de cuenta Cliente ↔ Personal sin cerrar la app.
- [ ] Pérdida de red durante una reserva → mensaje comprensible, sin reserva duplicada.
- [x] Etiqueta "Datos sintéticos" y fecha de carga visibles en dashboard real con MFA (auditoría §3.4).
- [x] Ensayo general sin mocks: 10/10; barrido final: 19/19. No se atribuye aquí un cronometraje no registrado.

## 7.4 Evidencias por sprint

| Sprint | Archivo |
|---|---|
| 1–3 | [evidencias-sprint-01-03.md](evidencias-sprint-01-03.md) |

## 7.5 Resultados ejecutados del cierre MVP1

| Evidencia | Resultado y entorno |
|---|---|
| CI local final sobre `50302b35` | 31/31 pasos; no equivale a nueva ejecución ni a todos los checks remotos de Actions |
| API / contratos | 284/284 y 25/25 |
| Web / móvil, Chile y UTC | 866 PASS + 1 omitido / 1.352 PASS + 1 pendiente |
| BI | ruff y pytest: 299 PASS + 21 omitidos; ensayo KPI 20/20 y analítica SQL 44 checks |
| Control de acceso | 90 rutas, 285 llamadas que debían bloquearse: cero accesos indebidos y cero 5xx |
| Sobreventa | 50/100/200 confirmaciones simultáneas al último cupo: una confirmada; cero sobreventas |
| Idempotencia | 100 envíos con misma clave: una reserva |
| Ensayo sin mocks / barrido final | 10/10 y 19/19; API real, app/web y DWH de pruebas |
| Dashboard | Seis KPI y seis gráficos, MFA y aviso de datos sintéticos |
| Paridad BI | KPI ETL/DWH coincidentes en 15/15 locales según acta de la 006 |

Estas evidencias respaldan el alcance académico integrado del MVP1. No se ejecutaron nuevamente las pruebas del aplicativo durante esta actualización documental.

## 7.6 Limitaciones no bloqueantes y verificaciones pendientes

La auditoría no informa cero fallos en todas sus baterías: API adversarial tuvo 50 PASS/10 FAIL; web/app 42 PASS/6 FAIL/6 NO EJ; E2E funcional 47 PASS/6 FAIL/2 NO EJ en sus respectivas puntas. El acta distingue correcciones posteriores y hallazgos no bloqueantes; las cifras no deben sumarse como un único porcentaje de cobertura ni equipararse al ensayo final.

Los hallazgos de tasa, cabeceras de API, revocación, registro de incidentes, EXIF y dependencias no invalidan el MVP1 demostrado; son trabajo de endurecimiento previo a producción. El checker de lockfile respecto de rama base figura NO EJ en CI local. JWT expirado por espera real, fuerza bruta TOTP y capacidad del plan gestionado tienen reservas de ejecución propias.

Los casos físicos DISP-G3/G4, H1/H2/H6 y S1–S10, rendimiento nativo y T1–T6 (mapa, llavero y escáner Android/iOS) siguen NO EJ. La auditoría conserva la comprobación manual de Inteligencia «Mañana»; el acta prospectiva del 12/11 sí documenta pruebas del servicio hoy/D+1 y una confirmación web de esa integración, que no reemplazan el checklist físico del cierre. ML-03 se valida como integración informativa experimental, no como inferencia individual de riesgo.

BI-03 conservó P5, segunda corrida idempotente en DWH real, como NO EJ, respaldada por el ensayo previo admitido en el acta. BI-06 incremental, BI-08 supresión, galería editable y hosting productivo corresponden a evolución posterior.

## 7.7 Fuentes y evidencias académicas

[Fuentes privadas fijadas al commit revisado](../../README.md#fuentes-y-alcance), [síntesis de pruebas de cierre](../../06BarMatchIA_Pruebas/Plan_y_Evidencias_de_Pruebas.md) y [videos históricos y del 15/11](../../../Evidencias%20de%20sistema%20aplicacion/). Los videos acreditan recorridos concretos; no sustituyen actas, resultados automatizados ni casos físicos NO EJ. El [registro de sprints 1–3](evidencias-sprint-01-03.md) conserva su corte original y referencia los resultados posteriores sin retrofecharlos.
