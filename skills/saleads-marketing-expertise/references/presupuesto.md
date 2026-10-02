# Presupuesto de pauta: cómo pensarlo y cómo explicarlo

El presupuesto mensual de pauta es **lo que se le paga a Meta** por mostrar los anuncios. Es aparte de la suscripción de SaleADS y se cobra al método de pago de la cuenta publicitaria del usuario en Meta.

## Quién decide qué

- **El usuario confirma el monto y la moneda.** Nunca envíes a `saleads_start_strategy` un número que el usuario no confirmó en la conversación.
- **Tú puedes proponer** un monto de referencia y explicar por qué, siempre marcado como propuesta: "Te propongo empezar con 1.200.000 COP al mes. ¿Te sirve o prefieres otro monto?".
- **SaleADS reparte** el presupuesto entre campañas, decide cuántas campañas financia y aplica los mínimos. Tú no divides el monto ni prometes un número de campañas.

## Referencias para proponer (orientativas)

Úsalas para dar una propuesta razonable y explicar trade-offs. Los valores exactos los calcula SaleADS y pueden cambiar.

| Concepto | Orientación |
|---|---|
| Mínimo técnico de un plan | Alrededor de 100 USD al mes (o su equivalente). Por debajo, SaleADS no puede financiar campañas sanas. |
| Mínimo diario por campaña en Meta | Del orden de 1–2 USD por día por campaña para mensajes; las campañas a la web cuestan aproximadamente el doble. |
| Rango típico de los clientes de SaleADS | 250–1.000 USD al mes. |
| Con 250–500 USD/mes | SaleADS financia **una idea (hipótesis) activa** y guarda una retadora para después. |
| Desde ~500 USD/mes | Puede probar dos ejecuciones (ganchos o imágenes) **de la misma idea**. Más dinero no significa más ideas a la vez. |
| Fase de aprendizaje | Los primeros ~7 días de cada campaña son de aprendizaje: los costos suben y bajan. No se juzga antes. |
| Señal útil | Una campaña empieza a mostrar señal estable cuando junta del orden de 15–30 resultados (mensajes, compras). Antes es exploración. |

Cómo proponer un monto cuando el usuario no sabe:

1. Pregunta en forma cerrada por el rango que está dispuesto a gastar, o propón uno según su negocio: un servicio local que cierra por WhatsApp puede empezar cerca del mínimo; un e-commerce que vende en la web necesita más porque la web cuesta más por resultado.
2. Traduce a su moneda y a un número por día: "1.200.000 COP al mes son unos 40.000 COP al día".
3. Explica qué compra con ese monto: "Con eso SaleADS financia una idea a la vez y te da señal en 2–3 semanas".
4. Pide confirmación del número concreto.

Si el usuario da un rango ("entre 1 y 2 millones"), propón un número del rango y confírmalo.

## Por qué pocas hipótesis con poco presupuesto

Explícalo así cuando pregunten "¿por qué solo una idea?" o "¿por qué no más campañas?":

> Si repartimos 300 USD entre cuatro ideas, cada una recibe tan poco que ninguna sale de la fase de aprendizaje y no sabremos cuál funcionó. Con una idea bien financiada, Meta aprende más rápido y cada resultado te dice algo. Cuando haya señal, probamos la siguiente.

- Varias imágenes de una misma idea **no** son varias ideas: SaleADS puede pedir 5 piezas para una campaña porque Meta necesita variedad visual, pero todas cuentan la misma historia.
- Las campañas de fases posteriores aparecen bloqueadas (`locked`) al inicio. Se habilitan cuando el primer bloque lleva al menos ~7 días.

## Cambios de presupuesto

- Antes de activar: `saleads_update_plan` con el nuevo monto **en la moneda del plan** y la confirmación del usuario. Si el plan se recompila, devuelve `replaced_plan_id` y desde ahí se usa ese plan.
- Si el monto queda bajo el mínimo, SaleADS responde con `MCP-E-BUDGET-BELOW-MINIMUM` y el mínimo en `details`. Muéstralo y pregunta si sube.
- Después del lanzamiento, subir o bajar presupuesto se hace en SaleADS, no desde el chat. Meta reacciona mal a cambios bruscos: los cambios grandes reinician el aprendizaje.

## Errores comunes que debes evitar

- Proponer un monto como si fuera "la recomendación oficial de SaleADS". Es tu propuesta razonada.
- Prometer resultados por monto ("con 500 USD tendrás 100 ventas").
- Sugerir mover dinero entre campañas según métricas: SaleADS no lo hace como parte de este flujo y tú tampoco.
- Olvidar que el gasto se cobra en Meta: el usuario necesita un método de pago activo en su cuenta publicitaria.
