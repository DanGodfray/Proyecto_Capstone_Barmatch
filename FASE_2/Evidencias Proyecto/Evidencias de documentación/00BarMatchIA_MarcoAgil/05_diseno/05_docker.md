# 5.5 Docker y reproducibilidad del cierre MVP1

**Corte:** 15/11/2026; revisión documental 25/11. Este documento corrige la plantilla de despliegue inicial conforme al estado verificado de `develop` (`99056d0`). La explicación académica no distribuye Dockerfiles ni configuración privada.

## 5.5.1 Alcance cumplido

Docker aportó entornos desechables de PostgreSQL y Supabase Auth para los laboratorios SQL y las pruebas de API, Motor, permisos, MFA y BI. La auditoría documenta una CI local final de 31/31 pasos que incluye esas validaciones. Son herramientas de verificación del MVP1 integrado, distintas del hosting productivo.

OLTP, Auth y Storage de la demostración se gestionan en Supabase; el DWH de pruebas es separado del OLTP de la aplicación y mantiene ubicación provisional. La app Expo se ejecuta en emulador/dispositivo, no como contenedor productivo.

## 5.5.2 Estado de los artefactos

| Elemento | Estado respaldado y límite |
|---|---|
| Laboratorios PostgreSQL/Auth | Utilizados en QA; aislados del entorno compartido para carga y pruebas adversariales |
| Imagen web | `apps/web-admin/DockerFile` corregido el 12/11: multi-etapa, contexto raíz, runtime fijado y servicio estático sin privilegios. El estado privado registra prueba local; su validación en hosting real queda posterior |
| Compose general | Incompleto: conserva contexto/puerto web desalineados y referencia a Dockerfile de API no existente. No acredita arranque integral |
| API contenerizada | Posibilidad de evolución; no se presenta un Dockerfile de referencia como archivo real probado |
| BI/ML contenerizados | No se certifica un Compose productivo ni un job batch único para toda la Inteligencia |
| Redis | No es fuente de consistencia de reservas; PostgreSQL y el Motor conservan esa responsabilidad |

## 5.5.3 Corrección de las instrucciones iniciales

Se retiran la plantilla YAML y el Dockerfile didáctico porque no coincidían con los artefactos reales y podían interpretarse como instrucciones ejecutables. No se debe afirmar que `docker compose up` levanta toda la solución ni que PostgreSQL/Auth nunca se usaron en Docker: sí se usaron como laboratorios.

La operación se realiza con los procedimientos vigentes del equipo en el repositorio privado, bajo autorización y entornos preparados; no se publican conexiones, migraciones, claves ni recetas de habilitación. Ver [manual técnico académico](../08_manual_tecnico.md).

## 5.5.4 Evolución posterior

Alinear Compose, completar imágenes de servicios, validar hosting, HTTPS/HSTS, gestión de secretos y DWH de producción son pasos previos al despliegue comercial. No invalidan la integración y las pruebas del MVP1 académico.

Fuentes: [estado de Docker, auditoría y trazabilidad](../../README.md#fuentes-y-alcance). Síntesis complementaria: [Despliegue Docker](../../05BarMatchIA_Docker/Despliegue_Docker.md).
