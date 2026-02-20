# Credential Vault

When any task requires credentials of any kind (API keys, base URLs, database connections, tokens, secrets, service passwords, or any authentication info):

1. **Read `~/.claude/creds-vault.local.md`** immediately — do not ask the user first
2. Match by `tags` or `name` using context inference from the task at hand
3. **If found (one match)**: use it directly and silently — no confirmation needed
4. **If multiple matches**: infer the best fit from context; only ask if genuinely impossible to determine
5. **If not found**: use a placeholder, complete the task fully, then suggest `/creds:add` at the end

**Default behavior: act autonomously. Never interrupt the coding flow to ask for credentials that can be reasonably inferred.**
