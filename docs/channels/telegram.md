# Telegram 配置指南

Telegram 配置最简单，只需创建一个 Bot 即可。

---

## 📋 配置流程

```
┌─────────────────────────────────────────────────────────────┐
│                  Telegram 配置流程                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1️⃣ 创建 Telegram Bot                                        │
│     └─ 在 @BotFather 中使用 /newbot 命令                     │
│                                                             │
│  2️⃣ 获取 Bot Token                                           │
│     └─ BotFather 会返回一个 Token                            │
│                                                             │
│  3️⃣ 配置 OpenClaw                                            │
│     └─ 将 Token 填入配置文件                                  │
│                                                             │
│  4️⃣ 启动并配对                                               │
│     └─ 给 Bot 发消息并批准配对                                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🤖 步骤 1：创建 Telegram Bot

### 1.1 找到 BotFather

**在 Telegram 中搜索：** `@BotFather`

```
┌────────────────────────────────────────┐
│  搜索框                                 │
│  ┌──────────────────────────────────┐  │
│  │ @BotFather                 🔍    │  │
│  └──────────────────────────────────┘  │
│                                        │
│  结果：                                 │
│  ✓ @BotFather (Official Bot)          │
│                                        │
└────────────────────────────────────────┘
```

### 1.2 创建新 Bot

**发送命令：**

```
/newbot
```

**BotFather 会引导你：**

```
BotFather: Alright, a new bot. How are we going to call it?
           Please choose a name for your bot.

你: OpenClaw Assistant

BotFather: Good. Now let's choose a username for your bot.
           It must end in `bot`. Like this, for example:
           TetrisBot or tetris_bot.

你: my_openclaw_bot

BotFather: Done! Congratulations on your new bot...
           
           Use this token to access the HTTP API:
           7123456789:AAHxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
           
           Keep your token secure...
```

### 1.3 保存 Token

**⚠️ 重要：立即复制并保存 Token！**

```
Token 格式：123456789:ABCdefGHIjklMNOpqrsTUVwxyz
           ^^^^^^^^ ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
           Bot ID   Token 字符串
