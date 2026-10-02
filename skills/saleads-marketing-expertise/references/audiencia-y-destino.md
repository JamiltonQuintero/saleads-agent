# Audiencia prioritaria y destino

## Elegir la audiencia prioritaria

SaleADS trabaja con **una audiencia prioritaria amplia** por oferta, descrita por su momento de compra, no solo por datos demográficos. Meta encuentra a las personas; tu trabajo es que el mensaje haga que **la persona correcta se reconozca**.

### Heurísticas

1. **Empieza por el momento, no por la edad.** "Quien acaba de mudarse y necesita amoblar", "quien tiene un evento en dos semanas", "dueño de negocio que ya intentó pautar y perdió plata".
2. **Elige el segmento con dolor más urgente y capacidad de pago.** Entre "estudiantes" y "profesionales que necesitan el título para un ascenso", el segundo compra antes.
3. **Prefiere quien ya compra la categoría.** Es más fácil cambiar de proveedor a alguien que ya paga por algo similar que educar a quien nunca lo ha comprado.
4. **Una audiencia, no tres.** Con presupuesto bajo, dividir audiencias divide el aprendizaje. Si hay dos segmentos muy distintos, elige uno ahora y deja el otro como siguiente prueba.
5. **Datos demográficos solo con evidencia.** Si el usuario no dijo "mis clientas son mujeres de 30–45", no lo afirmes. Puedes usarlo como supuesto creativo (quién aparece en la imagen), marcado como supuesto.
6. **Ubicación realista.** Servicios a domicilio o locales: la ciudad y alrededores donde realmente atiende. Envíos nacionales: país. Pregunta solo si no está claro en el perfil o la web.

### Cómo proponerla

> **Audiencia que te propongo:** papás y mamás de Medellín con hijos pequeños que buscan una actividad segura para el fin de semana. Por qué: son quienes deciden y pagan, y la seguridad es lo que más les preocupa. ¿Te suena a tu cliente real?

Ubicaciones: usa `saleads_search_locations` para encontrar la ubicación exacta antes de `saleads_update_plan`; no adivines entre ciudades con el mismo nombre.

## Destino: WhatsApp (`messages`) o web (`web`)

| Elige WhatsApp / mensajes cuando… | Elige web cuando… |
|---|---|
| El cliente pregunta antes de comprar (precio a medida, tallas, disponibilidad, agenda) | El cliente puede comprar solo: precio claro, carrito, pago en línea |
| El ticket es medio o alto, o requiere confianza (servicios, salud, educación) | Hay tienda en línea funcionando (Shopify, WooCommerce, etc.) |
| El negocio cierra ventas conversando y responde rápido | El catálogo es grande y la persona quiere explorar |
| No hay web, o la web es débil | Hay píxel de Meta instalado para medir compras |
| Se vende con pago contra entrega o transferencia | Se quiere escalar sin depender de la capacidad de atención |

Trade-offs que debes explicar en una línea:

- **WhatsApp** suele dar resultados más baratos y más rápido en LATAM, pero exige responder en minutos. Si el negocio tarda horas, los clientes se enfrían y la pauta se desperdicia.
- **Web** escala mejor y no depende de quién atiende, pero cuesta más por resultado (en SaleADS su mínimo es aproximadamente el doble) y necesita el píxel para que Meta aprenda.
- Si no sabes, **WhatsApp es el punto de partida más seguro** para un negocio pequeño de servicios o con ticket medio. Para e-commerce con tienda y píxel, la web.

Requisitos en Meta según destino (revísalos con `saleads_get_meta_status` pasando `destination`):

- `messages`: página de Facebook y, idealmente, WhatsApp Business conectado.
- `web`: sitio funcionando y píxel; sin píxel, Meta aprende peor.

## Capacidad de atención (pregúntala si el destino es WhatsApp)

Una pregunta vale más que diez: "¿Quién va a responder los mensajes y en cuánto tiempo?". Si la respuesta es "yo, cuando pueda", recomienda:

- Respuestas rápidas guardadas en WhatsApp Business (saludo, precio, horarios).
- Activar la pauta en horarios en que puede atender, o empezar con un presupuesto que pueda manejar.
- Ver la skill `saleads-ventas-whatsapp` para el guion de conversación.
