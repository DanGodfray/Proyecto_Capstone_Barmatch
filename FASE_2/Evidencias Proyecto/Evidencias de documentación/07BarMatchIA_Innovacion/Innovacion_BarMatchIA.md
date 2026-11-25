# Innovación y propuesta de valor — BarMatchIA

## ¿Qué problema resuelve?

Las personas que buscan bares y establecimientos de entretenimiento encuentran información dispersa y poca certeza sobre cupos disponibles. Los locales suelen gestionar reservas, cancelaciones, llegadas y ocupación en diversos canales o registros manuales. BarMatchIA propone centralizar ambos lados de la experiencia.

## ¿Qué hace diferente a la solución?

- **Reserva y operación integradas:** búsqueda y disponibilidad, reserva verificable con QR, recepción, asignación, atención y walk-ins en un mismo ecosistema.
- **Capacidad consistente:** control transaccional sobre PostgreSQL para evitar la sobreventa incluso ante solicitudes simultáneas, con evidencia de pruebas.
- **Cuatro actores con permisos diferenciados:** Cliente, Personal, Administrador Local y Administrador de Plataforma.
- **Analítica operacional:** datos estructurados provenientes del flujo de atención, ETL, DWH e indicadores administrativos.
- **Inteligencia experimental:** pronósticos y riesgo de inasistencia como módulos BETA, sin hacer depender el motor de reservas de su disponibilidad.

## ¿Qué valor agrega?

| Beneficiario | Valor esperado |
|---|---|
| Clientes | Menor incertidumbre y menos gestiones dispersas para confirmar una visita |
| Personal | Visibilidad de reservas y apoyo a la recepción y ocupación del salón |
| Administradores de locales | Centralización y trazabilidad operativa; información para evaluar demanda y ocupación |
| Plataforma | Administración diferenciada de establecimientos y categorías |

## Evidencia y límites de alcance

El MVP1 se validó en un entorno de demostración con datos ficticios o sintéticos. No se declara adopción comercial, impacto medido en locales reales ni despliegue productivo. Funcionalidades futuras como Promo on Demand, EntreMesa, galería editable y otras integraciones deben distinguirse de lo implementado.

**Base documental:** `00BarMatchIA_MarcoAgil/09_innovacion.md`, reglas de negocio 21.11, PR #159, #163 y #169 del repositorio privado.
