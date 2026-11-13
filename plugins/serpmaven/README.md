# SERP Maven plugin

Starts the [SERP Maven](https://github.com/nodemaven/serpmaven) MCP server
(`uvx serpmaven==0.2.3 mcp --prewarm`) in Claude Code or Cursor. It gives the agent one tool,
`google_search(query, pages, hl, gl)`: Google results from a real Chrome browser through your
residential proxy, with position, title, link and snippet, plus People also ask, related
searches, the AI Overview and local places when Google shows them.

## Install

```
/plugin marketplace add nodemaven/skills
/plugin install serpmaven@nodemaven
```

## Before the first search

Run these once in a terminal. They need [uv](https://docs.astral.sh/uv/).

```bash
uvx serpmaven login      # NodeMaven Proxy Username and Proxy Password from the dashboard
uvx serpmaven install    # the browser, about 150 MB, once per machine
```

For another provider with sticky sessions:
`uvx serpmaven login --proxy "http://USER-session-{session}:PASS@host:port"`.
No NodeMaven account yet: 2 GB free with the code GOOGLENM.

The first search of a session takes a few minutes while a fresh proxy IP warms up; later
searches take seconds.

## Credentials and network access

- **Proxy login.** The server uses the login saved by `serpmaven login`. The plugin passes no
  credentials and asks for none.
- **What the server contacts.** Google and the sites a warm-up visits, all through your proxy.
  Each results page is proxy traffic on your plan.

## License

MIT
