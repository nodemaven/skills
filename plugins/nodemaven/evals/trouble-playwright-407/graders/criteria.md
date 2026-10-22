---
type: llm
---

PASS if the answer says Playwright ignores a username and password written into `server` and shows the fix: `server` without credentials plus separate `username` and `password` fields.
FAIL if it blames the password, the account or the country parameter, or keeps the credentials inside the server URL.
