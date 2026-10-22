---
type: llm
---

PASS if the answer runs both providers in one command with `--providers` so they are interleaved in the same time window, sets both providers' credentials in `.env` (`NODEMAVEN_*`, `OXYLABS_*`), and suggests `--dry-run` before spending traffic.
FAIL if it suggests two separate runs at different times as the comparison, or omits how credentials are configured.
