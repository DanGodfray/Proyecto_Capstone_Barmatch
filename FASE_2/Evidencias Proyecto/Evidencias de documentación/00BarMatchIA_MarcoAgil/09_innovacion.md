# 9. Innovación

> Sección exigida por la asignatura para el documento final de cierre.

## 9.1 ¿Qué problema resuelve?

Reduce la incertidumbre de una salida a un bar ("¿dónde voy?, ¿habrá espacio?, ¿cómo aseguro mi lugar?") mostrando **disponibilidad real** por bloques de 30 minutos y entregando una reserva verificable con QR. Para el establecimiento, centraliza reservas y operación del salón (llegadas, mesas, walk-in, no-show) y transforma esa operación diaria en información para decidir, problema hoy resuelto con WhatsApp, teléfono o planillas.

**Alcance académico:** la propuesta se justifica por la fragmentación de información y gestión operacional descrita en los requisitos. Las citas de prensa y encuestas del borrador no se utilizan aquí como evidencia de adopción ni de impacto comercial, pues no se verificaron sus referencias exactas en esta revisión.

## 9.2 ¿Qué hace diferente a la solución?

1. **Modela la operación real de un bar**, no solo la "mesa a una hora": sectores, unidades, combinaciones válidas de mesas, holgura, bloque "Ahora", walk-in que compite por la misma capacidad, período de gracia y no-show automático (62 reglas de negocio formalizadas en RN 21.11).
2. **Garantía de no sobreventa por diseño:** la confirmación revalida la capacidad dentro de una transacción con bloqueo por establecimiento y verificación de factibilidad física en PostgreSQL, en vez de confiar en la disponibilidad leída segundos antes.
3. **Ciclo completo en una plataforma:** del descubrimiento del cliente al check-in con cámara en la tablet del personal y la asignación de mesa; el mismo dato alimenta la analítica del dueño.
4. **Analítica e Inteligencia desacopladas:** BI con ETL/DWH sintético y KPI validados; ML-01/ML-04 BETA mediante servicio separado. ML-03 es informativo experimental, sin inferencia individual validada. Ninguno bloquea la confirmación de reservas.
5. **Privacidad como criterio de diseño:** BI sin PII directa y datos sintéticos declarados, autorización por perfil/local y resguardo de credenciales. La auditoría conserva mejoras de minimización, EXIF y DPIA; no se declara certificación normativa.

## 9.3 ¿Qué valor agrega?

| Para | Valor |
|---|---|
| Cliente | Menos tiempo y menos incertidumbre; reserva confirmada con QR; opción "Ahora" para salidas espontáneas. |
| Establecimiento | Menos gestión manual y menos errores de capacidad; registro estructurado; menos no-shows por reglas claras; analítica descriptiva y predictiva para planificar personal y promociones. |
| Ecosistema | Datos agregados de demanda útiles para socios futuros (distribuidores, integraciones), sin exponer datos personales. |
| Académico / profesional | Integra competencias del perfil de egreso: modelado de datos escalable, arquitectura documentada, desarrollo sistematizado con CI y pruebas, gestión ágil y certificación de calidad. |

## 9.4 Evolución post-MVP (no comprometida)

Galería editable, BI incremental/supresión, proveedor definitivo de push, recomendaciones personalizadas, suscripciones y despliegue comercial. El mapa móvil ya está implementado; su validación física no se confunde con una funcionalidad futura. Cualquier funcionalidad nueva (p. ej., *Promo on Demand*) requiere aprobación del PO como cambio de alcance.

## 9.5 Aporte demostrado del MVP1

La integración funcional, reserva con controles de concurrencia, flujos operacionales y dashboard con DWH de pruebas convierten la propuesta en un incremento demostrable. La auditoría respalda ensayo general 10/10 y barrido final 19/19; los indicadores y datos sintéticos se identifican en pantalla.

El valor comercial de §9.3 se expresa como beneficio esperado, no como impacto medido en locales reales. Las limitaciones de endurecimiento y pruebas físicas no invalidan el logro académico; el hosting productivo y las capacidades experimentales requieren sus propias decisiones y validaciones posteriores. Véanse [fuentes de cierre](../README.md#fuentes-y-alcance) y [síntesis de innovación](../07BarMatchIA_Innovacion/Innovacion_BarMatchIA.md).
