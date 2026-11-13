# NodeMaven MCP plugin

Starts the read-only [NodeMaven MCP server](https://github.com/nodemaven/nodemaven-mcp)
(`uvx nodemaven-mcp==0.2.3`) in Claude Code or Cursor, for account data: geo-targeted proxy
URLs with sticky sessions, proxy checks and site tests, locations, traffic and statistics.
The skills are a separate plugin, `nodemaven`, in the same marketplace.

## Install

```
/plugin marketplace add nodemaven/skills
/plugin install nodemaven-mcp@nodemaven
```

It needs [uv](https://docs.astral.sh/uv/) on the machine.

## Credentials and network access

- **API key.** When the plugin is enabled, Claude Code asks for an optional NodeMaven API key
  from the dashboard. It is stored as a sensitive value and passed only to the MCP server, as
  `NODEMAVEN_APIKEY`. In Cursor the server reads `NODEMAVEN_APIKEY` from the environment.
- **What the server contacts.** The account tools call the NodeMaven dashboard API at
  `dashboard.nodemaven.com` with that key. `check_proxy` opens one connection through the
  NodeMaven gateway `gate.nodemaven.com` to `api.ipify.org`, to report the exit IP. Proxy
  passwords and the account email are removed from every tool result.
- **Writes stay off.** The tools that change sub-users and whitelisted IPs refuse to run
  unless `NODEMAVEN_MCP_ALLOW_WRITE=1` is set, and the plugin does not set it.

## License

MIT
