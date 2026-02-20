# aiconfig — Claude Code Credential Vault Plugin

A Claude Code plugin that automatically manages database credentials and API keys. No more manually typing passwords or API keys — Claude looks them up for you.

## Features

- **Auto-lookup**: Claude reads the vault before asking you for credentials
- **Flexible tag matching**: tag credentials with `local`, `openai`, `prod`, etc. — Claude picks the right one from context
- **Multiple vendors**: switch between OpenAI, Anthropic, DeepSeek, or any LLM provider seamlessly
- **Global vault**: one credential store shared across all your projects

## Installation

```bash
# Copy plugin to Claude Code plugins directory
cp -r . ~/.claude/plugins/creds-vault
```

Restart Claude Code to load the plugin.

## Usage

### Add a credential

```
/creds:add
```

Prompts you for type (database or API key), fields, and tags.

### List all credentials

```
/creds:list
```

Displays all stored credentials with sensitive values masked.

### Remove a credential

```
/creds:remove
```

### Auto-lookup

Just work normally. When Claude needs a database connection or API key, it reads `~/.claude/creds-vault.local.md` and uses the matching credential automatically.

Example: *"用 OpenAI 帮我调用这个接口"* → Claude finds the entry tagged `openai` and uses it.

## Credential Storage

Credentials are stored in `~/.claude/creds-vault.local.md` (global, never committed to git):

```yaml
---
credentials:
  - name: local-postgres
    type: database
    tags: [local, postgres, dev]
    host: localhost
    port: 5432
    user: postgres
    password: "your-password"
    dbname: myapp

  - name: openai-main
    type: api_key
    tags: [openai, gpt4, llm]
    api_key: "sk-xxx"
    base_url: "https://api.openai.com/v1"
    model: gpt-4o
---
```

## Plugin Structure

```
creds-vault/
├── plugin.json
├── skills/
│   └── lookup-creds.md      # Auto-triggers credential lookup
└── commands/
    ├── add.md               # /creds:add
    ├── list.md              # /creds:list
    └── remove.md            # /creds:remove
```

## License

MIT
