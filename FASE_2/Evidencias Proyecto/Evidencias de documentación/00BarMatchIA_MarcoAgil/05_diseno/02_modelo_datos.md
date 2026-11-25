# 5.2 Modelo de datos

> **Fuente:** DDL PostgreSQL OLTP v1 (`BarMatchIA_PostgreSQL_ER_v1.sql`) y Diccionario de Datos OLTP — autor Raúl Guerra. Data Mart: Propuesta Preliminar Final — responsable Josefa Morales.
> Tipo de modelo: **Entidad–Relación clásico (SQL, PostgreSQL)**. 28 tablas + 1 vista en el OLTP.

## 5.2.1 Principios de diseño

1. **PostgreSQL es la fuente de verdad** de capacidad, reservas y estados.
2. **Separación de identidad y perfil:** `cuenta` (vinculada a Supabase Auth por `auth_user_id`) + una tabla por perfil (`cliente`, `personal_local`, `admin_local`, `admin_platform`).
3. **Tres identificadores de reserva:** `id_reserva` (interno, nunca se expone), `codigo_reserva` (legible) y `qr_token` (UUID, solo lo ve el cliente dueño).
4. **Historia por eventos:** cada cambio de estado de una reserva se registra en `evento_reserva` (actor, motivo, fecha). La cancelación tardía y el origen de la cancelación **se derivan** de estos eventos; no existen estados extra.
5. **Tiempo normalizado:** catálogo global `bloque_semana` de 336 bloques (7 días × 48 bloques de 30 min), `dia_semana` 1 = lunes … 7 = domingo, zona `America/Santiago`. La jornada nocturna se deriva de `horario_funcionamiento.dia_offset_cierre`.
6. **Integridad en la base:** restricciones `CHECK` para enumeraciones, rangos (latitud/longitud, `duracion_referencia_min` múltiplo de 30, `holgura ≥ 0`) y claves foráneas en todas las relaciones.
7. **Escalabilidad hacia analítica:** el modelo retiene historial completo (eventos y auditoría) para alimentar el DWH sin rediseño (mitiga el riesgo de *cold-start* del modelo predictivo).

## 5.2.2 Diagrama ER — núcleo (identidad, establecimiento, reserva y operación)

