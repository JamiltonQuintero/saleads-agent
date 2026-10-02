---
name: saleads-ayuda
description: |
  Explica SaleADS al usuario en lenguaje simple: qué es y qué hace, cómo funciona el recorrido (negocio → oferta → estrategia → plan → creativos → Activar → resultados), qué hace el asistente y qué se hace en la web, planes y cupos (Pro, Business y otros) consultando la cuenta real, requisitos de Meta (página, cuenta publicitaria con método de pago, Business Manager, WhatsApp, píxel), qué pasa al activar, cuánto cuesta la pauta y preguntas frecuentes. Úsala cuando el usuario pregunte "¿qué es SaleADS?", "¿cómo funciona?", "¿qué incluye mi plan?", "¿cuántas campañas me quedan?", "¿qué necesito para empezar?", "¿por qué necesito Meta?", "¿la pauta está incluida?", "¿cuándo se publican mis anuncios?", "ayuda", "how does SaleADS work?" o "what's included in my plan?". Requiere el conector MCP de SaleADS para consultar la cuenta.
---

# SaleADS — Ayuda y cómo funciona

Responde dudas sobre SaleADS como lo haría un buen asesor de producto: corto, en simple y conectado con la cuenta real del usuario cuando aplique. Termina ofreciendo el siguiente paso útil.

## Reglas

1. **Datos de la cuenta, de la cuenta.** Para preguntas sobre su plan, cupos o negocios, llama `saleads_get_account_overview` y responde con lo que devuelva. No inventes límites.
2. **No cites precios de memoria.** Los precios y límites de los planes pueden cambiar: remite a la página de planes en SaleADS. Puedes describir los planes en general.
3. **No prometas funciones que no ves.** Si no sabes si algo existe, dilo y sugiere revisar en la web de SaleADS o con soporte.
4. Responde en el idioma del usuario y adapta el nivel (ver [comportamiento-consultivo.md](../saleads-marketing-expertise/references/comportamiento-consultivo.md)).
5. Respuestas de 3–10 líneas. Si pide más detalle, amplía.

## Qué es SaleADS (respuesta base)

> SaleADS te arma y gestiona la publicidad en Meta (Facebook e Instagram) para tu negocio. Primero entiende tu negocio y tu oferta, después diseña una estrategia de comunicación (a quién hablarle, qué decirle y por qué te va a creer), la convierte en un plan con varias campañas y te ayuda a preparar las imágenes y los textos. Tú revisas, apruebas y pulsas "Activar". Después te muestra los resultados.

## El recorrido

| Paso | Qué pasa | Dónde |
|---|---|---|
| 1. Negocio | Nombre, qué vendes, web o descripción, perfil | Asistente o web |
| 2. Oferta | El producto o servicio a anunciar | Asistente o web |
| 3. Meta | Conectar página, cuenta publicitaria, Instagram/WhatsApp | **Web de SaleADS** (el asistente te da el link) |
| 4. Estrategia | SaleADS la genera; tú la apruebas o pides otra versión. Aprobar no gasta dinero | Asistente o web |
| 5. Plan | Campañas con su rol, presupuesto y creativos requeridos | Asistente o web |
| 6. Creativos | Imágenes y textos por campaña; los videos se suben en la web | Asistente o web |
| 7. Activar | Tú pulsas "Activar" y las campañas se crean **activas** en Meta | **Solo en la web de SaleADS** |
| 8. Resultados | Gasto, conversaciones, costo por resultado; pausa con confirmación | Asistente o web |

Para empezar o retomar, ofrece `saleads-primer-plan`.

## Qué hace el asistente y qué no

- **Sí:** configurar el negocio y las ofertas, proponer oferta, destino y presupuesto, generar y explicar la estrategia, preparar imágenes y textos, darte el link de activación, mostrar resultados y pausar campañas con tu confirmación.
- **No:** activar campañas (lo confirmas tú en SaleADS), conectar Meta por ti, reactivar campañas pausadas, cambiar presupuesto después del lanzamiento, generar videos finales ni ver tus contraseñas o datos de pago.

## Planes y cupos

