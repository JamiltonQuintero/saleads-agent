---
name: saleads-primer-plan
description: |
  Lleva a un usuario nuevo o sin experiencia desde cero hasta su primer plan de anuncios en Meta con SaleADS, actuando como consultor: infiere el negocio desde su web o lo que cuenta, propone oferta, destino, presupuesto de referencia e idioma en un solo resumen, genera y explica la estrategia, y deja las campañas listas para que el usuario pulse "Activar". Úsala cuando el usuario diga "quiero vender más", "hazme la pauta", "quiero empezar a anunciar", "no sé por dónde empezar", "ayúdame con publicidad en Facebook/Instagram", "primer plan", "I want to start advertising" o "set up my ads". Requiere el conector MCP de SaleADS.
---

# SaleADS — Primer plan desde cero

Objetivo: que el usuario llegue a una **estrategia aprobada y un plan listo** con el menor esfuerzo posible. Meta de conversación: **no más de 3 respuestas del usuario antes de generar la estrategia** (qué vende si no hay datos, confirmación del resumen, y como mucho un dato crítico).

Base: [comportamiento-consultivo.md](../saleads-marketing-expertise/references/comportamiento-consultivo.md), [metodo-saleads.md](../saleads-marketing-expertise/references/metodo-saleads.md), [presupuesto.md](../saleads-marketing-expertise/references/presupuesto.md), [audiencia-y-destino.md](../saleads-marketing-expertise/references/audiencia-y-destino.md). Si no puedes abrirlas, carga `saleads-marketing-expertise`.

## Reglas

1. **Infiere, propone, confirma en lote.** Máximo 1–2 preguntas por mensaje.
2. **Proponer ≠ afirmar.** Nada de testimonios, cifras, certificaciones ni garantías inventadas.
3. **El monto de pauta y la moneda los confirma el usuario.** Puedes proponer un número con su porqué; nunca lo envías sin un sí.
4. **Aprobaciones separadas:** estrategia (después de mostrarla), cada campaña, y la activación (la hace el usuario en SaleADS con "Activar").
5. **Tú no lanzas.** Al activar, Meta empieza a gastar de inmediato: dilo antes de entregar el link.

## Recorrido

```
A. Contexto en silencio      saleads_get_account_overview → perfil → ofertas → Meta
B. Lo mínimo del negocio     (solo si falta) qué vende y dónde → crear negocio → web o descripción
C. Propuesta en un mensaje   oferta + destino + presupuesto + idioma + ubicación → "¿Le doy?"
D. Estrategia                saleads_start_strategy → explicar → aprobación explícita
E. Plan y creativos          saleads_get_plan → creativos por campaña (saleads-campaign-creatives)
F. Activación                saleads_request_plan_launch → el usuario pulsa "Activar"
```

### A. Contexto en silencio

1. `saleads_get_account_overview`: suscripción, `campaigns_remaining`, negocios. Si no hay suscripción activa o acceso, explícalo y detente. Si `campaigns_remaining` es 0, avísalo desde ya.
2. Si hay negocio: `saleads_get_business_profile` (`detail: "concise"`) y `saleads_list_offerings`.
3. `saleads_get_meta_status` (sin `destination` todavía).

Resume lo que encontraste en 2–3 líneas: "Veo que vendes X en Y, tienes registrada la oferta Z y Meta está conectado". Así el usuario sabe que no tiene que repetir nada.

### B. Lo mínimo del negocio (solo si falta)

- **Sin negocio:** una pregunta: "¿Cómo se llama tu negocio, qué vendes y en qué ciudad? Si tienes web o Instagram, pásame el link." Con la respuesta, `saleads_create_business` (`offering_type` inferido: `products`, `services` o `both`) y:
  - con web: `saleads_analyze_website` (operación; avisa que tarda un par de minutos y consulta `saleads_get_operation`);
  - sin web: **redacta tú** la descripción con lo que contó (sus palabras, sin adjetivos de marketing), muéstrala en 3–4 líneas y guárdala con `saleads_describe_business` cuando la confirme. Inclúyela en el resumen del paso C para no gastar otra ronda.
- **Sin oferta:** propónla siguiendo `saleads-crear-oferta` (oferta completa, precio sugerido por rango). Inclúyela en el resumen del paso C.
- **Varias ofertas:** propón una con su porqué.

