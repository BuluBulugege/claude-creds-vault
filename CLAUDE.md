# Credential Vault

When a task requires API keys, base URLs, database credentials, or any external service authentication:

1. **Read `~/.claude/creds-vault.local.md`** first
2. Search the `credentials` array — match by `tags` or `name` based on context (e.g., user says "OpenAI" → match tag `openai`; "本地数据库" → match tag `local`)
3. **If found**: use the credentials directly, no need to ask the user
4. **If multiple matches**: list them and ask user to pick one
5. **If not found**: use a placeholder (e.g., `<YOUR_API_KEY>`), complete the task, then remind user to run `/creds:add` to save the real credentials

Never ask the user to manually type credentials that already exist in the vault.
