# 安全配置

保护你的 OpenClaw 实例。

---

## 安全模型

OpenClaw 的安全模型包括：

| 层面 | 说明 |
|------|------|
| 访问控制 | 控制谁能触发 AI |
| 沙箱隔离 | 隔离执行环境 |
| 审计日志 | 记录所有操作 |
| 权限管理 | 限制工具权限 |

---

## 访问控制

### DM 策略

```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "pairing",  // pairing | allowlist | open | disabled
      allowFrom: ["+8613800138000"],
    },
  },
}
```

| 策略 | 说明 |
|------|------|
| `pairing` | 未知用户需配对码批准 |
| `allowlist` | 只允许白名单用户 |
| `open` | 允许所有人 |
| `disabled` | 禁止私信 |

### 群组策略

```json5
{
  channels: {
    telegram: {
      groupPolicy: "allowlist",
      groups: {
        "*": {
          requireMention: true,
        },
      },
    },
  },
}
```

---

## 配对管理

### 查看配对请求

```bash
openclaw pairing list <channel>
```

### 批准/拒绝

```bash
# 批准
openclaw pairing approve <channel> <code>

# 拒绝
openclaw pairing reject <channel> <code>
```

### 配对超时

```json5
{
  channels: {
    whatsapp: {
      pairingTimeout: 300,  // 5 分钟
    },
  },
}
```

---

## 沙箱隔离

Agent 沙箱在隔离环境中运行工具：

```json5
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",      // off | non-main | all
        scope: "agent",        // session | agent | shared
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

### 工作目录访问

| 权限 | 说明 |
|------|------|
| `none` | 无访问权限 |
| `ro` | 只读访问 |
| `rw` | 读写访问 |

---

## 安全审计

### 运行审计

```bash
# 检查安全问题
openclaw security audit

# 自动修复
openclaw security audit --fix
```

### 审计项目

- 配置文件权限
- 敏感信息暴露
- 不安全的设置
- 网络暴露风险

---

## 审计日志

### 查看日志

```bash
# 实时日志
openclaw logs --follow

# 最近日志
openclaw logs --lines 100

# 过滤日志
openclaw logs --follow | grep "security"
```

### 日志内容

- 用户操作
- 工具调用
- 配置变更
- 错误和警告

---

## 网络安全

### 限制监听地址

```json5
{
  gateway: {
    host: "127.0.0.1",  // 只监听本地
    port: 18789,
  },
}
```

### HTTPS

```json5
{
  gateway: {
    tls: {
      enabled: true,
      cert: "/path/to/cert.pem",
      key: "/path/to/key.pem",
    },
  },
}
```

---

## 敏感信息保护

### 环境变量

使用环境变量存储敏感信息：

```bash
export OPENAI_API_KEY="sk-xxx"
export ANTHROPIC_API_KEY="sk-xxx"
```

### 配置文件

不要在配置文件中硬编码密钥：

```json5
{
  // ❌ 不推荐
  channels: {
    telegram: {
      botToken: "123:abc",
    },
  },

  // ✅ 推荐（使用环境变量）
  channels: {
    telegram: {
      botToken: "${TELEGRAM_BOT_TOKEN}",
    },
  },
}
```

---

## 权限管理

### 工具权限

限制可用的工具：

```json5
{
  agents: {
    defaults: {
      tools: {
        allow: ["web_search", "calendar"],
        deny: ["exec", "browser"],
      },
    },
  },
}
```

### Skill 权限

限制可用的 Skills：

```json5
{
  agents: {
    defaults: {
      skills: {
        allow: ["weather", "github"],
        deny: ["coding-agent"],
      },
    },
  },
}
```

---

## 最佳实践

### 1. 最小权限原则

只授予必要的权限：
- 不需要 exec？禁用它
- 不需要浏览器？禁用它

### 2. 定期审计

```bash
# 每周运行安全审计
openclaw security audit
```

### 3. 更新维护

```bash
# 更新到最新版本
npm update -g openclaw
```

### 4. 监控日志

定期检查异常操作：

```bash
openclaw logs --lines 1000 | grep -i "error\|warning\|failed"
```

---

## 安全检查清单

- [ ] 配置 `dmPolicy` 和 `groupPolicy`
- [ ] 启用沙箱模式
- [ ] 使用环境变量存储密钥
- [ ] 限制监听地址
- [ ] 定期运行安全审计
- [ ] 定期检查日志
- [ ] 保持软件更新

---

## 下一步

- [配置文件详解](./configuration.md)
- [故障排查](../troubleshooting.md)
