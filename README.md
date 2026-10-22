# NodeMaven skills for Claude Code

A Claude Code plugin that teaches Claude to work with
[NodeMaven](https://nodemaven.com) residential proxies and the open-source tools
around them, plus the read-only NodeMaven MCP server.

## Install

```
/plugin marketplace add nodemaven/skills
/plugin install nodemaven@nodemaven
```

Claude Code asks for an optional NodeMaven API key when the plugin is enabled. It is used only
by the MCP server, and is stored as a sensitive value.

## Skills

| skill | Claude uses it to |
|---|---|
| `nodemaven-proxy` | connect code to the gateway: username format, geo-targeting, sticky sessions, rotation, setup for requests, httpx, aiohttp, curl, Node, Playwright, Puppeteer, Selenium, Scrapy |
| `nodemaven-sdk` | write code with the Python, Rust and Go SDKs: validated proxy URLs, session pools, `check()`, the API client |
| `nodemaven-account-api` | call the dashboard REST API: traffic, statistics, locations, sub-users, IP whitelist, and the quirks generated clients get wrong |
| `google-browser-scraper` | scrape Google results to JSON with a real browser: CLI, Python, local HTTP API, MCP server, Docker |
| `proxy-benchmark` | measure and compare proxy providers and scraping browsers with the open-source harness |
| `proxy-troubleshooting` | diagnose 407, 406, 410 and 500 answers, hangs, Playwright and Chrome proxy auth, sticky sessions that move, traffic from idle Chrome |

Skills load on their own when a request matches. They can also be called by
name, e.g. `/nodemaven:nodemaven-proxy`.

## MCP server

The plugin starts [`nodemaven-mcp`](https://github.com/nodemaven/nodemaven-mcp)
with `uvx`, so [uv](https://docs.astral.sh/uv/) has to be installed. Its tools
are read-only: `proxy_url`, `check_proxy`, `list_locations`, `account_status`,
`traffic_stats`, `top_domains`, `list_sub_users`. Proxy passwords are kept out of
tool results.

## Evals

Each skill has two cases under `plugins/nodemaven/evals/`. From
`plugins/nodemaven`:

```bash
claude plugin eval .
claude plugin eval . --case proxy-sid-with-dash --runs 1
```

Results go to `evals/results/`, which is not committed.

## Related

- [nodemaven-python](https://github.com/nodemaven/nodemaven-python),
  [nodemaven-rust](https://github.com/nodemaven/nodemaven-rust),
  [nodemaven-go](https://github.com/nodemaven/nodemaven-go)
- [nodemaven-mcp](https://github.com/nodemaven/nodemaven-mcp)
- [google-browser-scraper](https://github.com/nodemaven/google-browser-scraper)
- [proxy-benchmark](https://github.com/nodemaven/proxy-benchmark)

## License

MIT
