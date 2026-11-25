# 1. Product Vision — BarMatchIA

> **Corte técnico:** 15/11/2026; revisión documental 25/11/2026. Requisitos: Documento Maestro v0.2.1, RN 21.11 y Matriz de alineamiento. Resultados: [fuentes de cierre](../README.md#fuentes-y-alcance).

## 1.1 Declaración de visión

> **Para** personas que quieren salir a un bar con certeza de tener lugar, **y para** los bares y pubs pequeños y medianos que administran su capacidad de forma manual, **BarMatchIA es** una plataforma de descubrimiento, reserva y gestión operacional **que** reduce la incertidumbre de "¿habrá espacio?" y convierte la operación diaria del local en conocimiento para decidir. **A diferencia de** plataformas de reserva genéricas o la gestión por WhatsApp y teléfono, **nuestro producto** modela la operación real de un bar (bloques de 30 minutos, sectores, combinaciones de mesas, walk-in, no-show) y agrega una capa analítica que evoluciona de lo descriptivo a lo predictivo.

## 1.2 Problema

| Perspectiva | Problema |
|---|---|
| Cliente | Información distribuida entre redes sociales, mapas y llamadas; sin un filtro único por experiencia, ubicación y **disponibilidad real**; la reserva informal no da garantía al llegar. La secuencia "Quiero salir → ¿Dónde voy? → ¿Habrá espacio? → ¿Cómo aseguro mi lugar?" no tiene una sola respuesta. |
| Establecimiento | Reservas, ocupación, cancelaciones y no-shows se usan solo para operar la noche. No se transforman en conocimiento accionable (qué días concentran demanda, qué franjas fallan, qué tasa de no-show tiene). Además, la gestión manual de reservas simultáneas produce errores de **sobreventa** de capacidad. |

> *"No es un problema de falta de información: es información fragmentada para el cliente e información desaprovechada para el establecimiento."*

## 1.3 Usuarios (4 perfiles del sistema)

| Perfil | Canal | Necesidad principal |
|---|---|---|
| `CLIENTE` | App móvil | Descubrir, ver disponibilidad real, reservar, llegar con QR, cancelar |
| `PERSONAL` (del local) | App móvil / tablet | Ver reservas próximas (−2 h / +2 h), validar llegadas con QR, asignar mesas, atender walk-in |
| `ADMIN_LOCAL` | Web | Configurar el local, supervisar la operación, consultar reservas, ver analítica |
| `ADMIN_PLATFORM` | Web | Gestionar cuentas, establecimientos, categorías y trazabilidad global |

## 1.4 Propuesta de valor

| Para el cliente | Para el establecimiento |
|---|---|
| Ahorro de tiempo y menos incertidumbre | Nuevo canal digital de llegada de clientes |
| Descubrimiento por experiencia y ubicación | Menos gestión manual y menos errores de capacidad |
| Disponibilidad clara por bloque, incluida la opción "Ahora" | Registro estructurado e historial operacional |
| Reserva confirmada con código y QR | Analítica descriptiva → predictiva → recomendaciones |

**Capa analítica en tres niveles:** (1) *descriptiva*: qué pasó (ocupación, reservas por estado, cancelaciones); (2) *predictiva*: qué podría pasar (demanda, probabilidad de no-show); (3) *recomendaciones*: qué hacer (p. ej., una franja con demanda baja recurrente).

## 1.5 Modelo de sostenibilidad (contexto académico)

B2B2C: el **consumidor usa la plataforma gratis** y se propone que el **establecimiento pague** una suscripción mensual (Plan Base: perfil, reservas, disponibilidad, operación; Plan Avanzado: indicadores, históricos, predicciones y recomendaciones). La implementación comercial **no forma parte del MVP** (Documento Maestro §10).

## 1.6 Objetivos del producto

**Objetivo general.** Desarrollar el MVP de una plataforma B2B2C que permita descubrir establecimientos, reservar con disponibilidad real y gestionar la operación de reservas del local, generando datos históricos estructurados que alimenten una capa analítica desacoplada.

**Objetivos específicos.**
1. Implementar el ciclo de reserva de punta a punta (explorar → disponibilidad → reservar → QR → check-in → atención → cierre) respetando las RN 21.11.
2. Garantizar la consistencia de capacidad ante reservas simultáneas (sin sobreventa) con PostgreSQL como fuente de verdad.
3. Aplicar autorización por 4 perfiles en cada endpoint y minimización de datos conforme a la Ley 19.628 y la Ley 21.719.
4. Construir un pipeline batch ETL → DWH → KPIs que no afecte el rendimiento transaccional, con datos seudonimizados.
5. Entregar el sistema reproducible (Docker para servicios propios), documentado y probado (pruebas unitarias, de integración, de concurrencia y de seguridad).

## 1.7 Alcance del Release 1 (MVP)

**Incluye:** exploración y ficha pública; disponibilidad por bloques y "Ahora"; reserva y cancelación (con cancelación tardía derivada); check-in por QR y validación manual; asignación de mesas y atención; walk-in; no-show automático; panel del Personal con *polling*; consulta administrativa de reservas; configuración del establecimiento; analítica sobre datos sintéticos declarados.

**Fuera del MVP (decisión del PO):** reseñas y valoraciones, pagos en línea, proveedor definitivo de push y activación operacional del correo (componentes implementados, con entregas apagadas en el ensayo), Deep Learning sin justificación, integraciones externas, despliegue comercial. Funcionalidades nuevas (p. ej., *Promo on Demand*) solo entran como cambio de alcance aprobado por el PO.

## 1.8 Indicadores de éxito del MVP

| Indicador | Meta |
|---|---|
| Reservas simultáneas sobre el último cupo | 0 casos de sobreventa en la prueba de concurrencia |
| Acceso indebido entre perfiles | 100 % de intentos rechazados (403/404) en pruebas de seguridad |
| Ciclo central demostrable | Guion de demo completo sin intervención manual en la base de datos |
| Datos personales en BI/IA | 0 campos de identificación directa en el DWH |
| Panel del Personal | Reserva nueva visible en ≤ 30 s (polling) |

## 1.9 Alcance MVP1 cumplido y validado

La integración del MVP1 y la auditoría del 15/11 respaldan el ciclo de reserva, cancelación y operación, acceso por cuatro perfiles, MFA administrativo y analítica sobre DWH de pruebas. El ensayo general sin mocks obtuvo 10/10 y el barrido final 19/19. Las metas de §1.8 no son un SLA: sobreventa y acceso cuentan con pruebas de laboratorio específicas; el tiempo de actualización del panel no recibe una certificación individual por inferencia de esas cifras.

BI-03 cargó el histórico Parquet sintético y la 006 permitió leer KPI desde la API. El DWH sigue en una ubicación provisional. El mapa móvil existe en la solución revisada; permisos, escáner y comportamiento en Android/iOS físico conservan casos NO EJ.

ML-01/ML-04 tienen integración BETA con horizonte limitado; ML-03 ofrece un estado experimental, sin inferencia individual ni precisión operacional acreditada. La auditoría conserva la verificación manual de Inteligencia «Mañana» como pendiente, aunque el acta del 12/11 registra pruebas del servicio para hoy/D+1: esos alcances se distinguen.

Las limitaciones de endurecimiento y verificaciones físicas se registran con su impacto no bloqueante; no invalidan la integración funcional demostrada.

No-show y notificaciones cuentan con implementación y laboratorios; jobs, correo y entregas no se asumen activados en la demo. Galería editable, BI incremental, supresión hasta el DWH y hosting productivo son evolución MVP2. El [Product Backlog](02_product_backlog.md) detalla las reservas de validación por historia.
