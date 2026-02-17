# Calculadora ML — Motor de Pricing para MercadoLibre (Producto Comercial)

Sistema diseñado para proteger margen y reducir errores de pricing en vendedores de MercadoLibre Argentina.

No es una “calculadora”.  
Es un motor de decisión con control de acceso, suscripción recurrente y protección contra uso compartido.

Repositorio privado.

---

## Problema que resuelve

En MercadoLibre, el precio final no depende solo del costo y margen.

Intervienen:

- Comisiones variables
- Financiamiento en cuotas
- Descuentos y promociones
- Cambios frecuentes en reglas del marketplace

El error típico:
Calcular precio manualmente → olvidar cargos → erosionar margen.

Este sistema convierte:
**costo + margen objetivo → precio final sostenible**

con simulación de escenarios.

---

## Enfoque de diseño

El sistema fue construido con tres objetivos:

1. Separar reglas de negocio del frontend.
2. Proteger el acceso mediante suscripción real.
3. Evitar uso compartido sin depender solo de login básico.

---

## Capacidades principales

### Motor de pricing

- Cálculo de precio sugerido por margen objetivo
- Configuración parametrizable de comisiones
- Simulación con y sin descuento
- Simulación con cuotas manteniendo rentabilidad
- Comparación de escenarios para toma de decisión

El cálculo está desacoplado de la UI, permitiendo evolución futura a API o SaaS multiusuario.

---

## Sistema de acceso y monetización

### Autenticación y control

- Supabase Auth
- Row Level Security (RLS) en base de datos
- Estados de suscripción:
  - `trial`
  - `active`
  - `paused`
  - `expired`
- Usuarios `exempt` (lifetime / admin)

La autorización ocurre en base de datos, no solo en frontend.

---

### Suscripciones (MercadoPago)

- Modelo `preapproval` (suscripción recurrente)
- Webhooks para actualización automática de estado
- Edge Functions para procesar eventos de pago
- Actualización atómica de estado en base de datos

Diseñado para evitar:
- desincronización de estados
- acceso activo sin pago válido

---

## Control de dispositivos (Device Guard)

Para evitar uso compartido:

- Registro de dispositivo (fingerprint + metadata)
- Límite configurable por usuario (ej: 2 dispositivos)
- Invalidación en tiempo real vía Supabase Realtime
- Logout forzado cuando se excede el límite

El control ocurre en backend + base de datos.
No depende solo del navegador.

---

## Arquitectura (alto nivel)

Usuario → Frontend (SPA en Cloudflare Pages)  
Frontend → Supabase Auth  
Frontend → Postgres (con RLS activa)  
Frontend → Edge Functions  
Edge Functions ↔ MercadoPago (preapproval + webhooks)  
Webhooks → Actualización de estado en DB  
DB → Realtime → Invalidación en frontend

Principios aplicados:

- Separación clara entre cálculo, acceso y monetización
- Seguridad basada en base de datos (RLS)
- Eventos externos procesados vía funciones dedicadas
- Minimización de lógica sensible en cliente

---

## Stack real

Frontend:
- HTML + CSS + JavaScript (SPA liviana)

Backend:
- Supabase (Auth + Postgres + RLS + Realtime)
- Supabase Edge Functions (Deno / TypeScript)

Pagos:
- MercadoPago (suscripción recurrente + webhooks)

Infraestructura:
- Cloudflare Pages (deploy frontend)
- GitHub (ramas `logic` y `main`)

---

## Decisiones técnicas relevantes

- RLS activa para evitar exposición directa de datos
- Separación entre lógica de pricing y UI
- Procesamiento de pagos desacoplado mediante webhooks
- Control de dispositivos para reducir abuso comercial
- Estados explícitos de suscripción para trazabilidad

---

## Evolución prevista

- Migración del motor de cálculo a servicio desacoplado
- Panel de métricas por usuario
- Versionado de reglas de pricing
- Observabilidad estructurada (logs por cálculo y evento de pago)

---

## Código

Privado (producto comercial).
No se publica el repositorio.
