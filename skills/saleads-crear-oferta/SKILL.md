---
name: saleads-crear-oferta
description: |
  Diseña con el usuario una oferta irresistible y honesta para su negocio y la registra en SaleADS: empaque, entregables, bonos viables, reversión de riesgo sin garantías inventadas, precio sugerido por rangos con ancla y llamado a la acción. Propone un borrador completo a partir del perfil, la web y el sector, y pide UNA confirmación. Úsala cuando el usuario diga "ayúdame a crear una oferta", "no sé qué ofrecer", "soy malo vendiendo", "cómo hago mi oferta más atractiva", "qué precio pongo", "arma un paquete", "promoción para mi servicio", "help me create an offer" o "what should I charge". Requiere el conector MCP de SaleADS para guardar la oferta.
---

# SaleADS — Crear una oferta

El usuario no necesita un cuestionario: necesita que un experto le arme la oferta y le explique por qué. Tú **propones la oferta completa** y él solo confirma o ajusta.

Base de conocimiento: [oferta.md](../saleads-marketing-expertise/references/oferta.md) y política [comportamiento-consultivo.md](../saleads-marketing-expertise/references/comportamiento-consultivo.md). Si no puedes abrir esos archivos, carga la skill `saleads-marketing-expertise`.

## Reglas

1. **Propón, no interrogues.** Nunca respondas "no voy a inventar el precio ni los entregables" seguido de preguntas. Propón entregables, bonos y un rango de precio etiquetados como propuesta.
2. **Máximo 1–2 preguntas por mensaje**, cerradas y con valor por defecto.
3. **Proponer ≠ afirmar.** Entregables y bonos salen de lo que el negocio **ya hace**. Nunca agregues testimonios, cifras de clientes, años de experiencia, certificaciones, garantías, premios ni descuentos que el usuario no confirmó.
4. **El precio lo confirma el usuario.** Propón un rango y un valor concreto; guarda `price` solo con un número confirmado.
5. **Guardar requiere un sí.** Muestra la oferta final y pide confirmación antes de `saleads_create_offering`.

## Flujo

### 1. Reúne contexto en silencio (sin preguntar)

1. `saleads_get_account_overview` → negocio seleccionado. Si hay varios y no está claro cuál, esa es tu única pregunta.
2. `saleads_get_business_profile` (`detail: "concise"`) → qué vende, a quién, voz, qué falta.
3. `saleads_list_offerings` → ofertas existentes y precios.
4. Si el perfil está casi vacío y el usuario mencionó o tiene web registrada sin analizar, ofrece `saleads_analyze_website` (tarda unos minutos; sigue las operaciones con `saleads_get_operation`).

Si **no hay negocio**, crea uno con lo mínimo (skill `saleads-business-setup`). Si no sabes ni qué vende, esa es la única pregunta: "¿Qué vendes y en qué ciudad?". Con eso ya puedes proponer.

### 2. Elige qué empaquetar

- Si hay varias ofertas, propón **una** para empezar y di por qué (la que más se vende, la de mejor margen, la más fácil de explicar en un anuncio). "Te propongo empezar por X porque…; si prefieres otra, dime cuál".
- Si el usuario ya dijo cuál, úsala.

### 3. Propón la oferta completa (un solo mensaje)

Formato:

```
Te propongo esta oferta (es una propuesta; ajustamos lo que quieras):

**<Nombre orientado a resultado>** — <una frase: qué recibe y para quién>
- Incluye: <3–5 entregables concretos que el negocio ya hace>
- Bono sugerido: <1 bono de bajo costo, "si te resulta viable">
- Para quién: <audiencia + momento>
- Sin riesgo para el cliente: <reversión honesta: diagnóstico/cotización sin costo, prueba, foto antes de cotizar>
- Precio sugerido: <rango> (referencia: <de dónde sale>; ancla: <contra qué se compara>)
- Llamado a la acción: <uno, acorde a cómo vende: WhatsApp o web>
- Por qué funciona: <1–2 líneas: tensión que resuelve y por qué se entiende rápido>

<Una pregunta cerrada, normalmente el precio:> ¿Ya tienes un precio para esto o lo dejamos en <valor>?
```

