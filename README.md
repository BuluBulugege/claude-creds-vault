[中文](./README.zh.md)

# claude-creds-vault

A Claude Code plugin that automatically manages database credentials and API keys. No more manually typing passwords or API keys — Claude looks them up from a local vault based on context.

## Features

- **Auto-lookup**: Claude reads the vault before asking you for credentials
- **Tag-based matching**: tag entries with `local`, `openai`, `prod`, etc. — Claude picks the right one from context
- **Multi-vendor LLM support**: switch between OpenAI, Anthropic, DeepSeek, or any provider seamlessly
- **Global vault**: one credential store shared across all projects

## Installation

```bash
cp -r plugin ~/.claude/plugins/creds-vault
```

Add the CLAUDE.md rule to your global `~/.claude/CLAUDE.md` (or append if it already exists):

```bash
cat CLAUDE.md >> ~/.claude/CLAUDE.md
```

Restart Claude Code.

## Commands

| Command | Description |
|---------|-------------|
| `/creds:add` | Add a database or API key credential |
| `/creds:list` | List all credentials (sensitive values masked) |
| `/creds:remove` | Remove a credential by name |

## How It Works

Claude automatically reads `~/.claude/creds-vault.local.md` whenever a task requires credentials. It matches by tags using context from your request:

- *"用 OpenAI 帮我调用接口"* → matches tag `openai`
- *"连本地数据库"* → matches tag `local`
- Multiple matches → Claude lists options and asks you to pick

## Vault File Format

`~/.claude/creds-vault.local.md` (never committed to git):

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
---
```

## Repository Structure

```
plugin/
├── plugin.json
├── skills/
│   └── lookup-creds.md      # Auto-triggers credential lookup
└── commands/
    ├── add.md               # /creds:add
    ├── list.md              # /creds:list
    └── remove.md            # /creds:remove
CLAUDE.md                    # Global rule for auto-lookup behavior
```

## License

MIT
