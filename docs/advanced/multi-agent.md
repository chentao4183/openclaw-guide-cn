# 多 Agent 路由

为不同场景配置不同的 Agent。

---

## 什么是多 Agent？

多 Agent 允许你：
- 为工作和个人使用不同的 Agent
- 不同渠道使用不同的配置
- 隔离不同用途的会话历史

---

## 配置 Agent 列表

```json5
{
  agents: {
    list: [
      {
        id: "home",
        default: true,
        workspace: "~/.openclaw/workspace-home",
        model: "claude-3-5-sonnet-20241022",
      },
      {
        id: "work",
        workspace: "~/.openclaw/workspace-work",
        model: "claude-3-5-sonnet-20241022",
      },
      {
        id: "assistant",
        workspace: "~/.openclaw/workspace-assistant",
        model: "gpt-4o",
      },
    ],
  },
}
```

### Agent 配置项

| 参数 | 说明 | 必需 |
|------|------|------|
| `id` | Agent 唯一标识 | ✅ |
| `default` | 是否为默认 Agent | ❌ |
| `workspace` | 工作目录 | ❌ |
| `model` | 使用的模型 | ❌ |

---

## 路由绑定

将渠道绑定到特定 Agent：

```json5
{
  bindings: [
    // WhatsApp 使用 home Agent
    {
      agentId: "home",
      match: { channel: "whatsapp" },
    },

    // Telegram 使用 work Agent
    {
      agentId: "work",
      match: { channel: "telegram" },
    },

    // 特定 Telegram 账号使用 assistant Agent
    {
      agentId: "assistant",
      match: {
        channel: "telegram",
        accountId: "bot_business",
      },
    },

    // 特定群组使用特定 Agent
    {
      agentId: "work",
      match: {
        channel: "discord",
        guildId: "123456789",
      },
    },
  ],
}
```

### 匹配规则

| 参数 | 说明 |
|------|------|
| `channel` | 渠道类型 |
| `accountId` | 账号 ID |
| `guildId` | 服务器 ID（Discord） |
| `groupId` | 群组 ID |

---

## 使用场景

### 场景 1：工作/生活分离

```json5
{
  agents: {
    list: [
      { id: "personal", default: true },
      { id: "work" },
    ],
  },
  bindings: [
    { agentId: "personal", match: { channel: "whatsapp" } },
    { agentId: "work", match: { channel: "slack" } },
  ],
}
```

### 场景 2：多机器人

```json5
{
  agents: {
    list: [
      { id: "general", default: true },
      { id: "support" },
      { id: "sales" },
    ],
  },
  bindings: [
    { agentId: "support", match: { channel: "telegram", accountId: "support_bot" } },
    { agentId: "sales", match: { channel: "telegram", accountId: "sales_bot" } },
  ],
}
```

### 场景 3：不同模型

```json5
{
  agents: {
    list: [
      { id: "fast", model: "claude-3-5-haiku-20241022", default: true },
      { id: "smart", model: "claude-3-5-sonnet-20241022" },
    ],
  },
}
```

---

## 切换 Agent

在对话中切换 Agent：

```
/agent work
```

查看当前 Agent：

```
/status
```

---

## 工作目录隔离

每个 Agent 有独立的工作目录：

```
~/.openclaw/
├── workspace-home/      # home Agent
│   ├── MEMORY.md
│   └── skills/
├── workspace-work/      # work Agent
│   ├── MEMORY.md
│   └── skills/
└── workspace/           # 默认 Agent
```

---

## 下一步

- [Skills 技能](./skills.md)
- [安全配置](./security.md)
