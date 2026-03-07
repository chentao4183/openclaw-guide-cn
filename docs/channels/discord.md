# Discord 配置指南

将 OpenClaw 连接到 Discord 服务器。

---

## 前置要求

- 已安装 OpenClaw
- 有 Discord 账号
- 有 Discord 服务器管理权限

---

## 步骤 1：创建 Discord Bot

### 1.1 访问 Discord Developer Portal

👉 https://discord.com/developers/applications

### 1.2 创建应用

1. 点击 **New Application**
2. 输入应用名称（如：OpenClaw Bot）
3. 点击 **Create**

### 1.3 创建 Bot

1. 左侧菜单选择 **Bot**
2. 点击 **Add Bot**
3. 点击 **Yes, do it!**

### 1.4 获取 Token

1. 在 Bot 页面
2. 点击 **Reset Token**（如果是新 Bot）
3. 复制 Token（只显示一次！）

### 1.5 配置 Bot 权限

在 Bot 页面，启用以下 **Privileged Gateway Intents**：
- ✅ Message Content Intent
- ✅ Server Members Intent（可选）

保存更改。

---

## 步骤 2：邀请 Bot 到服务器

### 2.1 生成邀请链接

1. 左侧菜单选择 **OAuth2** → **URL Generator**
2. **Scopes**: 勾选 `bot`
3. **Bot Permissions**: 勾选以下权限
   - Read Messages/View Channels
   - Send Messages
   - Read Message History
   - Mention Everyone（可选）

### 2.2 邀请 Bot

1. 复制生成的 URL
2. 在浏览器中打开
3. 选择服务器
4. 点击 **Authorize**

---

## 步骤 3：配置 OpenClaw

编辑 `~/.openclaw/openclaw.json`：

```json5
{
  channels: {
    discord: {
      enabled: true,
      botToken: "YOUR_DISCORD_BOT_TOKEN",

      // 私信策略
      dmPolicy: "pairing",

      // 服务器配置
      guilds: {
        "*": {
          requireMention: true,
        },
      },
    },
  },
}
```

### 配置说明

| 参数 | 说明 |
|------|------|
| `botToken` | Discord Bot Token |
| `dmPolicy` | 私信访问策略 |
| `requireMention` | 是否需要 @提及 |

---

## 步骤 4：重启 Gateway

```bash
openclaw gateway restart
```

---

## 步骤 5：配对管理

### 5.1 发送消息给 Bot

在 Discord 中：
1. 找到你的 Bot
2. 发送私信

### 5.2 批准配对

Bot 会回复配对码：

```bash
# 查看待配对请求
openclaw pairing list discord

# 批准
openclaw pairing approve discord <CODE>
```

---

## 验证连接

```bash
openclaw channels status
```

看到 `discord: connected` 即表示成功。

---

## 服务器配置

### 配置特定服务器

```json5
{
  channels: {
    discord: {
      guilds: {
        // 特定服务器配置
        "123456789012345678": {
          requireMention: false,
          allowedChannels: ["general", "bot-commands"],
        },

        // 默认配置
        "*": {
          requireMention: true,
        },
      },
    },
  },
}
```

### 限制可用频道

```json5
{
  channels: {
    discord: {
      guilds: {
        "*": {
          allowedChannels: ["bot-commands", "ai-chat"],
        },
      },
    },
  },
}
```

---

## 常见问题

### Q: Bot 显示离线？

**A:**
1. 确认 `botToken` 正确
2. 检查 Gateway 日志：`openclaw logs --follow`
3. 重启 Gateway

### Q: Bot 不回复消息？

**A:**
1. 确认已完成配对
2. 检查 `dmPolicy` 设置
3. 确认 Bot 有 **Read Message History** 权限
4. 确认启用了 **Message Content Intent**

### Q: 群组中 Bot 无响应？

**A:**
1. 尝试 @提及 Bot
2. 检查 `requireMention` 设置
3. 确认 Bot 有 **Send Messages** 权限
4. 检查频道是否在 `allowedChannels` 白名单中

---

## 下一步

- [配置 WhatsApp](./whatsapp.md)
- [配置 Telegram](./telegram.md)
- [了解进阶功能](../advanced/README.md)
