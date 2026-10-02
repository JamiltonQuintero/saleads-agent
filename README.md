# SaleADS para asistentes de IA · SaleADS for AI assistants

Plugin oficial de SaleADS: conector MCP remoto + 13 skills para crear y gestionar planes estratégicos de Meta Ads desde Claude Code, Codex, Cursor, VS Code, Gemini CLI y claude.ai.

Official SaleADS plugin: a remote MCP connector + 13 skills to create and manage Meta Ads strategic plans from Claude Code, Codex, Cursor, VS Code, Gemini CLI and claude.ai.

> **Canal / Channel: dev.** Esta rama (`main`) apunta al entorno **dev** de SaleADS, que hoy funciona como producción: `https://mcp-dev.saleads.ai/mcp`. Inicia sesión con tu cuenta de https://dev.saleads.ai.
> This branch (`main`) points to the SaleADS **dev** environment, which currently acts as production: `https://mcp-dev.saleads.ai/mcp`. Sign in with your https://dev.saleads.ai account.

**Español** · [English](#english)

## Qué es SaleADS

SaleADS planifica, prepara y mide campañas de Meta Ads (Facebook, Instagram y WhatsApp) para negocios. Con este plugin, tu asistente de IA puede:

- Preparar tu negocio y tus ofertas, y revisar la conexión con Meta.
- Generar, revisar y aprobar la estrategia de comunicación (hipótesis, mensajes, creativos) sin inventar pruebas ni cifras.
- Armar el plan con todas sus campañas, subir imágenes y editar textos.
- Pedir el lanzamiento y leer resultados (gasto, clics, resultados, CPA) sin declarar ganadores causales.

**SaleADS nunca lanza desde el chat.** El asistente te entrega un link y tú pulsas **Activar** en SaleADS. Al activar, las campañas se crean activas en Meta y gastan presupuesto real.

## Requisitos

- Cuenta en **https://dev.saleads.ai** con plan **Pro, Business o Agency** activo. Este canal usa el entorno dev: tu cuenta de producción no sirve aquí.
- Para el plugin con skills: Claude Code, Codex CLI o Gemini CLI, y `git`. No necesitas acceso a ningún repo privado.
- La conexión con Meta (Facebook/Instagram/WhatsApp) se hace en la web de SaleADS; el asistente te da el link cuando falte.

## Instalación

### Claude Code (un comando, recomendado)

En la terminal:

```bash
claude plugin marketplace add JamiltonQuintero/saleads-agent
claude plugin install saleads@saleads
```

O dentro de una sesión de Claude Code:

```
/plugin marketplace add JamiltonQuintero/saleads-agent
/plugin install saleads@saleads
```

Luego abre `/mcp`, elige `plugin:saleads:saleads` y pulsa **Authenticate**. Las skills quedan como `/saleads:saleads-strategic-plan`, `/saleads:saleads-primer-plan`, etc.

### Codex

```bash
codex plugin marketplace add JamiltonQuintero/saleads-agent
codex plugin add saleads@saleads
codex mcp login saleads
```

### Cursor

Abre este link (instalación en un clic) o copia [`examples/cursor/mcp.json`](examples/cursor/mcp.json) en `~/.cursor/mcp.json`:

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=saleads&config=eyJ1cmwiOiJodHRwczovL21jcC1kZXYuc2FsZWFkcy5haS9tY3AiLCJhdXRoIjp7IkNMSUVOVF9JRCI6Im1jcC1jdXJzb3IifX0=
```

Después pulsa **Connect/Login** en `saleads` (Settings → MCP).

### VS Code (GitHub Copilot)

Copia [`examples/vscode/.vscode/mcp.json`](examples/vscode/.vscode/mcp.json) a la carpeta `.vscode/` de tu proyecto y pulsa **Start** sobre el servidor `saleads`.

### Gemini CLI

```bash
gemini extensions install https://github.com/JamiltonQuintero/saleads-agent
```

Reinicia Gemini CLI y ejecuta `/mcp auth saleads`.

### claude.ai, Claude Desktop y ChatGPT (conector personalizado)

Agrega un conector personalizado con esta URL:

```text
https://mcp-dev.saleads.ai/mcp
```

En claude.ai: **Customize → Connectors → + → Add custom connector**, nombre `SaleADS`. Si el registro dinámico falla, usa `mcp-claude` como **OAuth Client ID** (sin secreto). El conector no trae las skills; para tenerlas usa el plugin.

Guía completa por cliente: [INSTALL.md](INSTALL.md).

## Cómo funciona el inicio de sesión

1. La primera vez, tu cliente abre el navegador en la página de inicio de sesión de SaleADS. Entras con tu cuenta de SaleADS (correo y contraseña, Google o Microsoft).
2. Aceptas los permisos: `saleads:read`, `saleads:strategy`, `saleads:campaigns` y `saleads:launch` (pedir el link de activación y pausar campañas).
3. El cliente guarda y renueva el token. **Nunca** escribas tu contraseña en el chat.

## Actualizar

- Claude Code: `claude plugin marketplace update saleads` y `claude plugin update saleads@saleads`; reinicia la sesión. O activa **Enable auto-update** en `/plugin` → **Marketplaces** → `saleads`.
- Codex: `codex plugin marketplace upgrade`.
- Gemini CLI: `gemini extensions update saleads`.

## Problemas frecuentes

- **El asistente no ve las tools de SaleADS:** ejecuta `/mcp` y autentica `plugin:saleads:saleads`; si no aparece, `/reload-plugins` o reinicia la sesión.
- **`MCP-E-SUBSCRIPTION-REQUIRED` o "disponible en los planes Pro y Business":** tu cuenta no tiene un plan con acceso.
- **El login vuelve a pedirse o aparece `401`:** la sesión expiró; vuelve a autenticar.
- **El asistente dice que "lanzó" las campañas:** no puede; pídele el link de activación y pulsa **Activar** en SaleADS.

Más casos en [INSTALL.md](INSTALL.md#problemas-frecuentes). No compartas tokens, contraseñas ni URLs firmadas al pedir ayuda.

## Canales

- `main`: canal principal. Hoy es el canal **dev** porque producción aún no existe. Cuando exista, `main` pasará a producción y el canal dev se moverá a la rama `dev` (`JamiltonQuintero/saleads-agent#dev`, marketplace `saleads-dev`).
- Este repo se genera automáticamente desde el repositorio fuente de SaleADS; no envíes cambios aquí. Reporta problemas en [Issues](https://github.com/JamiltonQuintero/saleads-agent/issues).

---

## English

### What SaleADS is

SaleADS plans, prepares and measures Meta Ads campaigns (Facebook, Instagram and WhatsApp) for businesses. With this plugin, your AI assistant can set up your business and offers, generate and approve the communication strategy, build the plan with all its campaigns, upload images and edit copy, request the launch, and read results without declaring causal winners. Skills are written in Spanish; the assistant answers in your language.

**SaleADS never launches from the chat.** The assistant gives you a link and you press **Activar** (Activate) in SaleADS. Activated campaigns go live in Meta and spend real budget.

### Requirements

- An account on **https://dev.saleads.ai** with an active **Pro, Business or Agency** plan. This channel uses the dev environment; production accounts do not work here.
- For the plugin with skills: Claude Code, Codex CLI or Gemini CLI, plus `git`. No access to any private repository is needed.

### Install

Claude Code (one command per line):

```bash
claude plugin marketplace add JamiltonQuintero/saleads-agent
claude plugin install saleads@saleads
```

Then run `/mcp`, pick `plugin:saleads:saleads` and press **Authenticate**.

- Codex: `codex plugin marketplace add JamiltonQuintero/saleads-agent`, `codex plugin add saleads@saleads`, `codex mcp login saleads`.
- Cursor: use the one-click link above or [`examples/cursor/mcp.json`](examples/cursor/mcp.json).
- VS Code: copy [`examples/vscode/.vscode/mcp.json`](examples/vscode/.vscode/mcp.json) into your project's `.vscode/`.
- Gemini CLI: `gemini extensions install https://github.com/JamiltonQuintero/saleads-agent`, then `/mcp auth saleads`.
- claude.ai / Claude Desktop / ChatGPT custom connector URL: `https://mcp-dev.saleads.ai/mcp` (fallback OAuth client ID for Claude: `mcp-claude`, no secret).

### How sign-in works

Your client opens the SaleADS sign-in page in the browser (OAuth 2.1 with PKCE). You sign in with your SaleADS account and accept the `saleads:*` permissions. The client stores and refreshes the token. Never type your password in the chat.

### Updates

- Claude Code: `claude plugin marketplace update saleads` then `claude plugin update saleads@saleads`, and restart; or enable auto-update in `/plugin` → **Marketplaces**.
- Codex: `codex plugin marketplace upgrade`. Gemini CLI: `gemini extensions update saleads`.

### Troubleshooting

- The assistant does not see the SaleADS tools: run `/mcp` and authenticate `plugin:saleads:saleads`; otherwise `/reload-plugins` or restart the session.
- `MCP-E-SUBSCRIPTION-REQUIRED`: your account has no plan with access.
- Sign-in keeps coming back or `401`: the session expired; authenticate again.
- More cases (in Spanish) in [INSTALL.md](INSTALL.md#problemas-frecuentes). Never share tokens, passwords or signed URLs.

### Channels

`main` is currently the **dev** channel because production does not exist yet. When it does, `main` becomes production and dev moves to the `dev` branch (`JamiltonQuintero/saleads-agent#dev`, marketplace `saleads-dev`).

This repository is generated automatically from the SaleADS source repository; please report issues instead of sending pull requests.

## License

The plugin content in this repository (skills, manifests, examples and docs) is licensed under the [Apache License 2.0](LICENSE). SaleADS and the SaleADS service are not covered by this license.
