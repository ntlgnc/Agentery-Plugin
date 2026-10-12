# Agentery plugin

The official Agentery plugin for AI coding agents and assistants. It connects
your agent to Agentery's hosted MCP server so it can:

- **find suitable AI tools** — AI products, agents, MCP servers and APIs for a
  specific task, with task fit and delivery type stated;
- **compare their published prices** — plans, billing units and pricing for a
  shortlist, side by side on one basis;
- **check fair market prices** — the median and typical range for comparable
  products, by buyer tier and billing unit.

Agentery reports only what vendors publish, with the source and the date each
price was observed. A missing price is shown as unknown, never as zero, and
"contact sales" is not treated as a price.

This repository contains only the plugin wrapper (manifests, MCP configuration,
one skill and a logo). Agentery's server code, database and index methodology
are not part of it.

## Install

One repository works on every platform. Each one reads its own manifest and
connects to the same endpoint, `https://agentery.com/api/mcp`. No account,
no sign-in and no API key are needed on any of them.

| Platform | Install | Files it reads |
| --- | --- | --- |
| Grok Build | `grok plugin marketplace add xai-org/plugin-marketplace` then `grok plugin install agentery --trust` | `.grok-plugin/plugin.json`, `.mcp.json`, `skills/` |
| Claude Code | `claude --plugin-dir <clone>`, or install from any marketplace that lists it | `.claude-plugin/plugin.json`, `.mcp.json`, `skills/` |
| Cursor | From a clone as a local plugin, or from the Cursor marketplace once listed | `.cursor-plugin/plugin.json`, `mcp.json`, `skills/` |
| Codex and other Agent Plugins clients | Install from a clone or a marketplace that lists it | `plugin.json`, `mcp.json`, `skills/` |
| Gemini CLI | `gemini extensions install https://github.com/ntlgnc/Agentery-Plugin` | `gemini-extension.json` |
| Any other MCP client | Add a remote (streamable HTTP) MCP server with URL `https://agentery.com/api/mcp` and no auth | none |

Claude (claude.ai and Claude Desktop) can also connect directly: Settings →
Connectors → Add custom connector → `https://agentery.com/api/mcp`.

## Tools

The server exposes 23 tools. Each carries MCP `readOnlyHint` / `destructiveHint`
annotations.

| Group | Tools |
| --- | --- |
| Find | `search_providers`, `research_capability`, `find_market`, `suggest_alternatives` |
| Inspect & compare | `get_provider_profile`, `get_provider`, `compare_providers` |
| Fair prices | `price_benchmark`, `get_group_fair_price`, `market_report` |
| Price index | `get_price_index`, `get_price_index_history` (Agent Economy Price Index) |
| Demand | `demand_signals`, `market_gaps` |
| Product questions | `request_research`, `get_research`, `stop_research_updates` |
| Your own items | `refine_results`, `report_outcome`, `create_custom_benchmark`, `get_custom_benchmark`, `update_custom_benchmark`, `delete_custom_benchmark` |

16 tools are read-only. 7 write, and each changes only the caller's own items:
a research request, a refinement of your own search, an outcome report, or a
private benchmark. No tool changes Agentery's shared market data.

## Network endpoints and credentials

- **Endpoint:** `https://agentery.com/api/mcp` — Agentery's hosted MCP server
  over HTTPS (streamable HTTP). Tool arguments are sent there and nowhere else.
- **Authentication:** none. The public server is keyless; the plugin sends no
  headers and no token.
- **Authentication exception:** the server also has one partner-only tool,
  `rank_providers_for_workflow`, which requires a partner Bearer key. It is not
  listed to keyless connections, refuses keyless calls, and is not used by this
  plugin.
- **Private benchmarks:** `create_custom_benchmark` returns a private access
  token. It is a capability for that one benchmark, not an account credential;
  the plugin does not store it.
- **Rate limit:** 30 tool calls per minute per IP.
- **Not included:** no hooks, no commands, no lifecycle or install scripts, no
  local processes, no telemetry, no environment variables and no filesystem access.

## Privacy and terms

- Privacy policy: https://agentery.com/privacy
- Terms of service: https://agentery.com/terms
- Documentation and examples: https://agentery.com/connect
- Support: help@agentery.com

## License

The plugin wrapper in this repository is released under the [MIT License](LICENSE).
Use of the Agentery service is governed by its terms of service.
