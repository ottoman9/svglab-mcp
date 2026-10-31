---
name: svglab-mcp
description: Connect to the SVG Lab MCP server (svglab.app) to design app screens, icons, illustrations, charts and slides as clean SVG in the user's SVG Lab account. Use when a user wants SVG Lab added to their AI app, wants the connection checked, or wants finished vector design work made in SVG Lab.
---

# SVG Lab MCP

SVG Lab (svglab.app) has a remote MCP server, the SVG Lab MCP, that houses SVG Lab's Design Engine: the Design Engine for AI. The AI brings the idea (content, palette, direction); SVG Lab guarantees the structure (spacing, alignment, sizing, type, valid vector geometry). Everything it makes is saved to the user's SVG Lab projects as finished designs that export as SVG, PNG or PDF. It runs on SVG Lab's servers, so nothing needs to be open in SVG Lab and there is no pairing code.

## Facts

```text
name:              svglab
server_url:        https://svglab.app/mcp
transport:         streamable HTTP (remote). Not SSE, not stdio. Nothing to install.
auth:              OAuth 2.1, authorization code with PKCE (S256), dynamic client registration
resource_metadata: https://svglab.app/.well-known/oauth-protected-resource/mcp
auth_server_meta:  https://svglab.app/.well-known/oauth-authorization-server
registration:      https://svglab.app/oauth/register
scope:             mcp
client_id:         not needed; leave client ID and client secret empty
no_credentials:    POST /mcp answers 401 with a WWW-Authenticate header pointing at resource_metadata
key_alternative:   header "Authorization: Bearer svl_KEY" (the user creates the key at svglab.app:
                   account menu, SVG Lab MCP, Generate key). Never invent a key.
access:            every account gets 100 free credits (up to 24 app screens) once to try it, no form; then a plan (Starter, Plus, Max, Ultra or Team) or a top-up: https://svglab.app/billing
cost:              credits: 1 a tool call, 2 a reference image search, failed calls free; 100 free credits (up to 24 app screens) once per account, then monthly plan credits or top-up credits (never expire)
usage:             https://svglab.app/usage (the signed in account's credits and its calls by tool, project and day)
browser_tab:       not needed; the server runs the Design Engine and saves to the user's My Projects
first_call:        can take 10 to 30 seconds while the Design Engine starts
sessions:          up to 2 AI sessions per SVG Lab account at once, each in its own artboard group
one_click_claude:  https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=SVG%20Lab&connectorUrl=https%3A%2F%2Fsvglab.app%2Fmcp
                   (opens Add custom connector with SVG Lab filled in; the user presses Add, then Connect)
one_click_cursor:  cursor://anysphere.cursor-deeplink/mcp/install?name=svglab&config=eyJ1cmwiOiJodHRwczovL3N2Z2xhYi5hcHAvbWNwIn0=
one_click_vscode:  vscode:mcp/install?%7B%22name%22%3A%22svglab%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fsvglab.app%2Fmcp%22%7D
app_steps:         https://svglab.app/mcp-server?app=NAME opens one app's steps; NAME is claude, chatgpt, codex,
                   claude-code, cursor or vscode. Every app: https://svglab.app/mcp-server#connect
full_guide:        https://svglab.app/mcp-setup (send the header Accept: text/markdown for Markdown)
```

## Procedure

1. **Find the app.** Work out which app the user is in (Claude Code, OpenCode, Codex, Cursor, VS Code, Gemini CLI, Windsurf, Claude, ChatGPT). If you cannot tell, ask once. If the user is not sure their account has access, ask them to open https://svglab.app/billing while signed in: it shows the current plan and the credits left (plan, free and top-up). Every account gets 100 free credits (up to 24 app screens); after that the SVG Lab MCP needs a plan (Starter, Plus, Max, Ultra or Team) or a top-up.
2. **Add the server** with the name `svglab`, using the line for that app below. Prefer the user level config (every project) unless the user asks for one project. When editing a config file, merge the entry into the existing file and keep every other server and setting. Never put a key in a project file that could be committed to git; use an environment variable.
3. **Start the sign-in.** Run the app's sign-in step. It opens the browser and waits until the user signs in to SVG Lab and presses Approve, so either run it and wait, or ask the user to run it in their own terminal. You cannot approve for them; tell them what to expect.
4. **Reload.** Restart the app or start a new session so it loads the SVG Lab tools. If you are running inside the app you are configuring, a restart ends your own session: finish the steps above first, then ask the user to restart and paste the test prompt below into a new session.
5. **Verify.** In the new session, check your tool list for tools from svglab. With access there are dozens of design tools. If there is only one tool, an account status tool, the connection works but the account cannot design yet (its 100 free credits are used up and it has no plan credits and no top-up credits, the account is suspended, or a key was replaced): call it and pass its message to the user. With sign-in, an account without access usually stops earlier: the SVG Lab sign-in page says the free credits are used up; the user can still approve, and the account status tool then says how to get credits.
6. **Test.** Run the test prompt below and give the user the project link it returns. The first design step can take 10 to 30 seconds; that is not a failure.
7. **If access is not granted**, tell the user: "SVG Lab is set up, but your SVG Lab account has no SVG Lab credits left: its 100 free credits are used up. Choose a plan, or buy a top-up, at https://svglab.app/billing with the same account, and then it works with no more setup."

