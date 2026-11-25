# 5. Documento de diseño — índice

| Sección | Archivo | Elemento obligatorio del Anexo |
|---|---|---|
| Arquitectura, componentes y comunicación | [01_arquitectura.md](01_arquitectura.md) | 1. Arquitectura |
| Modelo de datos (ER OLTP + Data Mart) | [02_modelo_datos.md](02_modelo_datos.md) | 2. Modelo de datos |
| Diagramas UML | [03_uml.md](03_uml.md) | 3. UML mínimos |
| Requisitos no funcionales y seguridad | [04_requisitos_no_funcionales.md](04_requisitos_no_funcionales.md) | 4. RNF |
| Diseño Docker | [05_docker.md](05_docker.md) | 5. Docker |

Los diagramas están escritos en **Mermaid**, que GitHub renderiza directamente en el navegador (no requiere imágenes). Para exportarlos a PNG/PDF para el informe: <https://mermaid.live>.

## Lectura del cierre MVP1

Revisión del 25/11 contra `develop` (`99056d0`), corte técnico 15/11. Los diseños existentes se conservan como vistas conceptuales y se contrastan con [fuentes y actas](../../README.md#fuentes-y-alcance); no se publica código ni configuración del repositorio privado.

La integración, Motor, flujos operacionales y BI tienen validación en laboratorio y demostración. Las limitaciones no bloqueantes se contextualizan por entorno. El diagrama PNG corresponde a una presentación anterior. El ER v1 no es un inventario completo de migraciones finales. Docker integral, hosting productivo e incremental BI son evolución posterior, no condiciones que invaliden el MVP1 validado.
