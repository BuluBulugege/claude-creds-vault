[中文](./README.zh.md)

# claude-creds-vault

A Claude Code plugin for frictionless development. Claude autonomously resolves any credentials it needs — database connections, API keys, tokens, service passwords — without interrupting your coding flow.

**You code. Claude handles the rest.**

## The Problem

Every time Claude needs a credential, it stops and asks you. You break focus, dig up the value, paste it in. Multiply that by every API call, every DB connection, every service integration across every project.

## The Solution

Store credentials once, tagged with context. Claude reads the vault automatically, infers the right credential from what you're working on, and proceeds — silently.

## Installation

```bash
cp -r plugin ~/.claude/plugins/creds-vault
cat CLAUDE.md >> ~/.claude/CLAUDE.md
```

Restart Claude Code.

## Commands

| Command | Description |
|---------|-------------|
| `/creds:add` | Store a new credential with tags |
| `/creds:list` | List all credentials (values masked) |
| `/creds:remove` | Remove a credential |

## How It Works

Claude reads `~/.claude/creds-vault.local.md` whenever it needs any credential. It matches by tags inferred from context — no prompts, no interruptions:

- Working on an OpenAI integration → uses entry tagged `openai`
- Connecting to a local DB → uses entry tagged `local`
- Multiple matches → Claude picks the best fit; only asks if truly ambiguous

## Vault Format

`~/.claude/creds-vault.local.md` — global, never committed to git:

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
    tags: [openai, llm]
    api_key: "sk-xxx"
    base_url: "https://api.openai.com/v1"
    model: gpt-4o

  - name: github-token
    type: token
    tags: [github, git]
    token: "ghp_xxx"
---
```

Any credential type works — just add the fields you need.

## Repository Structure

```
plugin/          ← install to ~/.claude/plugins/creds-vault
CLAUDE.md        ← append to ~/.claude/CLAUDE.md
```

## License

MIT
