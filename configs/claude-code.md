# SVG Lab MCP in Claude Code

Add SVG Lab for every project:

```sh
claude mcp add --transport http --scope user svglab https://svglab.app/mcp
```

Leave out `--scope user` to add it to the current project only.

Sign in: start Claude Code, type `/mcp`, choose svglab and pick Authenticate. The browser opens the SVG Lab sign-in page; press Approve. From outside a session, `claude mcp login svglab` does the same. `/mcp` should then show svglab as connected.

With a key instead of sign-in (create it in SVG Lab: account menu, Connect your AI, Generate key):

```sh
claude mcp add --transport http --scope user svglab https://svglab.app/mcp --header "Authorization: Bearer svl_YOUR_KEY"
```

Replace `svl_YOUR_KEY` with your key. Claude Code saves it in the user settings (`~/.claude.json`), not in the project folder.

Full guide: https://svglab.app/mcp-setup
