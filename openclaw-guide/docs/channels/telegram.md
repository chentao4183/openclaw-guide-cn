# Telegram 连接指南

> 把 AI 助手接到 Telegram Bot 上

---

## 📋 准备工作

- ✅ OpenClaw 已安装并运行
- ✅ 一个 Telegram 账号
- ✅ 能访问 @BotFather

---

## 🤖 第一步：创建 Bot

1. **打开 Telegram**，搜索 `@BotFather`

2. **发送命令**：
```
/newbot
```

3. **按提示操作**：
   - Bot 名称（显示名）：例如 `My AI Assistant`
   - Bot 用户名（唯一）：例如 `my_ai_assistant_bot`

4. **保存 Token**：
```
Done! Congratulations on your new bot...
Use this token to access the HTTP API:
1234567890:ABCdefGHIjklMNOpqrsTUVwxyz

Keep your token secure...
```

**⚠️ 重要**：Token 格式是 `数字:字母数字`，例如 `1234567890:ABCdef...`

---

## 🔧 第二步：配置 OpenClaw

编辑 `~/.openclaw/openclaw.json`：

```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "1234567890:ABCdefGHIjklMNOpqrsTUVwxyz",  // 你的 token
      
      // DM 策略
      dmPolicy: "pairing",
      allowFrom: ["tg:123456789"],  // 你的 Telegram 用户 ID
      
      // 群组策略
      groups: { 
        "*": { requireMention: true }  // 群里需要 @ 提及
      },
    },
  },
}
```

---

## 🚀 第三步：启动

```bash
# 重启 Gateway
openclaw gateway restart

# 检查状态
openclaw channels status
```

---

## 💬 第四步：测试

1. **在 Telegram 搜索你的 Bot**（用刚才的用户名）

2. **点击 Start** 或发送 `/start`

3. **会收到配对码**：
```
Your pairing code is: ABC123
Approve it on your gateway.
```

4. **批准配对**：
```bash
openclaw pairing list telegram
openclaw pairing approve telegram ABC123
```

5. **开始对话**！

---

## 🔍 如何获取 Telegram 用户 ID

### 方法一：看日志

```bash
# 发送消息给 Bot，然后查看日志
openclaw logs --follow
```

会看到：
```json
{
  "from": {
    "id": 123456789,
    "username": "yourname"
  }
}
```

### 方法二：用第三方 Bot

1. 搜索 `@userinfobot` 或 `@getidsbot`
2. 发送任意消息
3. 会返回你的用户 ID

---

## 🎯 完整配置示例

```json5
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
      model: {
        primary: "anthropic/claude-sonnet-4-5",
      },
    },
  },
  
  channels: {
    telegram: {
      enabled: true,
      botToken: "1234567890:ABCdef...",
      
      dmPolicy: "allowlist",
      allowFrom: ["tg:123456789", "tg:987654321"],
      
      groupPolicy: "allowlist",
      groupAllowFrom: ["tg:123456789"],
      
      groups: {
        "*": { requireMention: true },
        "-1001234567890": { 
          requireMention: false,  // 这个群不需要 @
          groupPolicy: "open",
        },
      },
      
      // 可选设置
      textChunkLimit: 4000,
      linkPreview: true,
    },
  },
}
```

---

## 👥 群组配置

### 添加 Bot 到群组

1. 在群组设置中点击 "Add Member"
2. 搜索你的 Bot 用户名
3. 添加为管理员（推荐）

### 配置群组权限

```json5
{
  channels: {
    telegram: {
      groups: {
        "-1001234567890": {
          requireMention: true,
          groupPolicy: "allowlist",
          groupAllowFrom: ["tg:123456789"],
        },
      },
    },
  },
}
```

**获取群组 ID**：
```bash
# 方法一：转发群消息给 @userinfobot
# 方法二：看日志
openclaw logs --follow | grep "chat.id"
```

---

## 🔒 安全建议

### ✅ 推荐（私有 Bot）

```json5
{
  channels: {
    telegram: {
      dmPolicy: "allowlist",
      allowFrom: ["tg:你的ID"],
      groupPolicy: "allowlist",
    },
  },
}
```

### ⚠️ 公开 Bot（谨慎）

```json5
{
  channels: {
    telegram: {
      dmPolicy: "open",
      allowFrom: ["*"],
      // 必须配置工具限制
      tools: {
        deny: ["exec", "write", "edit"],
      },
    },
  },
}
```

---

## 🎨 高级功能

### 自定义命令菜单

BotFather 中设置：
```
/setcommands
start - Start the bot
help - Get help
new - Start new session
status - Check status
```

### Forum Topics（论坛主题）

```json5
{
  channels: {
    telegram: {
      groups: {
        "-1001234567890": {
          topics: {
            "1": { agentId: "main" },      // General
            "3": { agentId: "coding" },    // Coding topic
          },
        },
      },
    },
  },
}
```

### 流式回复（实时显示）

```json5
{
  channels: {
    telegram: {
      streaming: "partial",  // off | partial | block
    },
  },
}
```

---

## ❓ 常见问题

### Bot 无响应

```bash
# 检查 token 是否正确
openclaw channels status

# 查看 Telegram API 连接
curl "https://api.telegram.org/bot<你的token>/getMe"

# 重启
openclaw gateway restart
```

### 群消息看不到

- **隐私模式**：在 BotFather 中 `/setprivacy` → Disable
- **需要管理员**：将 Bot 设为群管理员
- **白名单**：检查 `groups` 配置

### Token 泄露了

1. 在 BotFather 中 `/revoke` 重新生成
2. 更新 `openclaw.json` 中的 `botToken`
3. 重启 Gateway

### 多个 Bot

```json5
{
  channels: {
    telegram: {
      accounts: {
        main: {
          botToken: "123:ABC...",
        },
        alerts: {
          botToken: "456:DEF...",
        },
      },
    },
  },
}
```

---

## 📚 相关文档

- [WhatsApp 连接](./whatsapp.md)
- [Discord 连接](./discord.md)
- [多 Agent 路由](../advanced/multi-agent.md)
- [安全配置](../security.md)

---

_预计耗时：5 分钟_
