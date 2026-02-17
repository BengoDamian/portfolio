# Calculadora ML (MercadoLibre Pricing Calculator) — Privado

Herramienta enfocada en **MercadoLibre Argentina** para calcular en segundos el **precio final de publicación** a partir del costo y un **margen objetivo**, contemplando comisiones, cuotas y escenarios con descuento **sin perder rentabilidad**.

> 📌 **Código:** privado (producto comercial). En este repo muestro **overview**, funcionalidades y stack.

---

## Qué resuelve
Cuando publicás en ML, el precio “a ojo” suele fallar porque hay variables que cambian la rentabilidad (comisiones, financiamiento/cuotas, promos/desc.).  
**Calculadora ML** te da un **precio sugerido** que mantiene tu margen y te permite simular escenarios antes de publicar.

---

## Funcionalidades
- **Precio sugerido por margen** (a partir de costo + margen objetivo)
- **Soporte de comisiones y cuotas/financiamiento**
- **Escenarios con descuento** manteniendo margen (simulaciones rápidas)
- **Login + control de acceso por suscripción** (MercadoPago)
- **Manejo de prueba (trial) y estados de suscripción**
- **Control de dispositivos** (límite por usuario) para evitar uso compartido

---

## Cómo funciona (alto nivel)
1. Ingresás **costo** y objetivo de **rentabilidad**.  
2. Seleccionás condiciones (comisiones / cuotas / descuento).  
3. La app calcula el **precio final recomendado** y te permite comparar escenarios.

---

## Stack (real)
- **Frontend:** HTML + CSS + JavaScript (SPA liviana)
- **Backend:** **Supabase** (Auth + Postgres + RLS + Realtime)
- **Funciones server-side:** Supabase **Edge Functions** (TypeScript/Deno) para flujos críticos (checkout/suscripción/webhooks)
- **Pagos / Suscripciones:** **MercadoPago** (Preapproval + Webhooks)
- **Hosting:** **Cloudflare Pages**
- **Repo / CI:** GitHub (workflow de ramas para pruebas y deploy)

---

## Seguridad y acceso
- Autenticación con Supabase Auth
- Reglas de seguridad con RLS en Postgres
- Suscripción obligatoria para habilitar uso completo
- Webhooks con manejo idempotente / estados de pago

---

## Capturas / Demo
*(Las agrego yo acá: screenshots, GIFs o link a demo.)*

---

## Estado
Producto en evolución: mejoras continuas en UX, casos borde de promociones/cuotas y resiliencia ante caídas temporales de servicios.

---

## Código
Este repositorio es **descriptivo (portfolio)**. El código fuente se mantiene **privado** por ser un producto comercial.
