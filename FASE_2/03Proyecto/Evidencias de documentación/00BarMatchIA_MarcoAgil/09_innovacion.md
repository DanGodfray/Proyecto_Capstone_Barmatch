# 9. Innovación

> Sección exigida por la asignatura para el documento final de cierre.

## 9.1 ¿Qué problema resuelve?

Reduce la incertidumbre de una salida a un bar ("¿dónde voy?, ¿habrá espacio?, ¿cómo aseguro mi lugar?") mostrando **disponibilidad real** por bloques de 30 minutos y entregando una reserva verificable con QR. Para el establecimiento, centraliza reservas y operación del salón (llegadas, mesas, walk-in, no-show) y transforma esa operación diaria en información para decidir, problema hoy resuelto con WhatsApp, teléfono o planillas.

**Contexto chileno:** el consumo de alcohol declarado está a la baja y los bares se resignifican como espacios de encuentro social, especialmente para adultos jóvenes (18–34) que reportan sensación de soledad (fuentes citadas en la presentación EA1: SENDA vía La Tercera, dic-2025; Encuesta Bicentenario UC 2025 — ⚠️ verificar citas exactas en el informe final). Un local que gestiona mejor su capacidad y su demanda compite por experiencia, no solo por consumo.

## 9.2 ¿Qué hace diferente a la solución?

1. **Modela la operación real de un bar**, no solo la "mesa a una hora": sectores, unidades, combinaciones válidas de mesas, holgura, bloque "Ahora", walk-in que compite por la misma capacidad, período de gracia y no-show automático (62 reglas de negocio formalizadas en RN 21.11).
2. **Garantía de no sobreventa por diseño:** la confirmación revalida la capacidad dentro de una transacción con candado por establecimiento + fecha en PostgreSQL, en vez de confiar en la disponibilidad leída segundos antes.
3. **Ciclo completo en una plataforma:** del descubrimiento del cliente al check-in con cámara en la tablet del personal y la asignación de mesa; el mismo dato alimenta la analítica del dueño.
4. **IA desacoplada y responsable:** predicciones batch sobre un DWH separado, con datos seudonimizados y sin afectar la operación; evolución progresiva *reglas → estadística → ML* que mitiga el arranque en frío.
5. **Privacidad desde el diseño** frente a la Ley 21.719: minimización por perfil en cada respuesta de la API, RLS en todas las tablas y DPIA antes de usar datos reales.

## 9.3 ¿Qué valor agrega?

| Para | Valor |
|---|---|
| Cliente | Menos tiempo y menos incertidumbre; reserva confirmada con QR; opción "Ahora" para salidas espontáneas. |
| Establecimiento | Menos gestión manual y menos errores de capacidad; registro estructurado; menos no-shows por reglas claras; analítica descriptiva y predictiva para planificar personal y promociones. |
| Ecosistema | Datos agregados de demanda útiles para socios futuros (distribuidores, integraciones), sin exponer datos personales. |
| Académico / profesional | Integra competencias del perfil de egreso: modelado de datos escalable, arquitectura documentada, desarrollo sistematizado con CI y pruebas, gestión ágil y certificación de calidad. |

## 9.4 Evolución post-MVP (no comprometida)

Notificaciones push, mapa en la app móvil, recomendaciones personalizadas al cliente, planes de suscripción en producción e integraciones externas. Cualquier funcionalidad nueva (p. ej., *Promo on Demand*) requiere aprobación del PO como cambio de alcance.
