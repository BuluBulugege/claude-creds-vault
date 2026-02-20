---
description: Remove a credential from the vault by name
---

Remove a credential from `~/.claude/creds-vault.local.md`.

1. Read the vault and list all credential names with their tags
2. Ask user which one to remove (by name or number)
3. Remove the matching entry from the `credentials` array
4. Write back and confirm deletion
