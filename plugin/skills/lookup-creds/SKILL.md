---
name: lookup-creds
description: When you need database credentials (host/port/user/password/dbname) or API keys (api_key/base_url/model), automatically look them up from the credentials vault at ~/.claude/creds-vault.local.md before asking the user. Use this skill whenever you encounter tasks requiring database connections or LLM/API authentication.
---

# Credential Vault Lookup

When you need credentials, follow these steps:

1. Read `~/.claude/creds-vault.local.md`
2. Parse the YAML frontmatter — all credentials are in the `credentials` array
3. Match by `tags`: use context clues from the user's request to find the best match
   - e.g., user says "用 OpenAI" → match tags containing `openai`
   - e.g., user says "本地数据库" → match tags containing `local`
   - e.g., user says "换成 Anthropic 的 key" → match tags containing `anthropic`
4. If one match: use it directly, no need to ask
5. If multiple matches: show a numbered list and ask user to pick
6. If no match: ask user for the credentials, then offer to save with `/creds:add`

## Vault File Format

```yaml
---
credentials:
  - name: local-postgres
    type: database
    tags: [local, postgres, dev]
    host: localhost
    port: 5432
    user: postgres
    password: "secret"
    dbname: myapp

  - name: openai-main
    type: api_key
    tags: [openai, gpt4, llm]
    api_key: "sk-xxx"
    base_url: "https://api.openai.com/v1"
    model: gpt-4o

  - name: anthropic-claude
    type: api_key
    tags: [anthropic, claude, llm]
    api_key: "sk-ant-xxx"
    base_url: "https://api.anthropic.com"
    model: claude-3-5-sonnet-20241022
---
```

## Rules

- Never expose full passwords or api_keys in chat — mask as `sk-****` or `pass****`
- Prefer the most specific tag match over generic ones
- If vault file doesn't exist, tell user to run `/creds:add` to create it
