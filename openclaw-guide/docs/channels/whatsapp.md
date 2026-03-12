# WhatsApp 连接指南

> 把 AI 助手接到 WhatsApp 上

---

## 📋 准备工作

- ✅ OpenClaw 已安装并运行
- ✅ 一个 WhatsApp 账号（建议用独立号码）
- ✅ 手机能扫码

---

## 🔧 配置步骤

### 第一步：编辑配置文件

打开 `~/.openclaw/openclaw.json`：

```json5
{
  channels: {
    whatsapp: {
      // DM 策略：pairing（需配对） | allowlist（白名单） | open（所有人）
      dmPolicy: "pairing",
      
      // 允许的手机号（E.164 格式）
      allowFrom: ["+15551234567"],
      
      // 群组策略
      groupPolicy: "allowlist",
      groupAllowFrom: ["+15551234567"],
    },
  },
}
```

**字段说明：**
- `dmPolicy: "pairing"` - 首次发消息需配对码批准
- `allowFrom` - 白名单手机号
- `groupPolicy` - 群组访问控制

---

### 第二步：扫码登录

```bash
openclaw channels login --channel whatsapp
```

**会看到二维码：**

```
█▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀█
█  ▄▄▄▄▄ █▀▄ █▄▄█ ▄▄▄▄▄ █  █
█  █   █ █▄▀  ▄ █ █   █ █▄▀▄█
█  █▄▄▄█ █▄ █▀▀█ █▄▄▄█ █  ▀█
...
```

**用手机扫码：**
1. 打开 WhatsApp
2. 设置 → 已关联的设备 → 关联设备
3. 扫描二维码

---

### 第三步：测试发送

用另一个手机给这个 WhatsApp 号发消息：

```
你好
```

**会收到配对码：**
```
Your pairing code is: ABC123
Approve it on your gateway.
```

---

### 第四步：批准配对

```bash
# 查看配对请求
openclaw pairing list whatsapp

# 批准
openclaw pairing approve whatsapp ABC123
```

**成功后**会收到：
```
✅ Pairing approved! You can now chat with me.
```

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
    whatsapp: {
      dmPolicy: "pairing",
      allowFrom: ["+15551234567", "+15559876543"],
      
      groupPolicy: "allowlist",
      groupAllowFrom: ["+15551234567"],
      
      // 群组配置（可选）
      groups: {
        "*": { requireMention: true },  // 所有群需 @ 提及
      },
      
      // 媒体设置（可选）
      mediaMaxMb: 50,
      sendReadReceipts: true,
    },
  },
}
```

---

## 🔒 安全建议

### ✅ 推荐配置

```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "pairing",  // 首次需配对
      allowFrom: ["+你的手机号"],
      groupPolicy: "allowlist",
    },
  },
}
```

### ⚠️ 谨慎使用

```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "open",  // 允许所有人（危险）
      allowFrom: ["*"],
    },
  },
}
```

---

## ❓ 常见问题

### 扫码后没反应

```bash
# 检查连接状态
openclaw channels status

# 重新登录
openclaw channels logout --channel whatsapp
openclaw channels login --channel whatsapp
```

### 收不到配对码

- 检查 `allowFrom` 是否包含你的手机号
- 确认 `dmPolicy` 不是 `disabled`
- 查看日志：`openclaw logs --follow`

### 群消息无响应

- 确认群在 `groups` 白名单中
- 需要 @ 提及机器人
- 检查 `groupPolicy` 设置

### 重复收到配对请求

```bash
# 清除配对缓存
rm ~/.openclaw/credentials/whatsapp-allowFrom.json

# 重启
openclaw gateway restart
```

---

## 📱 多账号支持

如果有多个 WhatsApp 号码：

```json5
{
  channels: {
    whatsapp: {
      accounts: {
        personal: {
          dmPolicy: "allowlist",
          allowFrom: ["+15551111111"],
        },
        work: {
          dmPolicy: "allowlist",
          allowFrom: ["+15552222222"],
        },
      },
    },
  },
}
```

```bash
# 登录不同账号
openclaw channels login --channel whatsapp --account personal
openclaw channels login --channel whatsapp --account work
```

---

## 💡 高级功能

### 自定义回复前缀

```json5
{
  channels: {
    whatsapp: {
      responsePrefix: "[AI Bot]",  // 消息前加前缀
    },
  },
}
```

### 群组特定配置

```json5
{
  channels: {
    whatsapp: {
      groups: {
        "1203630...@g.us": {
          requireMention: false,  // 这个群不需要 @
          groupPolicy: "open",
        },
      },
    },
  },
}
```

---

## 📚 相关文档

- [Telegram 连接](./telegram.md)
- [Discord 连接](./discord.md)
- [安全配置](../security.md)
- [配置参考](../configuration.md)

---

_预计耗时：3 分钟_