```mermaid
erDiagram
  cuenta ||--o| cliente : "perfil"
  cuenta ||--o| personal_local : "perfil"
  cuenta ||--o| admin_local : "perfil"
  cuenta ||--o| admin_platform : "perfil"
  establecimiento ||--o{ personal_local : "emplea"
  admin_local ||--o{ admin_establecimiento : "administra"
  establecimiento ||--o{ admin_establecimiento : "es administrado"
  establecimiento ||--o{ imagen_establecimiento : "tiene"
  establecimiento ||--o{ establecimiento_categoria : "clasifica"
  categoria ||--o{ establecimiento_categoria : "agrupa"
  establecimiento ||--o{ horario_funcionamiento : "abre"
  establecimiento ||--o{ sector : "se divide en"
  sector ||--o{ unidad_reservable : "contiene"
  sector ||--o{ combinacion_valida : "define"
  combinacion_valida ||--|{ combinacion_unidad : "une"
  unidad_reservable ||--o{ combinacion_unidad : "participa"
  cliente ||--o{ reserva : "realiza"
  establecimiento ||--o{ reserva : "recibe"
  bloque_semana ||--o{ reserva : "inicia en"
  reserva ||--|{ evento_reserva : "historial"
  motivo_cancelacion |o--o{ evento_reserva : "justifica"
  cuenta |o--o{ evento_reserva : "actor"
  reserva |o--o| atencion : "se atiende como"
  establecimiento ||--o{ atencion : "atiende"
  atencion ||--o{ asignacion : "ocupa"
  configuracion_fisica ||--o{ asignacion : "asignada"
  configuracion_fisica ||--|{ configuracion_fisica_unidad : "compuesta por"
  unidad_reservable ||--o{ configuracion_fisica_unidad : "forma"

  cuenta {
    bigint id_cuenta PK
    uuid auth_user_id UK
    varchar email UK
    varchar tipo_perfil "CLIENTE|PERSONAL|ADMIN_LOCAL|ADMIN_PLATFORM"
    varchar estado
  }
  cliente {
    bigint id_cliente PK
    bigint id_cuenta FK
    date fecha_nacimiento "18+"
    varchar genero
  }
  establecimiento {
    bigint id_establecimiento PK
    varchar nombre
    varchar comuna
    numeric latitud
    numeric longitud
    int holgura
    int duracion_referencia_min "multiplo de 30"
    boolean lista_espera_habilitada
    varchar estado
  }
  sector {
    bigint id_sector PK
    bigint id_establecimiento FK
    varchar nombre
  }
  unidad_reservable {
    bigint id_unidad PK
    bigint id_sector FK
    varchar codigo
    int capacidad
  }
  combinacion_valida {
    bigint id_combinacion PK
    bigint id_sector FK
    varchar codigo
  }
  horario_funcionamiento {
    bigint id_horario PK
    bigint id_establecimiento FK
    smallint dia_semana_inicio "1=lunes"
    time hora_apertura
    time hora_cierre
    smallint dia_offset_cierre "0 o 1"
  }
  bloque_semana {
    smallint id_bloque PK "336 bloques"
    smallint dia_semana
    time hora_inicio
  }
  reserva {
    bigint id_reserva PK
    varchar codigo_reserva UK
    uuid qr_token UK
    bigint id_cliente FK
    bigint id_establecimiento FK
    date fecha_reserva
    smallint id_bloque FK
    int cantidad_personas_original
    varchar estado
    varchar origen "NORMAL|AHORA|LISTA_ESPERA"
  }
  evento_reserva {
    bigint id_evento_reserva PK
    bigint id_reserva FK
    varchar tipo_evento
    bigint id_cuenta_actor FK
    bigint id_motivo_cancelacion FK
    timestamptz fecha_evento
  }
  atencion {
    bigint id_atencion PK
    bigint id_establecimiento FK
    bigint id_reserva FK "null si WALK_IN"
    varchar origen "RESERVA|WALK_IN"
    int cantidad_vigente
    varchar estado
  }
  asignacion {
    bigint id_asignacion PK
    bigint id_atencion FK
    bigint id_configuracion FK
    timestamptz inicio_at
    timestamptz fin_at
  }
  configuracion_fisica {
    bigint id_configuracion PK
    varchar tipo
  }
```

## 5.2.3 Diagrama ER — lista de espera, indisponibilidad y auditoría

```mermaid
erDiagram
  cliente ||--o{ solicitud_lista_espera : "solicita"
  establecimiento ||--o{ solicitud_lista_espera : "recibe"
  bloque_semana ||--o{ solicitud_lista_espera : "para"
  solicitud_lista_espera ||--o{ oportunidad_lista_espera : "genera"
  solicitud_lista_espera |o--o| reserva : "se convierte en"
  establecimiento ||--o{ indisponibilidad : "declara"
  sector |o--o{ indisponibilidad : "alcance"
  unidad_reservable |o--o{ indisponibilidad : "alcance"
  combinacion_valida |o--o{ indisponibilidad : "alcance"
  motivo_indisponibilidad ||--o{ indisponibilidad : "motivo"
  cuenta ||--o{ evento_auditoria : "actor"
  establecimiento |o--o{ evento_auditoria : "contexto"

  solicitud_lista_espera {
    bigint id_solicitud PK
    date fecha_solicitada
    smallint id_bloque FK
    int cantidad_personas
    varchar estado
  }
  oportunidad_lista_espera {
    bigint id_oportunidad PK
    bigint id_solicitud FK
    timestamptz inicio_prioridad_at
    timestamptz fin_prioridad_at
    varchar estado
  }
  indisponibilidad {
    bigint id_indisponibilidad PK
    varchar alcance "UNIDAD|COMBINACION|SECTOR|ESTABLECIMIENTO"
    timestamptz inicio_at
    timestamptz fin_at
    varchar estado
  }
  evento_auditoria {
    bigint id_evento_auditoria PK
    varchar tipo_accion
    varchar tipo_entidad_afectada
    varchar resultado
  }
```

