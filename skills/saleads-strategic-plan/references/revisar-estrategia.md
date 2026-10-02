# Cómo presentar la estrategia para aprobación

`saleads_get_strategy` (con `detail: "concise"`) devuelve: `status`, `revision`, `diagnosis`, `positioning`, `priority_audience`, `hypotheses[]` (tensión, promesa, prueba y variantes de mensaje), `quality_summary`, `unknowns[]` y `conflicts[]`.

El objetivo es que el usuario entienda **qué se va a comunicar y por qué**, en menos de un minuto de lectura, y decida si aprueba.

## Plantilla

```
**Estrategia para <oferta>** (versión <revision>)

**Diagnóstico:** <1–2 frases de diagnosis>

**Posicionamiento:** <1 frase>
**Audiencia prioritaria:** <quién, dónde, qué le importa>

**Hipótesis que vamos a probar**
1. <nombre o rol> — Tensión: <…> · Promesa: <…> · Prueba: <…>
   Mensaje de ejemplo: "<una variante tal cual>"
2. …

**Lo que falta o no está claro:** <unknowns y conflicts, tal cual>
**Advertencias de calidad:** <si quality_summary trae alguna>
**Mi lectura:** <1–2 líneas: qué la hace sólida, su punto débil y una acción concreta>

¿La apruebas para crear el plan de campañas? (No gasta dinero.) También puedo generar otra versión.
```

## Reglas

- **Cita, no reescribas.** Las variantes de mensaje se muestran como vienen. Puedes acortar la explicación, no el mensaje.
- **La prueba es la que trae la estrategia.** Si una hipótesis no tiene prueba, di "sin prueba aportada". No propongas testimonios, cifras, rankings, certificaciones ni garantías para llenar el hueco.
- **Vacíos visibles.** `unknowns` y `conflicts` se muestran siempre que existan. Si el usuario puede resolver uno (p. ej. "¿haces envíos a todo el país?"), sugiérelo: se corrige en el perfil del negocio y se regenera.
- **Sin promesas de resultado.** No digas "esta estrategia va a vender más". Son hipótesis a validar con pauta.
- **Una hipótesis principal por campaña.** Si el usuario pregunta cómo se reparten, explica que cada campaña del plan prueba una hipótesis con su propio rol; el reparto exacto aparece al compilar el plan.
- **Idioma.** Presenta en el idioma del usuario. Si la estrategia está en otro idioma (según `locale`), indícalo.
- **Lectura de consultor, no reescritura.** Tu opinión va aparte, marcada como "Mi lectura". Si propones mejorar algo (p. ej. aportar fotos reales como prueba, aclarar el precio), la mejora se hace en la fuente (perfil u oferta) y se regenera; no cambias el texto de la estrategia.
- **Lenguaje simple.** Para usuarios nuevos, traduce los términos: tensión = "lo que le preocupa", promesa = "lo que le ofrecemos lograr", prueba = "por qué nos va a creer".

## Ejemplo corto (ilustrativo)

> **Estrategia para Clases de natación infantil** (versión 1)
>
> **Diagnóstico:** los padres de la zona buscan seguridad en el agua, pero comparan por horario y cercanía.
> **Audiencia prioritaria:** madres y padres con hijos de 4–10 años en el norte de la ciudad.
>
> **Hipótesis**
> 1. Seguridad primero — Tensión: miedo a que el niño no sepa reaccionar en el agua · Promesa: aprende a flotar y pedir ayuda · Prueba: método por niveles descrito en la web.
>    Mensaje: "Que tu hijo aprenda a sentirse seguro en el agua, a su ritmo."
>
> **Falta:** precio por mes; no hay reseñas aportadas.
>
> ¿La apruebas para crear el plan? No gasta dinero.

El ejemplo muestra el formato. No uses su contenido para otros negocios.
