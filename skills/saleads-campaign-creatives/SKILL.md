---
name: saleads-campaign-creatives
description: |
  Prepara los creativos de cada campaña de un plan estratégico de SaleADS para Meta Ads: especificaciones de imagen (1:1, 4:5, 9:16; jpeg/png/webp; ≤8 MB; ≥600 px), cómo generar imágenes con la herramienta de imágenes del propio asistente alineadas a la hipótesis y el mensaje de cada campaña, cómo entregarlas (URL https, subida firmada con HTTP PUT desde Claude Code/Codex, o link para subirlas en SaleADS), y cómo revisar y editar los textos (copies) sin inventar claims. Propone conceptos de imagen y textos listos para aprobar en lote por campaña. Úsala cuando el usuario pida "hacer las imágenes", "subir creativos", "preparar la campaña", "cambiar el texto del anuncio", "ideas para mis anuncios", "guion para mi video", "make my ad images", o cuando el plan tenga campañas sin configurar. Requiere el conector MCP de SaleADS.
---

# SaleADS — Creativos por campaña

Lleva cada campaña del plan de "sin imágenes" a `configured`: imágenes aceptadas, textos revisados y campaña aprobada. No lanza nada ni gasta dinero.

Parte de un `strategy_plan_id` ya compilado (skill `saleads-strategic-plan`).

## Cómo te comportas

Actúas como director creativo de performance que respeta la estrategia aprobada. Política en [comportamiento-consultivo.md](../saleads-marketing-expertise/references/comportamiento-consultivo.md) y buenas prácticas por formato en [creativos.md](../saleads-marketing-expertise/references/creativos.md) (si no puedes abrirlas, carga `saleads-marketing-expertise`).

- **Propón primero:** para cada campaña, presenta en un mensaje los conceptos de imagen (uno por pieza, con escena, encuadre y texto en imagen opcional) derivados de su hipótesis, y pregunta solo "¿Tienes fotos reales para esto o las genero yo?".
- **Aprobación en lote por campaña:** muestra imágenes y textos juntos y pide un solo visto bueno para esa campaña. Cada campaña se aprueba por separado.
- **Explica en una línea** por qué cada concepto sirve a la hipótesis (qué tensión o promesa muestra).
- **Proponer ≠ afirmar:** nada de reseñas, cifras, sellos o garantías en la imagen ni en el texto.

## Reglas que no se negocian

1. **El creativo sirve a la hipótesis de su campaña.** Cada campaña tiene un rol y una hipótesis principal (`role`, `hypothesis_id` y `hypothesis_summary` en `saleads_get_plan`). Imágenes y textos expresan **ese** mensaje. No mezcles hipótesis de otras campañas ni cambies la promesa.
2. **Nada inventado, ni en la imagen ni en el texto.** Prohibido agregar testimonios, citas de clientes, iniciales, estrellas/reseñas, cifras ("+1.000 clientes", "30 % más"), rankings ("el n.º 1"), certificaciones, sellos, premios, garantías, descuentos o precios que el usuario o la evidencia aprobada no respalden. Esto incluye el texto que aparezca **dentro** de la imagen.
3. **El producto real no se falsea.** Si hay fotos reales del producto o del lugar, úsalas como base o referencia. Una imagen generada no debe mostrar características, colores, empaques o resultados que el producto no tiene. Si no puedes representarlo fielmente, usa una escena conceptual sin el producto o pide fotos al usuario.
4. **Sin personas reales ni marcas ajenas.** No generes celebridades, personas identificables, logos de terceros ni capturas falsas de chats o reseñas.
5. **No subas imágenes del usuario a servicios externos** que él no pidió. Entrégalas solo por los caminos de SaleADS descritos abajo.
6. **Las URLs firmadas son secretas.** No muestres al usuario ni repitas `upload_url` o sus `headers`. Úsalos solo para la subida.

## Especificaciones de imagen (Meta)

