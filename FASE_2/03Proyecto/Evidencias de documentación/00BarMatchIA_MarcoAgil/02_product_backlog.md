# 2. Product Backlog priorizado

> **Dueño:** Raúl Guerra (Product Owner). **Fuente:** briefs 21.6.x subordinados a RN 21.11 y a la Matriz de alineamiento. **Herramienta:** Jira, proyecto `KAN`.
> **Corte de estado:** 17/10/2026, leído desde Jira.
> ⚠️ **Validar PO:** la prioridad MoSCoW es una propuesta derivada del orden de sprints acordado (CONTEXTO_IA §12.2). Los recortes solo los decide el PO (Matriz #36) y la tarea **KAN-32 "R-17 Aprobar recortes y simplificación R3"** sigue abierta.

## 2.1 Convenciones

- **MoSCoW:** *Must* (sin esto no hay MVP), *Should* (importante, puede postergarse dentro del release), *Could* (deseable), *Won't* (fuera del Release 1).
- **Estado por capa:** el frontend avanzó contra **adaptadores mock** que respetan el contrato; una historia se considera *Terminada* solo cuando cumple la [DoD](04_definition_of_done.md) **con la API real**.
  - `F✅` front terminado sobre mock · `B✅` backend terminado · `B🔄` backend en curso · `B⏳` backend no iniciado · `⏳` no iniciado.
- **Estimación:** puntos de historia en escala Fibonacci (1, 2, 3, 5, 8, 13). `<TBD:sp>` donde el equipo aún no estimó en Jira.

## 2.2 Épicas

| ID | Épica | Brief | Prioridad | Sprint objetivo |
|---|---|---|---|---|
| EP-01 | Exploración y ficha del establecimiento | 21.6.15 | Must | 1 → 5 (completa) |
| EP-02 | Reserva con disponibilidad y "Ahora" | 21.6.1, 21.6.2 | Must | 1 |
| EP-03 | Cancelación de reserva | 21.6.4 | Must | 1–2 |
| EP-04 | Llegada QR, asignación y ocupación | 21.6.6, 21.6.8 | Must | 1–2 |
| EP-05 | No-show automático | 21.6.5 | Must | 2 |
| EP-06 | Atención walk-in | 21.6.7 | Must | 2 |
| EP-07 | Panel operacional del Personal | 21.6.17 | Must | 1–2 |
| EP-08 | Configuración inicial del establecimiento | 21.6.9 | Must | 3 |
| EP-09 | Supervisión y consulta administrativa (Admin Local) | 21.6.11, 21.6.18 | Must | 3 |
| EP-10 | Identidad, accesos y MFA | 21.6.14 | Must | 1 → 5 |
| EP-11 | Flujo de datos hacia BI y analítica | 21.6.12 | Must | 1 → 4 |
| EP-12 | Lista de espera | 21.6.3 | Should | 4 |
| EP-13 | Indisponibilidad excepcional | 21.6.10 | Should | 4 |
| EP-14 | Historial del Cliente | 21.6.16 | Should | 5 |
| EP-15 | Administración global de la plataforma | 21.6.19 | Should | 5 |
| EP-16 | Predicción y recomendación (IA) | 21.6.13 | Should | 4–5 |
| EP-17 | Endurecimiento de seguridad y calidad | Transversal | Must | 6 |
| — | Reseñas, pagos, push, integraciones | — | Won't | Post-MVP |

## 2.3 Historias de usuario (ordenadas por prioridad)

| ID | Historia de usuario | Épica | RN | MoSCoW | SP | Jira | Estado |
|---|---|---|---|---|---|---|---|
| HU-01 | Como **cliente**, quiero iniciar sesión y que la app me lleve a mis pantallas según mi perfil, para usar solo lo que me corresponde. | EP-10 | RN-041, RN-044 | Must | `<TBD:sp>` | KAN-64, KAN-78 | F✅ B🔄 |
| HU-02 | Como **cliente**, quiero buscar establecimientos por nombre y comuna, para encontrar dónde salir. | EP-01 | RN-049, RN-050, RN-053, RN-054 | Must | | KAN-65 | F✅ B🔄 |
| HU-03 | Como **cliente**, quiero ver la ficha pública del local (descripción, horarios, categorías, imagen), para decidir si me conviene. | EP-01 | RN-052, RN-060 | Must | | KAN-65 | F✅ B🔄 |
| HU-04 | Como **cliente**, quiero ver la disponibilidad por bloques de 30 min para una fecha y número de personas, para elegir un horario real. | EP-02 | RN-001, RN-002, RN-035 | Must | | KAN-65 | F✅ B🔄 |
| HU-05 | Como **cliente**, quiero reservar el bloque vigente ("Ahora") si quedan ≥ 5 min, para salir de inmediato con lugar asegurado. | EP-02 | RN-003 | Must | | KAN-65 | F✅ B🔄 |
| HU-06 | Como **cliente**, quiero confirmar la reserva y recibir un código legible y un QR, para presentarlo al llegar. | EP-02 | RN-024 | Must | | KAN-65, KAN-80 | F✅ B🔄 |
| HU-07 | Como **cliente**, quiero que el sistema impida que dos personas tomen el último cupo, para no llegar a un local sobrevendido. | EP-02 | RN-013, RN-024 | Must | | KAN-29, KAN-80 | F✅ (prueba) B🔄 |
| HU-08 | Como **cliente**, quiero ver mis reservas vigentes y pasadas, para hacer seguimiento. | EP-14 | RN-055 | Must (vigentes) / Should (historial) | | KAN-65 | F✅ B🔄 |
| HU-09 | Como **cliente**, quiero cancelar mi reserva y saber si la cancelación es tardía (< 60 min), para entender sus efectos. | EP-03 | RN-016, RN-017 | Must | | KAN-27, KAN-40 | F✅ B🔄 |
| HU-10 | Como **personal**, quiero ver las reservas de −2 h a +2 h actualizadas cada 15–30 s, para preparar las llegadas. | EP-07 | RN-056 | Must | | KAN-68, KAN-25 | F✅ B⏳ |
| HU-11 | Como **personal**, quiero escanear el QR del cliente con la cámara para registrar su llegada (check-in), para validar rápido. | EP-04 | RN-004, RN-005 | Must | | KAN-68 | F✅ B🔄 |
| HU-12 | Como **personal**, quiero buscar la reserva por nombre, correo o código si el QR falla, para no detener la atención. | EP-04 | RN-021 | Must | | KAN-68 | F✅ B🔄 |
| HU-13 | Como **personal**, quiero recibir sugerencias de mesa o combinación tras el check-in y asignarla, para ubicar al grupo. | EP-04 | RN-022, RN-034 | Must | | KAN-24, KAN-38 | F✅ B⏳ |
| HU-14 | Como **personal**, quiero ver las atenciones activas y liberar mesas con confirmación, para recuperar capacidad. | EP-04 | RN-028, RN-029 | Must | | KAN-26, KAN-39 | F✅ B⏳ |
| HU-15 | Como **personal**, quiero registrar un walk-in (grupo sin reserva) y asignarle ubicación sin superar la capacidad, para atender a quien llega sin reservar. | EP-06 | RN-026, RN-027 | Must | | KAN-30, KAN-41 | F✅ B⏳ |
| HU-16 | Como **sistema**, quiero marcar `NO_SHOW` cuando vence el período de gracia de 15 min sin check-in, para liberar capacidad. | EP-05 | RN-006, RN-007, RN-008 | Must | | KAN-37, KAN-33 | B⏳ (`<TBD:scheduler>`) |
| HU-17 | Como **admin local**, quiero elegir cuál de mis locales gestiono, para operar varios establecimientos. | EP-09 | RN-045 | Must | | KAN-86 | F✅ B⏳ |
| HU-18 | Como **admin local**, quiero consultar reservas filtrando por fecha y estado con paginación, para supervisar la operación. | EP-09 | RN-057 | Must | | KAN-69, KAN-31, KAN-87 | F✅ B⏳ |
| HU-19 | Como **admin local**, quiero ver el detalle y la línea de tiempo de una reserva, para entender qué ocurrió. | EP-09 | RN-058 | Must | | — | ⏳ |
| HU-20 | Como **admin local**, quiero configurar sectores, unidades, combinaciones, horarios y reglas, para habilitar mi local. | EP-08 | RN-030, RN-031, RN-034, RN-035 | Must | | KAN-89 | F✅ (mock) B⏳ |
| HU-21 | Como **admin local**, quiero iniciar sesión con MFA, para proteger el acceso administrativo. | EP-10 | RN-043 | Must | | KAN-89 | F✅ (mock) B⏳ |
| HU-22 | Como **admin local**, quiero un panel de supervisión de la ventana operacional, para reaccionar a tiempo. | EP-09 | RN-039, RN-040 | Must | | — | ⏳ |
| HU-23 | Como **admin local**, quiero ver KPIs (reservas por estado, tasa de no-show por día, demanda por bloque) marcados como datos sintéticos, para entender mi demanda. | EP-11 | — (Matriz #25) | Must | | KAN-85, KAN-48, KAN-81 | ⏳ |
| HU-24 | Como **analista**, quiero un ETL de solo lectura del OLTP a un DWH separado sin datos personales directos, para analizar sin afectar la operación. | EP-11 | Matriz #25, #26 | Must | | KAN-47, KAN-44, KAN-51 | ⏳ |
| HU-25 | Como **cliente**, quiero inscribirme en lista de espera para un bloque lleno, para obtener lugar si se libera capacidad. | EP-12 | RN-009 a RN-015, RN-025 | Should | | — | ⏳ |
| HU-26 | Como **admin local / personal**, quiero declarar una indisponibilidad temporal (unidad, sector o local), para reflejar imprevistos. | EP-13 | RN-019, RN-020, RN-036 a RN-038 | Should | | — | ⏳ |
| HU-27 | Como **admin platform**, quiero gestionar cuentas, establecimientos y categorías globales, para operar la plataforma. | EP-15 | RN-046 a RN-048, RN-059, RN-060, RN-062 | Should | | KAN-88 | F✅ (scaffolding) B⏳ |
| HU-28 | Como **admin local**, quiero ver predicciones batch (p. ej., probabilidad de no-show) y recomendaciones por franja, para planificar. | EP-16 | Matriz #27, #28 | Should | | KAN-49 | ⏳ (`<TBD:ml-problema>`) |
| HU-29 | Como **equipo**, queremos pruebas de autorización (403/IDOR), RLS fina y sesión web segura, para cumplir Security by Design. | EP-17 | RN-044, RN-048 | Must | | — | ⏳ (Sprint 6) |

## 2.4 Ítems técnicos habilitantes (no son historias de usuario)

| ID | Ítem | Jira | Estado |
|---|---|---|---|
| T-01 | Higiene del monorepo pnpm (`nodeLinker: hoisted`, `.nvmrc`, `packageManager`, README de onboarding) | KAN-52 (+ KAN-53…60) | ✅ |
| T-02 | `packages/contracts` v1 y v2 + fixtures | KAN-61, KAN-22 | ✅ |
| T-03 | Cliente HTTP + adaptadores http/mock + inyección de repositorios | KAN-63, KAN-23 | ✅ |
| T-04 | Alinear contratos con la API real (nomenclatura y errores) | KAN-93 | 🔄 |
| T-05 | Esqueleto NestJS por slices, CORS, filtro de errores, Swagger | KAN-77 | 🔄 |
| T-06 | Supabase: semilla, RLS sin políticas + revocación de `anon`, export de bloques | KAN-72 (+ KAN-73…76) | 🔄 |
| T-07 | ORM publicado (`<TBD:orm>`) | KAN-35 | 🔄 |
| T-08 | Job de CI para BI (ruff + pytest) | KAN-90 | ✅ |
| T-09 | ADRs con las decisiones del PO | KAN-50 | ⏳ (Josefa) |
