# Arquitectura de BarMatchIA — evidencia de cierre MVP1

**Corte documental:** 25/11/2026. **Alcance:** proyecto académico, entorno de demostración, no producción comercial.

## Evidencia gráfica

La imagen `ArquitecturaBarMatchIA_MVP.png`, ya publicada en esta carpeta, corresponde al diagrama utilizado en una presentación previa. **No se declara como diagrama definitivo de la versión implementada.** La arquitectura consolidada se describe a continuación.

## Componentes y responsabilidades

| Componente | Tecnología | Responsabilidad |
|---|---|---|
| Aplicación móvil | React Native y Expo | Cliente y Personal del Local: exploración, reservas, QR, check-in y operación |
| Aplicación web | React con Vite | Administrador Local y Administrador de Plataforma; configuración y panel analítico |
| API | NestJS | Autorización, reglas de negocio, transacciones, control de capacidad y exposición REST |
| Base operacional | PostgreSQL en Supabase | Persistencia OLTP; fuente de verdad de reservas, establecimientos y operación |
| Servicios gestionados | Supabase Auth y Storage | Identidad y almacenamiento de imágenes |
| Analítica | Python, ETL, Data Warehouse separado | Transformación de datos, indicadores y consultas de BI |
| Inteligencia | Modelos ML integrados en la solución | Predicción experimental, separada de las transacciones de reserva |

## Flujo de comunicación

1. Las aplicaciones móviles y web autentican mediante Supabase Auth y se comunican con la API REST.
2. La API valida identidad, perfil y pertenencia al establecimiento antes de ejecutar operaciones.
3. PostgreSQL aplica transacciones y controles de concurrencia para evitar la sobreventa.
4. Los datos de operación se transforman mediante ETL hacia el DWH de pruebas; el panel administrativo obtiene KPI por una ruta protegida de la API.
5. La Inteligencia BETA está desacoplada de la operación crítica; su indisponibilidad no debe bloquear las reservas.

## Decisiones técnicas

- Organización por funcionalidades (Vertical Slice) y contratos compartidos.
- PostgreSQL como fuente de verdad: no se utilizó Redis para mantener la consistencia del aforo.
- Cliente y Personal en móvil; administradores en web.
- Datos operacionales y analíticos separados.
- Cuatro perfiles: Cliente, Personal, Admin Local y Admin Platform.

## Alcance demostrado y limitaciones

La auditoría de cierre del 15/11 documentó un ensayo general sin mocks contra la API real y el DWH de pruebas, con 10/10 casos satisfactorios. No equivale a un despliegue productivo. Quedaron pendientes varios controles adicionales de seguridad, despliegue público y pruebas en dispositivos físicos. La galería editable permanece reservada para MVP2.

**Fuentes de trazabilidad:** documentación académica `00BarMatchIA_MarcoAgil/05_diseno/01_arquitectura.md`; repositorio privado de desarrollo, PR #159, #163, #169 y `docs/pruebas/auditoria-mvp1-2026-11-15.md` (consultables por el equipo; código fuente no incluido en esta entrega).
