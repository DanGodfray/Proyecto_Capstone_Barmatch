# 1. Product Vision — BarMatchIA

> Extensión objetivo: 2 páginas. Fuente: Documento Maestro v0.2.1 (§3–§10), RN 21.11, Matriz de alineamiento.

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

B2B2C: el **consumidor usa la plataforma gratis** y el **establecimiento paga** una suscripción mensual (Plan Base: perfil, reservas, disponibilidad, operación; Plan Avanzado: indicadores, históricos, predicciones y recomendaciones). La implementación comercial **no forma parte del MVP** (Documento Maestro §10).

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

**Fuera del MVP (decisión del PO):** reseñas y valoraciones, pagos en línea, notificaciones push (MVP = correo + estado en app), Deep Learning sin justificación, integraciones externas, despliegue comercial. Funcionalidades nuevas (p. ej., *Promo on Demand*) solo entran como cambio de alcance aprobado por el PO.

## 1.8 Indicadores de éxito del MVP

| Indicador | Meta |
|---|---|
| Reservas simultáneas sobre el último cupo | 0 casos de sobreventa en la prueba de concurrencia |
| Acceso indebido entre perfiles | 100 % de intentos rechazados (403/404) en pruebas de seguridad |
| Ciclo central demostrable | Guion de demo completo sin intervención manual en la base de datos |
| Datos personales en BI/IA | 0 campos de identificación directa en el DWH |
| Panel del Personal | Reserva nueva visible en ≤ 30 s (polling) |
