# 5.4 Requisitos no funcionales (RNF)

> Los valores **fijados** provienen de la Matriz de alineamiento, RN 21.11 y CONTEXTO_IA. Las **metas cuantitativas** marcadas con ⚠️ son propuestas del rol QA y deben validarse con el equipo antes del informe final.
> **Corte técnico:** 15/11/2026; revisión 25/11. La columna *Verificación* conserva los casos del plan original; no prueba una correspondencia caso a caso con las actas posteriores. Resultados: [plan de pruebas](../07_pruebas/README.md) y [RNF de cierre](../../04BarMatchIA_RNF/Requisitos_No_Funcionales.md).

## 5.4.1 Seguridad

| ID | Requisito | Verificación | Estado |
|---|---|---|---|
| RNF-SEG-01 | Toda ruta protegida exige `Authorization: Bearer <JWT de Supabase>`; la API verifica firma y vigencia del token. | PS-01, PS-02 | Validado: JWT manipulados rechazados y control de rutas en laboratorio. |
| RNF-SEG-02 | Autorización por **4 perfiles** en cada endpoint (guard + decorador de perfil) y validación de **pertenencia al establecimiento** (Personal: el suyo; Admin Local: sus asociados). La protección de rutas en el frontend es solo de experiencia de usuario. | PS-03, PS-04 | Validado: 90 rutas, 285 llamadas indebidas, cero accesos no autorizados. |
| RNF-SEG-03 | **Control de acceso a datos:** RLS y privilegios según el rol y objeto; acceso de las apps mediante la API. No se presupone que activar RLS certifique por sí solo las vistas o privilegios. | PS-05 | Validación por modelo de permisos y labs SQL; la auditoría conserva observación D-05 de RLS Backend. No equivale a certificación global. |
| RNF-SEG-04 | Las apps usan supabase-js **solo para Auth**; ningún secreto (`service_role`, `DATABASE_URL`) en `apps/mobile` ni `apps/web-admin`. | Revisión de PR + `grep` en CI (PS-06) | Validado: revisión de bundles/secretos en auditoría; sin credenciales públicas de servidor. |
| RNF-SEG-05 | **Minimización por DTO:** objetivo de minimizar IDs internos (hallazgo H-4); `qr_token` solo en la vista del cliente dueño; el Personal ve solo el nombre del cliente. Una reserva ajena responde `NO_ENCONTRADO` (no revela existencia → previene IDOR). | PS-07, PS-08 | Parcial: aislamiento validado; exposición de IDs internos H-4 pendiente de endurecimiento. |
| RNF-SEG-06 | MFA obligatorio para perfiles administrativos (RN-043); política de contraseña, bloqueo y recuperación (RN-042) delegada en Supabase Auth. | PS-09 | MFA real validado. Bloqueo por intentos RN-042 no aplicado en el entorno real, según auditoría. |
| RNF-SEG-07 | Trazabilidad: `evento_reserva` para cada cambio de estado y `evento_auditoria` para acciones administrativas (RN-048, RN-062). | PI-06 | Eventos de negocio implementados; registro general de solicitudes/incidentes de API H-6 pendiente, sin invalidar el alcance funcional. |
| RNF-SEG-08 | Cadena de suministro: lockfile obligatorio; scripts de instalación de dependencias deshabilitados salvo aprobación explícita (`pnpm approve-builds`). | Revisión de PR | Lockfile congelado y acciones fijadas en el cierre; checker comparativo NO EJ en CI local. Dependencias con avisos heredados. |
| RNF-SEG-09 | `.env` nunca se commitea; existe `.env.example` por app. | Pre-commit + revisión | Control de secretos documentado; no se publican entornos privados. |
| RNF-SEG-10 | Tokens móviles en SecureStore; sesión web en sessionStorage, con limpieza de caché y claves de idempotencia al cerrar sesión. | Checklist de release | Implementado: SecureStore móvil y sessionStorage web. Teléfono físico NO EJ; revocación global API H-3 pendiente. |

## 5.4.2 Privacidad y cumplimiento normativo

