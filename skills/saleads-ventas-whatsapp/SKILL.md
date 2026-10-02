---
name: saleads-ventas-whatsapp
description: |
  Consultoría de venta conversacional con SaleADS: campañas de Meta con destino WhatsApp/Messenger, requisitos (página, WhatsApp Business), capacidad de respuesta, primer mensaje, guion de conversación para cerrar ventas, seguimiento y cómo leer conversaciones vs ventas. Úsala cuando el usuario diga "quiero que me escriban por WhatsApp", "más mensajes", "me escriben pero no compran", "cómo respondo los mensajes", "vendo por WhatsApp", "anuncios de mensajes", "conversaciones", "click to WhatsApp" o "get more WhatsApp leads". Requiere el conector MCP de SaleADS para crear o revisar el plan.
---

# SaleADS — Ventas por WhatsApp

En LATAM, WhatsApp es el mostrador de la mayoría de negocios pequeños. Un anuncio con destino mensajes solo vale tanto como la conversación que sigue. Tu trabajo: dejar el plan listo **y** ayudar a que esas conversaciones se conviertan en ventas.

Base: [comportamiento-consultivo.md](../saleads-marketing-expertise/references/comportamiento-consultivo.md), [audiencia-y-destino.md](../saleads-marketing-expertise/references/audiencia-y-destino.md). Si no puedes abrirlas, carga `saleads-marketing-expertise`.

## Reglas

1. Propón; máximo 1–2 preguntas por mensaje. La pregunta clave aquí es la **capacidad de respuesta**.
2. Guiones y respuestas: sin testimonios, cifras, garantías ni descuentos que el usuario no confirme.
3. No envías mensajes por el usuario ni te conectas a su WhatsApp. Le entregas textos para que él los use.
4. Monto de pauta, aprobaciones y activación: del usuario. Tú no lanzas.

## Flujo

### 1. Revisa si está listo para mensajes

1. `saleads_get_account_overview` → negocio.
2. `saleads_get_meta_status` con `destination: "messages"`. Si faltan página o WhatsApp, entrega `action_url` y explica en simple qué conectar ("tu número de WhatsApp Business tiene que estar vinculado a tu página de Facebook").
3. `saleads_get_business_profile` y `saleads_list_offerings` para saber qué vende y cómo.

### 2. La pregunta de capacidad (una sola)

> ¿Quién va a responder los mensajes y en cuánto tiempo, más o menos?

| Respuesta | Recomendación |
|---|---|
| Responde en minutos | Ideal. Sigue con el plan. |
| Responde en horas | Propón respuestas rápidas y mensaje de bienvenida automático en WhatsApp Business; empezar con un presupuesto que pueda atender. |
| No tiene quién responda | Explica el trade-off: con mensajes sin atender se pierde la pauta. Propón destino `web` si hay tienda, o empezar con poco presupuesto. |

### 3. Plan con destino mensajes

Si no hay plan, sigue `saleads-primer-plan` o `saleads-strategic-plan` con `destination: "messages"`. En el resumen único incluye la oferta, el llamado a la acción tipo "Escríbenos por WhatsApp con…" y el presupuesto propuesto.

Un buen llamado a la acción para WhatsApp le dice a la persona **qué escribir**: "Envíanos una foto de tu sala y te cotizamos", "Escribe QUIERO y te mandamos el catálogo". Reduce la vergüenza de preguntar.

### 4. Kit de conversación (entrégalo después de armar el plan)

Ofrécelo en una línea: "¿Te dejo listo un guion para responder los mensajes que van a llegar?". Si acepta, entrega en un solo mensaje, adaptado a su oferta:

1. **Bienvenida (respuesta rápida):** saludo con nombre del negocio, agradecimiento y una pregunta para entender qué necesita.
2. **Calificar con 1–2 preguntas:** lo mínimo para dar precio o recomendar (tamaño, ciudad, fecha, talla).
3. **Propuesta:** qué incluye, precio (el confirmado), cómo se paga y cómo se entrega. Fotos reales si tiene.
4. **Objeciones frecuentes**, con respuestas honestas:
   - "Está caro" → recordar qué incluye y el ancla, ofrecer la opción más simple si existe.
   - "Lo pienso" → preguntar qué duda tiene; ofrecer agendar o apartar sin presión.
   - "¿Es confiable?" → ubicación, fotos reales, formas de pago seguras, Instagram activo.
5. **Cierre:** una acción concreta (agendar, confirmar dirección, enviar datos de pago).
6. **Seguimiento:** un mensaje a las 24 h y otro a los 3 días para quienes no respondieron, sin insistir más.

Usa etiquetas de WhatsApp Business (nuevo, cotizado, pagado) para no perder conversaciones. No inventes urgencias ("solo hoy") salvo que la promoción exista.

### 5. Leer resultados de mensajes

- `saleads_get_results` muestra conversaciones iniciadas y costo por conversación. **Las ventas se cierran en el chat**: Meta no las ve.
- Propón llevar una cuenta simple: conversaciones de la semana → cotizaciones → ventas. Con eso se sabe si el problema está en el anuncio (pocos mensajes) o en la conversación (muchos mensajes, pocas ventas).
- Muchos mensajes y pocas ventas: revisa tiempo de respuesta, precio sorpresa, guion. No es necesariamente culpa del anuncio.
- Sin ganadores causales; detalle en `saleads-diagnostico-resultados`.

### 6. Pausar si no puede atender

Si el usuario no podrá responder (viaje, sin stock, temporada alta), sugiere pausar temporalmente. Solo con confirmación explícita y motivo: `saleads_pause_campaign` (ver `saleads-launch-and-results`).
