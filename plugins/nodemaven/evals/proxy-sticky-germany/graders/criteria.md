---
type: llm
---

PASS if the username carries a sticky session (`sid-<id>`) with a `ttl` written in lower-case minutes or hours (for example `ttl-10m`), the session id contains no `-`, and the credentials are read from the environment rather than hardcoded.
FAIL if there is no `sid`, the ttl is upper case or in seconds, or a login or password is written into the code.
