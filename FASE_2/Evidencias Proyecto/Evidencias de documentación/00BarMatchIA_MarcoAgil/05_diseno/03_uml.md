# 5.3 Diagramas UML mínimos

> Mermaid no tiene un tipo nativo de "casos de uso"; se representan con un *flowchart* usando la notación de actores y elipses.

## 5.3.1 Casos de uso

```mermaid
flowchart LR
  C(["👤 Cliente"])
  P(["👤 Personal del local"])
  AL(["👤 Admin Local"])
  AP(["👤 Admin Platform"])
  SCH(["⏱️ Sistema (scheduler)"])

  subgraph SYS["BarMatchIA — Release 1"]
    UC1(("Iniciar sesión"))
    UC2(("Explorar y ver ficha"))
    UC3(("Consultar disponibilidad"))
    UC4(("Reservar / Reservar Ahora"))
    UC5(("Cancelar reserva"))
    UC6(("Ver mis reservas"))
    UC7(("Inscribirse en lista de espera"))
    UC8(("Ver panel −2h/+2h"))
    UC9(("Check-in por QR"))
    UC10(("Validación manual"))
    UC11(("Asignar mesa / combinación"))
    UC12(("Registrar walk-in"))
    UC13(("Liberar / finalizar atención"))
    UC14(("Declarar indisponibilidad"))
    UC15(("Configurar establecimiento"))
    UC16(("Consultar reservas y timeline"))
    UC17(("Ver analítica y predicciones"))
    UC18(("Gestionar cuentas, locales y categorías"))
    UC19(("Marcar no-show"))
    UC20(("Autenticarse con MFA"))
  end

  C --- UC1 & UC2 & UC3 & UC4 & UC5 & UC6 & UC7
  P --- UC1 & UC8 & UC9 & UC10 & UC11 & UC12 & UC13 & UC14
  AL --- UC20 & UC14 & UC15 & UC16 & UC17
  AP --- UC20 & UC18
  SCH --- UC19
  UC4 -. "«include»" .-> UC3
  UC9 -. "«include»" .-> UC11
  UC10 -. "«extend»" .-> UC9
  UC12 -. "«include»" .-> UC11
  UC20 -. "«include»" .-> UC1
```

| Caso de uso | Actor | Brief | Estado (front / back) |
|---|---|---|---|
| UC2–UC6 | Cliente | 21.6.1, 21.6.2, 21.6.4, 21.6.15, 21.6.16 | Front ✅ / Back 🔄 |
| UC7 | Cliente | 21.6.3 | ⏳ Sprint 4 |
| UC8–UC13 | Personal | 21.6.6, 21.6.7, 21.6.8, 21.6.17 | Front ✅ / Back ⏳ |
| UC14 | Personal, Admin Local | 21.6.10 | ⏳ Sprint 4 |
| UC15–UC17, UC20 | Admin Local | 21.6.9, 21.6.11, 21.6.18, 21.6.13 | Front ✅ (mock) / Back ⏳ |
| UC18 | Admin Platform | 21.6.14, 21.6.19 | Scaffolding ✅ / Back ⏳ |
| UC19 | Sistema | 21.6.5 | ⏳ (`<TBD:scheduler>`) |

## 5.3.2 Diagrama de clases — acceso a datos en el frontend (puertos y adaptadores)

```mermaid
classDiagram
  direction LR
  class ReservasRepository {
    <<interface>>
    +listarEstablecimientos(q, comuna) EstablecimientoResumenDTO[]
    +obtenerDisponibilidad(id, fecha, personas) DisponibilidadDTO
    +crearReserva(dto: CrearReservaDTO) ReservaClienteDTO
    +misReservas(vigentes) ReservaClienteDTO[]
    +cancelar(codigo, dto) ReservaClienteDTO
  }
  class ReservasHttpRepository {
    -http: HttpClient
  }
  class ReservasMockRepository {
    -estado: Map
    -latenciaMs: number
  }
  class RepositoriosProvider {
    +dataSource: string
    +useRepositorios()
  }
  class HttpClient {
    +baseUrl: string
    +obtenerToken() string
    +http~T~(ruta, init) T
  }
  class ApiError {
    +codigo: CodigoError
    +status: number
  }
  ReservasRepository <|.. ReservasHttpRepository
  ReservasRepository <|.. ReservasMockRepository
  ReservasHttpRepository --> HttpClient
  HttpClient ..> ApiError : lanza
  RepositoriosProvider --> ReservasRepository : inyecta
```

El mismo patrón se repite en cada slice (`OperacionRepository`, `AtencionRepository`, `AdminReservasRepository`). La suite de contrato de Jest corre contra ambos adaptadores: si las dos pasan, cambiar de `mock` a `api` es solo configuración.

## 5.3.3 Diagrama de clases — dominio de la reserva (simplificado)

