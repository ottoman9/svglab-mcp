# Client configs for the SVG Lab MCP

Each file adds the SVG Lab MCP (`https://svglab.app/mcp`, streamable HTTP, OAuth sign-in) under the name `svglab`. If the target file already exists, merge the `svglab` entry into it and keep every other server and setting.

| Client | File here | Where it goes |
|---|---|---|
| Claude Code | [claude-code.md](claude-code.md) | A command, no file to copy |
| Codex | [codex/config.toml](codex/config.toml) | `~/.codex/config.toml` |
| Cursor | [cursor/mcp.json](cursor/mcp.json) | `~/.cursor/mcp.json` (every project) or `.cursor/mcp.json` (one project) |
| VS Code | [vscode/mcp.json](vscode/mcp.json) | `.vscode/mcp.json`, or "MCP: Open User Configuration" for every project |
| OpenCode | [opencode/opencode.json](opencode/opencode.json) | `~/.config/opencode/opencode.json` (every project) or `opencode.json` in the project |
| Windsurf (Devin Desktop) | [windsurf/mcp_config.json](windsurf/mcp_config.json) | `~/.config/devin/mcp_config.json` (macOS, Linux), `%APPDATA%\devin\mcp_config.json` (Windows); older versions `~/.codeium/windsurf/mcp_config.json` |
| Gemini CLI | [gemini-cli/settings.json](gemini-cli/settings.json) | `~/.gemini/settings.json` |
| Any other MCP client | [generic/mcp.json](generic/mcp.json) | Wherever the client reads its MCP servers |

Claude (claude.ai, Claude Desktop) and ChatGPT are set up in their settings screens, with no file. See the main [README](../README.md#quick-setup).

After adding the server, run the app's sign-in step, sign in to SVG Lab and press Approve. No key is needed. Key-based versions of every config (for machines with no browser) are in the full guide: https://svglab.app/mcp-setup . Never put a key in a file that could be committed to git.