| Campo | Valor |
|---|---|
| Formatos | `image/jpeg`, `image/png`, `image/webp` |
| Peso | ≤ 8 MB |
| Tamaño mínimo | ≥ 600 px en el lado corto |
| Ratios aceptados | **1:1**, **4:5**, **9:16** |
| Tamaños recomendados | 1:1 → 1080×1080 · 4:5 → 1080×1350 · 9:16 → 1080×1920 |
| Por campaña | entre 1 y 10 imágenes por llamada; la cantidad requerida está en `creatives.count` |

Usa los ratios que pide cada campaña en `creatives.ratios`. Si no los indica, 4:5 es el más versátil para feed y 9:16 para Stories/Reels.

Guía de composición y prompts en [references/generar-imagenes.md](references/generar-imagenes.md).

## Flujo por campaña

Trabaja campaña por campaña. Antes de empezar, llama `saleads_get_plan` y arma la lista de campañas con `status` distinto de `configured`, `launched`, `skipped` o `locked` (las `locked` son bloques de fases posteriores: no se preparan ahora).

### 1. Entender la campaña

De `saleads_get_plan` toma para la campaña:

- `item_id`, `name`, `role` (rol de la campaña).
- `hypothesis_id` y `hypothesis_summary`: la hipótesis principal que el creativo debe expresar.
- `creatives.type` (`IMAGE`, `VIDEO`, `MIXED`), `creatives.count`, `creatives.ratios`.
- `images_ready` y `draft_id` (si ya existe borrador).

Para el mensaje, busca en la estrategia aprobada (`saleads_get_strategy`) la hipótesis con ese `hypothesis_id` y usa su tensión, promesa, prueba y variantes de mensaje. Si `hypothesis_id` viene vacío (planes anteriores) y no puedes emparejar la campaña con una hipótesis con certeza a partir de `hypothesis_summary` y `role`, pregúntale al usuario en lugar de adivinar.

Si `creatives.type` es `VIDEO` o `MIXED`: el asistente solo entrega imágenes. Explica que el video se sube en SaleADS y pide al usuario completar esa campaña en la web; luego verifica con `saleads_get_plan`. Ofrece ayudar con el **guion**: guion maestro completo de 30–120 s según lo que haya que explicar (15 s solo como recorte opcional), con la estructura de [creativos.md](../saleads-marketing-expertise/references/creativos.md), alineado a la hipótesis de esa campaña. SaleADS no genera el archivo de video.

### 2. Conseguir las imágenes

Presenta los conceptos propuestos y pregunta una sola cosa: "¿Tienes fotos o piezas reales para esta campaña, o las genero yo?". Recomienda fotos reales del negocio cuando existan: son la mejor prueba y evitan falsear el producto.

- **Imágenes del usuario:** revisa que cumplan las specs. Si el ratio no encaja, propón un recorte (y hazlo si tu entorno lo permite) antes de enviarlas.
- **Generadas por ti:** usa la herramienta de imágenes de tu propio entorno. SaleADS no genera imágenes desde el MCP. Crea exactamente las que pide `creatives.count`, en los ratios de `creatives.ratios`. Muestra las imágenes al usuario y pide su visto bueno antes de entregarlas.
- **Ninguna opción funciona** (tu entorno no genera imágenes y el usuario no tiene): ofrece el paso de imágenes en la web de SaleADS (ver 3C).

### 3. Entregar las imágenes a SaleADS

Elige el camino según tu entorno:

| Entorno | Camino |
|---|---|
| La imagen ya tiene una URL **https pública** (p. ej. el usuario la tiene alojada, o tu herramienta devuelve una URL pública descargable) | **3A. URL directa** |
| La imagen es un **archivo local** y puedes ejecutar comandos o HTTP (Claude Code, Codex, Cursor, agentes CLI) | **3B. Subida firmada** |
| No hay URL pública ni forma de hacer HTTP PUT (p. ej. la imagen solo existe dentro del chat) | **3C. Subir en SaleADS** |

#### 3A. URL directa

Llama `saleads_set_campaign_images` con `strategy_plan_id`, `item_id` e `images: [{ "url": "https://…" }, …]`.

