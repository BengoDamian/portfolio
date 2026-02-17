# Calculadora ML (MercadoLibre Pricing Calculator) — README PRIVADO (FULL)

> **Proyecto privado / producto comercial.**  
> Este README documenta: objetivos, arquitectura, flujos críticos, modelo de datos, setup, deploy, pagos (MercadoPago), suscripciones, control de dispositivos, seguridad (RLS), troubleshooting y runbooks.

---

## 1) Qué es y por qué existe

**Calculadora ML** es una herramienta para **MercadoLibre Argentina** que calcula el **precio final de publicación** partiendo del costo y un **margen objetivo**, incorporando variables típicas del negocio:

- comisiones / cargos
- cuotas / financiamiento
- descuentos / promociones
- escenarios comparativos (para decidir rápido y mantener rentabilidad)

Objetivo: que el usuario calcule **en segundos** un precio sugerido que mantenga rentabilidad y no se rompa por “costos ocultos”.

---

## 2) Alcance del proyecto (lo que resuelve)

- Convertir **costo + margen objetivo** en **precio final recomendado**
- Simular escenarios (sin descuento / con descuento / con cuotas) manteniendo margen
- Controlar acceso mediante **login + suscripción**
- Cobros recurrentes con **MercadoPago**
- Evitar uso compartido mediante **Device Guard** (límite de dispositivos)

---

## 3) Features (actual)

### Pricing / simulación
- Precio sugerido por margen
- Configuración de comisiones/cargos
- Soporte de cuotas/financiamiento
- Escenarios con descuento manteniendo margen
- Comparación de escenarios (para decidir precio y promo)

### Producto / acceso
- Login (Supabase Auth)
- Paywall por suscripción (MercadoPago)
- Trial automático (ej: 5 días) cuando corresponde
- Estados de suscripción: `trial` / `active` / `paused` / `expired` (y los que se definan)
- Usuarios “exempt” (acceso especial / lifetime / admin)

### Control de dispositivos (anti-share)
- Registro de dispositivo (fingerprint + metadata)
- Límite por usuario (ej: **2 dispositivos**)
- “Realtime logout” / invalidación cuando se remueve un dispositivo

---

## 4) Stack (real)

- **Frontend:** HTML + CSS + JavaScript (SPA liviana)
- **Backend:** Supabase
  - Auth
  - Postgres
  - RLS (Row Level Security)
  - Realtime (para eventos/invalidaciones)
  - Edge Functions (Deno/TypeScript)
- **Pagos / Suscripciones:** MercadoPago
  - Preapproval (suscripción)
  - Webhooks (eventos de estado)
- **Hosting:** Cloudflare Pages
- **Repo:** GitHub  
  - rama `logic` para pruebas  
  - rama `main` estable / deploy

---

## 5) Arquitectura (alto nivel)

```mermaid
flowchart LR
  U["Usuario"] -->|Browser| FE["Frontend (Cloudflare Pages)"]
  FE -->|Auth| SA["Supabase Auth"]
  FE -->|RPC/SQL| DB["Supabase Postgres + RLS"]
  FE -->|Edge Functions| EF["Supabase Edge Functions"]
  EF -->|Preapproval| MP["MercadoPago"]
  MP -->|Webhooks| EF
  EF -->|Update status| DB
  DB -->|Realtime| FE
