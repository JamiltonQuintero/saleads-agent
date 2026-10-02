# Generar imágenes para una campaña de SaleADS

Lee esto antes de generar imágenes con la herramienta de tu entorno (generador de imágenes del chat, un modelo de imagen local, etc.). SaleADS no genera imágenes desde el MCP en esta versión.

## 1. Del mensaje a la imagen

Parte de la hipótesis de la campaña, no de una idea genérica del producto.

| Elemento de la hipótesis | Qué muestra la imagen |
|---|---|
| **Tensión** (el problema o deseo) | El momento o la situación que vive la audiencia. |
| **Promesa** | El estado deseado o el uso del producto/servicio que lo resuelve. |
| **Prueba** | Solo lo que la evidencia aprobada respalda (producto real, lugar real, proceso). Si no hay prueba, no la simules. |
| **Audiencia prioritaria** | Edad, contexto y entorno plausibles para esa audiencia, sin personas identificables. |
| **Variante de mensaje** | Si se incluye texto en la imagen, sale de aquí y es breve. |

Para una campaña, todas sus imágenes comparten la misma hipótesis. Varía el encuadre, la escena o el formato, no la promesa.

## 2. Plantilla de prompt

```
Anuncio para Meta, formato <ratio> (<ancho>×<alto>).
Negocio: <tipo de negocio> en <ciudad/país>.
Escena: <situación que refleja la tensión o la promesa>.
Sujeto principal: <producto real / servicio en uso / lugar>, fiel a <descripción o foto de referencia>.
Estilo: <fotografía natural | producto en estudio | ilustración simple>, luz <…>, paleta <colores de la marca si se conocen>.
Composición: sujeto principal centrado y legible en móvil; espacio libre para texto en <zona>.
Texto en la imagen: <ninguno | "frase corta de la variante de mensaje">.
Evitar: logos de terceros, personas famosas o identificables, sellos, estrellas, reseñas, cifras, precios, porcentajes, "garantizado", marcas de agua.
```

## 3. Reglas de composición por ratio

| Ratio | Tamaño | Uso | Consejos |
|---|---|---|---|
| 1:1 | 1080×1080 | Feed | Sujeto grande y centrado. |
| 4:5 | 1080×1350 | Feed móvil (ocupa más pantalla) | Sujeto en el centro, aire arriba y abajo. |
| 9:16 | 1080×1920 | Stories y Reels | Deja márgenes amplios arriba y abajo: la interfaz tapa esas zonas. Nada importante en los bordes. |

Si generas una imagen y necesitas otro ratio, genera de nuevo en ese ratio o recorta sin cortar el sujeto. No estires la imagen.

## 4. Texto dentro de la imagen

- Opcional. Menos es mejor: 3–7 palabras.
- Sale de una variante de mensaje aprobada o del nombre de la oferta.
- Revisa la ortografía en la imagen final; los generadores suelen deformar letras. Si el texto sale mal, genera sin texto.
- Nunca: precios o descuentos no confirmados, cifras, "n.º 1", "garantía", "certificado", estrellas, citas de clientes.

## 5. Fidelidad del producto

- Si el usuario tiene fotos del producto, pídelas y úsalas como referencia (si tu herramienta lo permite) o úsalas directamente.
- Si la imagen generada cambia el producto (forma, color, logo, empaque), descártala.
- Para servicios, muestra el contexto del servicio en vez de un "resultado" que no se puede garantizar (p. ej. no muestres un "antes y después" inventado).

## 6. Revisión antes de entregar

Muestra las imágenes al usuario y confirma:

- [ ] Ratio y tamaño correctos (≥600 px en el lado corto).
- [ ] Formato jpeg, png o webp; peso ≤ 8 MB.
- [ ] La imagen expresa la hipótesis de **esta** campaña.
- [ ] El producto o servicio se ve como es.
- [ ] No hay claims, cifras, sellos, reseñas ni personas identificables.
- [ ] El usuario dio su visto bueno.

## 7. Según el entorno

- **Chat web (claude.ai, ChatGPT):** la imagen generada suele quedar solo dentro del chat, sin URL https pública. Pide al usuario descargarla y subirla en SaleADS (link del paso de imágenes), o que te dé una URL https pública si la aloja él.
- **Claude Code, Codex, Cursor u otro agente con archivos:** guarda la imagen en un archivo, verifica dimensiones y peso, y usa `saleads_create_media_upload` + PUT.
- **Herramienta que devuelve URL https pública y descargable sin login:** pásala directo a `saleads_set_campaign_images`.
