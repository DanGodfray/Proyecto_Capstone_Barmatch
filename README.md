# BarMatchIA — Proyecto Capstone

Repositorio académico de BarMatchIA: plataforma B2B2C de descubrimiento, reservas y operación de establecimientos, con analítica e inteligencia experimental.

**Cierre técnico MVP1:** 15/11/2026. **Revisión documental:** 25/11/2026. El MVP1 alcanzó una integración funcional validada en laboratorio y demostración con API real y DWH de pruebas.

## Producto y equipo

Cliente y Personal utilizan una aplicación móvil Expo/React Native; Admin Local y Admin Platform utilizan la web React/Vite. La API NestJS aplica autorización y reglas transaccionales sobre PostgreSQL. Supabase aporta Auth y Storage. BI integra ETL y un DWH separado del OLTP de la aplicación. La Inteligencia BETA opera desacoplada de la confirmación de reservas.

| Integrante | Responsabilidad |
|---|---|
| Raúl Guerra Gutiérrez | Backend y Product Owner |
| Josefa Morales Jiménez | BI y Scrum Master; IA coordinada con Backend |
| Daniel Godfray Abatte | Frontend, QA e integración |

El modelo B2B2C y la suscripción de establecimientos constituyen la propuesta de sostenibilidad; su operación comercial es una etapa posterior.

## Logros verificables del MVP1

- Integración de app, web y API real: 15 slices móviles y 8 web sin pantallas simuladas en el ensayo general.
- Motor de reservas con controles de concurrencia, factibilidad física e idempotencia: una confirmación ante 50/100/200 solicitudes simultáneas al último cupo y cero sobreventas en laboratorio.
- Flujos de reserva, cancelación, check-in, asignación, atenciones y walk-in; acceso por perfil/local y MFA administrativo.
- ETL e histórico Parquet sintético en DWH de pruebas; dashboard de seis KPI y seis gráficos, con paridad ETL/DWH en 15/15 locales.
- CI local final 31/31; ensayo general 10/10 y barrido final 19/19. Cifras y entornos en las [evidencias de pruebas](FASE_2/Evidencias%20Proyecto/Evidencias%20de%20documentación/06BarMatchIA_Pruebas/Plan_y_Evidencias_de_Pruebas.md).

## Documentación académica

| Entrega | Acceso |
|---|---|
| Fase 1 | [Documentación histórica](FASE_1/) |
| Fase 2 — individuales | [Tres diarios 2.1](FASE_2/Evidencias%20Individuales/) |
| Fase 2 — grupales | [Word definitivos 2.4/2.6 y ambas planillas](FASE_2/Evidencias%20Grupales/) |
| Fase 2 — proyecto | [Evidencias del proyecto](FASE_2/Evidencias%20Proyecto/) |
| Fase 3 | [Entregables publicados](FASE_3/) |

## Guía de lectura

- [Índice documental y fuentes](FASE_2/Evidencias%20Proyecto/Evidencias%20de%20documentación/README.md).
- [Product Vision](FASE_2/Evidencias%20Proyecto/Evidencias%20de%20documentación/00BarMatchIA_MarcoAgil/01_product_vision.md), [Product Backlog](FASE_2/Evidencias%20Proyecto/Evidencias%20de%20documentación/00BarMatchIA_MarcoAgil/02_product_backlog.md) y [Sprint Backlogs](FASE_2/Evidencias%20Proyecto/Evidencias%20de%20documentación/00BarMatchIA_MarcoAgil/03_sprint_backlog/README.md).
- [Diseño](FASE_2/Evidencias%20Proyecto/Evidencias%20de%20documentación/00BarMatchIA_MarcoAgil/05_diseno/README.md), [manual técnico académico](FASE_2/Evidencias%20Proyecto/Evidencias%20de%20documentación/00BarMatchIA_MarcoAgil/08_manual_tecnico.md) y [videos del 18/10 y 15/11](FASE_2/Evidencias%20Proyecto/Evidencias%20de%20sistema%20aplicacion/).

## Alcance y evolución

El cierre distingue **MVP1 cumplido y validado**, **limitaciones técnicas no bloqueantes** y **evolución o requisitos previos a producción**. Los hallazgos de endurecimiento y los casos físicos NO EJ no invalidan la integración demostrada; tampoco se presentan como pruebas aprobadas. El despliegue comercial, la galería editable y el BI incremental/supresión pertenecen a evolución posterior. ML-03 es experimental, sin inferencia individual validada.

El diagrama `ArquitecturaBarMatchIA_MVP.png` corresponde a una presentación anterior y no se declara arquitectura final certificada. Los videos son evidencia de recorridos, no sustituyen las actas ni cierran automáticamente pruebas físicas.

Este repositorio contiene evidencias académicas. El código y la configuración operativa permanecen en el repositorio privado del equipo; aquí no se distribuyen credenciales ni procedimientos sensibles.
