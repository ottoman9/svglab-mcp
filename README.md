# SVG Lab MCP

**SVG Lab MCP** is the remote MCP server of [SVG Lab (svglab.app)](https://svglab.app/mcp-server). It gives Claude, ChatGPT, Codex, Cursor and any other MCP client a design engine that builds app screens, icons, illustrations and charts as clean SVG, saved to your SVG Lab projects and ready to export as SVG, PNG or PDF.

The AI brings the idea (content, palette, direction); SVG Lab guarantees the structure (spacing, alignment, sizing, type, valid vector geometry). It runs on SVG Lab's servers, so there is nothing to install and no browser tab needs to stay open.

```text
https://svglab.app/mcp
```

Streamable HTTP, OAuth sign-in, no key needed.

This repository holds documentation, ready-made client configs and an agent skill. It contains no server code. The full, always current setup guide is at **https://svglab.app/mcp-setup**.

## Contents

- [For AI agents](#for-ai-agents)
- [Before you start](#before-you-start)
- [Quick setup](#quick-setup): [Claude](#claude-claudeai-and-claude-desktop) · [Claude Code](#claude-code) · [ChatGPT](#chatgpt) · [Codex](#codex) · [Cursor](#cursor) · [VS Code](#vs-code-github-copilot-agent-mode) · [OpenCode](#opencode) · [Windsurf](#windsurf-devin-desktop) · [Gemini CLI](#gemini-cli) · [Any MCP client](#any-other-mcp-client)
- [Check it works](#check-it-works)
- [Examples](#examples)
- [Access](#access)
- [FAQ](#faq)
- [Links](#links)
- [Files in this repository](#files-in-this-repository)

## For AI agents

You are probably setting this up for a user. Facts:

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
                   account menu, Connect your AI, Generate key). Never invent a key.
access:            the account needs SVG Lab MCP early access: https://svglab.app/mcp-early-access
cost:              early access is free until Friday 2 October 2026; pricing announced at general release
browser_tab:       not needed; the server runs the design engine and saves to the user's My Projects
first_call:        can take 10 to 30 seconds while the design engine starts
sessions:          one AI session per SVG Lab account at a time
registry_name:     app.svglab/svglab
setup_markdown:    https://svglab.app/mcp-setup (send the header Accept: text/markdown)
agent_skill:       skills/svglab-mcp/SKILL.md in this repository (npx skills add ottoman9/svglab-mcp)
```

Procedure:

1. **Find the app.** Work out which app the user is in (Claude Code, OpenCode, Codex, Cursor, VS Code, Gemini CLI, Windsurf, Claude, ChatGPT). If you cannot tell, ask once. If the user is not sure their account has access, ask them to open https://svglab.app/mcp-early-access while signed in: it says "MCP is already on for you" when access is on.
2. **Add the server.** Use that app's block below exactly as written, with the name `svglab`. Prefer the user level config (every project) unless the user asks for one project. When editing a config file, merge the entry into the existing file and keep every other server and setting. Never put a key in a project file that could be committed to git; use the environment variable form.
3. **Start the sign-in.** Run the app's sign-in step (for example `opencode mcp auth svglab`, or `/mcp` in Claude Code). It opens the browser and waits until the user signs in to SVG Lab and presses Approve, so either run it and wait, or ask the user to run it in their own terminal. You cannot approve for them; tell them what to expect.
4. **Reload.** Restart the app or start a new session so it loads the SVG Lab tools. If you are running inside the app you are configuring, a restart ends your own session: finish the steps above first, then ask the user to restart and paste the test prompt from [Check it works](#check-it-works) into a new session.
5. **Verify.** In the new session, check your own tool list for tools from svglab. With access there are dozens of design tools. If there is only one tool, an account status tool, the connection works but the account cannot design yet (access is off, the account is suspended, or a key was replaced): call it and pass its message to the user. With sign-in, an account without early access usually stops earlier: the SVG Lab sign-in page says the MCP is in early access and does not let the user approve.
6. **Test.** Run the test prompt from [Check it works](#check-it-works) and give the user the project link it returns. The first design step can take 10 to 30 seconds; that is not a failure.
7. **If access is not granted**, tell the user: "SVG Lab is set up, but your SVG Lab account does not have MCP early access yet. Request it at https://svglab.app/mcp-early-access with the same account. You will get an email when it is switched on, and then it works with no more setup."

## Before you start

1. An SVG Lab account. Sign in at https://svglab.app/ with Google or an email link.
2. SVG Lab MCP early access switched on for that account. Request it at https://svglab.app/mcp-early-access . You get a confirmation email straight away and another one when your account is switched on.

Sign in with the same account every time: when the AI app opens the SVG Lab sign-in page, use the account that has early access. Designs are saved to that account's My Projects.

Every app signs in the same way: add the address, the app opens a "Sign in to SVG Lab" page, sign in, check which app is asking, press Approve.

## Quick setup

Short versions below. Ready config files are in [configs/](configs/). The full guide, with key-based setups and troubleshooting, is at https://svglab.app/mcp-setup .

### Claude (claude.ai and Claude Desktop)

Custom connectors are available on the Free, Pro, Max, Team and Enterprise plans; a Free account can add one custom connector.

1. In Claude, open Customize, then Connectors (on the web: https://claude.ai/customize/connectors).
2. Press +, then Add custom connector.
3. Name it `SVG Lab` and paste `https://svglab.app/mcp` as the remote MCP server URL. Leave Advanced settings (OAuth Client ID and Secret) empty.
4. Press Add, then Connect next to SVG Lab. Sign in to SVG Lab and press Approve.
5. In a chat, press + at the lower left, open Connectors and make sure SVG Lab is switched on.

Team or Enterprise: an owner adds the connector once under Organization settings, then Connectors; each person presses Connect and signs in with their own SVG Lab account.

### Claude Code

```sh
claude mcp add --transport http --scope user svglab https://svglab.app/mcp
```

`--scope user` makes SVG Lab available in every project. Leave it out to add it to the current project only. Then start Claude Code, type `/mcp`, choose svglab and pick Authenticate. The browser opens the SVG Lab sign-in page; press Approve. `/mcp` should show svglab as connected.

### ChatGPT

Needs developer mode, available on the web for ChatGPT Plus, Pro, Business, Enterprise and Education (in a Business, Enterprise or Education workspace an admin has to allow it first).

1. Settings, then Security and login: turn on Developer mode.
2. Open Plugins and press + to create a developer mode app.
3. Name: `SVG Lab`. Description: `Design engine: app screens, icons, illustrations and charts as clean SVG`. MCP server URL: `https://svglab.app/mcp`. Authentication: OAuth.
4. Create it. When ChatGPT asks you to sign in, sign in to SVG Lab and press Approve.
5. In a chat, open the + menu, choose Developer mode and select SVG Lab.

ChatGPT renames these menus from time to time. If Plugins is missing, look for Apps or Connectors in Settings; the fields are the same.

### Codex

The desktop app, IDE extension and CLI share `~/.codex/config.toml`, so add SVG Lab once:

```sh
codex mcp add svglab --url https://svglab.app/mcp
```

Or add to `~/.codex/config.toml` by hand:

```toml
[mcp_servers.svglab]
url = "https://svglab.app/mcp"
```

Open the Codex app, go to its MCP server settings, find svglab and press Authenticate. The browser opens the SVG Lab sign-in page; press Approve. Start a new Codex chat so it loads the SVG Lab tools.

### Cursor

Add to `~/.cursor/mcp.json` (every project) or `.cursor/mcp.json` (this project only), merging into an existing `mcpServers`:

```json
{
  "mcpServers": {
    "svglab": {
      "url": "https://svglab.app/mcp"
    }
  }
}
```

Open Cursor Settings and find svglab in the MCP list. When Cursor asks you to sign in, press Approve on the SVG Lab sign-in page. Then use it from a chat in Agent mode.

### VS Code (GitHub Copilot agent mode)

Create `.vscode/mcp.json` in the project (or run "MCP: Open User Configuration" for every project). The top key is `servers`, not `mcpServers`:

```json
{
  "servers": {
    "svglab": {
      "type": "http",
      "url": "https://svglab.app/mcp"
    }
  }
}
```

Or run "MCP: Add Server", choose HTTP and paste `https://svglab.app/mcp`. Start the server; VS Code opens the browser for the SVG Lab sign-in on the first connection. Then open Copilot Chat in Agent mode.

### OpenCode

Add the `svglab` entry inside `mcp` in `~/.config/opencode/opencode.json` (every project; on Windows `C:\Users\YOU\.config\opencode\opencode.json`) or `opencode.json` in the project folder. It may be named `opencode.jsonc`; edit the one that exists.

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "svglab": {
      "type": "remote",
      "url": "https://svglab.app/mcp",
      "enabled": true
    }
  }
}
```

Then sign in from a terminal and press Approve in the browser:

```sh
opencode mcp auth svglab
```

Restart OpenCode (or start a new session) and run `opencode mcp list` to see svglab connected.

### Windsurf (Devin Desktop)

In the Cascade panel, open the ... menu and choose Open MCP config file (`~/.config/devin/mcp_config.json` on macOS and Linux, `%APPDATA%\devin\mcp_config.json` on Windows; older Windsurf versions use `~/.codeium/windsurf/mcp_config.json`). Add:

```json
{
  "mcpServers": {
    "svglab": {
      "serverUrl": "https://svglab.app/mcp"
    }
  }
}
```

Save, refresh the MCP list in Cascade, then sign in to SVG Lab and press Approve when asked.

### Gemini CLI

```sh
gemini mcp add --transport http --scope user svglab https://svglab.app/mcp
```

Or add to `~/.gemini/settings.json`. Use `httpUrl`, not `url`: in Gemini CLI, `url` means the older SSE transport.

```json
{
  "mcpServers": {
    "svglab": {
      "httpUrl": "https://svglab.app/mcp"
    }
  }
}
```

Start Gemini CLI; it opens the browser for the sign-in. To start it yourself, type `/mcp auth svglab`. Type `/mcp` to check that svglab is connected.

### Any other MCP client

- Server URL: `https://svglab.app/mcp`
- Transport: streamable HTTP (sometimes shown as "HTTP" or "remote"). Not SSE, and not a local command.
- Authentication: OAuth. SVG Lab supports automatic app registration and PKCE, so leave any client ID and secret fields empty.
- Or a header: `Authorization: Bearer svl_YOUR_KEY` if the app has no sign-in support. Create the key in SVG Lab: account menu, Connect your AI, Generate key.

```json
{
  "mcpServers": {
    "svglab": {
      "url": "https://svglab.app/mcp"
    }
  }
}
```

## Check it works

Start a new chat in the AI app and paste this prompt:

```text
Use the SVG Lab MCP server (svglab). First check my SVG Lab account status and tell me which account you are connected to and whether design access is on. Then create a new SVG Lab project called "Setup test", design a simple 64 by 64 paper plane icon in it, and give me the link to the project.
```

Expected: the assistant says it is connected to SVG Lab and names the account; it designs the icon (the first design step can take 10 to 30 seconds while SVG Lab starts the design engine; later steps are quick); it gives a link, and the new project is in My Projects, ready to edit or export as SVG, PNG or PDF.

## Examples

App screens made through the SVG Lab MCP, as shown on [svglab.app/mcp-server](https://svglab.app/mcp-server).

<table>
  <tr>
    <td><img src="https://svglab.app/img/examples/sanguine-01.webp" width="220" alt="Sanguine welcome screen: a dancer silhouette in a stopwatch dial on a sanguine red field, and the headline Catch the pose in 30 seconds."></td>
    <td><img src="https://svglab.app/img/examples/cadence-01.webp" width="220" alt="Cadence welcome screen: a dancer mid-leap over a pink panel, and the headline Move on the beat."></td>
    <td><img src="https://svglab.app/img/examples/lampshow-01.webp" width="220" alt="Lampshow welcome screen: a fox shadow puppet on rods in front of a lamp, and the headline Tell stories with shadows."></td>
  </tr>
  <tr>
    <td><img src="https://svglab.app/img/examples/sanguine-02.webp" width="220" alt="Sanguine today screen with the day's warm-up card and colour-coded pose decks."></td>
    <td><img src="https://svglab.app/img/examples/cadence-02.webp" width="220" alt="Cadence today screen with a Tempo Run 5K card, later classes and minutes this week."></td>
    <td><img src="https://svglab.app/img/examples/sanguine-04.webp" width="220" alt="Sanguine sketchbook screen with a weekly bar chart of poses and the poses you drew."></td>
  </tr>
</table>

## Access

The SVG Lab MCP is in **early access**. Early access is free until Friday 2 October 2026. Request it at https://svglab.app/mcp-early-access with a free SVG Lab account; you get an email when it is switched on. Pricing will be announced at general release.

## FAQ

**Do I need an API key?**
No. Every app above can sign in: it opens SVG Lab's sign-in page, you press Approve, and you are connected. A key is only an alternative for command line apps or machines with no browser.

**Do I need to keep SVG Lab open in a tab?**
No. SVG Lab runs the design engine on its own servers, and everything your assistant makes is saved to My Projects. To watch it work live, open SVG Lab, click Connect to AI in the top bar and give your assistant the code it shows.

**Which account does my work go to?**
The SVG Lab account you signed in with when you pressed Approve.

**Can I connect more than one app?**
Yes. Run one AI session per SVG Lab account at a time; two sessions working on one account at once can interrupt each other.

**How do I disconnect an app?**
In SVG Lab, open the account menu (your email, top right) and choose Connect your AI. Under Connected apps, press Disconnect next to the app.

**Is there a package to install or a separate MCP subdomain?**
No. There is nothing to install and no mcp.svglab.app. The one and only server address is https://svglab.app/mcp.

**The first request is slow.**
Normal. The first design step starts the design engine and can take 10 to 30 seconds.

More answers and troubleshooting: https://svglab.app/mcp-setup . Still stuck? Email support@svglab.app with the app and the exact message.

## Links

- Website: https://svglab.app/mcp-server
- Setup guide: https://svglab.app/mcp-setup
- Early access: https://svglab.app/mcp-early-access
- llms.txt: https://svglab.app/llms.txt
- MCP server card: https://svglab.app/.well-known/mcp/server-card.json
- Official MCP Registry entry `app.svglab/svglab`: https://registry.modelcontextprotocol.io/v0.1/servers?search=app.svglab

## Files in this repository

| Path | What it is |
|---|---|
| [configs/](configs/) | One ready config file per client |
| [skills/svglab-mcp/SKILL.md](skills/svglab-mcp/SKILL.md) | Agent skill: add, sign in to and verify the SVG Lab MCP. Install with `npx skills add ottoman9/svglab-mcp` |
| [server.json](server.json) | Copy of the official MCP Registry manifest, for reference |
| [LICENSE](LICENSE) | Creative Commons Attribution 4.0 International |

## License

The documentation, configs and skill in this repository are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). The example images are hosted on svglab.app and are not part of this repository. SVG Lab itself is a hosted service; no server code is published here.
