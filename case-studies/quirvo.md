# Quirvo (WIP) — Timbre QR con notificaciones y chat por evento

**Quirvo** es un sistema de timbre digital para edificios y casas: el visitante escanea un **QR**, valida cercanía (**geofence**), toca timbre a una **unidad**, el residente recibe **notificaciones** (Web Push/PWA y/o Telegram) y se abre un **chat ligado al evento** con trazabilidad completa.

> Enfoque: **simple de usar**, **confiable en producción**, **sin hardware** y con **registro de eventos**.

---

## Qué resuelve

- Reemplaza/acompaña al portero tradicional con un flujo moderno **sin obra ni cableado**.
- Permite **atender desde cualquier lugar** sin exponer el número del residente.
- Deja **auditoría** (eventos + mensajes) para administración y seguimiento.

---

## Estado actual

✅ **MVP usable end-to-end**  
Visitante → `/timbre` → geofence → elige unidad → timbre → residente recibe aviso → abre chat → **cierre automático**

---

## Features implementadas

### Core
- **Puertas / Unidades** (alta en batch)
- **Roles**: `PUERTA_ADMIN` / `UNIDAD_ADMIN` / `RESIDENTE`
- **Geofence** por puerta (validación por radio)
- **Eventos + mensajes persistidos** (chat por evento)
- **Anti-ruido**: cooldown de timbre + **auto-cierre** (cleanup)

### Notificaciones
- Preferencias por usuario: **Web Push / Telegram / Ambos**
- **Telegram Bot** (avisos + deep link al evento/chat)
- **Web Push / PWA** (notificaciones en navegador/dispositivo)

---

## Flujo del producto (alto nivel)

1. **Visitante** escanea QR (puerta)  
2. Se valida **geofence** (evita timbres remotos)  
3. Selecciona **unidad** y toca timbre  
4. Se crea **evento** y se notifica al/los residentes según preferencias  
5. Se abre **chat** asociado al evento  
6. El sistema aplica **cooldown** y **cierre automático** del evento para reducir ruido

---

## Stack

- **Frontend / App:** Next.js + TypeScript  
- **Backend:** Supabase (Auth + Postgres)  
- **Deploy:** Vercel  
- **Notificaciones:** Telegram Bot + Web Push/PWA  

---

## Decisiones de diseño (por qué así)

- **Chat “por evento”**: mantiene contexto y trazabilidad (quién tocó, cuándo, qué se habló).
- **Geofence en puerta**: reduce spam/uso indebido sin fricción para el visitante.
- **Preferencias de canal por usuario**: cada residente elige cómo recibir alertas.
- **Cleanup / auto-cierre**: evita eventos “zombies” y mejora UX en edificios con mucho tráfico.

---

## Seguridad y privacidad (resumen)

- Acceso con **roles** y autorización por unidad/puerta
- Mensajes y eventos **persistidos** con controles por usuario/rol
- Canales de notificación configurables (no se expone información sensible al visitante)

---

## Capturas / Demo

> Agregá acá imágenes (recomendado: 3–6) o un link a video demo.

- `docs/screenshots/timbre.png`
- `docs/screenshots/notificacion.png`
- `docs/screenshots/chat.png`

---

## Roadmap (próximo)

- Invitaciones/alta de residentes por unidad (flujo admin → invite → join)
- Panel de administración (puertas, unidades, residentes, preferencias)
- Métricas básicas (timbres por puerta/unidad, tiempos de respuesta, tasa de cierre)
- Mejoras de UX para edificios grandes (búsqueda de unidad / favoritos / accesibilidad)

---

## Código

Repositorio de portfolio: **documentación y overview del producto**.  
El código fuente es **privado** (producto comercial).

---
