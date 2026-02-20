[English](./README.md)

# claude-creds-vault

Claude Code 凭证管理插件，自动管理数据库连接信息和 API Key。无需手动输入密码或密钥 —— Claude 根据上下文从本地 vault 中自动查找并使用。

## 功能特性

- **自动查找**：Claude 在需要凭证时自动读取 vault，无需提示
- **标签匹配**：给凭证打上 `local`、`openai`、`prod` 等标签，Claude 根据上下文选择正确的条目
- **多厂商 LLM 支持**：在 OpenAI、Anthropic、DeepSeek 等任意厂商之间无缝切换
- **全局 vault**：所有项目共享同一份凭证存储

## 安装

```bash
cp -r plugin ~/.claude/plugins/creds-vault
```

将 CLAUDE.md 规则追加到全局 `~/.claude/CLAUDE.md`：

```bash
cat CLAUDE.md >> ~/.claude/CLAUDE.md
```

重启 Claude Code 生效。

## 命令

| 命令 | 说明 |
|------|------|
| `/creds:add` | 添加数据库或 API Key 凭证 |
| `/creds:list` | 列出所有凭证（敏感值自动脱敏） |
| `/creds:remove` | 按名称删除凭证 |

## 工作原理

当任务需要凭证时，Claude 自动读取 `~/.claude/creds-vault.local.md`，根据请求上下文进行标签匹配：

- *"用 OpenAI 帮我调用接口"* → 匹配标签 `openai`
- *"连本地数据库"* → 匹配标签 `local`
- 多个匹配项 → Claude 列出选项让你选择

## Vault 文件格式

`~/.claude/creds-vault.local.md`（不会提交到 git）：

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

## 仓库结构

```
plugin/
├── plugin.json
├── skills/
│   └── lookup-creds.md      # 自动触发凭证查找
└── commands/
    ├── add.md               # /creds:add
    ├── list.md              # /creds:list
    └── remove.md            # /creds:remove
CLAUDE.md                    # 全局自动查找规则
```

## License

MIT
