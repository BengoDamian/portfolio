# Quirvo (WIP / MVP en producción)

Sistema de acceso para edificios y casas basado en **QR + eventos + notificaciones desacopladas**.

No es un “QR con bot”.  
Es un sistema orientado a eventos con control de estado, trazabilidad y reducción de ruido operativo.

---

## Problema que resuelve

Digitaliza el acceso físico evitando:

- Dependencia de portero o intermediarios  
- Falta de trazabilidad en visitas  
- Fricción en comunicación visitante–residente  
- Ruido por timbres reiterados  

Convierte el timbre en un **evento persistido y gestionable**.

---

## Flujo principal

visitante → `/timbre` → comparte ubicación → resolución por geofence → selección de unidad → creación de evento → notificación (Telegram/PWA) → chat por evento → cierre automático

Cada interacción genera estado controlado en base de datos.

---

## Decisiones de arquitectura

### Modelo orientado a eventos

- Cada timbre crea una entidad persistida (`eventos_timbre`).
- Los mensajes se asocian a ese evento (`mensajes_timbre`).
- El estado del evento (OPEN / CLOSED) es explícito.
- Permite trazabilidad completa.

### Separación de canales de notificación

- Telegram y Web Push funcionan como canales independientes.
- El sistema no depende de un único proveedor.
- Deep links conectan notificación → frontend → evento específico.

Reduce single point of failure en comunicación.

### Control de acceso y roles

Modelo de permisos granular:

- `PUERTA_ADMIN`
- `UNIDAD_ADMIN`
- `RESIDENTE`

Implementado con Supabase Auth + RLS.

Los permisos forman parte del diseño, no un parche posterior.

### Geolocalización contextual (geofence)

- La puerta se resuelve por radio configurable.
- Evita uso remoto indebido.
- La validación ocurre antes de crear el evento.

### Anti-ruido y control operativo

- Cooldown por unidad/IP configurable.
- Endpoint de limpieza (`/api/cron/cleanup/<secret>`)
- Auto-cierre por tiempo (`EVENT_AUTO_CLOSE_MINUTES`)

Evita acumulación de eventos abiertos y abuso del sistema.

---

## Estado actual

Sistema operativo en un edificio real.

Flujo completo funcional:

- Creación de puerta en `/setup`
- Carga batch de unidades desde backoffice
- Timbre con geofence
- Notificación vía Telegram (probado en producción)
- Chat persistente por evento
- Auto-cierre configurado

---

## Stack (alto nivel)

Frontend:
- Next.js (PWA)
- TypeScript

Backend:
- Supabase (Auth + Postgres + RLS + Realtime)

Notificaciones:
- Telegram Bot API
- Web Push / PWA

Deploy:
- Vercel

Arquitectura orientada a:
- Persistencia explícita de estado
- Separación de responsabilidades
- Minimización de ruido
- Mantenibilidad

---

## Próximas iteraciones

- Observabilidad (logs estructurados por evento)
- Métricas por puerta/unidad
- Panel de auditoría
- Hardening de permisos y rate limiting avanzado

---

## Código

Repositorio privado (producto comercial).
No se publica código fuente.