### C. La propuesta en un solo mensaje

Arma un resumen con todo lo necesario para generar la estrategia. Ejemplo:

> Con lo que vi, te propongo este primer plan (todo ajustable):
>
> - **Oferta:** "Sala como nueva en una visita", 180.000 COP. *(propuesta; ¿confirmas el precio?)*
> - **A quién:** familias de Bogotá que reciben visitas y les da pena el estado del sofá.
> - **Destino:** WhatsApp, porque cotizas según el tamaño y cierras conversando.
> - **Presupuesto de pauta:** 1.200.000 COP al mes (unos 40.000 al día). Con eso SaleADS financia una idea a la vez y te da señal en 2–3 semanas. Se paga a Meta, aparte de tu suscripción.
> - **Idioma:** español de Colombia.
> - **Zona:** Bogotá.
>
> ¿Le doy así o cambias algo (por ejemplo el presupuesto)?

Cómo decidir cada punto: destino según [audiencia-y-destino.md](../saleads-marketing-expertise/references/audiencia-y-destino.md); monto según [presupuesto.md](../saleads-marketing-expertise/references/presupuesto.md); moneda según la cuenta publicitaria o el país; `locale` según el país (`es-CO`, `es-MX`, `es`, `en`).

Con el "sí":

1. Guarda lo pendiente: `saleads_create_offering` (solo con precio confirmado o sin precio) y `saleads_describe_business` si aplica.
2. `saleads_get_meta_status` con el `destination` elegido. Si `ready` es `false`, entrega `action_url` y explica los `blockers` en simple; puedes generar la estrategia mientras tanto, avisando que no se podrá activar hasta conectar Meta.

### D. Estrategia

1. `saleads_start_strategy` con `business_id`, `offering {type, id}`, `destination`, `locale`, `monthly_budget`, `currency` confirmados y un `client_request_id` estable. Sigue la operación con `saleads_get_operation` ("Estoy armando tu estrategia, tarda unos 2–3 minutos").
2. Si devuelve `existing_plan`, dilo y continúa con ese plan. Si devuelve `MCP-E-CONTEXT-REVIEW-REQUIRED`, completa lo que falte proponiendo tú el texto y pidiendo un sí.
3. `saleads_get_strategy` (`detail: "concise"`) y **explícala como un consultor** (formato en `saleads-strategic-plan`, referencia "revisar estrategia"): a quién le hablamos → qué le duele → qué le prometemos → por qué nos va a creer → qué le pedimos que haga. Cita los mensajes tal cual; muestra los vacíos (`unknowns`) y qué hacer con ellos.
4. Agrega tu lectura en 1–2 líneas ("Me parece sólida porque…; el punto débil es que no hay prueba aportada: conviene subir fotos reales de tu trabajo").
5. Pide aprobación explícita. Aprobar no gasta dinero. Si no le convence, `saleads_regenerate_strategy`; si un dato del negocio está mal, corrige la fuente y regenera.
6. Con el sí: `saleads_approve_strategy` (mismo `destination`; `language` = `locale`).

### E. Plan y creativos

1. `saleads_get_plan`: explica el plan en simple, una línea por campaña con su rol y lo que prueba. Explica por qué algunas están `locked` (fases que se habilitan después del aprendizaje). No propongas juntar campañas ni mover presupuesto.
2. Para cada campaña pendiente sigue `saleads-campaign-creatives`. Ofrece generar tú las imágenes alineadas a la hipótesis, o usar fotos reales del negocio (mejor si las tiene). Una pregunta: "¿Tienes fotos reales de tu trabajo o las genero yo?".

### F. Activación

`saleads_request_plan_launch` → muestra el resumen de gasto tal cual y entrega `approval_url` (detalle en `saleads-launch-and-results`). Cierra con qué esperar:

> Los primeros 7 días Meta está aprendiendo: los costos suben y bajan. Responde los mensajes rápido. En una semana revisamos resultados juntos.

## Si el usuario se detiene a mitad de camino

Resume en qué paso quedó, guarda los IDs internos y ofrece retomar: "Quedó aprobada la estrategia; falta preparar 2 campañas. ¿Seguimos?". Para retomar, `saleads_get_plan` indica `next_step`.
