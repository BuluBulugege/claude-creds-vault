---
name: creds:add
description: Add a new credential (database or API key) to the global vault
---

Add a new credential to `~/.claude/creds-vault.local.md`.

1. Ask: type? `database` or `api_key`
2. Ask for fields:
   - **database**: name, tags (逗号分隔), host, port, user, password, dbname
   - **api_key**: name, tags (逗号分隔), api_key, base_url, model (optional)
3. Read `~/.claude/creds-vault.local.md` (if not exists, create with empty `credentials: []`)
4. Append the new entry to the `credentials` array in the YAML frontmatter
5. Write back and confirm

Tags 建议：数据库用 `local/remote/dev/prod/postgres/mysql`，API key 用服务商名如 `openai/anthropic/deepseek/llm`