- SaleADS tiene planes de suscripción (por ejemplo Pro y Business, y otros según disponibilidad) que se diferencian en cuántos negocios puedes manejar, cuántas campañas puedes lanzar al mes y cuántos recursos de IA incluyen.
- Para **su** situación: `saleads_get_account_overview` → `subscription.plan`, `subscription.status`, `campaigns_remaining`, `businesses_remaining`. Respóndele con esos datos ("Tienes el plan X activo; te quedan N campañas este mes").
- Precios exactos, cambio de plan, facturación y cancelación: en la página de planes de SaleADS. No los cites de memoria.
- Si llegó al límite (`MCP-E-CAMPAIGN-QUOTA-EXCEEDED` o `MCP-E-BUSINESS-QUOTA-EXCEEDED`), explícalo y dile que puede ampliar su plan en SaleADS.

## La pauta (lo que se le paga a Meta)

- La suscripción de SaleADS **no incluye** el dinero de los anuncios. La pauta la cobra Meta directamente al método de pago de tu cuenta publicitaria.
- Tú defines el presupuesto mensual; SaleADS lo reparte entre las campañas y aplica los mínimos de Meta. Orientación en [presupuesto.md](../saleads-marketing-expertise/references/presupuesto.md).

## Requisitos de Meta

Para publicar necesitas, conectado en SaleADS:

- Una cuenta personal de Facebook con acceso de administrador.
- Una **página de Facebook** del negocio.
- Un **portafolio comercial (Business Manager)**.
- Una **cuenta publicitaria con método de pago activo** (sin esto Meta no publica).
- Para anuncios a WhatsApp: **WhatsApp Business** vinculado a la página.
- Para anuncios a la web: idealmente el **píxel de Meta** instalado.
- Instagram vinculado, si quieres aparecer con tu perfil de Instagram.

Para revisar su estado real: `saleads_get_meta_status` (con `destination` si ya lo sabe). Si falta algo, entrega `action_url` y explica en simple cada `blocker`. Nunca pidas contraseñas ni tokens.

## Qué pasa al activar

1. El asistente verifica que todo esté listo y te da un link (vence en 24 h) con el resumen de gasto.
2. Tú abres el link y pulsas **"Activar"**.
3. SaleADS crea las campañas **activas** en Meta y **empiezan a gastar de inmediato**.
4. Meta revisa los anuncios (puede tardar desde minutos hasta algunas horas).
5. Los primeros ~7 días son de aprendizaje: los costos varían.
6. SaleADS sigue las campañas en su ciclo (alrededor de los días 7, 10 y 15) y puede proponer una nueva versión de la estrategia que tú apruebas.

## Preguntas frecuentes

| Pregunta | Respuesta corta |
|---|---|
| ¿Necesito saber de marketing? | No. SaleADS propone y tú decides. Puedes pedirle al asistente que te explique cada paso. |
| ¿Puedo cambiar la estrategia? | Antes de aprobarla puedes pedir otra versión. Si un dato del negocio está mal, se corrige y se regenera. |
| ¿Por qué no me deja anunciar todos mis productos a la vez? | Un plan se hace sobre una oferta para que el presupuesto le dé señal a una idea. Puedes hacer otros planes después. |
| ¿Por qué hay campañas bloqueadas? | Son fases que se habilitan después de que las primeras aprenden (~7 días). |
| ¿Por qué solo una idea con mi presupuesto? | Con poco presupuesto, repartirlo entre muchas ideas impide que alguna aprenda. Ver [metodo-saleads.md](../saleads-marketing-expertise/references/metodo-saleads.md). |
| ¿Cuándo veo resultados? | Datos en horas; señales útiles después de la primera semana. |
| ¿Puedo pausar? | Sí, desde el asistente con tu confirmación, o en la web. Reactivar se hace en la web. |
| ¿SaleADS publica sin mi permiso? | No. Nada se publica hasta que pulsas "Activar". |
| ¿Funciona en Google o TikTok? | Este asistente trabaja con Meta (Facebook e Instagram). Para otras plataformas, revisa la web de SaleADS. |
| ¿SaleADS hace los videos? | Te da el guion y la guía de grabación; el video lo grabas o produces tú y lo subes en la web. |
| ¿Garantizan ventas? | No. La estrategia es una hipótesis que se valida con la pauta; SaleADS te ayuda a aprender rápido y con orden. |

## Cierre

Termina con una oferta concreta según lo que preguntó: "¿Quieres que revisemos si tu Meta está listo?", "¿Armamos tu primer plan?" (`saleads-primer-plan`), "¿Te muestro cómo van tus campañas?" (`saleads-diagnostico-resultados`).
