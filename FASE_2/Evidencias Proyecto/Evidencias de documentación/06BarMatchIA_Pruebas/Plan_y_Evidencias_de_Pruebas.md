# Pruebas y aseguramiento de calidad — cierre MVP1

**Corte técnico respaldado:** auditoría del 15/11/2026, integrada por PR #169 al repositorio privado de desarrollo. Los resultados son de laboratorio y demostración académica.

## Evidencias cuantitativas

| Prueba | Resultado documentado |
|---|---|
| CI local final | 31 de 31 pasos satisfactorios |
| API | 284 de 284 pruebas satisfactorias |
| Contratos | 25 de 25 |
| Web | 866 satisfactorias y 1 omitida (corte de auditoría) |
| Mobile | 1.352 satisfactorias y 1 pendiente (corte de auditoría) |
| Ensayo general sin mocks | 10/10 casos satisfactorios |
| Barrido final | 19/19 casos satisfactorios |
| Sobreventa | 50, 100 y 200 reservas simultáneas al último cupo: 1 confirmada, 0 sobreventas |
| Idempotencia | 100 envíos con una misma clave: una sola reserva |
| Control de acceso | 90 rutas, 285 llamadas prohibidas: 0 accesos indebidos |
| BI | ruff/pytest 299 PASS + 21 omitidos; ETL/DWH, analítica SQL 44 checks y paridad KPI ETL/DWH en 15/15 locales |

## Cómo se evaluó

Pruebas unitarias, de integración, SQL, concurrencia, seguridad y E2E; QA de interfaz web y app; ensayos adversariales de autenticación, XSS, inyección, solicitudes malformadas y carga; validación de consulta analítica contra DWH de pruebas.

## Limitaciones

La auditoría identifica casos no ejecutados en teléfono físico: escaneo QR, enlaces profundos, audio/vibración, modo avión, doble toque físico, rendimiento real y verificación de Inteligencia para la jornada siguiente. También registra hallazgos de hardening no bloqueantes para el MVP1. No se debe presentar la auditoría como certificación de producción ni omitir los casos NO EJ.

## Evidencias complementarias

Los videos de la carpeta hermana `Evidencias de sistema aplicacion` documentan recorridos del 18/10 y del 15/11: reserva/QR, cancelación, escaneo/asignación, walk-in y bienvenida. Son evidencias audiovisuales de funcionalidades concretas y no reemplazan los registros de pruebas automatizadas.

**Fuente de cifras:** [auditoría y actas privadas](../README.md#fuentes-y-alcance). El [registro de pruebas de los sprints 1–3](../00BarMatchIA_MarcoAgil/07_pruebas/evidencias-sprint-01-03.md) conserva sus fechas originales.

## Alcance cumplido, limitaciones y evolución

La integración funcional, controles de concurrencia e idempotencia, flujos operacionales y BI sustentan el cierre académico del MVP1. Ensayo y barrido satisfactorios no significan ausencia de hallazgos en todas las baterías: API adversarial 50 PASS/10 FAIL; web/app 42 PASS/6 FAIL/6 NO EJ; E2E 47 PASS/6 FAIL/2 NO EJ, en sus respectivas puntas. Las correcciones y hallazgos no bloqueantes se interpretan según el acta, sin sumar corridas distintas como porcentaje de cobertura.

Los casos físicos NO EJ y el endurecimiento previo a producción se conservan como límites del resultado; no se convierten en incumplimiento del incremento funcional. ML-03 acredita integración experimental informativa, no calidad de inferencia individual. La auditoría mantiene «Mañana» como comprobación manual; otra acta registra pruebas del servicio hoy/D+1, que se distinguen del checklist físico.

El fixture KPI (#175) y la enmienda ADR (#176) ya están integrados. En BI-03, la segunda corrida idempotente real P5 quedó NO EJ, con ensayo previo admitido. BI-06/BI-08, galería editable y despliegue productivo son evolución posterior.

Consulta el [plan y catálogo actualizado](../00BarMatchIA_MarcoAgil/07_pruebas/README.md), los [videos publicados](../../Evidencias%20de%20sistema%20aplicacion/) y las [fuentes del cierre](../README.md#fuentes-y-alcance). Esta revisión documental no ejecutó nuevas pruebas del aplicativo.
