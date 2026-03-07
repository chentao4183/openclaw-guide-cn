# 配置文件详解

深入了解 OpenClaw 的配置选项。

---

## 配置文件位置

- **主配置**: `~/.openclaw/openclaw.json`
- **格式**: JSON5（支持注释和尾随逗号）
- **热重载**: 大部分配置修改后自动生效

---

## 最小配置

```json5
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
    },
  },
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
    },
  },
}
```

---

## 完整配置结构

```json5
{
  // Agent 配置
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
      model: "claude-3-5-sonnet-20241022",
      sandbox: {
        mode: "non-main",
        scope: "agent",
        workspaceAccess: "none",
      },
    },
    list: [
      { id: "home", default: true },
      { id: "work", workspace: "~/.openclaw/workspace-work" },
    ],
  },

  // 渠道配置
  channels: {
    whatsapp: { /* ... */ },
    telegram: { /* ... */ },
    discord: { /* ... */ },
  },

  // Agent 路由绑定
  bindings: [
    { agentId: "home", match: { channel: "whatsapp" } },
    { agentId: "work", match: { channel: "telegram" } },
  ],

  // 会话配置
  session: {
    dmScope: "per-channel-peer",
    maintenance: {
      prune: { maxAgeDays: 30 },
      rotate: { maxMessages: 100 },
    },
  },

  // Hooks 配置
  hooks: {
    enabled: ["session-memory", "command-logger"],
  },
}
```

---

## 核心配置项

### agents - Agent 配置

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `defaults.workspace` | 工作目录 | `~/.openclaw/workspace` |
| `defaults.model` | 默认模型 | `claude-3-5-sonnet-20241022` |
| `defaults.sandbox.mode` | 沙箱模式 | `non-main` |
| `list` | Agent 列表 | - |

### channels - 渠道配置

| 参数 | 说明 |
|------|------|
| `enabled` | 是否启用 |
| `dmPolicy` | 私信策略 |
| `groupPolicy` | 群组策略 |
| `allowFrom` | 白名单 |

### session - 会话配置

| 参数 | 说明 |
|------|------|
| `dmScope` | 会话范围 |
| `maintenance.prune.maxAgeDays` | 清理旧会话 |
| `maintenance.rotate.maxMessages` | 消息轮换 |

---

## 沙箱配置

Agent 沙箱在隔离环境中运行工具：

```json5
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",  // off | non-main | all
        scope: "agent",    // session | agent | shared
        workspaceAccess: "none",  // none | ro | rw
      },
    },
  },
}
```

### 沙箱模式

| 模式 | 说明 |
|------|------|
| `off` | 禁用沙箱 |
| `non-main` | 非 main 会话使用沙箱 |
| `all` | 所有会话使用沙箱 |

---

## 环境变量

| 变量 | 说明 |
|------|------|
| `OPENCLAW_HOME` | 主目录 |
| `OPENCLAW_STATE_DIR` | 状态目录 |
| `OPENCLAW_CONFIG_PATH` | 配置文件路径 |

---

## 配置验证

```bash
# 检查配置
openclaw doctor

# 查看配置 schema
openclaw config schema

# 获取当前配置
openclaw config get
```

---

## 常见配置示例

### 多用户配置

```json5
{
  session: {
    dmScope: "per-channel-peer",
  },
  channels: {
    whatsapp: {
      dmPolicy: "pairing",
    },
  },
}
```

### 开放访问

```json5
{
  channels: {
    telegram: {
      dmPolicy: "open",
      allowFrom: ["*"],
    },
  },
}
```

### 严格限制

```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "allowlist",
      allowFrom: ["+8613800138000"],
      groupPolicy: "disabled",
    },
  },
}
```

---

## 下一步

- [多 Agent 路由](./multi-agent.md)
- [安全配置](./security.md)
