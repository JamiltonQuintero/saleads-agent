# Leer resultados sin inventar causas

Las métricas de `saleads_get_results` son **observaciones**. Sirven para entender qué está pasando y decidir el siguiente paso, no para declarar ganadores ni prometer el futuro.

## Orden de lectura

1. **¿Cuánto tiempo lleva?** Menos de ~7 días por campaña = fase de aprendizaje. Los costos suben y bajan; dilo antes de cualquier número.
2. **¿Cuánto volumen hay?** Con menos de ~15 resultados (mensajes, compras) por campaña, todo es exploratorio.
3. **Resultado de negocio primero:** conversaciones o compras y costo por resultado. Impresiones, clics y CTR son diagnóstico, no objetivo.
4. **Por campaña, con su rol e hipótesis**, para que el usuario sepa qué está probando cada una.
5. **Una siguiente acción concreta**, si existe.

## Qué puedes decir y qué no

| Puedes decir | No puedes decir |
|---|---|
| "En 7 días la campaña A registró 12 conversaciones y la B 4, con gasto parecido. Es una diferencia observada, todavía con poco volumen." | "La campaña A ganó." |
| "El costo por conversación bajó desde el día 5, algo normal al salir del aprendizaje." | "El mensaje de precio funciona mejor." |
| "Esta campaña lleva 6 días con gasto y sin ningún mensaje; vale la pena revisar." | "A este ritmo vas a vender X." |
| "No hay datos de compras porque el destino es WhatsApp; las ventas se cierran en el chat." | "Tu negocio está por encima del promedio del sector." |

Por qué no se declaran ganadores: las campañas tienen roles, audiencias, presupuestos y tiempos distintos. Un número agregado no aísla la causa. Un buen resultado de una sola imagen no valida un ángulo.

## Diagnóstico simple cuando algo no va bien

Usa preguntas, no conclusiones:

| Síntoma | Posibles causas a revisar con el usuario |
|---|---|
| Muchas impresiones, pocos clics | El gancho o la imagen no detienen a la audiencia; la promesa no es clara. |
| Clics, pero pocos mensajes o compras | La página o el primer mensaje de WhatsApp no continúa la promesa; precio o envío sorprenden. |
| Muchos mensajes, pocas ventas | Tiempo de respuesta, guion de conversación, seguimiento. Ver la skill `saleads-ventas-whatsapp`. |
| Gasto sin resultados por varios días con volumen | Error o rechazo en Meta, método de pago, o la hipótesis no conecta. Revisar estado en SaleADS. |
| Costos subiendo después de semanas | Posible cansancio del creativo; renovar piezas dentro de la misma idea. |

Fuera del anuncio también se vende o se pierde: tiempo de respuesta, stock, fotos de la web, comentarios negativos sin responder en el anuncio.

## Qué hace SaleADS con los resultados

- Revisa las campañas en una cadencia fija (alrededor de los días 7, 10 y 15).
- Hacia el día 15 puede proponer una nueva versión de la estrategia que conserva lo que tuvo señal y cambia una sola cosa. El usuario la aprueba.
- No mueve presupuesto entre campañas ni cambia pujas o audiencias por su cuenta dentro de este aprendizaje, y no compara con otros negocios.

Tú tampoco: no propongas reasignar presupuesto entre campañas a partir de métricas.

## Cuándo sugerir pausar (sin ejecutarlo)

- El usuario quiere dejar de gastar (presupuesto, stock agotado, cierre).
- La oferta cambió o terminó la promoción.
- Hay un error o rechazo de Meta.
- Gasto sostenido sin ningún resultado durante varios días con volumen suficiente.
- No puede atender los mensajes que llegan.

No sugieras pausar solo porque una campaña cuesta más que otra en pocos días. Pausar exige confirmación explícita y un motivo (`saleads_pause_campaign`).
