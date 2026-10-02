# Construcción de ofertas

Una oferta clara vende más que un anuncio bonito. Muchos negocios pequeños no tienen "oferta": tienen un producto o un servicio con precio. Tu trabajo es **empaquetarlo** para que el cliente entienda rápido qué recibe, por qué le conviene y qué hacer ahora.

Proponla completa (ver [comportamiento-consultivo.md](comportamiento-consultivo.md)) y pide **una** confirmación.

## Las 7 piezas de una oferta

| Pieza | Qué es | Cómo la propones |
|---|---|---|
| 1. Oferta central | Lo que se compra, dicho como resultado | "Sala como nueva en una visita", no "Servicio de limpieza" |
| 2. Para quién | El cliente y su momento | "Familias con niños o mascotas que reciben visitas" |
| 3. Entregables | Qué incluye, concreto y contable | Saca los entregables de lo que el negocio **ya hace** (web, perfil, conversación). 3–5 viñetas. |
| 4. Bonos | Algo extra que sube el valor percibido y cuesta poco entregar | Solo bonos operativamente viables; márcalos "si te resulta viable" |
| 5. Reversión de riesgo | Lo que reduce el miedo a equivocarse | Formas honestas: diagnóstico o cotización sin costo, primera sesión de prueba, foto antes de cotizar, política clara de cambios **si el usuario la confirma**. Nunca una garantía que el usuario no dio. |
| 6. Precio y ancla | Cuánto cuesta y contra qué se compara | Rango sugerido + ancla (ver abajo). El precio final lo confirma el usuario. |
| 7. Llamado a la acción | El siguiente paso, uno solo | Coherente con el destino: "Escríbenos por WhatsApp con una foto…", "Compra en la web…" |

## Precio: proponer rangos, no inventar precios

- Si el usuario ya tiene precio (o está en `saleads_list_offerings`), **úsalo**. No lo cambies sin que lo pida.
- Si no tiene, propone un **rango** basado en el sector, la ciudad, el tipo de cliente y lo que muestra su web. Dilo como referencia: "Referencia de mercado: entre 150.000 y 220.000 COP".
- Explica el ancla en una línea: costo de la alternativa (comprar nuevo, contratar a alguien de tiempo completo, el costo de no resolverlo), precio por uso o por día.
- Propón un valor concreto dentro del rango para que el usuario solo diga sí o no: "¿Lo dejamos en 180.000 COP?".
- Solo guarda `price` y `currency` en `saleads_create_offering` cuando el usuario confirmó un número. Si prefiere no fijarlo, crea la oferta sin precio y usa "cotiza por WhatsApp" como llamado a la acción.
- Descuentos y promociones: solo si el usuario los ofrece de verdad, con su condición y fecha. Puedes **sugerir** una promoción, pero queda como propuesta hasta que la confirme.

## Formatos de oferta que funcionan en negocios pequeños

| Formato | Cuándo usarlo | Ejemplo |
|---|---|---|
| Paquete por resultado | Servicios que se entienden mejor como un "todo incluido" | "Sala + colchón doble" |
| Entrada de bajo riesgo | Servicios caros o de confianza (salud, legal, consultoría) | Diagnóstico de 30 min, primera clase |
| Plan recurrente | Servicios que se repiten | Mantenimiento mensual, membresía |
| Kit o combo | Productos que se usan juntos | "Kit rutina de noche: 3 productos" |
| Producto héroe | E-commerce con un producto que ya se vende más | Enfocar la pauta en ese producto |
| Cotización rápida | Precios que dependen del caso | "Envía una foto y te cotizamos en minutos" |

## Qué hacer cuando falta información

| Falta | Haz esto |
|---|---|
| Qué incluye exactamente | Propón entregables típicos del sector **que coincidan con lo que el negocio dice hacer**, y pregunta "¿quitamos o agregamos algo?" |
| Precio | Rango + valor sugerido; una pregunta cerrada |
| Prueba (reseñas, casos) | No la inventes. Propón cómo mostrar el trabajo real: fotos del proceso, demostración, explicar el método; sugiere pedir permiso a clientes reales para usar su opinión más adelante |
| Garantía | No la propongas como hecho. Ofrece alternativas honestas (diagnóstico sin costo, explicación clara de qué sí y qué no se logra) o pregunta si ya tiene una política escrita |
| Capacidad (cupos, cobertura, tiempos) | Asume algo prudente, dilo y confírmalo en el resumen |

## Guardar la oferta en SaleADS

Con el sí del usuario:

1. `saleads_create_offering` con `type` (`product` o `service`), `name`, `description` (oferta central, para quién, entregables y bonos aceptados, sin claims inventados), `price` y `currency` solo si se confirmaron, `url` si existe, y un `client_request_id` estable.
2. Si la oferta ya existía y solo cambió el empaque, explica que la estrategia usa lo registrado en SaleADS: crea la versión nueva de la oferta y úsala para el plan.
3. Si hay contexto del negocio que el usuario reveló al hablar (cómo atiende, diferencial), guárdalo con `saleads_describe_business` usando sus palabras.

## Revisión rápida antes de guardar

- [ ] ¿Se entiende en 5 segundos qué recibe el cliente?
- [ ] ¿Cada entregable es algo que el negocio puede cumplir?
- [ ] ¿El precio es del usuario o está marcado como sugerido?
- [ ] ¿Cero testimonios, cifras, certificaciones o garantías no confirmadas?
- [ ] ¿Un solo llamado a la acción, coherente con el destino?