```

---

## ⚙️ 步骤 2：配置 OpenClaw

### 编辑配置文件

**位置：** `~/.openclaw/openclaw.json`

```json5
{
  channels: {
    telegram: {
      enabled: true,
      
      // 🔑 粘贴你的 Bot Token
      botToken: "7123456789:AAHxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",

      // 私信策略
      dmPolicy: "pairing",

      // 群组配置
      groups: {
        "*": {
          requireMention: true,  // 需要 @提及 才响应
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

## 🔗 步骤 3：配对管理

### 3.1 开始对话

**在 Telegram 中找到你的 Bot：**

1. 搜索你的 Bot 用户名（如 `@my_openclaw_bot`）
2. 点击 **Start** 或发送 `/start`

### 3.2 获取配对码

Bot 会自动回复：

```
🤖 OpenClaw Bot

欢迎使用 OpenClaw！

你的配对码是：ABC123

请在你的终端中运行以下命令批准配对：
openclaw pairing approve telegram ABC123
```

### 3.3 批准配对

```bash
# 查看所有待配对请求
openclaw pairing list telegram

# 批准配对
openclaw pairing approve telegram ABC123
```

**输出：**

```
✓ Pairing approved for telegram user @your_username
You can now interact with the bot
```

---

## ✅ 验证连接

```bash
openclaw channels status
```

**预期输出：**

```
Channel      Status      Account
───────────────────────────────────
telegram     connected   @my_openclaw_bot
whatsapp     not_configured
discord      not_configured
```

---

## 👥 群组配置

### 将 Bot 添加到群组

```
1. 进入群组设置
   └─ 点击群组名称 → 添加成员

2. 搜索你的 Bot
   └─ 输入 @my_openclaw_bot

3. 添加为管理员（推荐）
   └─ 管理员权限可确保 Bot 读取所有消息
```

### 群组策略配置

```json5
{
  channels: {
    telegram: {
      groups: {
        // 所有群组的默认配置
        "*": {
          requireMention: true,  // 需要 @Bot 才响应
          mentionPatterns: ["@my_openclaw_bot", "/ask"],
        },

        // 特定群组的配置
        "-1001234567890": {
          requireMention: false,  // 不需要 @提及
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
Bot: 你好！我是 OpenClaw AI 助手，有什么可以帮助你的吗？

你: 帮我翻译这段话成英文：你好世界
Bot: Hello World

你: /help
Bot: 可用命令：
     /status - 查看状态
     /reset  - 重置对话
     /help   - 显示帮助
```

### 群组对话

```
用户 A: @my_openclaw_bot 今天天气怎么样？
Bot:    我来查询天气信息...
        今天晴，温度 20-28°C，适合外出。

用户 B: /ask 什么是 OpenClaw？
Bot:    OpenClaw 是一个自托管网关，连接聊天应用到 AI 助手...
```

---

## 🎯 高级配置

### 自定义命令

**在 @BotFather 中设置：**

```
/setcommands

你: /setcommands
BotFather: Choose a bot to change the list of commands.

你: @my_openclaw_bot
BotFather: OK. Send me a list of commands for your bot...

你: start - 开始使用
     help - 显示帮助
     status - 查看状态
     reset - 重置对话
```

### Webhook 模式（高级）

如果需要使用 Webhook 而非轮询：

```json5
{
  channels: {
    telegram: {
      webhook: {
        enabled: true,
        url: "https://your-domain.com/telegram/webhook",
        port: 8443,
      },
    },
  },
}
```

---

## ❓ 常见问题

### Q: Bot 不回复消息？

**检查清单：**

```bash
# 1. 确认 Token 正确
cat ~/.openclaw/openclaw.json | grep botToken

# 2. 确认 Gateway 运行中
openclaw gateway status

# 3. 确认已完成配对
openclaw pairing list telegram

# 4. 查看日志
openclaw logs --follow | grep telegram
```

### Q: 群组中 Bot 无响应？

**解决方案：**

1. **确认 Bot 已加入群组**
   ```
   群组设置 → 成员列表 → 查找你的 Bot
   ```

2. **尝试 @提及**
   ```
   @my_openclaw_bot 你好
   ```

3. **检查 Bot 权限**
   ```
   确保 Bot 可以读取消息
   管理员权限 → 勾选 "阅读消息"
   ```

4. **检查 requireMention 设置**
   ```json5
   {
     groups: {
       "*": {
         requireMention: false,  // 临时关闭测试
       },
     },
   }
   ```

### Q: 如何获取群组 ID？

**方法 1：使用 @raw_data_bot**

```
1. 将 @raw_data_bot 添加到群组
2. 发送任意消息
3. 它会返回群组信息（包含 ID）
4. 移除 @raw_data_bot
```

**方法 2：查看日志**

```bash
# 在群组中发送消息，然后查看日志
openclaw logs --lines 50 | grep "chat_id"
```

### Q: Token 泄露了怎么办？

**立即重新生成：**

```
1. 在 Telegram 中找到 @BotFather
2. 发送 /mybots
3. 选择你的 Bot
4. 点击 "API Token" → "Revoke Token"
5. 用新 Token 更新配置文件
6. 重启 Gateway
```

### Q: 如何让 Bot 响应所有消息（不需要 @提及）？

```json5
{
  channels: {
    telegram: {
      groups: {
        "*": {
          requireMention: false,
        },
      },
    },
  },
}
```

---

## 🔗 下一步

- [配置 WhatsApp](/docs/channels/whatsapp)
- [配置 Discord](/docs/channels/discord)
- [了解进阶功能](/docs/advanced/)
- [故障排查](/docs/troubleshooting)
