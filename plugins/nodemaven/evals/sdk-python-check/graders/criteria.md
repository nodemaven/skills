---
type: llm
---

PASS if the answer uses `Proxy(..., country="fr")` from the `nodemaven` package and calls `.check()`, and says it opens a single CONNECT and returns the status (a refusal comes back as a value, not an exception).
FAIL if it sends an HTTP request through the proxy to a website instead, or invents SDK calls that do not exist.
