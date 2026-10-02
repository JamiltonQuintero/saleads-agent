# Instalar SaleADS en tu asistente de IA

SaleADS se conecta a tu asistente como un **servidor MCP remoto**. Con él puedes preparar tu negocio, generar y aprobar la estrategia de comunicación, configurar las campañas del plan y pedir el lanzamiento desde el chat. **El lanzamiento siempre lo confirmas tú en SaleADS** con el botón "Activar": el asistente nunca lanza campañas ni gasta dinero por su cuenta.

| Qué | Valor |
|---|---|
| URL del MCP | `https://mcp-dev.saleads.ai/mcp` |
| Repositorio público (plugin + skills) | https://github.com/JamiltonQuintero/saleads-agent |
| Transporte | Streamable HTTP |
| Inicio de sesión | OAuth con tu cuenta de SaleADS |

Índice: [Antes de empezar](#antes-de-empezar) · [Cómo funciona el inicio de sesión](#cómo-funciona-el-inicio-de-sesión) · [claude.ai](#claudeai-conector-personalizado) · [Claude Desktop](#claude-desktop) · [Claude Code](#claude-code) · [ChatGPT](#chatgpt-modo-desarrollador) · [Codex](#codex) · [Cursor](#cursor) · [VS Code](#vs-code-github-copilot) · [Gemini CLI](#gemini-cli) · [Problemas frecuentes](#problemas-frecuentes)

## Antes de empezar

- **Cuenta de SaleADS en https://dev.saleads.ai** con plan **Pro, Business o Agency** activo. Este canal usa el entorno dev de SaleADS: inicia sesión con tu cuenta de dev, no con la de producción.
- **Cuenta de SaleADS con suscripción activa.** Sin suscripción, las tools responden `MCP-E-SUBSCRIPTION-REQUIRED`.
- **Meta.** La conexión con Facebook/Instagram/WhatsApp no se hace desde el chat: el asistente te da un link a SaleADS cuando falte algo.
- **Las skills.** Además del conector, SaleADS trae 13 skills (`skills/`) que enseñan al asistente el orden correcto del flujo y a comportarse como consultor de marketing (propone, pregunta poco y no inventa pruebas). Los plugins de Claude Code, Codex y Gemini CLI las instalan solas. En los demás clientes, el conector funciona sin ellas.

## Cómo funciona el inicio de sesión

1. Agregas la URL del MCP en tu cliente.
2. La primera vez, el cliente abre el navegador en la página de inicio de sesión de SaleADS (Keycloak). Entras con tu cuenta de SaleADS: correo y contraseña, Google o Microsoft, igual que en la web.
3. Aceptas los permisos que pide el asistente:

   | Permiso | Para qué |
   |---|---|
   | `saleads:read` | Leer tu cuenta, negocios, estrategias, planes, estado y métricas |
   | `saleads:strategy` | Crear negocios y ofertas, generar y aprobar estrategias, editar el plan |
   | `saleads:campaigns` | Subir imágenes, preparar borradores y editar textos |
   | `saleads:launch` | Pedir el link de activación y pausar campañas |

   Algunos clientes piden primero solo `saleads:read` y vuelven a pedir permiso cuando una tool necesita otro (respuesta `403 insufficient_scope`). Acéptalo cuando aparezca.
4. El cliente guarda el token y lo renueva. **Nunca** escribas tu contraseña en el chat ni se la des al asistente.

Detalles técnicos: el MCP publica su metadata OAuth en `https://mcp-dev.saleads.ai/.well-known/oauth-protected-resource`. Cada cliente se registra en Keycloak por registro dinámico (DCR) o con un `client_id` público pre-registrado (PKCE S256, sin secreto):

| Cliente | `client_id` pre-registrado |
|---|---|
| Claude (claude.ai, Desktop, Claude Code) | `mcp-claude` |
| ChatGPT | `mcp-chatgpt` |
| Codex | `mcp-codex` |
| VS Code | `mcp-vscode` |
| Cursor | `mcp-cursor` |

Usa el `client_id` solo si el registro dinámico falla o si este documento lo indica.

## claude.ai (conector personalizado)

Disponible en los planes Free (un conector), Pro, Max, Team y Enterprise.

**Pro y Max:**

1. Abre **Customize → Connectors** (`https://claude.ai/customize/connectors`).
2. Pulsa **+** → **Add custom connector**.
3. Nombre: `SaleADS`. URL: `https://mcp-dev.saleads.ai/mcp`.
4. Si el registro dinámico no está habilitado, abre **Advanced settings** y escribe `mcp-claude` en **OAuth Client ID**. Deja el secreto vacío.
5. Pulsa **Add** y luego **Connect**. Inicia sesión con tu cuenta de SaleADS.
6. En cada conversación, actívalo desde el botón **+** → **Connectors**.

**Team y Enterprise:** un Owner lo agrega primero en **Organization settings → Connectors** (`https://claude.ai/admin-settings/connectors`) → **Add** → **Custom** → **Web**, con la misma URL. Después cada miembro lo busca en **Customize → Connectors** y pulsa **Connect**.

Las skills no se instalan con el conector. Para usarlas, instala el plugin en Claude Code, Codex o Gemini CLI (secciones siguientes).

## Claude Desktop

Claude Desktop usa los mismos conectores que claude.ai: agrega el conector en claude.ai (sección anterior) o desde **Customize → Connectors** en la app, y aparecerá en ambos. El inicio de sesión se abre en el navegador.

## Claude Code

**Opción A — Plugin (recomendada: conector + 13 skills).** No necesitas acceso a ningún repo privado: el plugin se publica en GitHub.

```bash
claude plugin marketplace add JamiltonQuintero/saleads-agent
claude plugin install saleads@saleads
```

O, dentro de una sesión: `/plugin marketplace add JamiltonQuintero/saleads-agent` y `/plugin install saleads@saleads`.

Luego, en una sesión, ejecuta `/mcp`, elige `plugin:saleads:saleads` y **Authenticate**. Las skills quedan como `/saleads:saleads-strategic-plan`, `/saleads:saleads-business-setup`, etc., y Claude las usa solo cuando corresponde.

Para actualizar: `claude plugin marketplace update saleads` y `claude plugin update saleads@saleads` (o activa **Enable auto-update** en `/plugin` → **Marketplaces** → `saleads`). Reinicia la sesión después.

**Opción B — Solo el conector.**

```bash
claude mcp add --transport http --scope user --client-id mcp-claude saleads https://mcp-dev.saleads.ai/mcp
```

Luego `/mcp` → `saleads` → **Authenticate**.

## ChatGPT (modo desarrollador)

Según la documentación de OpenAI (2026-09-30), el MCP completo (lectura y escritura) está disponible en Business, Enterprise y Edu; en Pro el modo desarrollador puede limitarse a lectura. En workspaces, un admin debe habilitar antes el modo desarrollador y los conectores MCP personalizados.

1. **Settings → Security and login** → activa **Developer mode**.
2. Ve a **Plugins** y pulsa **+** para crear una app en modo desarrollador.
3. Nombre: `SaleADS`. Descripción: "Planes estratégicos de Meta Ads con SaleADS". URL del servidor MCP: `https://mcp-dev.saleads.ai/mcp`. Conexión: endpoint público.
4. Autenticación: **OAuth**. Si ChatGPT te deja elegir el registro del cliente, usa el dinámico; si falla, usa el client ID `mcp-chatgpt` sin secreto.
5. Guarda, conecta e inicia sesión con tu cuenta de SaleADS.
6. En el chat, activa SaleADS en el selector de herramientas o menciónalo con `@SaleADS`.

Las skills llegarán con el plugin de ChatGPT cuando se publique (post-canary).

## Codex

**Opción A — Plugin (conector + 13 skills).**

```bash
codex plugin marketplace add JamiltonQuintero/saleads-agent
codex plugin add saleads@saleads
codex mcp login saleads
```

`codex mcp login` abre el navegador para iniciar sesión. El plugin usa registro dinámico; si falla, usa la opción B. Para actualizar: `codex plugin marketplace upgrade`.

**Opción B — `config.toml`.** Agrega a `~/.codex/config.toml` el bloque de [`examples/codex/config.toml`](examples/codex/config.toml):

```toml
[mcp_servers.saleads]
url = "https://mcp-dev.saleads.ai/mcp"
oauth_resource = "https://mcp-dev.saleads.ai/mcp"

[mcp_servers.saleads.oauth]
client_id = "mcp-codex"
```

O con un comando:

```bash
codex mcp add saleads --url https://mcp-dev.saleads.ai/mcp --oauth-client-id mcp-codex --oauth-resource https://mcp-dev.saleads.ai/mcp
codex mcp login saleads
```

No instales el plugin y el bloque manual a la vez: tendrías dos servidores `saleads`.

## Cursor

**Instalación en un clic** (abre Cursor y confirma):

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=saleads&config=eyJ1cmwiOiJodHRwczovL21jcC1kZXYuc2FsZWFkcy5haS9tY3AiLCJhdXRoIjp7IkNMSUVOVF9JRCI6Im1jcC1jdXJzb3IifX0=
```

El parámetro `config` es el JSON `{"url":"https://mcp-dev.saleads.ai/mcp","auth":{"CLIENT_ID":"mcp-cursor"}}` en base64.

**Manual:** agrega a `~/.cursor/mcp.json` (global) o `.cursor/mcp.json` (proyecto) el contenido de [`examples/cursor/mcp.json`](examples/cursor/mcp.json):

```json
{
  "mcpServers": {
    "saleads": {
      "url": "https://mcp-dev.saleads.ai/mcp",
      "auth": {
        "CLIENT_ID": "mcp-cursor"
      }
    }
  }
}
```

En **Settings → MCP**, pulsa **Connect/Login** en `saleads` e inicia sesión.

## VS Code (GitHub Copilot)

Copia [`examples/vscode/.vscode/mcp.json`](examples/vscode/.vscode/mcp.json) a la carpeta `.vscode/` de tu proyecto (o a la configuración MCP de tu perfil):

```json
{
  "servers": {
    "saleads": {
      "type": "http",
      "url": "https://mcp-dev.saleads.ai/mcp",
      "oauth": {
        "clientId": "mcp-vscode"
      }
    }
  }
}
```

Pulsa **Start** sobre el servidor en `mcp.json`; VS Code abre el navegador para iniciar sesión la primera vez. Usa las tools desde el chat en modo agente.

## Gemini CLI

La extensión incluye el conector, `GEMINI.md` y las 13 skills.

```bash
gemini extensions install https://github.com/JamiltonQuintero/saleads-agent
```

Para actualizar: `gemini extensions update saleads`.

Reinicia Gemini CLI y ejecuta `/mcp auth saleads` para iniciar sesión.

Sin la extensión, solo el conector: `gemini mcp add --transport http saleads https://mcp-dev.saleads.ai/mcp` y luego `/mcp auth saleads`.

## Problemas frecuentes

| Síntoma | Causa probable | Qué hacer |
|---|---|---|
| Cada tool responde `MCP-E-ACCESS-NOT-ENABLED` | Tu cuenta no está en la lista del canary | Pide acceso a SaleADS. No reinstales. |
| `MCP-E-SUBSCRIPTION-REQUIRED` | Sin suscripción activa | Activa tu plan en SaleADS. |
| El login vuelve a pedirse a cada rato o aparece `401` | La sesión expiró o el token se revocó | Vuelve a autenticar (`/mcp` en Claude Code, `codex mcp login saleads`, **Connect** en claude.ai). |
| `403 insufficient_scope` | Falta un permiso (p. ej. `saleads:launch`) | Acepta el nuevo permiso cuando el cliente lo pida; si no lo pide, desconecta y vuelve a conectar. |
| "invalid redirect_uri" o "client not found" en la página de login | El registro dinámico no está habilitado o tu cliente usa otra URL de retorno | Usa el `client_id` pre-registrado de tu cliente (tabla de inicio de sesión). Si persiste, repórtalo con el nombre del cliente y su versión. |
| El cliente no encuentra el servidor (`ENOTFOUND`, timeout) | URL mal escrita o el entorno aún no está publicado | Revisa la URL (termina en `/mcp`) y prueba desde el navegador `https://mcp-dev.saleads.ai/.well-known/oauth-protected-resource`. |
| Una operación larga parece colgada | Generar la estrategia o analizar la web tarda hasta ~3 min | Espera; el asistente consulta `saleads_get_operation`. No repitas la petición. |
| El asistente dice que "lanzó" las campañas | No puede: el lanzamiento se confirma en SaleADS | Pídele el link (`saleads_request_plan_launch`) y pulsa "Activar" en SaleADS. |
| Claude Code no muestra las skills | El plugin no se recargó | Ejecuta `/reload-plugins` o reinicia la sesión; revisa `claude plugin details saleads`. |
| Dos servidores `saleads` en Codex | Plugin y bloque manual a la vez | Quita uno: `codex plugin remove saleads@saleads` o borra el bloque de `config.toml`. |

Si nada funciona, comparte con soporte el cliente, su versión, la hora y el código de error. **No compartas tokens, contraseñas ni capturas con URLs firmadas.**
