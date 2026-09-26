---
name: svglab-mcp
description: Connect to the SVG Lab MCP server (svglab.app) to design app screens, icons, illustrations, charts and slides as clean SVG in the user's SVG Lab account. Use when a user wants SVG Lab added to their AI app, wants the connection checked, or wants finished vector design work made in SVG Lab.
---

# SVG Lab MCP

SVG Lab (svglab.app) has a remote MCP server that gives AI apps a design engine. The AI brings the idea (content, palette, direction); SVG Lab guarantees the structure (spacing, alignment, sizing, type, valid vector geometry). Everything it makes is saved to the user's SVG Lab projects and exports as SVG, PNG or PDF. It runs on SVG Lab's servers, so no browser tab needs to stay open.

## Facts

```text
name:              svglab
server_url:        https://svglab.app/mcp
transport:         streamable HTTP (remote). Not SSE, not stdio. Nothing to install.
auth:              OAuth 2.1, authorization code with PKCE (S256), dynamic client registration
resource_metadata: https://svglab.app/.well-known/oauth-protected-resource/mcp
client_id:         not needed; leave client ID and client secret empty
key_alternative:   header "Authorization: Bearer svl_KEY" (the user creates the key at svglab.app:
                   account menu, Connect your AI, Generate key). Never invent a key.
access:            the account needs SVG Lab MCP early access: https://svglab.app/mcp-early-access
cost:              early access is free until Friday 2 October 2026; pricing announced at general release
first_call:        can take 10 to 30 seconds while the design engine starts
sessions:          one AI session per SVG Lab account at a time
full_guide:        https://svglab.app/mcp-setup (send the header Accept: text/markdown for Markdown)
```

## Procedure

1. **Find the app.** Work out which app the user is in (Claude Code, OpenCode, Codex, Cursor, VS Code, Gemini CLI, Windsurf, Claude, ChatGPT). If you cannot tell, ask once. If the user is not sure their account has access, ask them to open https://svglab.app/mcp-early-access while signed in: it says "MCP is already on for you" when access is on.
2. **Add the server** with the name `svglab`, using the block for that app below. Prefer the user level config (every project) unless the user asks for one project. When editing a config file, merge the entry into the existing file and keep every other server and setting. Never put a key in a project file that could be committed to git; use an environment variable.
3. **Start the sign-in.** Run the app's sign-in step. It opens the browser and waits until the user signs in to SVG Lab and presses Approve, so either run it and wait, or ask the user to run it in their own terminal. You cannot approve for them; tell them what to expect.
4. **Reload.** Restart the app or start a new session so it loads the SVG Lab tools. If you are running inside the app you are configuring, a restart ends your own session: finish the steps above first, then ask the user to restart and paste the test prompt below into a new session.
5. **Verify.** In the new session, check your tool list for tools from svglab. With access there are dozens of design tools. If there is only one tool, an account status tool, the connection works but the account cannot design yet: call it and pass its message to the user. With sign-in, an account without early access usually stops earlier: the SVG Lab sign-in page says the MCP is in early access and does not let the user approve.
6. **Test.** Run the test prompt below and give the user the project link it returns. The first design step can take 10 to 30 seconds; that is not a failure.
7. **If access is not granted**, tell the user: "SVG Lab is set up, but your SVG Lab account does not have MCP early access yet. Request it at https://svglab.app/mcp-early-access with the same account. You will get an email when it is switched on, and then it works with no more setup."

## Add the server, per app

- **Claude (claude.ai, Claude Desktop):** Customize, Connectors, +, Add custom connector. Name `SVG Lab`, URL `https://svglab.app/mcp`, leave Advanced settings empty. Press Add, then Connect, sign in to SVG Lab and press Approve. The user does this in Claude's settings.
- **Claude Code:** `claude mcp add --transport http --scope user svglab https://svglab.app/mcp`, then `/mcp` in Claude Code, choose svglab, Authenticate (or `claude mcp login svglab` from a terminal).
- **ChatGPT:** Settings, Security and login, turn on Developer mode. Then Plugins, +, create a developer mode app named `SVG Lab` with the MCP server URL `https://svglab.app/mcp` and OAuth, and sign in to SVG Lab. The user does this in ChatGPT's settings.
- **Codex:** `codex mcp add svglab --url https://svglab.app/mcp` (or `[mcp_servers.svglab]` with `url = "https://svglab.app/mcp"` in `~/.codex/config.toml`), then press Authenticate next to svglab in the Codex app's MCP settings.
- **Cursor:** in `~/.cursor/mcp.json`: `{ "mcpServers": { "svglab": { "url": "https://svglab.app/mcp" } } }`, then sign in when Cursor asks.
- **VS Code:** in `.vscode/mcp.json` (top key `servers`): `{ "servers": { "svglab": { "type": "http", "url": "https://svglab.app/mcp" } } }`, then start the server and sign in.
- **OpenCode:** in `~/.config/opencode/opencode.json` under `"mcp"`: `"svglab": { "type": "remote", "url": "https://svglab.app/mcp", "enabled": true }`, then run `opencode mcp auth svglab`.
- **Windsurf (Devin Desktop):** in the MCP config file (Cascade, ..., Open MCP config file): `{ "mcpServers": { "svglab": { "serverUrl": "https://svglab.app/mcp" } } }`, then refresh the MCP list and sign in.
- **Gemini CLI:** `gemini mcp add --transport http --scope user svglab https://svglab.app/mcp` (or `"httpUrl": "https://svglab.app/mcp"` in `~/.gemini/settings.json`; not `url`, which means SSE there), then `/mcp auth svglab`.
- **Any other MCP client:** remote MCP over streamable HTTP with OAuth at `https://svglab.app/mcp`; leave client ID and secret empty.

Key-based versions for every app, and troubleshooting: https://svglab.app/mcp-setup .

## Test prompt

```text
Use the SVG Lab MCP server (svglab). First check my SVG Lab account status and tell me which account you are connected to and whether design access is on. Then create a new SVG Lab project called "Setup test", design a simple 64 by 64 paper plane icon in it, and give me the link to the project.
```

## Using it

After connecting, call tools/list; the server's instructions explain the routine. Designs are saved to the signed-in account's My Projects. Run one AI session per SVG Lab account at a time.

Docs: https://svglab.app/mcp-server
