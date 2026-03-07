# Discord 配置指南

将 OpenClaw 连接到 Discord 服务器。

---

## 📋 配置流程

```
┌─────────────────────────────────────────────────────────────┐
│                  Discord 配置流程                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1️⃣ 创建 Discord 应用                                        │
│     └─ 访问 Discord Developer Portal                         │
│                                                             │
│  2️⃣ 创建 Bot 用户                                            │
│     └─ 在应用中添加 Bot 并获取 Token                          │
│                                                             │
│  3️⃣ 配置权限                                                 │
│     └─ 启用 Message Content Intent                           │
│                                                             │
│  4️⃣ 邀请 Bot 到服务器                                        │
│     └─ 生成邀请链接并授权                                     │
│                                                             │
│  5️⃣ 配置 OpenClaw                                            │
│     └─ 填入 Token 并重启                                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🤖 步骤 1：创建 Discord Bot

### 1.1 访问开发者门户

**打开浏览器访问：**

👉 https://discord.com/developers/applications

```
┌────────────────────────────────────────────────┐
│  Discord Developer Portal                      │
│                                                │
│  [New Application]                             │
│                                                │
│  Your Applications:                            │
│  ┌──────────────────────────────────────────┐  │
│  │ + Create New Application                  │  │
│  └──────────────────────────────────────────┘  │
│                                                │
└────────────────────────────────────────────────┘
```

### 1.2 创建应用

**点击 "New Application"**

```
┌────────────────────────────────────────┐
│  Create an application                 │
│                                        │
│  App Name                              │
│  ┌──────────────────────────────────┐  │
│  │ OpenClaw Bot                     │  │
│  └──────────────────────────────────┘  │
│                                        │
│  By clicking "Create", you agree to    │
│  the Discord Developer Terms of Service│
│                                        │
│  [Cancel]              [Create]        │
└────────────────────────────────────────┘
```

### 1.3 创建 Bot

**导航到 Bot 页面**

```
左侧菜单：
├── General Information
├── Bot                    ← 点击这里
├── OAuth2
└── ...
```

**点击 "Add Bot"**

```
┌────────────────────────────────────────┐
│  Build-A-Bot                           │
│                                        │
│  Do you want to add a bot to this app? │
│                                        │
│  [Yes, do it!]                         │
└────────────────────────────────────────┘
```

### 1.4 获取 Token

**点击 "Reset Token"（新 Bot）**

```
┌────────────────────────────────────────┐
│  Build-A-Bot                           │
│                                        │
│  Token                                 │
│  ┌──────────────────────────────────┐  │
│  •••••••••••••••••••••••••••••••••  │  │
│  └──────────────────────────────────┘  │
│  [Reset Token]                         │
│                                        │
└────────────────────────────────────────┘
```

**⚠️ 立即复制并保存 Token！**

```
Token 格式：[你的 Discord Bot Token]
示例结构：数字.字母数字组合.字母数字组合
```

---

## ⚙️ 步骤 2：配置 Bot 权限

### 2.1 启用 Privileged Gateway Intents

**在 Bot 页面向下滚动：**

```
┌────────────────────────────────────────┐
│  Privileged Gateway Intents            │
│                                        │
│  ✅ PRESENCE INTENT                    │
│     Receive presence update events     │
│                                        │
│  ✅ SERVER MEMBERS INTENT              │
│     Receive server member events       │
│                                        │
│  ✅ MESSAGE CONTENT INTENT   ← 必需！   │
│     Receive message content            │
│                                        │
│  [Save Changes]                        │
└────────────────────────────────────────┘
```

**⚠️ 必须启用 "Message Content Intent"，否则 Bot 无法读取消息！**

---

## 🔗 步骤 3：邀请 Bot 到服务器

### 3.1 生成邀请链接

**导航到 OAuth2 → URL Generator**

```
左侧菜单：
├── OAuth2
│   ├── General
│   └── URL Generator     ← 点击这里
```

**配置 Scopes 和 Permissions：**

```
┌────────────────────────────────────────┐
│  Scopes                                │
│  ☑ bot                                 │
│                                        │
│  Bot Permissions                       │
│  ☑ Read Messages/View Channels         │
│  ☑ Send Messages                       │
│  ☑ Read Message History                │
│  ☑ Mention Everyone (可选)             │
│                                        │
│  Generated URL                         │
│  ┌──────────────────────────────────┐  │
│  │ https://discord.com/api/oauth2/  │  │
│  │ authorize?client_id=xxx...       │  │
│  └──────────────────────────────────┘  │
│  [Copy]                                │
└────────────────────────────────────────┘
```

### 3.2 邀请 Bot

1. 复制生成的 URL
2. 在浏览器中打开
3. 选择你的服务器
4. 点击 **Authorize**
5. 完成人机验证

```
┌────────────────────────────────────────┐
│  Add to Server                         │
│                                        │
│  Select a server                       │
│  ┌──────────────────────────────────┐  │
│  │ My Discord Server          ▼     │  │
│  └──────────────────────────────────┘  │
│                                        │
│  Permissions                           │
│  ✓ Read Messages                       │
│  ✓ Send Messages                       │
│  ✓ Read Message History                │
│                                        │
│  [Authorize]                           │
└────────────────────────────────────────┘
```

---

## 📝 步骤 4：配置 OpenClaw

### 编辑配置文件

**位置：** `~/.openclaw/openclaw.json`

```json5
{
  channels: {
    discord: {
      enabled: true,
      
      // 🔑 粘贴你的 Bot Token
      botToken: "YOUR_DISCORD_BOT_TOKEN_HERE",

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

### 重启 Gateway

```bash
openclaw gateway restart
```

---

## 🔗 步骤 5：配对管理

### 5.1 发送消息给 Bot

**在 Discord 中：**

1. 找到你的 Bot（在服务器成员列表中）
2. 右键点击 → **Message**
3. 发送任意消息

### 5.2 批准配对

Bot 会回复配对码：

```bash
# 查看待配对请求
openclaw pairing list discord

# 批准
openclaw pairing approve discord <CODE>
```

---

## ✅ 验证连接

```bash
openclaw channels status
```

**预期输出：**

```
Channel      Status      Account
────────────────────────────────────
discord      connected   OpenClaw Bot#1234
telegram     not_configured
whatsapp     not_configured
```

---

## 🎯 服务器配置

### 限制可用频道

```json5
{
  channels: {
    discord: {
      guilds: {
        "*": {
          requireMention: true,
          // 只允许这些频道
          allowedChannels: [
            "bot-commands",
            "ai-chat",
            "123456789012345678",  // 也可以用频道 ID
          ],
        },
      },
    },
  },
}
```

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

---

## 💬 使用示例

### 私信对话

```
你: 你好
Bot: 你好！我是 OpenClaw AI 助手。有什么可以帮助你的吗？

你: 帮我生成一段 Python 代码
Bot: 好的，请告诉我你需要什么功能的代码？
```

### 服务器频道对话

```
用户 A: @OpenClaw Bot 今天有什么新闻？
Bot:    我来搜索最新的新闻...
        
        📰 今日头条：
        1. [标题 1]
        2. [标题 2]
        3. [标题 3]
```

---

## ❓ 常见问题

### Q: Bot 显示离线？

**检查清单：**

```bash
# 1. 确认 Token 正确
cat ~/.openclaw/openclaw.json | grep botToken

# 2. 确认 Gateway 运行中
openclaw gateway status

# 3. 查看日志
openclaw logs --follow | grep discord

# 4. 重启 Gateway
openclaw gateway restart
```

### Q: Bot 不回复消息？

**可能原因：**

1. **未启用 Message Content Intent**
   ```
   Discord Developer Portal → Bot → 
   启用 "Message Content Intent"
   ```

2. **权限不足**
   ```
   确认 Bot 有以下权限：
   ✓ Read Messages/View Channels
   ✓ Send Messages
   ✓ Read Message History
   ```

3. **未完成配对**
   ```bash
   openclaw pairing list discord
   ```

### Q: 群组中 Bot 无响应？

**解决方案：**

1. **尝试 @提及**
   ```
   @OpenClaw Bot 你好
   ```

2. **检查 requireMention 设置**
   ```json5
   {
     guilds: {
       "*": {
         requireMention: false,  // 临时关闭测试
       },
     },
   }
   ```

3. **检查频道白名单**
   ```json5
   {
     guilds: {
       "*": {
         allowedChannels: null,  // 清空白名单测试
       },
     },
   }
   ```

### Q: 如何获取服务器 ID？

1. **开启开发者模式**
   ```
   Discord 设置 → 高级 → 开启开发者模式
   ```

2. **右键点击服务器名称**
   ```
   右键 → 复制 ID
   ```

### Q: 如何获取频道 ID？

1. **开启开发者模式**
2. **右键点击频道名称**
   ```
   右键 → 复制 ID
   ```

### Q: Token 泄露了怎么办？

**立即重新生成：**

```
1. Discord Developer Portal → Bot
2. 点击 "Reset Token"
3. 用新 Token 更新配置文件
4. 重启 Gateway
```

---

## 🔗 下一步

- [配置 WhatsApp](/docs/channels/whatsapp)
- [配置 Telegram](/docs/channels/telegram)
- [了解进阶功能](/docs/advanced/)
- [故障排查](/docs/troubleshooting)
