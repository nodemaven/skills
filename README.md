# NodeMaven skills for Claude Code

[![2 GB free with code GITHUB](https://img.shields.io/badge/%F0%9F%8E%81%202%20GB%20free-code%20GITHUB-1153FC)](https://dashboard.nodemaven.com/accounts/signup/?utm_source=github&utm_medium=readme&utm_campaign=skills&utm_content=promo_badge)

A Claude Code plugin that teaches Claude to work with
[NodeMaven](https://nodemaven.com) residential proxies and the open-source tools
around them, plus the read-only NodeMaven MCP server and the SERP Maven search server.

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

The read-only MCP server for the account is a second plugin in the same marketplace:
`/plugin install nodemaven-mcp@nodemaven`. Claude Code asks for an optional NodeMaven API key
when it is enabled; it is used only by the MCP server and stored as a sensitive value.

Live Google search for the agent is a third plugin, `/plugin install serpmaven@nodemaven`. It
starts the [SERP Maven](https://github.com/nodemaven/serpmaven) MCP server and uses the proxy
login saved once with `uvx serpmaven login`; `uvx serpmaven install` downloads its browser.

All three plugins work in Cursor too (`.cursor-plugin/`); there the NodeMaven MCP server reads
the API key from `NODEMAVEN_APIKEY` in the environment Cursor starts in.

> [!TIP]
> 🎁 **2 GB of free traffic.** No account yet? [Sign up](https://dashboard.nodemaven.com/accounts/signup/?utm_source=github&utm_medium=readme&utm_campaign=skills&utm_content=signup_promo) and enter the code **GITHUB**. Once per account.

## Skills

| skill | Claude uses it to |
|---|---|
| `nodemaven-proxy` | connect code to the gateway: username format, geo-targeting, sticky sessions, rotation, setup for requests, httpx, aiohttp, curl, Node, Playwright, Puppeteer, Selenium, Scrapy |
| `nodemaven-sdk` | write code with the Python, Rust and Go SDKs: validated proxy URLs, session pools, `check()`, the API client |
| `nodemaven-account-api` | call the dashboard REST API: traffic, statistics, locations, sub-users, IP whitelist, and the quirks generated clients get wrong |
| `serpmaven` | scrape Google results to JSON with a real browser (SERP Maven): CLI, Python, local HTTP API, MCP server, Docker |
| `proxy-benchmark` | measure and compare proxy providers and scraping browsers with the open-source harness |
| `proxy-troubleshooting` | diagnose 407, 406, 410 and 500 answers, hangs, Playwright and Chrome proxy auth, sticky sessions that move, traffic from idle Chrome |

Skills load on their own when a request matches. They can also be called by
name, e.g. `/nodemaven:nodemaven-proxy`.

## MCP server

The NodeMaven MCP server is a separate plugin in this marketplace, `nodemaven-mcp`, so this
one is skills only. It starts [`nodemaven-mcp`](https://github.com/nodemaven/nodemaven-mcp)
0.2.3 with `uvx nodemaven-mcp==0.2.3`, so [uv](https://docs.astral.sh/uv/) has to be
installed, and asks for the dashboard API key when it is enabled. Its read tools are
`proxy_url`, `check_proxy`, `test_proxy_on_site`, `list_locations`, `account_status`,
`traffic_stats`, `top_domains`, `list_sub_users` and `list_whitelist_ips`. Its tools that
change sub-users and whitelisted IPs stay off: the plugin does not set
`NODEMAVEN_MCP_ALLOW_WRITE`. Proxy passwords are kept out of tool results.

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
- [SERP Maven](https://github.com/nodemaven/serpmaven)
- [proxy-benchmark](https://github.com/nodemaven/proxy-benchmark)

## License

MIT
