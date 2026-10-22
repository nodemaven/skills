---
type: llm
---

PASS if the answer explains that a session id cannot contain `-` because the gateway splits values on it, and builds the username with a dash-free id such as `order4417` or `order_4417` together with `country-us`.
FAIL if it uses `sid-order-4417` or does not mention the dash problem.
