---
name: saleads-temporada
description: |
  Planea campañas de temporada y fechas especiales con SaleADS: Black Friday, Cyber Monday, Buen Fin, Hot Sale, Día de la Madre, Día del Padre, Amor y Amistad, San Valentín, regreso a clases, Navidad y fin de año. Propone la oferta de temporada (solo con descuentos y fechas reales), el calendario (empezar antes por la fase de aprendizaje), el mensaje y el presupuesto, y usa el flujo normal de plan estratégico. Úsala cuando el usuario diga "Black Friday", "promoción de temporada", "campaña para el Día de la Madre", "Navidad", "fecha especial", "descuento por temporada", "quiero aprovechar la fecha" o "holiday campaign". Requiere el conector MCP de SaleADS.
---

# SaleADS — Campañas de temporada

Las fechas comerciales concentran la intención de compra, pero también la competencia: el costo de los anuncios sube cerca de la fecha. Gana quien **llega preparado** con una oferta clara y real.

Base: [comportamiento-consultivo.md](../saleads-marketing-expertise/references/comportamiento-consultivo.md), [oferta.md](../saleads-marketing-expertise/references/oferta.md), [presupuesto.md](../saleads-marketing-expertise/references/presupuesto.md), [contexto-latam.md](../saleads-marketing-expertise/references/contexto-latam.md). Si no puedes abrirlas, carga `saleads-marketing-expertise`.

## Reglas

1. **Descuentos, precios, fechas límite, stock y regalos: solo los reales.** Puedes proponer una mecánica de promoción; queda como propuesta hasta que el usuario la confirme con sus condiciones.
2. **Sin urgencia ni escasez falsa.** "Últimas unidades" o "solo hoy" solo si es verdad.
3. Confirma la fecha de la celebración en el país del usuario: varía (Día de la Madre, regreso a clases, Buen Fin).
4. Monto de pauta, aprobaciones y activación: del usuario. Tú no lanzas.

## Plan especial de fechas en la web

SaleADS puede ofrecer en su web un flujo específico para fechas especiales (plan temporal). Desde este conector no hay una tool para ese flujo. Si el usuario lo menciona o lo tiene disponible, explícale que se configura en la web de SaleADS. Desde aquí lo que haces es preparar la **oferta de temporada** y un **plan estratégico normal** enfocado en la fecha.

## Flujo

### 1. Contexto y fecha

`saleads_get_account_overview` → `saleads_get_business_profile` → `saleads_list_offerings`. Calcula cuántos días faltan para la fecha y dilo.

### 2. Calendario recomendado

| Momento | Qué pasa | Recomendación |
|---|---|---|
| 3–4 semanas antes | Costos normales, la gente empieza a buscar ideas | Ideal para lanzar: la campaña sale de la fase de aprendizaje (~7 días) antes del pico |
| 1–2 semanas antes | Intención alta, costos subiendo | Aún sirve; la oferta debe ser muy clara |
| Semana de la fecha | Pico de competencia y costos | Solo si ya hay campañas aprendidas o la oferta es muy fuerte |
| Después | Cae la intención | Pausar o cambiar la oferta |

Si faltan menos de 7 días, dilo con honestidad: la campaña pasará buena parte del tiempo aprendiendo. Propón igual el plan si el usuario quiere, con expectativas claras.

### 3. Propón la oferta de temporada (un mensaje)

Mecánicas que puedes sugerir (el usuario elige y confirma condiciones):

- **Kit o combo regalo** ("Kit Día de la Madre": 2–3 productos que se regalan juntos). Sube el ticket sin destruir el margen.
- **Bono por compra** (empaque de regalo, tarjeta, envío) si es viable.
- **Descuento** con porcentaje, condición y fecha de fin **que el usuario defina**.
- **Preventa o reserva** para servicios con cupos (cupos reales).
- **Guía de regalo** (para quién es cada producto) cuando el catálogo es amplio.

Formato:

> Para <fecha> te propongo: **<nombre de la oferta>** — <qué incluye>. Mecánica sugerida: <kit/bono/descuento> *(tú defines el valor y la fecha de fin)*. Mensaje central: <tensión de la fecha, p. ej. "encontrar un regalo que sí use">. Arrancar el <fecha recomendada>.
> ¿Qué descuento o beneficio real puedes dar, y hasta qué día?

Guarda la oferta de temporada con `saleads_create_offering` incluyendo en `description` la mecánica, condiciones y fecha de fin confirmadas. Usa un `name` que identifique la temporada.

### 4. Presupuesto

La competencia sube los costos cerca de la fecha. Propón un monto pensando en la duración real de la campaña (si es de 3 semanas, explica cuánto se gasta en ese periodo según el monto mensual). El monto lo confirma el usuario.

### 5. Estrategia, plan y creativos

Sigue `saleads-strategic-plan` con la oferta de temporada: resumen único → `saleads_start_strategy` → explicar → aprobación explícita → plan → creativos.

Creativos de temporada: elementos visuales de la fecha sin logos ni personajes de terceros; el beneficio real visible; la fecha límite solo si es real.

### 6. Al terminar la temporada

Recuérdale al usuario pausar o cambiar la campaña cuando termine la promoción, para no anunciar una oferta vencida. Pausar exige su confirmación explícita (`saleads_pause_campaign`, ver `saleads-launch-and-results`).
