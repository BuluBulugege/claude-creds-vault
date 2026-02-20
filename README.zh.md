[English](./README.md)

# claude-creds-vault

Claude Code 流畅编码插件。Claude 自主解析所需的任何凭证 —— 数据库连接、API Key、Token、服务密码 —— 不打断你的编码节奏。

**你专注写代码，凭证的事交给 Claude。**

## 问题所在

每次 Claude 需要凭证时，它都会停下来问你。你被迫中断思路，翻找数值，粘贴进去。这种情况在每个 API 调用、每个数据库连接、每个服务集成中反复发生。

## 解决方案

一次性存储凭证并打上标签。Claude 自动读取 vault，根据当前任务推断正确的凭证，然后静默继续执行。

## 安装

```bash
cp -r plugin ~/.claude/plugins/creds-vault
cat CLAUDE.md >> ~/.claude/CLAUDE.md
```

重启 Claude Code 生效。

## 命令

| 命令 | 说明 |
|------|------|
| `/creds:add` | 存储新凭证并打标签 |
| `/creds:list` | 列出所有凭证（值已脱敏） |
| `/creds:remove` | 删除凭证 |

## 工作原理

Claude 在需要任何凭证时自动读取 `~/.claude/creds-vault.local.md`，根据上下文推断标签匹配 —— 无需提示，不打断流程：

- 正在做 OpenAI 集成 → 使用标签 `openai` 的条目
- 连接本地数据库 → 使用标签 `local` 的条目
- 多个匹配项 → Claude 选择最合适的；只有真正无法判断时才询问

## Vault 格式

`~/.claude/creds-vault.local.md` —— 全局存储，永不提交到 git：

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

任何凭证类型均可 —— 按需添加字段即可。

## 仓库结构

```
plugin/          ← 安装到 ~/.claude/plugins/creds-vault
CLAUDE.md        ← 追加到 ~/.claude/CLAUDE.md
```

## License

MIT
