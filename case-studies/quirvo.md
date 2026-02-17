# Quirvo (WIP) — QR timbre + notificaciones + chat

## Qué resuelve
Sistema para edificios/casas: timbre por QR con trazabilidad (eventos), notificaciones y chat por evento.

## Estado actual (ya usable)
✅ visitante → /timbre → geofence → elige unidad → timbre → residente recibe aviso → abre chat → cierre automático

## Features implementadas
- Puertas/unidades (batch)
- Roles: PUERTA_ADMIN / UNIDAD_ADMIN / RESIDENTE
- Preferencias por usuario: Push / Telegram / Ambos
- Geofence (puerta por radio)
- Eventos + mensajes persistidos
- Notificaciones Telegram + PWA/Web Push
- Anti-ruido: cooldown + auto-cierre (cleanup)

## Stack (alto nivel)
Next.js + TypeScript + Supabase (Auth/Postgres) + Vercel. Notificaciones via Telegram Bot + Web Push/PWA.

## Capturas / Demo
(agregá imágenes o link a video)

## Código
Privado (producto comercial)
