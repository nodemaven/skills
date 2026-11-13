# NodeMaven plugin for Claude

Skills that teach Claude to work with [NodeMaven](https://nodemaven.com)
residential proxies and the open-source tools around them. The read-only NodeMaven MCP
server for account data is a separate plugin, `nodemaven-mcp`, in the same marketplace.

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
| `serpmaven` | scrape Google search results to JSON with the open-source SERP Maven: CLI, Python, local HTTP API, MCP server or Docker |
| `proxy-benchmark` | measure and compare proxy providers and scraping browsers with the open-source proxy-benchmark harness |
| `proxy-troubleshooting` | diagnose proxy failures: 407, 406, 410 and 500 answers, hangs, Playwright and Chrome proxy authentication, sticky sessions that change IP, and unexpected traffic from Chrome |

The skills are instructions and code examples only. They run nothing by
themselves; any command or code they suggest runs only when you or Claude run it
in your project.

## MCP server

The NodeMaven MCP server is a separate plugin in this marketplace, `nodemaven-mcp`, so this
one is skills only. It starts [`nodemaven-mcp`](https://github.com/nodemaven/nodemaven-mcp)
0.2.3 with `uvx nodemaven-mcp==0.2.3`, so [uv](https://docs.astral.sh/uv/) has to be
installed, and asks for the dashboard API key when it is enabled. Its read tools are
`proxy_url`, `check_proxy`, `test_proxy_on_site`, `list_locations`, `account_status`,
`traffic_stats`, `top_domains`, `list_sub_users` and `list_whitelist_ips`. Its tools that
change sub-users and whitelisted IPs stay off: the plugin does not set
`NODEMAVEN_MCP_ALLOW_WRITE`. Proxy passwords are kept out of tool results.

## Credentials and network access

- **No credential is read by this plugin.** It ships skills only: no MCP server, no
  scripts, no stored key.
- **What the skills suggest.** Code examples read proxy credentials and the API
  key from environment variables (`NODEMAVEN_LOGIN`, `NODEMAVEN_PASSWORD`,
  `NODEMAVEN_APIKEY`) instead of writing them into source, and send them only to
  NodeMaven's gateway and dashboard API.

## Evals

`evals/` holds two test cases per skill for `claude plugin eval`. They are not
loaded as part of the plugin.

## License

MIT, see [LICENSE](LICENSE).