| ID | Requisito | Verificación | Estado |
|---|---|---|---|
| RNF-PRI-01 | Diseño contra la **Ley 21.719** (reforma de la Ley 19.628, vigencia 01/12/2026): principio de minimización, finalidad y registro de tratamiento. | Revisión documental | Diseño de privacidad documentado; no constituye certificación legal ni de producción. |
| RNF-PRI-02 | El DWH no contiene PII directa: rango etario, género agregado y clave seudonimizada (HMAC-SHA256 con sal estable). | PB-02 | Validado: smoke sin columnas personales y KPI sin PII; seudonimización en BI. |
| RNF-PRI-03 | Datos de desarrollo y demo **ficticios o sintéticos**, declarados como tales en la UI ("Datos sintéticos"). | Revisión visual | Validado: dashboard real con etiqueta Datos sintéticos y fecha de carga. |
| RNF-PRI-04 | El perfilamiento con IA (predicción de no-show/recomendaciones) requiere **Evaluación de Impacto (DPIA)** antes de usar datos reales. | Documento DPIA `<TBD:dpia>` | DPIA ML-03 pendiente antes de activación con datos reales/producción; no bloquea la demostración experimental. |
| RNF-PRI-05 | Edad mínima de registro: 18 años cumplidos (Matriz #15D). | PU-xx (validación de formulario) + backend | Implementado; registro corregido en PR #153. No se agrega un PASS individual sin caso trazado. |
| RNF-PRI-06 | Locales de OpenStreetMap usados en la demo muestran el crédito `© OpenStreetMap contributors, ODbL 1.0` y no se presentan como afiliados. | Revisión visual | Requisito de atribución conservado; esta revisión no repite la validación visual histórica. |

## 5.4.3 Rendimiento

| ID | Requisito | Meta | Verificación | Estado |
|---|---|---|---|---|
| RNF-REN-01 | Consulta de disponibilidad | ⚠️ p95 < 800 ms con 50 usuarios concurrentes | PR-01 (k6) | Carga medida: ~1.000 solicitudes/s en PC de desarrollo, quiebre a 700 VU. No demuestra esta meta p95 específica. |
| RNF-REN-02 | Creación de reserva bajo concurrencia | 0 sobreventas; ⚠️ p95 < 1,5 s con 20 confirmaciones simultáneas sobre el mismo bloque | PR-02 / PC-01 | Validado: una confirmación en 50/100/200 solicitudes al último cupo, cero sobreventas; meta p95 no certificada. |
| RNF-REN-03 | Frescura del panel del Personal | Reserva nueva visible en ≤ 30 s (polling 20 s) | PI-04 | Panel integrado con API real; el tiempo ≤30 s no tiene medición individual reportada aquí. |
| RNF-REN-04 | El pipeline BI/IA no afecta la operación | ETL en batch, solo lectura, fuera de horario operacional | PB-01 | ETL/DWH separado y KPI real validados; incremental OLTP en vivo queda para BI-06. |

## 5.4.4 Escalabilidad

| ID | Requisito |
|---|---|
| RNF-ESC-01 | Objetivo de escalabilidad horizontal sujeto al modelo real del Motor, cachés, workers y presencia de notificaciones; no se certifica coordinación entre instancias ni capacidad productiva. |
| RNF-ESC-02 | Multi-establecimiento desde el modelo: un Admin Local administra *n* locales (`admin_establecimiento`). |
| RNF-ESC-03 | El Motor serializa la operación por **establecimiento**, con relectura y verificación de factibilidad física; no es un bloqueo global de todos los locales. |
| RNF-ESC-04 | El modelo de datos retiene historial (eventos) para escalar de analítica descriptiva → predictiva sin rediseño. |

## 5.4.5 Disponibilidad y tolerancia a fallos

| ID | Requisito |
|---|---|
| RNF-DIS-01 | Base, Auth y Storage son gestionados; no se atribuye un SLA comercial al plan de pruebas ni al MVP1. |
| RNF-DIS-02 | Si el QR falla (cámara, luz, pantalla rota), existe validación manual por nombre, correo o código (RN-021). |
| RNF-DIS-03 | Los adaptadores mock son herramientas de desarrollo/pruebas. El ensayo general de cierre utilizó API real, sin pantallas simuladas; un modo degradado no sustituye esa evidencia. |
| RNF-DIS-04 | Errores con catálogo estable (catálogo del contrato HTTP vigente) para que la UI muestre mensajes comprensibles y no técnicos. |

## 5.4.6 Portabilidad

| ID | Requisito |
|---|---|
| RNF-POR-01 | Reproducibilidad de laboratorios con Docker validada; Compose integral incompleto (ver [5.5](05_docker.md)). |
| RNF-POR-02 | Versiones de Node y pnpm fijadas (`.nvmrc`, `packageManager`); CI usa las mismas versiones. |
| RNF-POR-03 | App móvil multiplataforma (Android/iOS) con Expo; Personal en teléfono o tablet. |
| RNF-POR-04 | Configuración por variables de entorno, sin rutas ni IP fijas en el código. |

## 5.4.7 Mantenibilidad y usabilidad

| ID | Requisito |
|---|---|
| RNF-MAN-01 | Vertical Slice Architecture: un slice por PR; lógica de negocio en `hooks/`, nunca en `ui/`. |
| RNF-MAN-02 | Contratos centralizados en `packages/contracts`; cambios requieren aprobación del PO. |
| RNF-MAN-03 | Todo código nuevo con prueba (Jest / pytest); lint y formato automáticos en pre-commit. |
| RNF-USA-01 | Zona horaria `America/Santiago`; el frontend nunca calcula zonas horarias (usa `inicio_at` del backend). |
| RNF-USA-02 | Interfaz en español de Chile, tema nocturno con contraste suficiente; ⚠️ meta de accesibilidad WCAG 2.1 AA en textos y botones principales (Sprint 6). |

## 5.4.8 Balance del cierre

**Alcance MVP1 cumplido y validado:** integración funcional, control transaccional de capacidad e idempotencia, autorización/MFA y BI en entorno de demostración. **Limitaciones identificadas:** la auditoría registra FAIL no bloqueantes (incluidos dependencias, revocación, cabeceras, registro de incidentes y EXIF) y pruebas físicas NO EJ. No se convierten en fallos del ciclo funcional ni se ocultan como PASS.

**Evolución y preproducción:** definir SLA y carga objetivo, completar endurecimiento, decisiones de privacidad/retención, pruebas nativas y despliegue productivo. Las referencias normativas son objetivos de diseño del proyecto, no una certificación de cumplimiento. La evidencia está fijada en el [índice de fuentes](../../README.md#fuentes-y-alcance).
