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
| BI | ETL, DWH y consulta KPI en laboratorio; dashboard con datos sintéticos declarados |

## Cómo se evaluó

Pruebas unitarias, de integración, SQL, concurrencia, seguridad y E2E; QA de interfaz web y app; ensayos adversariales de autenticación, XSS, inyección, solicitudes malformadas y carga; validación de consulta analítica contra DWH de pruebas.

## Limitaciones

La auditoría identifica casos no ejecutados en teléfono físico: escaneo QR, enlaces profundos, audio/vibración, modo avión, doble toque físico, rendimiento real y verificación de Inteligencia para la jornada siguiente. También registra hallazgos de hardening no bloqueantes para el MVP1. No se debe presentar la auditoría como certificación de producción ni omitir los casos NO EJ.

## Evidencias complementarias

Los videos de la carpeta hermana `Evidencias de sistema aplicacion` documentan recorridos del 18/10 y del 15/11: reserva/QR, cancelación, escaneo/asignación, walk-in y bienvenida. Son evidencias audiovisuales de funcionalidades concretas y no reemplazan los registros de pruebas automatizadas.

**Fuente de cifras:** repositorio privado `barmatchia-team/barmatchia-mvp`, documento `docs/pruebas/auditoria-mvp1-2026-11-15.md`, PR #169. La documentación de pruebas inicial sigue en `00BarMatchIA_MarcoAgil/07_pruebas/`.