## Add the server, per app

- **Claude (claude.ai, Claude Desktop):** give the user the one-click link https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=SVG%20Lab&connectorUrl=https%3A%2F%2Fsvglab.app%2Fmcp : Claude opens Add custom connector with SVG Lab and the URL filled in. They press Add, then Connect, sign in to SVG Lab and press Approve. By hand: Customize, Connectors, +, Add custom connector, name `SVG Lab`, URL `https://svglab.app/mcp`, leave Advanced settings empty. The user does this in Claude's settings.
- **Claude Code:** `claude mcp add --transport http --scope user svglab https://svglab.app/mcp`, then `/mcp` in Claude Code, choose svglab, Authenticate (or `claude mcp login svglab` from a terminal).
- **ChatGPT:** nothing to add, no developer mode: the user installs the SVG Lab plugin in one click at https://chatgpt.com/plugins/plugin_asdk_app_6ad2bf1ec4988191a371f507e64ed23b (or searches SVG Lab under Plugins in ChatGPT), presses +, signs in to SVG Lab and approves.
- **Codex:** Codex installs the same SVG Lab plugin: in the Codex app (or the ChatGPT desktop app), Plugins, search SVG Lab, press +; in the Codex CLI, `/plugins`. Then sign in to SVG Lab and approve, and start a new thread. Only the IDE extension, which has no plugins, adds the server by hand: `codex mcp add svglab --url https://svglab.app/mcp`, then Authenticate on the SVG Lab row in the Codex app (Settings, Plugins, MCPs).
- **Cursor:** in `~/.cursor/mcp.json`: `{ "mcpServers": { "svglab": { "url": "https://svglab.app/mcp" } } }`, then sign in when Cursor asks. Or one click: the user opens the `one_click_cursor` link above (or https://svglab.app/mcp-server?app=cursor) on a computer with Cursor and presses Install.
- **VS Code:** in `.vscode/mcp.json` (top key `servers`): `{ "servers": { "svglab": { "type": "http", "url": "https://svglab.app/mcp" } } }`, then start the server and sign in. Or one click: the user opens the `one_click_vscode` link above (or https://svglab.app/mcp-server?app=vscode) on a computer with VS Code and presses Install.
- **OpenCode:** in `~/.config/opencode/opencode.json` under `"mcp"`: `"svglab": { "type": "remote", "url": "https://svglab.app/mcp", "enabled": true }`, then run `opencode mcp auth svglab`.
- **Windsurf (Devin Desktop):** in the MCP config file (Cascade, ..., Open MCP config file): `{ "mcpServers": { "svglab": { "serverUrl": "https://svglab.app/mcp" } } }`, then refresh the MCP list and sign in.
- **Gemini CLI:** `gemini mcp add --transport http --scope user svglab https://svglab.app/mcp` (or `"httpUrl": "https://svglab.app/mcp"` in `~/.gemini/settings.json`; not `url`, which means SSE there), then `/mcp auth svglab`.
- **Any other MCP client:** remote MCP over streamable HTTP with OAuth at `https://svglab.app/mcp`; leave client ID and secret empty.

Key based versions for every app, and troubleshooting: https://svglab.app/mcp-setup .

## Test prompt

```text
Use the SVG Lab MCP server (svglab). First check my SVG Lab account status and tell me which account you are connected to and whether design access is on. Then create a new SVG Lab project called "Setup test", design a simple 64 by 64 paper plane icon in it, and give me the link to the project.
```

## Using it

After connecting, call tools/list; the server's instructions explain the routine. Designs are saved to the signed in account's My Projects. Up to two AI sessions can design on one SVG Lab account at once, each in its own group of artboards.

## Access

SVG Lab counts in credits. Every SVG Lab account gets 100 free credits (up to 24 app screens) once to try it, with no form to fill in. A tool call costs 1 credit, a reference image search 2, and failed calls cost nothing. After that, a plan adds credits every month: Starter ($7 a month, 2,500 credits, up to 600 app screens), Plus ($20, 7,400 credits, up to 1,800 app screens), Max ($40, 15,000 credits, up to 3,700 app screens) or Ultra ($99, 37,400 credits, up to 9,200 app screens), and for a team Team ($10 per editor seat a month, at least 2 seats, each seat 2,500 credits, up to 600 app screens, viewers free); top-ups add credits that never expire, for any account: $10 for 5,000 credits (up to 1,200 app screens), $25 for 10,000 credits (up to 2,400 app screens) and $50 for 25,000 credits (up to 6,100 app screens). Plans and top-ups: https://svglab.app/billing. Usage: https://svglab.app/usage.

Docs: https://svglab.app/mcp-server
