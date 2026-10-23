# NodeMaven plugin for Claude

Skills that teach Claude to work with [NodeMaven](https://nodemaven.com)
residential proxies and the open-source tools around them, plus the read-only
NodeMaven MCP server for account data.

## Install

In Claude Code 2.1.275 or later:

```
/plugin install nodemaven --marketplace nodemaven/skills
```

On earlier versions:

```
/plugin marketplace add nodemaven/skills
/plugin install nodemaven@nodemaven
```

## Skills

| skill | Claude uses it to |
|---|---|
| `nodemaven-proxy` | connect code to the NodeMaven gateway: username format, country, region, city and ISP targeting, sticky sessions, rotation, and setup for requests, httpx, aiohttp, curl, Node, Playwright, Puppeteer, Selenium and Scrapy |
| `nodemaven-sdk` | write code with the official Python, Rust and Go SDKs: validated proxy URLs, session pools, connection checks and the account API client |
| `nodemaven-account-api` | call the NodeMaven dashboard REST API: traffic, statistics, locations, sub-users and the IP whitelist, including the quirks generated clients get wrong |
| `google-browser-scraper` | scrape Google search results to JSON with the open-source google-browser-scraper: CLI, Python, local HTTP API, MCP server or Docker |
| `proxy-benchmark` | measure and compare proxy providers and scraping browsers with the open-source proxy-benchmark harness |
| `proxy-troubleshooting` | diagnose proxy failures: 407, 406, 410 and 500 answers, hangs, Playwright and Chrome proxy authentication, sticky sessions that change IP, and unexpected traffic from Chrome |

The skills are instructions and code examples only. They run nothing by
themselves; any command or code they suggest runs only when you or Claude run it
in your project.

## MCP server

The plugin starts [`nodemaven-mcp`](https://github.com/nodemaven/nodemaven-mcp)
0.1.0, an open-source server published on PyPI, with `uvx nodemaven-mcp==0.1.0`.
It needs [uv](https://docs.astral.sh/uv/) on the machine and runs locally.

Its tools are read-only: `proxy_url`, `check_proxy`, `list_locations`,
`account_status`, `traffic_stats`, `top_domains` and `list_sub_users`. Nothing in
your account is created, changed or deleted.

## Credentials and network access

- **API key.** When the plugin is enabled, Claude asks for an optional NodeMaven
  API key from the dashboard. It is stored as a sensitive value and passed only
  to the MCP server, as `NODEMAVEN_APIKEY`. Without it the server still starts
  and each tool reports that the key is missing; the skills work either way.
- **What the MCP server contacts.** The account tools call the NodeMaven
  dashboard API at `dashboard.nodemaven.com` with that key. `check_proxy` opens
  one connection through the NodeMaven gateway `gate.nodemaven.com` to
  `api.ipify.org`, to report the exit IP. Proxy passwords and the account email
  are removed from every tool result.
- **What the skills suggest.** Code examples read proxy credentials and the API
  key from environment variables (`NODEMAVEN_LOGIN`, `NODEMAVEN_PASSWORD`,
  `NODEMAVEN_APIKEY`) instead of writing them into source, and send them only to
  NodeMaven's gateway and dashboard API.

## Evals

`evals/` holds two test cases per skill for `claude plugin eval`. They are not
loaded as part of the plugin.

## License

MIT, see [LICENSE](LICENSE).