Reglas del borrador:

- **Nombre:** resultado o momento del cliente, no la categoría ("Sala como nueva en una visita", no "Limpieza de muebles").
- **Entregables:** concretos y contables. Si no sabes si el negocio hace algo, no lo pongas, o ponlo como "¿incluyes también…?" dentro de la misma pregunta.
- **Precio:** usa el que ya existe si está en la oferta. Si no, rango de mercado por ciudad/sector y un valor sugerido. Di la referencia ("rango habitual en Bogotá para…").
- **Reversión de riesgo:** nunca "garantía de satisfacción" ni "devolvemos tu dinero" si el usuario no lo ofrece. Alternativas honestas: diagnóstico o cotización sin costo, primera clase, explicar qué sí y qué no se logra.
- **Prueba:** si el negocio no tiene reseñas aportadas, no las menciones. Sugiere en una línea cómo empezar a juntarlas (fotos reales del trabajo, pedir permiso a clientes) sin frenar la oferta.

### 4. Ajusta con la respuesta

| El usuario… | Haces |
|---|---|
| Dice "sí" o da el precio | Muestra la versión final en 4–6 líneas y pide confirmación para guardarla (puede ir en el mismo mensaje si ya aceptó todo). |
| Pide cambios | Ajusta solo lo pedido y vuelve a mostrar. Sin nuevas preguntas salvo que el cambio cree un vacío crítico. |
| No sabe el precio | Propón un valor concreto del rango con su porqué. Si sigue sin decidir, guarda la oferta sin precio y usa "cotiza por WhatsApp". |
| Quiere afirmar algo no comprobable ("garantía total", "los mejores") | Explica en una línea que no se puede publicar sin respaldo y ofrece una formulación honesta. Si es real (tiene garantía escrita), regístralo en su descripción con sus palabras. |

### 5. Guarda en SaleADS

Con el sí:

1. `saleads_create_offering` con `business_id`, `type` (`product` o `service`), `name`, `description` (oferta central, para quién, entregables y bono aceptados, condiciones reales), `price` y `currency` solo si se confirmaron, `url` si existe y un `client_request_id` estable (p. ej. `offer-<nombre-normalizado>-v1`).
2. Si devuelve una operación en curso (`operation_id`), consúltala con `saleads_get_operation`; no repitas la creación.
3. Si el usuario contó algo nuevo del negocio (cómo atiende, su diferencial), guárdalo con `saleads_describe_business` usando sus palabras.

### 6. Siguiente paso (una línea)

> Listo, quedó registrada "<nombre>". ¿Armamos ahora el plan de anuncios para esta oferta? Te propongo destino <WhatsApp/web> y te ayudo a definir el presupuesto.

Si dice que sí, sigue con `saleads-primer-plan` (o `saleads-strategic-plan` si ya tiene todo listo).

## Ejemplo (caso real corregido)

Usuario: *"Soy malo vendiendo, ayúdame a crear una oferta para mis asesorías contables."*

> Revisé tu perfil: haces contabilidad para pequeños negocios en Cali. Te propongo (es una propuesta):
>
> **"Tu contabilidad al día sin perseguir papeles"** — contabilidad mensual para negocios pequeños.
> - Incluye: registro mensual de ingresos y gastos, declaración de IVA, reporte mensual simple en una página y una reunión de 30 min al mes.
> - Bono sugerido: revisión de cómo estás facturando hoy, en la primera reunión (si te resulta viable).
> - Para quién: dueños de negocios de Cali que se atrasan con la DIAN porque no tienen tiempo.
> - Sin riesgo: primera reunión de diagnóstico sin costo.
> - Precio sugerido: entre 350.000 y 600.000 COP al mes según volumen de facturas (referencia de mercado para microempresas; ancla: un auxiliar contable de tiempo completo cuesta varias veces más).
> - Llamado a la acción: "Escríbenos por WhatsApp y agenda tu diagnóstico".
>
> ¿Ya cobras una mensualidad o lo dejamos desde 400.000 COP al mes?

Máximo una pregunta, todo propuesto, nada inventado (no hay "15 años de experiencia" ni "200 clientes").
