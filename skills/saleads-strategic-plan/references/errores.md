# Errores del MCP de SaleADS y cómo responder

Los errores de negocio llegan dentro del resultado de la tool con `isError: true` y `structuredContent.error`:

```json
{
  "code": "MCP-E-…",
  "message": "Qué pasó",
  "next_action": "Qué hacer después",
  "retryable": false,
  "details": { },
  "action_url": null
}
```

Regla general:

1. Lee `message` y `next_action`: están escritos para ti.
2. Si `retryable` es `false`, **no reintentes lo mismo**. Cambia algo (dato, confirmación, paso previo) o informa al usuario.
3. Si trae `action_url`, entrégala al usuario con una explicación simple.
4. Nunca muestres al usuario trazas, IDs internos ni JSON crudo. Traduce el error a una frase y la siguiente acción.
5. Si el error viene de una operación (`saleads_get_operation` con `status: "failed"`), aplica la misma tabla usando `error.code`; el `error` de la operación trae los mismos `next_action`, `retryable`, `details` y `action_url`. Si `status` es `cancelled`, aplica `MCP-E-OPERATION-CANCELLED`.

## Tabla

| Código | Cuándo pasa | Qué haces | ¿Reintentar? |
|---|---|---|---|
| `MCP-E-ACCESS-NOT-ENABLED` | El usuario aún no tiene acceso al asistente. | Explícalo y no sigas. | no |
| `MCP-E-SUBSCRIPTION-REQUIRED` | Sin suscripción activa. | Pide activar su plan en SaleADS; `action_url` es la página informativa de planes (no un pago). No sigas. | no |
| `MCP-E-BUSINESS-NOT-FOUND` | `business_id` inexistente o de otro usuario. | `saleads_get_account_overview` y usa un negocio de la lista. | no |
| `MCP-E-BUSINESS-QUOTA-EXCEEDED` | Límite de negocios del plan. | Explica el límite; usa un negocio existente. | no |
| `MCP-E-OFFERING-REQUIRED` | Falta la oferta o no es del negocio. | `saleads_list_offerings` o `saleads_create_offering`. | no |
| `MCP-E-META-NOT-READY` | Meta sin conectar o con activos incompletos. | Llama `saleads_get_meta_status` y entrega el `action_url` de cada elemento de `blocker_details` (pantalla exacta del bloqueo). | no |
| `MCP-E-CONTEXT-REVIEW-REQUIRED` | Falta contexto del negocio para la estrategia, o un dato del negocio (`details.missing`, p. ej. `offering_type` o `category`) para completar el perfil. | `saleads_get_business_profile` para ver `missing`; completa con `saleads_describe_business` o `saleads_complete_business_profile`; vuelve a `saleads_start_strategy`. | no |
| `MCP-E-BUDGET-INVALID` | Monto o moneda inválidos al crear la estrategia. | Pide un monto y una moneda válidos. | no |
| `MCP-E-FX-UNAVAILABLE` | No hay tasa de cambio disponible. | Reintenta más tarde o usa USD. | sí |
| `MCP-E-BUDGET-BELOW-MINIMUM` | Presupuesto bajo el mínimo al editar el plan (`saleads_update_plan`). | Muestra `details.minimum` y `details.currency`; pregunta si sube; solo con un "sí" repite con el nuevo valor. | no |
| `MCP-E-STRATEGY-QUALITY-FAILED` | La estrategia no pasó la calidad o tenía claims sin evidencia. | Muestra el resumen de calidad; ofrece `saleads_regenerate_strategy`. No corrijas el texto a mano. | no |
| `MCP-E-STRATEGY-NOT-APPROVED` | Se intentó compilar sin aprobar. | Presenta la estrategia, pide aprobación y usa `saleads_approve_strategy`. | no |
| `MCP-E-PLAN-EXISTS` | Ya hay un plan activo para la oferta. | Continúa con ese `strategy_plan_id` (`saleads_get_plan`). No crees otro. | no |
| `MCP-E-CONFLICT` | Otro cambio llegó antes (versión o plan), o el plan está cancelado (`details.reason = plan_not_active`). | Relee el recurso (`saleads_get_strategy` o `saleads_get_plan`) y repite la operación con los datos vigentes. Si el plan está cancelado, usa `details.replaced_by_plan_id` (o busca el vigente con `saleads_start_strategy`). | sí |
| `MCP-E-IN-PROGRESS` | Ya hay una operación igual en curso. | Espera y consulta `saleads_get_operation`. No relances la tool. | sí |
| `MCP-E-WEBSITE-UNREADABLE` | No se pudo leer el sitio web. | Pide otra URL o describe el negocio con `saleads_describe_business`. No reintentes la misma URL. | no |
| `MCP-E-IMAGE-INVALID` | Formato, peso, tamaño o ratio inválido. | Genera o recorta la imagen a 1:1, 4:5 o 9:16, jpeg/png/webp, ≤8 MB, ≥600 px, y reenvía. | no |
| `MCP-E-IMAGE-URL-REJECTED` | URL no https, privada o no descargable. | Usa `saleads_create_media_upload` + PUT, o el link para subir imágenes en SaleADS. | no |
| `MCP-E-IMAGES-INCOMPLETE` | La campaña no tiene todas sus imágenes. | `saleads_set_campaign_images` con las faltantes. | no |
| `MCP-E-CAMPAIGN-NOT-READY` | El borrador no pasa sus validaciones. | Resuelve cada check de la lista (`failed_checks`) y vuelve a aprobar. | no |
| `MCP-E-CAMPAIGN-QUOTA-EXCEEDED` | Sin cupo de campañas en el plan de suscripción. | Explica el límite; si quiere ampliarlo, entrega el link `upgrade-plan` (blocker de `saleads_request_plan_launch` o `actions` de `saleads_get_help`). | no |
| `MCP-E-PLAN-NOT-READY-TO-LAUNCH` | Hay campañas activas sin `configured` (las `locked`, `skipped` y `launched` no cuentan). | Completa las campañas listadas (flujo de creativos). | no |
| `MCP-E-OPERATION-CANCELLED` | La operación fue cancelada. | Díselo al usuario; vuelve a iniciar la acción solo si él lo pide. | no |
| `MCP-E-OPERATION-NOT-FOUND` | Operación inexistente, expirada o ajena. | Vuelve a iniciar la acción con la tool original. | no |
| `MCP-E-RATE-LIMITED` | Demasiadas llamadas. | Espera `details.retry_after_s` y reintenta. | sí |
| `MCP-E-INVALID-INPUT` | Un servicio rechazó campos que el schema no cubre. Con `details.reason = currency_mismatch`, el presupuesto de `saleads_update_plan` no venía en la moneda del plan. | Corrige los campos de `details.fields` (sin inventar valores; pregunta al usuario) y reintenta. Para `currency_mismatch`, pide el monto en `details.plan_currency`. | no |
| `MCP-E-UPSTREAM-TIMEOUT` | Un servicio tardó demasiado. | Si hay operación, consúltala; si no, reintenta más tarde. | sí |
| `MCP-E-UPSTREAM-UNAVAILABLE` | Servicio caído o error no mapeado. | Reintenta una vez más tarde; si persiste, informa al usuario. | sí |
| `MCP-E-LAUNCH-NOT-ALLOWED` | Se pidió lanzar desde el chat. | Explica que el lanzamiento se confirma en SaleADS; usa `saleads_request_plan_launch` para obtener el link. | no |

## Errores de autenticación

No son parte de este catálogo; los gestiona el cliente MCP.

- `401`: la sesión expiró. El cliente pedirá volver a iniciar sesión en SaleADS. Tú no escribes contraseñas.
- `403 insufficient_scope`: falta un permiso (p. ej. `saleads:launch`). Pide al usuario volver a autorizar el conector aceptando el permiso que se solicita.
