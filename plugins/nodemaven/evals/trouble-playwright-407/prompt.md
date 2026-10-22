---
description: "Playwright drops credentials written into proxy.server"
tags: [troubleshooting, playwright]
max_turns: 10
allowed_tools: [Skill, Read, Glob, Grep]
---

My Playwright script gets 407 from the NodeMaven proxy. I launch Chromium with proxy={'server': 'http://mylogin-country-us:mypass@gate.nodemaven.com:8080'}. curl with the same credentials works. What is wrong?
