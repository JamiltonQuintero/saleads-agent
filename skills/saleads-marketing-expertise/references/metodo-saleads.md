# Cómo piensa SaleADS

SaleADS no empieza por "hacer anuncios". Primero **decide qué conversación abrir con quién**, y después convierte esa decisión en campañas, imágenes, textos y guiones. Usa este método para explicar, proponer y revisar.

## La cadena de decisión

```
Oferta real + objetivo
  → Diagnóstico      ¿qué está pasando en el mercado y con este cliente?
  → Audiencia        quién, en qué momento, qué intenta lograr (no solo edad y ciudad)
  → Tensión          qué le duele o le frena, en sus palabras
  → Promesa          qué cambia para esa persona si compra
  → Prueba           por qué creerlo: solo evidencia real, o un plan honesto para mostrarla
  → Hipótesis        "Si le hablamos a X sobre Y con Z, va a escribir/comprar"
  → Variantes        2–3 formas de decir lo mismo (cambia el gancho, no la promesa)
  → Campañas         cada campaña del plan tiene un rol y UNA hipótesis principal
  → Creativos        imagen, texto y guion de video ejecutan esa hipótesis sin cambiarla
```

Cuando expliques una estrategia al usuario, usa esta misma cadena en lenguaje simple: "A quién le hablamos → qué le duele → qué le prometemos → por qué nos va a creer → qué le pedimos que haga".

### Diagnóstico

Una o dos frases sobre la situación real: qué busca el cliente, contra qué compara, qué lo frena. Ejemplo: "Los padres buscan seguridad en el agua, pero eligen por horario y cercanía".

### Audiencia: momento y trabajo por hacer

Una buena audiencia describe **un momento**: "dueña de casa que va a recibir visita el fin de semana y le da pena el sofá". La edad y el género solo se usan si hay evidencia; si no, quedan como desconocidos. SaleADS prefiere **una audiencia prioritaria amplia** antes que muchas pequeñas: con poco presupuesto, dividir audiencias divide el aprendizaje.

### Tensión, promesa, prueba

- **Tensión:** dolor funcional (no funciona), emocional (vergüenza, miedo), social (qué dirán) o costo de no actuar. Mejor si usa las palabras del cliente.
- **Promesa:** el resultado que la persona quiere, específico y sostenible. No "el mejor servicio"; sí "tu sala lista para la visita en una tarde".
- **Prueba:** fotos reales, demostración del proceso, explicación del método, datos del negocio confirmados. Si no hay, la estrategia define un **plan de prueba** (mostrar el proceso, antes/después real con permiso, explicar el método) y el claim queda restringido. Nunca se inventa.

### Creencia y mecanismo (para estrategias más finas)

- **Creencia actual → creencia deseada:** "Las manchas viejas no salen" → "Con inyección-succión salen la mayoría; te decimos cuáles no".
- **Mecanismo del problema y de la solución:** por qué falló lo que probó antes y qué hace distinto esta oferta.

### Familias de ángulo

Un ángulo es la puerta de entrada a la conversación. Familias útiles (no es una lista cerrada):

consecuencia del problema · transformación deseada · nueva forma / mecanismo · objeción invertida · identidad o estatus · demostración · comparación · valor económico · seguridad / riesgo · oportunidad.

Gancho, tono, formato, tipo de prueba, urgencia y CTA son **ejes de ejecución**, no ángulos. Se pueden variar sin cambiar la hipótesis.

## Una hipótesis principal por campaña

- Cada campaña prueba **una** hipótesis. Así, cuando hay resultados, se sabe a qué idea pertenecen.
- Las variantes de una campaña cambian el gancho o la imagen, no la promesa.
- Con presupuesto bajo, SaleADS **no reparte el dinero entre muchas ideas**. Financia una idea activa y guarda una "retadora" para después (ver [presupuesto.md](presupuesto.md)).

## Quién decide qué

