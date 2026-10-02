---
name: saleads-servicios-profesionales
description: |
  Consultoría y flujo SaleADS para conseguir clientes de servicios profesionales y locales con Meta Ads: consultoría, abogados, contadores, salud y estética, educación, coaching, agencias, oficios y servicios a domicilio. Propone la oferta de entrada de bajo riesgo (diagnóstico, primera sesión, cotización), el segmento con el dolor más urgente, el destino (WhatsApp o agenda web), cómo generar confianza sin testimonios inventados y cuidados en sectores regulados. Úsala cuando el usuario diga "quiero más clientes", "conseguir pacientes", "vendo servicios", "soy consultor", "asesorías", "agendar citas", "generar leads", "mi consultorio", "lead generation for my services" o "get clients for my agency". Requiere el conector MCP de SaleADS.
---

# SaleADS — Servicios profesionales y locales

Los servicios se compran por **confianza**. La persona no puede "ver el producto" antes de pagar, así que la oferta debe bajar el riesgo de dar el primer paso y la comunicación debe mostrar cómo trabajas, sin inventar pruebas.

Base: [comportamiento-consultivo.md](../saleads-marketing-expertise/references/comportamiento-consultivo.md), [oferta.md](../saleads-marketing-expertise/references/oferta.md), [contexto-latam.md](../saleads-marketing-expertise/references/contexto-latam.md). Si no puedes abrirlas, carga `saleads-marketing-expertise`.

## Reglas

1. Infiere desde perfil, web y ofertas. Máximo 1–2 preguntas por mensaje.
2. **Sectores regulados** (salud, estética, legal, finanzas, educación con títulos, inmuebles): sin promesas de resultado, sin títulos, licencias o registros que el usuario no confirme. Prefiere hablar del proceso y del acompañamiento.
3. Sin testimonios, casos, "X años de experiencia", número de clientes ni garantías inventadas.
4. Monto de pauta, aprobaciones y activación: del usuario. Tú no lanzas.

## Flujo

### 1. Contexto

`saleads_get_account_overview` → `saleads_get_business_profile` → `saleads_list_offerings` → `saleads_get_meta_status`. Resume en 2 líneas lo que ya sabes.

### 2. Propón la oferta de entrada

Un servicio caro o de confianza se vende mejor en dos pasos: una **entrada de bajo riesgo** que inicia la relación, y el servicio completo después.

| Tipo de servicio | Entrada que puedes proponer |
|---|---|
| Consultoría, contabilidad, legal | Diagnóstico de 20–30 min sin costo o a precio simbólico |
| Salud, estética, bienestar | Valoración inicial (precio confirmado por el usuario) |
| Educación, coaching, clases | Clase muestra o primera sesión |
| Oficios y servicios a domicilio | Cotización rápida por foto o visita de diagnóstico |
| Agencias y freelancers | Auditoría corta de lo que el cliente tiene hoy |

Presenta la oferta completa en un mensaje (formato de `saleads-crear-oferta`): nombre orientado a resultado, qué incluye la entrada, para quién, cómo sigue después, precio sugerido o "sin costo" **solo si el usuario lo acepta**, y llamado a la acción. Una pregunta: normalmente si la entrada es sin costo o con precio.

Guarda con `saleads_create_offering` (`type: "service"`) tras el sí.

### 3. Segmento prioritario

Elige el segmento con **el dolor más urgente y capacidad de pago** ([audiencia-y-destino.md](../saleads-marketing-expertise/references/audiencia-y-destino.md)). Ejemplos de momentos:

- Contador: "dueño de negocio que recibió un requerimiento de la DIAN/SAT".
- Odontología: "adulto que evita sonreír en fotos" (sin prometer resultados clínicos).
- Agencia: "negocio que ya invirtió en anuncios y no vio clientes".

Ubicación: la zona real donde atiende (presencial) o el país (servicio remoto).

### 4. Destino

- **WhatsApp (`messages`)** es el punto de partida habitual: la gente quiere preguntar antes de agendar. Llamado a la acción: "Escríbenos y agenda tu diagnóstico".
- **Web (`web`)** cuando hay página de agenda o formulario funcionando con píxel, y el usuario prefiere no atender chats.

### 5. Confianza sin inventar

En la estrategia y los creativos, la prueba se construye con lo real:

- Mostrar el proceso: "así es tu primera sesión", "qué revisamos en el diagnóstico".
- La persona detrás del servicio: foto o video real hablando a cámara.
- El lugar real, el equipo real (si el usuario lo aporta).
- Explicar el método y qué sí y qué no se puede lograr.
- Credenciales solo si el usuario las confirma (tarjeta profesional, registro).

Si no hay reseñas, propone un plan para conseguirlas (pedir permiso a clientes actuales) sin frenar la campaña.

### 6. Estrategia, plan, creativos y activación

Sigue `saleads-strategic-plan`: resumen único con oferta + destino + presupuesto propuesto + idioma → `saleads_start_strategy` → explicar → aprobación explícita → plan → creativos (`saleads-campaign-creatives`) → `saleads_request_plan_launch` → el usuario pulsa "Activar".

Para video, propón un guion de "persona hablando a cámara" de 30–90 s: gancho con el momento del cliente → por qué pasa → cómo trabajas → qué incluye la entrada → llamado a la acción. Grabado con el celular, con subtítulos.

### 7. Después

Los servicios tienen ciclos de decisión más largos: no todos los mensajes agendan el mismo día. Sugiere medir conversaciones → citas agendadas → clientes. Lectura de resultados en `saleads-diagnostico-resultados`.
