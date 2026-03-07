# WhatsApp 配置指南

WhatsApp 是 OpenClaw 最受欢迎的渠道之一。

---

## 前置要求

- 已安装 OpenClaw
- 有可用的 WhatsApp 账号（手机端）
- 手机能扫码

---

## 快速配置

### 1. 登录 WhatsApp

```bash
openclaw channels login --channel whatsapp
```

### 2. 扫码绑定

命令执行后会显示一个二维码：
1. 打开手机 WhatsApp
2. 进入 **设置** → **已关联的设备**
3. 点击 **关联设备**
4. 扫描终端显示的二维码

### 3. 验证连接

```bash
openclaw channels status
```

看到 `whatsapp: connected` 即表示成功。

---

## 访问控制配置

编辑 `~/.openclaw/openclaw.json`：

```json5
{
  channels: {
    whatsapp: {
      enabled: true,

      // 私信策略
      dmPolicy: "pairing",  // pairing | allowlist | open | disabled

      // 允许的用户（电话号码格式）
      allowFrom: ["+8613800138000"],

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

### 配置说明

| 参数 | 说明 |
|------|------|
| `dmPolicy` | 私信访问策略 |
| `allowFrom` | 白名单用户（电话号码） |
| `groupPolicy` | 群组访问策略 |
| `requireMention` | 群组中是否需要 @提及 |

---

## 配对管理

### 查看待配对请求

```bash
openclaw pairing list whatsapp
```

### 批准配对

```bash
openclaw pairing approve whatsapp <CODE>
```

### 拒绝配对

```bash
openclaw pairing reject whatsapp <CODE>
```

---

## 常见问题

### Q: 扫码后显示 "连接失败"？

**A:**
1. 确保手机 WhatsApp 版本是最新的
2. 检查网络连接
3. 重启 Gateway：`openclaw gateway restart`

### Q: 收不到消息？

**A:**
1. 检查 `allowFrom` 配置是否正确
2. 运行 `openclaw channels status` 确认连接状态
3. 查看 Gateway 日志：`openclaw logs --follow`

### Q: 群组中机器人不响应？

**A:**
1. 确认 `groupPolicy` 允许该群组
2. 尝试 @提及 机器人
3. 检查 `requireMention` 设置

---

## 下一步

- [了解进阶配置](../advanced/configuration.md)
- [配置 Telegram](./telegram.md)
- [探索 Skills](../advanced/skills.md)
