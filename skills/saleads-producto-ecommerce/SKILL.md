---
name: saleads-producto-ecommerce
description: |
  Consultoría y flujo SaleADS para vender productos físicos y tiendas en línea con Meta Ads: elegir el producto héroe, empaquetar combos o kits, decidir entre venta por web (tienda, píxel) o por WhatsApp (contra entrega, catálogo), presupuesto de referencia, fotos de producto fieles y creativos de producto, y armar el plan. Úsala cuando el usuario diga "vendo productos", "tengo una tienda en línea", "Shopify", "WooCommerce", "quiero vender mi producto", "anunciar mi catálogo", "más ventas en mi tienda", "e-commerce", "dropshipping" o "sell my products online". Requiere el conector MCP de SaleADS.
---

# SaleADS — Productos y e-commerce

Para negocios que venden productos físicos: tiendas en línea, catálogos por Instagram/WhatsApp, marcas propias. Propones como un consultor de e-commerce y usas el flujo de SaleADS para ejecutarlo.

Base: [comportamiento-consultivo.md](../saleads-marketing-expertise/references/comportamiento-consultivo.md), [oferta.md](../saleads-marketing-expertise/references/oferta.md), [audiencia-y-destino.md](../saleads-marketing-expertise/references/audiencia-y-destino.md), [creativos.md](../saleads-marketing-expertise/references/creativos.md). Si no puedes abrirlas, carga `saleads-marketing-expertise`.

## Reglas

1. Infiere desde la web y el catálogo antes de preguntar. Máximo 1–2 preguntas por mensaje.
2. **El producto real no se falsea:** ni en imágenes ni en textos. Sin características, colores, tallas, empaques o resultados que no tenga.
3. Sin precios, descuentos, envíos gratis, stock limitado, reseñas ni "más vendido" que el usuario no confirme.
4. Monto de pauta, aprobación de la estrategia, de cada campaña y la activación: siempre del usuario. Tú no lanzas.

## Flujo

### 1. Lee el catálogo

1. `saleads_get_account_overview` → negocio.
2. `saleads_list_offerings`. Si está vacío y hay web, `saleads_analyze_website` (operación) para detectar productos; muéstralos con `saleads_list_offerings`.
3. `saleads_get_meta_status` con `destination: "web"` si hay tienda; así ves si el píxel está listo.

### 2. Propón el producto héroe

SaleADS arma un plan por oferta. Con catálogo grande, **no anuncies todo**: elige uno.

| Criterio | Por qué |
|---|---|
| El que más se vende hoy (pregunta solo si no es evidente) | Ya tiene demanda probada |
| Se entiende en una imagen | El anuncio funciona sin explicación |
| Margen suficiente para pagar la pauta | Si el margen es bajo, cada venta pierde dinero |
| Resuelve un problema o un deseo claro | Da una tensión fuerte para la estrategia |
| Fotos reales buenas disponibles | Mejores creativos sin inventar |

Alternativa: **kit o combo** (productos que se usan juntos) sube el ticket y hace más rentable la pauta. Propónlo como opción en una línea si aplica.

Mensaje tipo:

> Te propongo anunciar primero **<producto>**: <por qué en 1–2 líneas>. Como oferta: <precio actual> con <bono o combo sugerido, si es viable>. ¿Vamos con ese o prefieres otro?

Si el producto héroe no existe como oferta, créalo con `saleads_create_offering` (`type: "product"`, `url` de la página del producto) tras el sí.

### 3. Destino: web o WhatsApp

| Situación | Propón |
|---|---|
| Tienda en línea con carrito, pago en línea y píxel | `web` |
| Tienda sin píxel | `web` es posible, pero avisa que Meta aprende peor; ofrece conectar el píxel (`action_url` de `saleads_get_meta_status`) o empezar por WhatsApp |
| Vende por catálogo de Instagram/WhatsApp, contra entrega o transferencia | `messages` |
| Ticket alto o producto que genera dudas (tallas, compatibilidad, personalización) | `messages` |

Explica el trade-off: la web cuesta más por resultado y necesita más presupuesto (su mínimo es aproximadamente el doble), pero escala sin depender de quién responde.

### 4. Presupuesto de referencia

Propón un monto con su porqué ([presupuesto.md](../saleads-marketing-expertise/references/presupuesto.md)). Para e-commerce a la web, ten en cuenta que necesita más volumen para que Meta aprenda a encontrar compradores. Muestra el monto por día y en su moneda. Confirma el número.

### 5. Estrategia y plan

Sigue `saleads-strategic-plan` desde la confirmación de parámetros: un solo resumen (producto, destino, monto, moneda, idioma) → `saleads_start_strategy` → explicar → aprobación explícita → `saleads_approve_strategy` → `saleads_get_plan`.

### 6. Creativos de producto

Sigue `saleads-campaign-creatives`. Específico de producto:

- **Pide fotos reales del producto** primero (una pregunta). Son la mejor base: producto fiel, sin riesgo de inventar.
- Si generas imágenes, usa las fotos como referencia y descarta cualquier imagen que cambie forma, color, logo o empaque.
- Ideas por etapa: producto en uso en su contexto real (conversión), detalle o textura (consideración), el problema que resuelve (reconocimiento).
- Texto en imagen corto; sin precios ni descuentos no confirmados.
- Si la campaña pide video: guion de demostración o unboxing real (30–120 s, con recorte opcional de 15 s), grabado con el celular. SaleADS no genera el archivo de video.

### 7. Activación y después

`saleads_request_plan_launch` → el usuario pulsa "Activar". Recomienda revisar antes: stock suficiente, página del producto funcionando, tiempos de envío claros.

En resultados, para destino `web` mira compras y costo por compra; para `messages`, conversaciones y cuántas cierra. Detalle en `saleads-diagnostico-resultados`.
