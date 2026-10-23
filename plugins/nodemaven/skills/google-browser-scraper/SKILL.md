---
name: google-browser-scraper
description: Scrape Google search results (SERP) to JSON with google-browser-scraper - a real Chrome browser on a residential proxy with sticky sessions. Run it as a CLI, a Python library, a local SerpApi-style HTTP API (`serve`), an MCP server, or a Docker image; set workers, pages, cost tracking and NodeMaven credentials. Use when the user wants Google results, rank tracking, a self-hosted SERP API, or live Google search for an AI agent.
license: MIT
---

# google-browser-scraper

Open source, MIT: [github.com/nodemaven/google-browser-scraper](https://github.com/nodemaven/google-browser-scraper).
It warms a sticky proxy session with ordinary browsing, opens Google's results
page for each query in a real Chrome (Patchright), checks whether Google served
results or refused, and writes one JSON record per results page. Captchas and
blocks come back labelled, never as empty results.

## Install

```bash
pip install google-browser-scraper            # Python 3.10+
patchright install chromium
google-browser-scraper doctor                 # what a site would see; sends nothing
```

With NodeMaven: `pip install "google-browser-scraper[nodemaven]"`.

## Proxy

It needs a residential proxy **with sticky sessions**. With any provider, put
`{session}` where the session id goes in the username:

```bash
google-browser-scraper search "best running shoes" \
  --proxy "http://USER-session-{session}:PASS@gate.example.com:7000" -o results.jsonl
```

With NodeMaven, the session id is handled and credentials come from
`NODEMAVEN_LOGIN` and `NODEMAVEN_PASSWORD`:

```bash
google-browser-scraper search -f queries.txt --nodemaven --country us -o results.jsonl
```

To cap spend, `--nodemaven-demo --traffic-limit-mb 300` creates a sub-user with a
traffic limit from the dashboard API key in **`NODEMAVEN_API_KEY`** (this tool's
name; the SDK's is `NODEMAVEN_APIKEY`) and deletes it when the run ends. The
account needs sub-users enabled. A fresh warm-up alone is about 60 MB, so keep
the cap well above that.

`--no-proxy` uses the machine's own connection: fine for a try, captchas come
sooner. SOCKS5 needs an IP-whitelisted endpoint, because Chrome cannot send a
SOCKS5 username and password.

## Expectations to set

- The first query takes minutes: a new exit is warmed up first. Later queries
  on the same exit take seconds.
- The warm-up costs far more traffic than a search; served exits are held and
  reused, and new exits start from copied cookies.
- A run that gets refused exit after exit stops instead of spending more.
- The summary at the end prints pages served, traffic and the hosts that took
  it; `--price-per-gb` adds cost per 1000 pages.

## Ways to run it

HTTP API, same record format:

```bash
google-browser-scraper serve --nodemaven --country us --prewarm
curl "http://127.0.0.1:8000/search.json?q=best+running+shoes"
```

`/health` answers 503 `warming` until the prewarm is done. Requests run one at a
time. Set `GBS_API_KEY` to require a key; without one it listens on localhost
only.

MCP server for an AI client (one tool, `google_search(query, pages)`):

```json
{
  "mcpServers": {
    "google-search": {
      "command": "google-browser-scraper",
      "args": ["mcp", "--nodemaven", "--country", "us", "--prewarm"],
      "env": {"NODEMAVEN_LOGIN": "...", "NODEMAVEN_PASSWORD": "..."}
    }
  }
}
```

Python:

```python
from google_browser_scraper import ProxyTemplate, Scraper, Settings

proxy = ProxyTemplate("http://USER-session-{session}:PASS@gate.example.com:7000")
for page in Scraper(proxy, Settings(pages=2)).run(["best running shoes"]):
    for result in page.get("organic_results", []):
        print(result["position"], result["title"], result["link"])
```

Docker: `ghcr.io/nodemaven/google-browser-scraper`, e.g.
`docker run --rm -p 8000:8000 -e GBS_API_KEY=... -e GBS_PROXY='http://USER-session-{session}:PASS@host:port' ghcr.io/nodemaven/google-browser-scraper serve --host 0.0.0.0`.

## Output

One JSON object per results page: `search_metadata` (`status`, `page_verdict`,
`timings`), `search_parameters`, `organic_results` (`position`, `title`, `link`,
`snippet`), `ads`, `related_questions`, `related_searches`, `pagination`, and an
`exit` block with the session and bytes. Field names follow common SERP APIs.
Knowledge panels, shopping and images are not parsed. A failed query is a record
with an `error`. `--format csv` writes organic results as a table.

## Options worth knowing

| option | default | |
|---|---|---|
| `--workers N` | 1 | N browsers on N exits from one query list |
| `--pages N` | 1 | result pages per query |
| `--country CC` | | with `--nodemaven` |
| `--max-per-exit N` | 300 | queries before a served exit is retired |
| `--gap LOW,HIGH` | 2,5 | seconds between queries on one exit |
| `--no-resolve-links` | | keep Google's `/goto` redirect links |
| `--save-html DIR` | | keep page HTML, for reporting a parsing problem |

`google-browser-scraper search --help` lists the rest. Use within Google's terms
and the law where it runs.