No uses URLs `http://`, `localhost`, IPs privadas, `data:` ni links que exijan login: serán rechazadas (`MCP-E-IMAGE-URL-REJECTED`).

#### 3B. Subida firmada (HTTP PUT)

Por cada imagen:

1. Verifica localmente formato, peso y dimensiones si puedes (p. ej. `sips -g pixelWidth -g pixelHeight archivo.png` en macOS, `identify` de ImageMagick o Python/Pillow).
2. Llama `saleads_create_media_upload` con `business_id`, `content_type` (`image/jpeg`, `image/png` o `image/webp`, el tipo **real** del archivo) y `filename` opcional. Devuelve `upload_id`, `upload_url`, `method: "PUT"`, `headers`, `expires_at` (15 min) y `max_bytes`.
3. Sube el archivo con PUT, enviando **todos** los `headers` devueltos (incluido `x-goog-content-length-range`, si viene) y el cuerpo binario:

   ```bash
   curl -sS -X PUT \
     -H "Content-Type: image/png" \
     --data-binary @imagen-4x5.png \
     "$UPLOAD_URL" -o /dev/null -w "%{http_code}\n"
   ```

   Agrega un `-H` por cada header adicional que devolvió la tool. Un `200` indica éxito. No imprimas la URL en el chat ni en logs.
4. Si el PUT falla o pasaron 15 minutos, pide una subida nueva con `saleads_create_media_upload`. No reutilices una URL vencida.
5. Llama `saleads_set_campaign_images` con `images: [{ "upload_id": "…" }, …]`.

Cada elemento de `images` lleva **exactamente uno** de `upload_id` o `url`.

#### 3C. Subir en SaleADS (fallback)

Si `saleads_set_campaign_images` devuelve `action_url` (en el resultado o en el error: link al paso de imágenes de esa campaña), entrégalo:

> No puedo enviar estas imágenes desde aquí. Abre este link, sube las imágenes de la campaña *nombre* en SaleADS y avísame cuando termines: *action_url*.

Si no tienes link, pide al usuario abrir su plan en SaleADS y subirlas en el paso de imágenes de esa campaña. Después verifica con `saleads_get_plan` (`images_ready`).

#### Resultado de `saleads_set_campaign_images`

Puede devolver el resultado o una operación (`operation_id`, `poll_after_s` ≈ 5 s): consulta con `saleads_get_operation` sin relanzar la tool.

El resultado trae `slots[]` (`slot_index`, `ratio`, `accepted`, `reason`), `required_count`, `complete` y, a veces, `action_url`.

- La imagen en la posición *i* de `images` llena el slot *i*. Los slots que no envías conservan la imagen que ya tenían.
- Una imagen rechazada no hace fallar la llamada: vuelve con `accepted: false` y `reason` en su slot. Corrige según `reason` (recortar, reducir peso, cambiar formato) y reenvía la lista con las imágenes ya aceptadas en sus mismas posiciones y la corregida en la posición del slot rechazado.
- Repite hasta `complete: true`.
- Usa un `client_request_id` estable por lote (p. ej. `images-<item_id>-v1`) y cámbialo (`-v2`) cuando envíes imágenes distintas.

### 4. Preparar el borrador y generar textos

Con `complete: true`, llama `saleads_prepare_campaign` con `strategy_plan_id`, `item_id` y `client_request_id`. Tarda ~1 minuto; puede devolver una operación (consulta con `saleads_get_operation`, `poll_after_s` ≈ 10 s).

Devuelve `draft_id` y `copies[]` (`copy_index`, `primary_text`, `headline`, `description`). Si responde `MCP-E-IMAGES-INCOMPLETE`, vuelve al paso 3.

### 5. Revisar y editar los textos

Muestra los textos al usuario junto con las imágenes de la campaña, uno por `copy_index`. Si ves una mejora clara (primera línea más fuerte, CTA más concreto para el destino), propónla ya redactada en lugar de preguntar qué quiere cambiar (la posición del copy en el borrador; es la llave para editarlo). Antes de aceptar cualquier cambio, aplica estas reglas:

