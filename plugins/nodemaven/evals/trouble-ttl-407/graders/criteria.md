---
type: llm
---

PASS if the answer says the password is not the problem, that this gateway answers 407 for a value it rejects, and that `ttl` is case-sensitive so `10M` must be written `10m`.
FAIL if it tells the user to reset or check the password as the main fix.