| Decisión | Dueño |
|---|---|
| Datos del negocio y sus ofertas | El usuario (y la web analizada), guardados en SaleADS |
| Estrategia de comunicación (diagnóstico, hipótesis, mensajes) | SaleADS la genera; el usuario la aprueba o pide otra versión |
| Número de campañas, reparto del presupuesto, formatos y cantidad de creativos | SaleADS (motor de planes "Cerebro") |
| Monto mensual de pauta, moneda, destino, idioma | El usuario confirma; tú puedes proponer con razones |
| Imágenes y textos de cada campaña | Tú o el usuario los preparan, **dentro** de la hipótesis aprobada |
| Activar el plan | Solo el usuario, en SaleADS ("Activar") |
| Pausar una campaña | El usuario confirma en el chat; tú llamas la tool de pausa |
| Reanudar una campaña, cambiar de plan o comprar créditos | Solo el usuario, en SaleADS: tú entregas el link (`saleads_request_resume_campaign`, `actions` de `saleads_get_help`) |
| Resolver un bloqueo de Meta | El usuario, en la pantalla que abre el `action_url` de cada elemento de `blocker_details` (algunos se resuelven en Meta, no en SaleADS) |

Tú **propones insumos y revisas**. No reescribes la estrategia aprobada en el chat: si algo del negocio está mal, se corrige la fuente (perfil u oferta) y se regenera la estrategia.

## Fases del plan: F1, F2, F3

El plan que compila SaleADS ordena las campañas por fase. En la web aparecen como:

| Fase | Nombre en SaleADS | Para qué sirve | Destino típico |
|---|---|---|---|
| **F2** | Ventas | Conversaciones o ventas con gente que ya tiene la necesidad. Suele ir primero: con poco presupuesto, primero se busca venta. | WhatsApp |
| **F1** | Reconocimiento | Que la persona correcta reconozca su problema y conozca la marca. Crea público para después. | Instagram |
| **F3** | Escala | Llevar a la web y escalar lo que ya mostró señal, incluido remarketing cuando hay público acumulado. | Web |

- Las campañas se agrupan en **bloques**. El primer bloque se activa al lanzar; los siguientes se habilitan después, cuando el anterior lleva al menos ~7 días (fase de aprendizaje). Por eso en el plan algunas campañas aparecen como `locked`: no es un error, es la secuencia.
- Qué fases entran depende del tipo de oferta, del destino y del presupuesto. No prometas un número de campañas: lo calcula SaleADS.

## Etapas de comunicación

Cada campaña tiene además una etapa de comunicación que define qué puede pedir el anuncio:

| Etapa | Trabajo del anuncio | CTA permitido | No hagas |
|---|---|---|---|
| Reconocimiento | Que la persona se reconozca en un momento, dolor o deseo | descubrir, ver, guardar | "compra ya" |
| Consideración | Explicar mecanismo, prueba, diferencia u objeción | conocer más, comparar, preguntar | reversión de riesgo sin respaldo |
| Conversión | Unir promesa, oferta, prueba y acción | comprar, escribir por WhatsApp, cotizar, agendar | urgencia o garantía inventada |
| Remarketing | Resolver una objeción o recordar algo pendiente | volver, completar, retomar | escasez falsa o reseñas inventadas |

"Agresivo pero verdadero": más especificidad, emoción, contraste y manejo de objeciones. Nunca resultados, garantías, escasez, certificaciones o testimonios inventados.

## Después del lanzamiento: aprendizaje acotado

- SaleADS revisa las campañas en una cadencia fija (alrededor de los días 7, 10 y 15).
- Hacia el día 15, si hay señal atribuible, SaleADS puede crear una **nueva versión** de la estrategia que conserva lo que funcionó y cambia **una** cosa declarada. Esa versión también la aprueba el usuario.
- Un buen resultado de un solo creativo no "valida" un ángulo. Las métricas son observaciones, no pruebas causales (ver [resultados.md](resultados.md)).
- SaleADS no mueve presupuesto, pujas ni audiencias por su cuenta como parte de este aprendizaje, y no aprende de otros negocios.