**Se puede:**

- Ajustar tono, longitud, orden o claridad.
- Corregir ortografía y datos que el usuario confirma (horario, ciudad, forma de contacto).
- Adaptar el llamado a la acción al destino del plan (escribir por WhatsApp o visitar la web).
- Usar variantes de mensaje que ya están en la estrategia aprobada.

**No se puede:**

- Cambiar la promesa o la tensión de la hipótesis de esa campaña.
- Agregar testimonios, citas, cifras, porcentajes, rankings, certificaciones, garantías, descuentos, urgencia falsa ("últimas unidades") o precios no confirmados.
- Copiar claims de competidores o de la web de terceros.

Si el usuario pide agregar algo de la lista prohibida, explícale que no se puede publicar sin evidencia aprobada. Si el dato es real (p. ej. tiene una garantía escrita), sugiere registrarlo en el perfil del negocio y regenerar la estrategia; no lo metas solo en el anuncio.

Para guardar, llama `saleads_update_campaign_copies` con `draft_id` y `copies[]`: cada elemento con su `copy_index` original (obligatorio, el mismo que devolvió `saleads_prepare_campaign`), `primary_text` (obligatorio, ≤2200), `headline` (≤255) y `description` (≤255). No inventes índices nuevos ni cambies el orden. Envía la lista completa de copies de ese borrador, incluidas las que no cambiaron, para no perder ninguna.

### 6. Aprobar la campaña

Con el visto bueno del usuario, llama `saleads_approve_campaign` con `strategy_plan_id`, `item_id` y `draft_id`.

| `status` | Qué haces |
|---|---|
| `configured` | Lista. Di "Campaña *nombre* lista (k de N)". |
| `pending_configuration` | Quedó aprobada, pero el plan aún no la marca `configured`. Verifica en un momento con `saleads_get_plan`; no la apruebes de nuevo mientras tanto. |
| `not_ready` | Resuelve cada `failed_checks[].code` según su `next_action` (imágenes, textos, presupuesto o Meta) y vuelve a aprobar. |
| `launched` | Ya estaba lanzada: no hay nada que configurar. |

Errores:

- `MCP-E-CAMPAIGN-NOT-READY`: resuelve cada check de la lista (`failed_checks` en `details`) y vuelve a aprobar.
- `MCP-E-META-NOT-READY`: entrega el `action_url` para conectar Meta.
- `MCP-E-CONFLICT`: relee con `saleads_get_plan` y repite.

Aprobar una campaña **no la lanza**. Cuando todas estén `configured`, vuelve a `saleads-strategic-plan` (paso de lanzamiento) o usa `saleads-launch-and-results`.

## Errores de esta etapa

| Código | Qué haces |
|---|---|
| `MCP-E-IMAGE-INVALID` | Ajusta formato, peso, tamaño o ratio (1:1, 4:5, 9:16) y reenvía. |
| `MCP-E-IMAGE-URL-REJECTED` | Cambia a subida firmada (3B) o a subir en SaleADS (3C). |
| `MCP-E-IMAGES-INCOMPLETE` | Envía las imágenes faltantes con `saleads_set_campaign_images`. |
| `MCP-E-CAMPAIGN-NOT-READY` | Resuelve los `failed_checks`. |
| `MCP-E-CAMPAIGN-QUOTA-EXCEEDED` | Explica el límite de campañas del plan de suscripción. |
| `MCP-E-INVALID-INPUT` | Corrige los campos de `details.fields` (p. ej. longitudes de copies) y reintenta. |
| `MCP-E-IN-PROGRESS` | Consulta la operación en curso con `saleads_get_operation`. |
| `MCP-E-OPERATION-CANCELLED` | La operación se canceló: reinicia el paso solo si el usuario lo pide. |
| `MCP-E-RATE-LIMITED` | Espera `details.retry_after_s`. |

Tabla completa en la skill `saleads-strategic-plan` (`references/errores.md`).
