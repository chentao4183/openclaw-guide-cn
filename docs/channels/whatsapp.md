# WhatsApp 配置指南

WhatsApp 是 OpenClaw 最受欢迎的渠道之一。

---

## 📋 配置流程

```
┌─────────────────────────────────────────────────────────────┐
│                  WhatsApp 配置流程                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1️⃣ 运行登录命令                                             │
│     └─ openclaw channels login --channel whatsapp           │
│                                                             │
│  2️⃣ 扫描二维码                                               │
│     └─ 用手机 WhatsApp 扫描终端显示的二维码                   │
│                                                             │
│  3️⃣ 配置访问控制                                             │
│     └─ 编辑 ~/.openclaw/openclaw.json                       │
│                                                             │
│  4️⃣ 测试对话                                                 │
│     └─ 给自己发消息测试                                       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🚀 快速配置（3 分钟）

### 步骤 1：登录 WhatsApp

```bash
openclaw channels login --channel whatsapp
```

**终端会显示一个二维码：**

```
╔══════════════════════════════════════════════════╗
║                                                  ║
║    ████████████████████████████████████████      ║
║    ██                                    ██      ║
║    ██  ▓▓▓▓▓▓  ▓▓▓▓  ▓▓▓▓▓▓▓▓  ▓▓▓▓▓▓  ██      ║
║    ██  ▓▓▓▓▓▓  ▓▓▓▓  ▓▓▓▓▓▓▓▓  ▓▓▓▓▓▓  ██      ║
║    ██  ▓▓  ▓▓  ▓▓▓▓  ▓▓  ▓▓  ▓▓  ▓▓    ██      ║
║    ██  ▓▓  ▓▓  ▓▓▓▓  ▓▓  ▓▓  ▓▓  ▓▓    ██      ║
║    ██  ▓▓▓▓▓▓  ▓▓    ▓▓  ▓▓  ▓▓▓▓▓▓  ██      ║
║    ██                                    ██      ║
║    ████████████████████████████████████████      ║
║                                                  ║
║         请用 WhatsApp 扫描此二维码               ║
║                                                  ║
╚══════════════════════════════════════════════════╝
```

---

### 步骤 2：扫码绑定

**在手机上操作：**

```
1. 打开 WhatsApp
   └─ 确保 WhatsApp 是最新版本

2. 进入设置
   └─ iOS: 右下角 "设置"
   └─ Android: 右上角 "⋮" → "已关联的设备"

3. 点击 "关联设备"

4. 扫描终端显示的二维码
   └─ 将手机摄像头对准二维码

5. 等待连接
   └─ 看到 "已连接" 即成功
```

---

### 步骤 3：验证连接

```bash
openclaw channels status
```

**预期输出：**

```
Channel      Status      Account
─────────────────────────────────
whatsapp     connected   +86138****1234
telegram     not_configured
discord      not_configured
```

---

## ⚙️ 访问控制配置

编辑配置文件 `~/.openclaw/openclaw.json`：

```json5
{
  channels: {
    whatsapp: {
      enabled: true,

      // 私信策略
      dmPolicy: "pairing",  // pairing | allowlist | open | disabled

      // 允许的用户（电话号码格式）
      allowFrom: [
        "+8613800138000",  // 用户 A
        "+8613900139000",  // 用户 B
      ],

      // 群组策略
      groupPolicy: "allowlist",

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

---

## 📊 访问控制策略详解

### dmPolicy（私信策略）

| 策略 | 说明 | 适用场景 | 配置示例 |
|------|------|----------|----------|
| `pairing` | 未知用户需配对码批准 | 个人使用，安全 | `"dmPolicy": "pairing"` |
| `allowlist` | 只允许白名单用户 | 严格限制 | `"allowFrom": ["+86138****1234"]` |
| `open` | 允许所有人 | 公开服务 | `"allowFrom": ["*"]` |
| `disabled` | 禁止所有私信 | 只用群组 | `"dmPolicy": "disabled"` |

### 配置示例

**场景 1：个人使用（推荐）**

```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "pairing",  // 新用户需要批准
      groupPolicy: "allowlist",
    },
  },
}
```

**场景 2：家庭/小团队**

```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "allowlist",
      allowFrom: [
        "+86138****0001",  // 成员 1
        "+86138****0002",  // 成员 2
        "+86138****0003",  // 成员 3
      ],
    },
  },
}
```

**场景 3：公开服务**

```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "open",
      allowFrom: ["*"],  // 允许所有人
    },
  },
}
```

---

## 🔐 配对管理

### 查看待配对请求

当新用户给你发消息时，需要批准配对：

```bash
openclaw pairing list whatsapp
```

**输出示例：**

```
Channel      Code      Phone         Created
─────────────────────────────────────────────
whatsapp     ABC123    +86138****0001  2 min ago
whatsapp     XYZ789    +86139****0002  5 min ago
```

### 批准配对

```bash
openclaw pairing approve whatsapp ABC123
```

**输出：**

```
✓ Pairing approved for +86138****0001
The user can now interact with the bot
```

### 拒绝配对

```bash
openclaw pairing reject whatsapp XYZ789
```

---

## 💬 使用示例

### 私信对话

```
用户: 你好
AI:   你好！我是你的 AI 助手，有什么可以帮助你的吗？

用户: 帮我查一下北京明天的天气
AI:   好的，我来查询北京明天的天气...
      明天北京晴，温度 15-25°C，适合外出。
```

### 群组对话

```
用户: @bot 今天有什么安排？
AI:   你好！今天的主要安排有：
      1. 10:00 - 项目会议
      2. 14:00 - 客户拜访
      3. 16:00 - 团队复盘
```

---

## ❓ 常见问题

### Q: 扫码后显示 "连接失败"？

**解决方案：**

1. **检查 WhatsApp 版本**
   ```
   确保 WhatsApp 是最新版本
   iOS: App Store → 更新
   Android: Play Store → 更新
   ```

2. **检查网络连接**
   ```bash
   ping web.whatsapp.com
   ```

3. **重启 Gateway**
   ```bash
   openclaw gateway restart
   ```

4. **重新登录**
   ```bash
   openclaw channels logout --channel whatsapp
   openclaw channels login --channel whatsapp
   ```

### Q: 收不到消息？

**检查清单：**

```bash
# 1. 确认连接状态
openclaw channels status

# 2. 检查 allowFrom 配置
cat ~/.openclaw/openclaw.json | grep -A 5 "whatsapp"

# 3. 查看日志
openclaw logs --follow

# 4. 确认配对状态
openclaw pairing list whatsapp
```

### Q: 群组中机器人不响应？

**解决方案：**

1. **检查 groupPolicy**
   ```json5
   {
     channels: {
       whatsapp: {
         groupPolicy: "allowlist",  // 或 "open"
       },
     },
   }
   ```

2. **尝试 @提及**
   ```
   @bot 你好
   ```

3. **检查 requireMention 设置**
   ```json5
   {
     groups: {
       "*": {
         requireMention: false,  // 改为 false 试试
       },
     },
   }
   ```

### Q: 如何查看已连接的设备？

**在手机 WhatsApp 中：**

```
设置 → 已关联的设备 → 查看所有设备
```

### Q: 如何断开连接？

```bash
openclaw channels logout --channel whatsapp
```

---

## 🔗 下一步

- [配置 Telegram](/docs/channels/telegram)
- [配置 Discord](/docs/channels/discord)
- [了解进阶功能](/docs/advanced/)
- [故障排查](/docs/troubleshooting)
