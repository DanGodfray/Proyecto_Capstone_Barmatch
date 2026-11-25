# 4. Definition of Done (DoD) y criterios de aceptación

> **Fuente:** CONTEXTO_IA §10 (convenciones del repo de desarrollo). Criterios conservados del artefacto original; no se certifica una aprobación nueva ni el cumplimiento administrativo de cada ticket. Contraste al cierre: [fuentes](../README.md#fuentes-y-alcance).

La DoD es la lista de condiciones que **todo** ítem debe cumplir para considerarse terminado. Es distinta de los *criterios de aceptación*, que son propios de cada historia.

## 4.1 DoD por nivel

### Nivel ítem (cada PR / ticket)
- [ ] Compila sin errores (`pnpm -r build` / `tsc`) y el lint queda limpio (ESLint + Prettier; `ruff` en Python).
- [ ] Incluye pruebas: herramientas efectivamente usadas por área (Node test runner en API/contratos, Vitest en web, Jest en móvil, pytest en BI).
- [ ] Las pruebas pasan en CI (no solo en local).
- [ ] Un *slice* (funcionalidad vertical) por PR, con commits en formato Conventional Commits (`feat:`, `fix:`, `test:`, `docs:`, `chore:`, `refactor:`).
- [ ] PR aprobado por un integrante distinto del autor. **Raúl es aprobador obligatorio** si el cambio toca `packages/contracts`, base de datos o autenticación.
- [ ] Ticket de Jira enlazado en el PR y movido a *Finalizado*.
- [ ] Sin valores `<TBD>` inventados: lo no decidido conserva su marcador.
- [ ] Sin secretos en el código (`.env` nunca se commitea; `.env.example` sí). Ninguna app contiene `service_role` ni `DATABASE_URL`.
- [ ] Datos de prueba ficticios (Ley 19.628 / 21.719).
- [ ] Revisión visual con *checklist* de pruebas físicas cuando el cambio es visible en la app o la web (rol QA).

### Nivel historia de usuario
- [ ] Cumple todos sus criterios de aceptación **contra la API real** (no solo contra el adaptador mock).
- [ ] La misma suite de contrato pasa con el adaptador `mock` y con el adaptador `http`.
- [ ] La autorización por perfil está validada en backend (guard + pertenencia al establecimiento); la UI no es la única barrera.
- [ ] Los errores se muestran al usuario con el código del catálogo vigente del contrato HTTP, no con mensajes técnicos.

### Nivel sprint / incremento
- [ ] El incremento se puede demostrar con el guion de demo vigente.
- [ ] `develop` queda estable; los hitos se promueven a `main` con *tag*.
- [ ] Sprint backlog y retrospectiva actualizados en este repositorio.

## 4.2 Criterios de aceptación de historias críticas (formato Dado / Cuando / Entonces)

**HU-05 Reservar "Ahora"** (RN-003)
- *Dado* un bloque vigente al que le quedan ≥ 5 min, *cuando* el cliente lo elige, *entonces* se ofrece como "Ahora" y se puede reservar.
- *Dado* un bloque al que le quedan < 5 min, *cuando* el cliente consulta, *entonces* ese bloque no aparece como "Ahora".

**HU-07 Sin sobreventa** (RN-013, Matriz #3)
- *Dado* un establecimiento con capacidad para 1 grupo más en un bloque, *cuando* dos clientes confirman al mismo tiempo, *entonces* exactamente una reserva queda `CONFIRMADA` y la otra recibe un conflicto de capacidad (409) según el contrato vigente.

**HU-09 Cancelar** (RN-016, RN-017)
- *Dado* una reserva `CONFIRMADA` a más de 60 min de su inicio, *cuando* el cliente cancela, *entonces* queda `CANCELADA` con `cancelacion_tardia = false`.
- *Dado* la misma reserva a menos de 60 min, *cuando* cancela, *entonces* se acepta (no es error) y `cancelacion_tardia = true`.

**HU-11 Check-in por QR** (RN-004)
- *Dado* una reserva `CONFIRMADA` y la hora actual dentro de la ventana (desde 20 min antes del bloque), *cuando* el personal escanea el QR, *entonces* la reserva pasa a `VALIDADA`.
- *Dado* un QR de otro establecimiento o inexistente, *cuando* se escanea, *entonces* se responde `QR_INVALIDO` sin revelar datos de la reserva.

**HU-16 No-show** (RN-006)
- *Dado* una reserva sin check-in, *cuando* pasan 15 min desde el inicio del bloque, *entonces* el sistema la marca `NO_SHOW` y registra un `evento_reserva`, siempre que los jobs estén habilitados de forma autorizada. En el ensayo de cierre estaban apagados; los laboratorios no acreditan su activación operacional.

**HU-18 Consulta administrativa** (RN-057, RN-045)
- *Dado* un admin local asociado a los locales A y B, *cuando* consulta reservas del local C, *entonces* recibe `SIN_PERMISO` (403).

## 4.3 Aplicación al corte de cierre

La auditoría acredita el incremento funcional integrado del MVP1 y sus ensayos satisfactorios. La DoD permite distinguir ese logro técnico de la validación administrativa de cada ticket. La CI local de 31 pasos no equivale a verificar todos los runs de GitHub Actions: el control de lockfile respecto de una rama base figura NO EJ localmente. Los estados de Jira, aprobaciones o tags no se completan por inferencia. Se conservan criterios normativos como listas, sin marcarlos como ejecutados.

El corte auditado mantiene FAIL de seguridad no bloqueantes, casos físicos NO EJ y la comprobación manual de Inteligencia «Mañana». La ausencia deseada de IDs internos no está plenamente cumplida (hallazgo H-4). No se declara certificación de seguridad ni despliegue comercial.