**Vista `vw_capacidad_establecimiento`:** capacidad física (suma de unidades activas en sectores activos) menos `holgura` = capacidad reservable. ⚠️ Las vistas no respetan RLS por defecto: se crea con `security_invoker = on` o se revoca su acceso a `anon`/`authenticated` (CONTEXTO_IA §11).

## 5.2.4 Ciclo de estados de la reserva

```mermaid
stateDiagram-v2
  [*] --> CONFIRMADA : POST /reservas
  CONFIRMADA --> VALIDADA : check-in (QR o manual)
  CONFIRMADA --> CANCELADA : cliente o local
  CONFIRMADA --> NO_SHOW : gracia 15 min vencida
  VALIDADA --> PENDIENTE_ASIGNACION : falla asignación automática
  VALIDADA --> EN_ATENCION : mesa asignada
  PENDIENTE_ASIGNACION --> EN_ATENCION : asignación manual
  EN_ATENCION --> FINALIZADA : liberar / finalizar
  CANCELADA --> [*]
  NO_SHOW --> [*]
  FINALIZADA --> [*]
```
> ⚠️ **Validar Raúl:** transiciones excepcionales (RN-005 validación anticipada, RN-008 llegada posterior al no-show, RN-023 resolución operacional) no se dibujan para mantener el diagrama legible.

## 5.2.5 Modelo analítico (Data Mart, preliminar)

> Responsable: **Josefa Morales**. Estado: propuesta preliminar final; ubicación física del DWH `<TBD:dwh-ubicacion>` (KAN-44). Este resumen no reemplaza su documento.

- **Separación física y de credenciales** respecto del OLTP; el ETL lee en modo solo lectura.
- **Hechos propuestos:** `FACT_RESERVA` (1 fila por reserva), `FACT_ATENCION`, `FACT_USO_FISICO`, `FACT_LISTA_ESPERA`, `FACT_INDISPONIBILIDAD`, `FACT_OPERACION_BLOQUE`.
- **Dimensiones:** `DIM_FECHA`, `DIM_BLOQUE_HORARIO`, `DIM_ESTABLECIMIENTO` (SCD tipo 2), `DIM_SECTOR`, `DIM_UNIDAD_RESERVABLE`, `DIM_CLIENTE`, `DIM_CATEGORIA`, `DIM_MOTIVO_CANCELACION`, `DIM_MOTIVO_INDISPONIBILIDAD`.
- **Privacidad (Matriz #25):** sin PII directa. `DIM_CLIENTE` solo con rango etario, género agregado y clave seudonimizada (hash con sal). `qr_token` no se traslada al Data Mart.

```mermaid
erDiagram
  DIM_FECHA ||--o{ FACT_RESERVA : ""
  DIM_BLOQUE_HORARIO ||--o{ FACT_RESERVA : ""
  DIM_ESTABLECIMIENTO ||--o{ FACT_RESERVA : ""
  DIM_CLIENTE ||--o{ FACT_RESERVA : "seudonimizado"
  DIM_MOTIVO_CANCELACION |o--o{ FACT_RESERVA : ""
  FACT_RESERVA {
    int sk_fecha_reserva FK
    int sk_bloque FK
    int sk_establecimiento FK
    int sk_cliente FK
    int cantidad_personas_original
    int indicador_cancelada
    int indicador_no_show
    int indicador_ahora
    int anticipacion_minutos
  }
```
