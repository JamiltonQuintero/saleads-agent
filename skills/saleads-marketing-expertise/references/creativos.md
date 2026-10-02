# Creativos: buenas prácticas por formato y ubicación

Los creativos **ejecutan** la hipótesis aprobada de cada campaña. Cambian el gancho, la escena o el formato; no cambian la promesa, la prueba ni la audiencia. Detalle operativo (specs, subida, edición de textos) en la skill `saleads-campaign-creatives`.

## Principios

1. **El problema antes que el producto.** La persona tiene que reconocerse en el primer vistazo; después aparece la solución.
2. **El gancho decide.** En el feed la persona decide en menos de un segundo si se queda. La primera imagen o los primeros 3 segundos del video llevan la tensión o la promesa, no el logo.
3. **Una idea por pieza.** Un mensaje, un beneficio, un llamado a la acción.
4. **Se entiende sin sonido.** La mayoría ve videos en silencio: subtítulos siempre y texto en pantalla corto.
5. **Nativo, no publicidad de revista.** En Meta funcionan mejor las piezas que parecen contenido real: fotos reales del negocio, personas usando el producto, manos, procesos.
6. **Producto fiel.** Si se muestra el producto, se ve como es: forma, color, empaque.
7. **Nada inventado.** Ni en el texto ni dentro de la imagen: sin estrellas, reseñas, cifras, sellos, "garantizado", "n.º 1", precios o descuentos no confirmados.
8. **Variedad dentro de la idea.** Meta necesita varias piezas para aprender; varía encuadre, escena, persona o gancho, manteniendo la hipótesis.

## Por ratio y ubicación

| Ratio | Tamaño | Dónde se ve | Cómo componer |
|---|---|---|---|
| 1:1 | 1080×1080 | Feed de Facebook e Instagram | Sujeto grande y centrado. Funciona en casi todo. |
| 4:5 | 1080×1350 | Feed en móvil (ocupa más pantalla) | El más versátil para feed. Sujeto al centro, aire arriba y abajo. |
| 9:16 | 1080×1920 | Stories y Reels | Deja ~14 % libre arriba y ~20 % abajo: la interfaz tapa esas zonas. Nada importante en los bordes. |

En SaleADS, las campañas de reconocimiento suelen pedir 9:16 y las de ventas y escala 1:1; usa siempre lo que indique `creatives.ratios` de cada campaña.

## Imagen según la etapa

| Etapa de la campaña | Qué mostrar | Texto en imagen (opcional, 3–7 palabras) |
|---|---|---|
| Reconocimiento | El momento o el problema que vive la audiencia; personas en contexto | La tensión o el deseo, en sus palabras |
| Consideración | El proceso, el mecanismo, el detalle del producto, el antes/después **real** | La diferencia o el "cómo" |
| Conversión | Producto o servicio en uso, resultado deseado, contexto de compra | La promesa + acción ("Escríbenos hoy") |
| Remarketing | Recordatorio del producto, respuesta a la objeción | La objeción resuelta |

## Video: guion completo, no solo 15 segundos

SaleADS entrega guías de video; no produce el archivo final. El usuario graba o usa otra herramienta. Cuando ayudes con un guion:

- **El guion maestro debe estar completo en argumento**, normalmente de 30 a 120 segundos según lo que haya que explicar. Una versión de 15 segundos es un recorte opcional, no el punto de partida.
- Estructura flexible (la "columna narrativa"):
  1. **Gancho (0–3 s):** un momento cargado o una pregunta que abre curiosidad.
  2. **Reconocimiento:** "eso me pasa a mí".
  3. **Tensión:** la consecuencia, sin melodrama.
  4. **Giro:** por qué lo que probó antes no funcionó.
  5. **Mecanismo:** la nueva forma; el producto como puente.
  6. **Prueba o demostración:** lo que se puede mostrar de verdad.
  7. **Resultado:** cómo se siente y qué logra.
  8. **Llamado a la acción:** uno, acorde a la etapa y al destino.
- Ritmo: un cambio de plano, de ángulo o de texto cada 3–5 segundos. Sin intros lentas ni logo al inicio.
- Formatos que funcionan para negocios pequeños: persona hablando a cámara con el celular, demostración del producto o del proceso, "un día en el negocio", respuesta a una pregunta frecuente. El formato sirve al mensaje, no al revés.
- Graba vertical (9:16), con buena luz natural y audio claro; subtítulos siempre.
- Testimonios en video: solo de clientes reales que dieron permiso. No se simulan con actores presentados como clientes.

## Textos (copies)

- **Texto principal:** abre con la tensión o la promesa en la primera línea (es lo que se ve antes de "ver más"). Después, 2–3 líneas de beneficio concreto y el llamado a la acción.
- **Título:** la promesa o la oferta en pocas palabras.
- **Llamado a la acción coherente con el destino:** "Escríbenos por WhatsApp…" si es `messages`; "Compra en…" o "Mira el catálogo…" si es `web`.
- Lenguaje del cliente, no del negocio: "tu sofá sin manchas para la visita", no "soluciones integrales de limpieza".
- Ver qué se puede y qué no se puede editar en la skill `saleads-campaign-creatives`.

## Errores que queman presupuesto

- Logo o intro lenta al inicio.
- Video sin subtítulos.
- Mostrar el producto antes del problema.
- CTA genérico ("Más información") en una campaña de conversión.
- Imágenes de banco que no se parecen al negocio real.
- Texto de la imagen ilegible en móvil o deformado por el generador.
