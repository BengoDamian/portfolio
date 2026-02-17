# Quirvo (WIP) — QR timbre + notificaciones + chat

## Qué resuelve
Sistema para edificios/casas: **timbre por QR** con trazabilidad (eventos), **notificaciones** y **chat por evento**.

## Estado actual (ya usable)
✅ Visitante → entra a `/timbre` → comparte ubicación → geofence resuelve la puerta → elige unidad → toca timbre → residente recibe aviso (Telegram/PWA) → abre chat y responde → eventos se cierran solos con limpieza.

## Features implementadas

### 1) Puertas y unidades (modelo “edificio/casa”)
- El encargado/dueño crea una **puerta** en `/setup` (nombre + ubicación + radio).
- Desde el backoffice se pueden cargar **unidades en batch** (1A, 1B, 2A, 3A…) sin tocar SQL.
- Las unidades quedan asociadas a esa puerta.

### 2) Roles y acceso
- **PUERTA_ADMIN (encargado):** administra la puerta, unidades e invites.
- **UNIDAD_ADMIN (dueño del depto):** administra su unidad e invita convivientes.
- **RESIDENTE:** participa en su unidad.
- Preferencias por usuario: **Push / Telegram / Ambos** (probado: “solo Telegram” funciona).

### 3) Timbre con geolocalización
- El visitante entra a `/timbre` y comparte ubicación.
- La app resuelve la puerta por **geofence** (radio).
- El visitante elige la unidad y toca timbre → se crea un evento en `eventos_timbre`.

### 4) Chat por evento
- Cada timbre abre un **evento** (con chat).
- Los mensajes se guardan en `mensajes_timbre`.
- El residente ve eventos en `/residente` y puede chatear.

### 5) Notificaciones
- **Telegram:** aviso de timbre y de mensajes del visitante (botones: “Abrir timbre / Abrir chat”).
- **Web Push / PWA:** opción de activar push (Android/desktop suele andar; en iPhone es más limitado).
- Banner tipo Chrome “Instalar Quirvo” para empujar la PWA.

### 6) Anti-spam / ruido
- **Cooldown:** evita tocar timbre seguido (por unidad y/o IP según config).
- **Auto-cierre:**
  - Endpoint: `/api/cron/cleanup/<secret>` cierra OPEN viejos por `EVENT_AUTO_CLOSE_MINUTES`.
  - En Vercel gratis: se usa como limpieza diaria (o se ejecuta manual cuando querés).

### 7) Deep links (abrir directo)
- Los botones de Telegram llevan a la webapp con link directo al timbre/chat del evento.

## Stack (alto nivel)
- Frontend: Next.js (PWA) + TypeScript
- Backend: Supabase (Auth + Postgres + RLS + Realtime)
- Notificaciones: Telegram Bot + Web Push/PWA
- Deploy: Vercel

## Capturas / Demo
- (agregar screenshots o link a video)

## Código
Privado (producto comercial). No se publica el repositorio.