```mermaid
classDiagram
  class Cuenta {
    +id_cuenta
    +email
    +tipo_perfil: TipoPerfil
    +estado: EstadoCuenta
  }
  class Cliente {
    +fecha_nacimiento
    +genero
  }
  class Establecimiento {
    +nombre
    +comuna
    +holgura
    +duracion_referencia_min
    +estado
  }
  class Sector {
    +nombre
    +activo
  }
  class UnidadReservable {
    +codigo
    +capacidad
  }
  class CombinacionValida {
    +codigo
  }
  class Reserva {
    +codigo_reserva
    +qr_token
    +fecha_reserva
    +id_bloque
    +cantidad_personas_original
    +estado: EstadoReserva
    +origen: OrigenReserva
  }
  class EventoReserva {
    +tipo_evento
    +fecha_evento
    +detalle
  }
  class Atencion {
    +origen: OrigenAtencion
    +cantidad_vigente
    +estado
  }
  class Asignacion {
    +inicio_at
    +fin_at
  }
  Cuenta "1" -- "0..1" Cliente
  Cliente "1" -- "*" Reserva
  Establecimiento "1" -- "*" Sector
  Sector "1" -- "*" UnidadReservable
  Sector "1" -- "*" CombinacionValida
  CombinacionValida "*" -- "*" UnidadReservable
  Establecimiento "1" -- "*" Reserva
  Reserva "1" -- "1..*" EventoReserva
  Reserva "1" -- "0..1" Atencion
  Atencion "1" -- "*" Asignacion
```

## 5.3.4 Secuencia de la funcionalidad principal — crear reserva sin sobreventa

```mermaid
sequenceDiagram
  autonumber
  actor C as Cliente
  participant APP as App móvil (Expo)
  participant AUTH as Supabase Auth
  participant API as API NestJS
  participant DB as PostgreSQL

  C->>APP: Elige local, fecha, personas
  APP->>API: GET /establecimientos/:id/disponibilidad?fecha&personas
  API->>DB: Capacidad reservable − reservas vigentes por bloque
  DB-->>API: Bloques
  API-->>APP: DisponibilidadDTO (disponible, es_ahora, motivo)
  C->>APP: Confirma bloque
  APP->>AUTH: getSession() → access_token
  APP->>API: POST /reservas (Bearer JWT, CrearReservaDTO)
  API->>API: Guard: verifica JWT, carga cuenta, exige CLIENTE
  API->>DB: BEGIN
  API->>DB: pg_advisory_xact_lock(establecimiento + fecha)
  Note over API,DB: Una segunda transacción concurrente espera aquí
  API->>DB: Revalida capacidad de los bloques que ocupa la reserva
  alt Hay capacidad
    API->>DB: INSERT reserva (codigo_reserva, qr_token) + INSERT evento_reserva
    API->>DB: COMMIT (libera el candado)
    API-->>APP: 201 ReservaClienteDTO (sin id_reserva)
    APP-->>C: Confirmación con código y QR
  else Sin capacidad
    API->>DB: ROLLBACK
    API-->>APP: 409 SIN_CAPACIDAD
    APP-->>C: "Ese horario se acaba de llenar"
  end
```

## 5.3.5 Secuencia — check-in por QR y asignación

```mermaid
sequenceDiagram
  autonumber
  actor P as Personal
  participant APP as App tablet (Expo)
  participant API as API NestJS
  participant DB as PostgreSQL

  loop cada 20 s (polling)
    APP->>API: GET /operacion/reservas?desde=-2h&hasta=+2h
    API-->>APP: ReservaOperacionDTO[]
  end
  P->>APP: Escanea QR (expo-camera)
  APP->>API: POST /operacion/checkin { qr_token }
  API->>API: Guard PERSONAL + pertenencia al establecimiento
  API->>DB: Busca reserva por qr_token del mismo local
  alt QR válido y dentro de ventana (≥ −20 min)
    API->>DB: estado = VALIDADA + evento CHECK_IN
    API-->>APP: ReservaOperacionDTO
    APP->>API: Sugerencias de mesa/combinación
    P->>APP: Elige ubicación
    APP->>API: Asignar (transacción con lock, R-12)
    API-->>APP: Atención EN_ATENCION
  else QR de otro local o inexistente
    API-->>APP: 404 QR_INVALIDO (no revela datos)
    P->>APP: Búsqueda manual por nombre/correo/código
  end
```

## 5.3.6 Diagrama de componentes

```mermaid
flowchart TB
  subgraph mobile["apps/mobile"]
    direction TB
    M1["app/(cliente)"] --> MF["features/reservas · cancelacion · lista-espera"]
    M2["app/(personal)"] --> MO["features/operacion · checkin · atencion · walk-in"]
    MF & MO --> MS["shared/data<br/>HttpClient · RepositoriosProvider"]
    MF & MO --> MM["mocks/ (adaptadores en memoria)"]
  end
  subgraph web["apps/web-admin"]
    W1["routes/admin-local"] --> WF["features/reservas-admin · configuracion · mfa"]
    W2["routes/plataforma"] --> WP["features/plataforma"]
    W3["features/analitica (Recharts/D3)"]
    WF & WP & W3 --> WS["shared/data"]
  end
  subgraph api["apps/api (NestJS)"]
    A1["AuthGuard (JWT Supabase) + PerfilGuard"]
    A2["slices: me · establecimientos · disponibilidad · reservas · operacion · admin · analitica"]
    A3["Filtro global de errores · Swagger"]
    A1 --> A2
  end
  CT["packages/contracts"]
  MS --> A1
  WS --> A1
  mobile -.-> CT
  web -.-> CT
  api -.-> CT
```
