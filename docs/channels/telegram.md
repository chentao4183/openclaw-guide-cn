# Telegram 配置指南

Telegram 配置最简单，只需创建一个 Bot 即可。

---

## 前置要求

- 已安装 OpenClaw
- 有 Telegram 账号

---

## 步骤 1：创建 Telegram Bot

### 1.1 找到 BotFather

1. 打开 Telegram
2. 搜索 `@BotFather`
3. 开始对话

### 1.2 创建新 Bot

发送命令：
```
/newbot
```

按提示操作：
1. 输入 Bot 名称（显示名，如：My OpenClaw Bot）
2. 输入 Bot 用户名（必须以 `bot` 结尾，如：`my_openclaw_bot`）

### 1.3 保存 Token

BotFather 会返回类似这样的内容：
```
Done! Congratulations on your new bot...
Use this token to access the HTTP API:
7123456789:AAHxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

Keep your token secure...
```

**复制并保存这个 Token**，下一步需要用到。

---

## 步骤 2：配置 OpenClaw

编辑 `~/.openclaw/openclaw.json`：

```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "7123456789:AAHxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",

      // 私信策略
      dmPolicy: "pairing",

      // 群组配置
      groups: {
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
| `botToken` | BotFather 提供的 Token |
| `dmPolicy` | 私信访问策略 |
| `requireMention` | 群组中是否需要 @提及 |

---

## 步骤 3：重启 Gateway

```bash
openclaw gateway restart
```

---

## 步骤 4：配对管理

### 4.1 开始对话

1. 在 Telegram 中找到你的 Bot
2. 点击 **Start** 或发送任意消息

### 4.2 获取配对码

Bot 会回复一个配对码（如：`ABC123`）

### 4.3 批准配对

```bash
# 查看待配对请求
openclaw pairing list telegram

# 批准
openclaw pairing approve telegram ABC123
```

---

## 验证连接

```bash
openclaw channels status
```

看到 `telegram: connected` 即表示成功。

---

## 群组配置

### 将 Bot 添加到群组

1. 进入群组设置
2. 点击 **添加成员**
3. 搜索你的 Bot 用户名
4. 添加为管理员（推荐，否则可能无法读取所有消息）

### 群组策略配置

```json5
{
  channels: {
    telegram: {
      groups: {
        "*": {
          requireMention: true,  // 需要 @Bot 才响应
          mentionPatterns: ["@my_openclaw_bot", "/ask"],
        },
      },
    },
  },
}
```

---

## 常见问题

### Q: Bot 不回复消息？

**A:**
1. 确认 `botToken` 正确
2. 检查 `dmPolicy` 设置
3. 确认已完成配对流程
4. 查看 Gateway 日志

### Q: 群组中 Bot 无响应？

**A:**
1. 确认 Bot 已加入群组
2. 尝试 @提及 Bot
3. 检查 `requireMention` 设置
4. 确认 Bot 有管理员权限

### Q: 如何获取群组 ID？

**A:**
1. 将 `@raw_data_bot` 添加到群组
2. 发送任意消息
3. 它会返回群组信息（包含 ID）

---

## 下一步

- [配置 WhatsApp](./whatsapp.md)
- [配置 Discord](./discord.md)
- [了解进阶功能](../advanced/README.md)
