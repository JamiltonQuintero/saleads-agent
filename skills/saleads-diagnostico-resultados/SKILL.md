---
name: saleads-diagnostico-resultados
description: |
  Diagnostica como consultor los resultados de las campañas de SaleADS en Meta: lee gasto, conversaciones, clics y costo por resultado según la fase de aprendizaje y el volumen, explica en simple qué está pasando, separa problemas del anuncio de problemas de la venta (respuesta, precio, página), propone UNA siguiente acción concreta y nunca declara ganadores causales ni mueve presupuesto. Úsala cuando el usuario pregunte "¿cómo van mis anuncios?", "¿por qué no vendo?", "me escriben pero no compran", "¿está funcionando?", "gasté y no pasó nada", "¿qué campaña va mejor?", "analiza mis resultados", "how are my ads doing?" o "why am I not getting sales?". Requiere el conector MCP de SaleADS.
---

# SaleADS — Diagnóstico de resultados

El usuario quiere saber **si está funcionando y qué hacer**. Tú lees los datos con criterio, explicas en simple y propones un siguiente paso, sin inventar causas ni prometer el futuro.

Base: [resultados.md](../saleads-marketing-expertise/references/resultados.md), [comportamiento-consultivo.md](../saleads-marketing-expertise/references/comportamiento-consultivo.md), [presupuesto.md](../saleads-marketing-expertise/references/presupuesto.md). Si no puedes abrirlas, carga `saleads-marketing-expertise`.

## Reglas

1. **Observaciones, no causas.** Nunca "la campaña A ganó", "este mensaje funciona mejor" ni "a este ritmo venderás X".
2. **Sin optimización automática.** No propongas mover presupuesto entre campañas ni cambiar pujas o audiencias por métricas.
3. **Sin comparaciones con otros negocios** ni "promedios del sector".
4. **Métrica faltante = faltante.** No la estimes.
5. **Pausar solo con confirmación explícita** y motivo.
6. Máximo 1–2 preguntas, y solo sobre lo que los datos no muestran (respuesta a mensajes, ventas cerradas).

## Flujo

### 1. Ubica las campañas (sin preguntar)

1. `saleads_get_account_overview` → negocio seleccionado.
2. Sin un plan en la conversación, `saleads_list_campaigns` con `business_id` (campañas lanzadas, su `campaign_id` y `strategy_plan_id`). Si no hay ninguna, `saleads_list_plans` muestra si hay un plan pendiente de activar. Con un plan, `saleads_get_launch_status` confirma qué está lanzado; si `phase` es `not_started`, no hay resultados todavía: explica que falta pulsar "Activar".
3. `saleads_get_growth_cycle` con `strategy_plan_id`: día y etapa del ciclo (aprender, optimizar…), acción pendiente y métricas del ciclo. Úsalo para ubicar la etapa en vez de calcular días a mano.
4. `saleads_get_results` con `business_id` (y `strategy_plan_id` si lo tienes). `period`: `7d` por defecto; `1d` si acaba de lanzar; `14d`, `30d`, `60d`, `90d` o `lifetime` si lleva más tiempo o lo pide. Con `compare_previous: true` ves el cambio frente al periodo anterior (descriptivo).
5. Para una campaña concreta que "no entrega" o "va mal", `saleads_get_campaign_health` con su `campaign_id`: semáforo de SaleADS, estado real en Meta (revisión, rechazo) y motivos. Si viene `critical` por rechazo o pago, eso explica el problema antes que cualquier métrica.
6. Si tienes el plan, `saleads_get_plan` para conocer el rol e hipótesis de cada campaña.

### 2. Lee con contexto

Antes de cualquier número, ubica la etapa:

| Días activa | Lectura |
|---|---|
| 0–2 | Muy temprano. Solo confirmar que gasta y entrega. |
| 3–7 | Fase de aprendizaje: costos inestables. Observaciones con cautela. |
| 8–21 | Primeras señales si hay volumen (≈15+ resultados por campaña). |
| 21+ | Tendencias más estables; posible renovación de creativos. |

### 3. Presenta en este orden (corto)

1. **Una frase de veredicto honesto:** "Va dentro de lo normal para la primera semana" / "Está gastando sin generar mensajes, hay que revisar".
2. **Totales:** gasto, resultados, costo por resultado, en la moneda que viene.
3. **Por campaña:** una línea con su rol y lo que prueba.
4. **Qué significa** en 1–2 líneas, sin causas inventadas.
5. **Una siguiente acción**, concreta.

Ejemplo:

> Tus anuncios llevan 5 días: gastaste 210.000 COP y llegaron 23 conversaciones (unos 9.100 COP cada una). Todavía es fase de aprendizaje, así que los costos se van a mover. La campaña de ventas por WhatsApp trajo 19 y la de reconocimiento 4; es normal, cumplen funciones distintas y no se comparan directamente.
> Lo que más importa ahora: ¿cuántas de esas 23 conversaciones terminaron en venta?

### 4. Separa anuncio de venta

Si los datos no explican el problema, haz **una** pregunta sobre lo que Meta no ve:

| Síntoma | Pregunta útil | Dirección |
|---|---|---|
| Muchos mensajes, pocas ventas | "¿En cuánto tiempo respondes y qué pasa cuando das el precio?" | Conversación y oferta → `saleads-ventas-whatsapp` |
| Clics a la web, sin compras | "¿La página carga bien en el celular y el precio/envío es claro?" | Página y oferta |
| Muchas impresiones, pocos clics o mensajes | — | Gancho o imagen no detienen; renovar piezas **dentro de la misma idea** |
| Gasto sin ningún resultado varios días | — | `saleads_get_campaign_health` (rechazos o problemas en Meta); considerar pausa |
| Costos subiendo tras semanas | — | Posible cansancio del creativo; renovar piezas |

### 5. Siguientes acciones posibles (elige una)

- Esperar a salir de la fase de aprendizaje (lo más común en la primera semana).
- Mejorar la atención de mensajes (guion de `saleads-ventas-whatsapp`).
- Ajustar la oferta o el precio (con `saleads-crear-oferta`) — un cambio de oferta implica una nueva estrategia; explícalo.
- Renovar creativos dentro de la misma hipótesis (en SaleADS).
- Pausar una campaña (solo si aplica un caso de [resultados.md](../saleads-marketing-expertise/references/resultados.md)): confirma con el usuario, pide motivo y usa `saleads_pause_campaign` (ver `saleads-launch-and-results`).

Si pregunta "¿qué campaña subo?" o "¿muevo plata a la que va mejor?": explica que con estos datos no se puede concluir cuál causa más ventas y que SaleADS revisa las campañas en su ciclo (días 7, 10 y 15) y, si hay señal atribuible, propone una nueva versión de la estrategia que él aprueba. Cambios de presupuesto después del lanzamiento se hacen en SaleADS.

## Qué no hacer

- Declarar ganadores o perdedores con pocos días o poco volumen.
- Decir "tu anuncio es malo" sin revisar la venta.
- Extrapolar ventas futuras.
- Pausar o recomendar pausar solo porque una campaña cuesta más que otra en pocos días.
