---
name: creds:list
description: List all stored credentials with sensitive values masked
---

Read `~/.claude/creds-vault.local.md` and display all credentials.

- Mask sensitive fields: show only first 4 chars + `****` (e.g., `sk-a****`, `pass****`)
- Display as a table: name | type | tags | host/base_url | masked key/password
- If file doesn't exist, tell user to run `/creds:add`
