# 5.4 Requisitos no funcionales (RNF)

> Los valores **fijados** provienen de la Matriz de alineamiento, RN 21.11 y CONTEXTO_IA. Las **metas cuantitativas** marcadas con ⚠️ son propuestas del rol QA y deben validarse con el equipo antes del informe final.
> Columna *Verificación*: cómo se demuestra que se cumple (enlaza con el [plan de pruebas](../07_pruebas/README.md)).

## 5.4.1 Seguridad

| ID | Requisito | Verificación | Estado |
|---|---|---|---|
| RNF-SEG-01 | Toda ruta protegida exige `Authorization: Bearer <JWT de Supabase>`; la API verifica firma y vigencia del token. | PS-01, PS-02 | 🔄 (KAN-78) |
| RNF-SEG-02 | Autorización por **4 perfiles** en cada endpoint (guard + decorador de perfil) y validación de **pertenencia al establecimiento** (Personal: el suyo; Admin Local: sus asociados). La protección de rutas en el frontend es solo de experiencia de usuario. | PS-03, PS-04 | 🔄 |
| RNF-SEG-03 | **RLS** activado en todas las tablas de `public` sin políticas desde el día 1 + revocación de privilegios de `anon` y `authenticated` (tablas, secuencias, funciones, vistas). La *anon key* no puede leer tablas. | PS-05 | ⏳ (KAN-75) |
| RNF-SEG-04 | Las apps usan supabase-js **solo para Auth**; ningún secreto (`service_role`, `DATABASE_URL`) en `apps/mobile` ni `apps/web-admin`. | Revisión de PR + `grep` en CI (PS-06) | ✅ |
| RNF-SEG-05 | **Minimización por DTO:** `id_reserva` nunca se expone; `qr_token` solo en la vista del cliente dueño; el Personal ve solo el nombre del cliente. Una reserva ajena responde `NO_ENCONTRADO` (no revela existencia → previene IDOR). | PS-07, PS-08 | Front ✅ / Back 🔄 |
| RNF-SEG-06 | MFA obligatorio para perfiles administrativos (RN-043); política de contraseña, bloqueo y recuperación (RN-042) delegada en Supabase Auth. | PS-09 | Front (mock) ✅ / Back ⏳ |
| RNF-SEG-07 | Trazabilidad: `evento_reserva` para cada cambio de estado y `evento_auditoria` para acciones administrativas (RN-048, RN-062). | PI-06 | ⏳ |
| RNF-SEG-08 | Cadena de suministro: lockfile obligatorio; scripts de instalación de dependencias deshabilitados salvo aprobación explícita (`pnpm approve-builds`). | Revisión de PR | ✅ |
| RNF-SEG-09 | `.env` nunca se commitea; existe `.env.example` por app. | Pre-commit + revisión | ✅ |
| RNF-SEG-10 | Tokens en móvil: almacenamiento por defecto de supabase-js solo con datos ficticios; migrar a almacenamiento cifrado antes de usar datos reales. Sesión web `<TBD:sesion-web>`. | Checklist de release | ⏳ |

## 5.4.2 Privacidad y cumplimiento normativo

| ID | Requisito | Verificación | Estado |
|---|---|---|---|
| RNF-PRI-01 | Diseño contra la **Ley 21.719** (reforma de la Ley 19.628, vigencia 01/12/2026): principio de minimización, finalidad y registro de tratamiento. | Revisión documental | 🔄 |
| RNF-PRI-02 | El DWH no contiene PII directa: rango etario, género agregado y clave seudonimizada (hash con sal). | PB-02 | ⏳ (KAN-51) |
| RNF-PRI-03 | Datos de desarrollo y demo **ficticios o sintéticos**, declarados como tales en la UI ("Datos sintéticos"). | Revisión visual | ✅ (front) |
| RNF-PRI-04 | El perfilamiento con IA (predicción de no-show/recomendaciones) requiere **Evaluación de Impacto (DPIA)** antes de usar datos reales. | Documento DPIA `<TBD:dpia>` | ⏳ |
| RNF-PRI-05 | Edad mínima de registro: 18 años cumplidos (Matriz #15D). | PU-xx (validación de formulario) + backend | 🔄 |
| RNF-PRI-06 | Locales de OpenStreetMap usados en la demo muestran el crédito `© OpenStreetMap contributors, ODbL 1.0` y no se presentan como afiliados. | Revisión visual | ✅ |

## 5.4.3 Rendimiento

| ID | Requisito | Meta | Verificación | Estado |
|---|---|---|---|---|
| RNF-REN-01 | Consulta de disponibilidad | ⚠️ p95 < 800 ms con 50 usuarios concurrentes | PR-01 (k6) | ⏳ |
| RNF-REN-02 | Creación de reserva bajo concurrencia | 0 sobreventas; ⚠️ p95 < 1,5 s con 20 confirmaciones simultáneas sobre el mismo bloque | PR-02 / PC-01 | Front ✅ (KAN-29) / Back ⏳ |
| RNF-REN-03 | Frescura del panel del Personal | Reserva nueva visible en ≤ 30 s (polling 20 s) | PI-04 | ✅ (mock) |
| RNF-REN-04 | El pipeline BI/IA no afecta la operación | ETL en batch, solo lectura, fuera de horario operacional | PB-01 | ⏳ |

## 5.4.4 Escalabilidad

| ID | Requisito |
|---|---|
| RNF-ESC-01 | La API es *stateless* (sin estado en memoria entre peticiones en modo `db`), por lo que puede replicarse horizontalmente. |
| RNF-ESC-02 | Multi-establecimiento desde el modelo: un Admin Local administra *n* locales (`admin_establecimiento`). |
| RNF-ESC-03 | El candado de concurrencia es por **establecimiento + fecha**, no global: locales distintos no se bloquean entre sí. |
| RNF-ESC-04 | El modelo de datos retiene historial (eventos) para escalar de analítica descriptiva → predictiva sin rediseño. |

## 5.4.5 Disponibilidad y tolerancia a fallos

| ID | Requisito |
|---|---|
| RNF-DIS-01 | La base de datos, Auth y Storage se apoyan en el SLA del servicio gestionado (Supabase Cloud). |
| RNF-DIS-02 | Si el QR falla (cámara, luz, pantalla rota), existe validación manual por nombre, correo o código (RN-021). |
| RNF-DIS-03 | Modo de demostración degradado: `API_DATA_MODE=memory` en la API o `DATA_SOURCE=mock` en las apps, para presentar aunque falle la conexión a la base. |
| RNF-DIS-04 | Errores con catálogo estable (`<TBD:errores>`) para que la UI muestre mensajes comprensibles y no técnicos. |

## 5.4.6 Portabilidad

| ID | Requisito |
|---|---|
| RNF-POR-01 | Servicios propios ejecutables con **Docker Compose** en cualquier máquina con Docker (ver [5.5](05_docker.md)). |
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
