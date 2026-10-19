# 8. Manual técnico y de despliegue

> Extensión objetivo: 3–5 páginas. Repositorio de código: `barmatchia-mvp` (`<TBD:url-repo-desarrollo>`).
> ⚠️ **Validar contra el repo:** los comandos de la API dependen de los scripts de la rama de Raúl (`<TBD:confirmar-scripts-api>`).

## 8.1 Requisitos previos

| Herramienta | Versión | Para qué |
|---|---|---|
| Node.js | La indicada en `.nvmrc` | Ejecutar app, web y API |
| pnpm | La indicada en `packageManager` del `package.json` raíz (se activa con `corepack enable`) | Gestor del monorepo |
| Docker + Docker Compose | Estable reciente | Servicios propios contenerizados |
| Python | `<TBD:version-python>` | ETL, DWH y ML |
| Expo Go (en el teléfono) o emulador Android | Compatible con el SDK de Expo del proyecto | Ejecutar la app móvil |
| Cuenta en el proyecto de Supabase | — | URL y *anon key* (las entrega Raúl) |

## 8.2 Estructura del monorepo

```
barmatchia-mvp/
├── apps/
│   ├── mobile/       # Expo: Cliente + Personal
│   ├── web-admin/    # React + Vite: Admin Local + Admin Platform (+ analítica)
│   ├── api/          # NestJS
│   └── bi/           # Python: generador, ETL, ML
├── packages/
│   └── contracts/    # @barmatchia/contracts: tipos, reglas, errores, fixtures
├── docs/             # ADR y contexto normativo para IA
├── pnpm-workspace.yaml   # packages: apps/*, packages/*  ·  nodeLinker: hoisted
└── README.md
```

## 8.3 Instalación paso a paso

```bash
git clone <TBD:url-repo-desarrollo> barmatchia-mvp
cd barmatchia-mvp
corepack enable              # activa la versión de pnpm fijada en package.json
pnpm install                 # SIEMPRE desde la raíz (instala todas las apps)
pnpm approve-builds          # aprueba solo los scripts de instalación necesarios (seguridad de dependencias)
pnpm contracts:build         # compila packages/contracts a dist/ (obligatorio antes de correr apps o tests)
```

> **¿Por qué `nodeLinker: hoisted`?** Expo y Jest esperan encontrar las dependencias en un `node_modules` "plano". Desde pnpm v11 esta opción se lee **solo** desde `pnpm-workspace.yaml` (no desde `.npmrc`).

## 8.4 Variables de entorno

Cada app tiene un `.env.example`. Copiarlo a `.env` y completar. **Nunca** commitear `.env`.

| App | Variable | Valor | ¿Secreta? |
|---|---|---|---|
| mobile | `EXPO_PUBLIC_DATA_SOURCE` | `api` o `mock` | No |
| mobile | `EXPO_PUBLIC_API_URL` | `http://<IP-LAN-del-notebook>:3000` (no `localhost`: el teléfono es otro equipo) | No |
| mobile | `EXPO_PUBLIC_SUPABASE_URL`, `EXPO_PUBLIC_SUPABASE_ANON_KEY` | Del proyecto Supabase | No (pública, protegida por RLS) |
| web-admin | `VITE_DATA_SOURCE`, `VITE_API_URL`, `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY` | Ídem | No |
| api | `API_DATA_MODE` | `db` (base real) o `memory` (demo con fixtures) | No |
| api | `DATABASE_URL` | Cadena de conexión a PostgreSQL de Supabase | **Sí** |
| api | `SUPABASE_URL`, `SUPABASE_JWT_SECRET` o JWKS | Verificación del JWT | **Sí** |
| bi | Credencial OLTP **de solo lectura** + credencial del DWH | `<TBD:dwh-ubicacion>` | **Sí** |

> Las variables con prefijo `EXPO_PUBLIC_` o `VITE_` quedan visibles dentro de la app compilada. Por eso **nunca** se pone ahí la `service_role` ni `DATABASE_URL`.

## 8.5 Ejecución local

```bash
# API (NestJS) — escucha en 0.0.0.0:3000 para que el teléfono la alcance por la red local
pnpm --filter api start:dev            # <TBD:confirmar-scripts-api>
# Documentación OpenAPI/Swagger: http://localhost:3000/<TBD:ruta-swagger>

# App móvil (Expo): escanear el QR de la terminal con Expo Go
pnpm --filter mobile start

# Web administrativa
pnpm --filter web-admin dev            # abre http://localhost:5173

# BI (Python)
cd apps/bi
python -m venv .venv && source .venv/bin/activate   # en Windows: .venv\Scripts\activate
pip install -r requirements.txt
python -m <TBD:modulo-generador>       # genera datos sintéticos con semilla fija
python -m <TBD:modulo-etl>             # OLTP (lectura) → DWH → KPIs
```

**Modos de datos para demostración** (de más a menos realista):
1. `DATA_SOURCE=api` + `API_DATA_MODE=db` — API contra la base real.
2. `DATA_SOURCE=api` + `API_DATA_MODE=memory` — misma API y rutas, estado en memoria compartido entre dispositivos.
3. `DATA_SOURCE=mock` — sin API; estado por dispositivo (solo si la API no está disponible).

## 8.6 Ejecución con Docker

```bash
docker compose build                   # construye las imágenes de api, web-admin y bi
docker compose up -d                   # levanta api y web-admin en segundo plano
docker compose logs -f api             # ver logs de la API
docker compose run --rm bi             # ejecuta el job batch de BI una vez
docker compose down                    # detiene y elimina los contenedores
```
Diseño y plantilla comentada: [05_diseno/05_docker.md](05_diseno/05_docker.md).

## 8.7 Pruebas y calidad

```bash
pnpm -r lint                           # lint en todo el monorepo
pnpm --filter mobile test              # Jest (jest-expo)
pnpm --filter web-admin test
pnpm --filter api test                 # <TBD:confirmar-scripts-api>
cd apps/bi && ruff check . && pytest -q
```
Cada commit ejecuta `lint-staged` (Husky). CI repite lint, `contracts:build` y pruebas de todas las apps.

## 8.8 Flujo de ramas y publicación

| Tipo | Convención |
|---|---|
| Funcionalidad | `feature/<app>-<slice>` (app ∈ `mobile`, `web`, `api`, `bi`, `contracts`) |
| Corrección | `fix/<app>-<desc>` |
| Documentación / mantenimiento | `docs/<desc>`, `chore/<desc>` |

PR → `develop` (integración). `develop` → `main` solo en hitos, con *tag* (p. ej., `demo-2026-10-05`).

## 8.9 Problemas frecuentes

| Síntoma | Causa | Solución |
|---|---|---|
| Jest o Metro no encuentran un paquete | `nodeLinker` no está en `hoisted` | Revisar `pnpm-workspace.yaml`; borrar `node_modules` y reinstalar desde la raíz |
| `Cannot find module '@barmatchia/contracts'` | Contratos sin compilar | `pnpm contracts:build` |
| Vite falla al importar contratos (CommonJS) | Vite no pre-empaqueta paquetes enlazados | `optimizeDeps.include: ['@barmatchia/contracts']` y `build.commonjsOptions.include` |
| El teléfono no conecta a la API | Se usó `localhost` o la API no escucha en `0.0.0.0` | Usar la IP LAN del notebook; misma red Wi-Fi; revisar firewall |
| Dependencia nueva no ejecuta su *postinstall* | Scripts bloqueados por seguridad | `pnpm approve-builds` (solo si el paquete es confiable) |
