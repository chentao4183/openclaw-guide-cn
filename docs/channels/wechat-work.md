# 企业微信配置指南

> 对接企业微信开放平台，支持消息收发、群机器人、事件回调与通讯录同步

![企业微信](https://img.shields.io/badge/企业微信-官方支持-blue)
![难度](https://img.shields.io/badge/难度-⭐⭐-yellow)

---

## 📋 准备工作

### 前置要求
- 企业微信管理员权限
- 企业微信企业 ID（CorpID）
- 应用 AgentId 和 Secret
- 公网可访问的服务器（用于接收回调）

### 获取必要信息

1. **获取企业 ID**
   - 登录 [企业微信管理后台](https://work.weixin.qq.com/)
   - 我的企业 → 企业信息 → 企业 ID

2. **创建应用**
   - 应用管理 → 自建应用 → 创建应用
   - 获取 AgentId 和 Secret

3. **设置回调 URL**
   - 应用详情 → 企业可信 IP → 设置
   - 回调 URL 格式：`https://your-domain.com/webhook/wechat-work`

---

## ⚙️ 配置 OpenClaw

### 1. 基础配置

编辑 `~/.openclaw/openclaw.json`：

```json5
{
  channels: {
    "wechat-work": {
      enabled: true,
      
      // 企业信息
      corpId: "your_corp_id",
      agentId: 1000001,
      secret: "your_agent_secret",
      
      // 消息策略
      dmPolicy: "pairing",  // pairing | allowlist | open
      allowFrom: ["user_id_1", "user_id_2"],
      
      // 群组配置
      groupPolicy: "allowlist",
      groups: {
        "*": {
          requireMention: true,  // 是否需要 @提及
        },
      },
      
      // 回调配置
      webhook: {
        enabled: true,
        path: "/webhook/wechat-work",
      },
    },
  },
}
```

### 2. 配置说明

#### dmPolicy（私聊策略）
- `pairing`：未知用户需一次性配对码批准（推荐）
- `allowlist`：仅允许 `allowFrom` 列表中的用户
- `open`：允许所有用户（需设置 `allowFrom: ["*"]`）

#### groupPolicy（群组策略）
- `allowlist`：仅允许配置的群组
- `open`：允许所有群组

---

## 🔐 企业微信登录

### 扫码配对

```bash
# 启动企业微信渠道
openclaw channels login --channel wechat-work

# 查看配对请求
openclaw pairing list wechat-work

# 批准配对
openclaw pairing approve wechat-work <CODE>
```

---

## 📨 消息接收与发送

### 接收消息

OpenClaw 会自动处理以下消息类型：
- 文本消息
- 图片消息
- 文件消息
- 位置消息
- 事件消息（进入应用、审批等）

### 发送消息

通过 AI 助手自动发送，或使用 API：

```bash
# 示例：通过命令行发送消息
curl -X POST http://localhost:18789/api/send \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "wechat-work",
    "to": "user_id",
    "message": "你好，这是测试消息"
  }'
```

---

## 🤖 群机器人配置

### 1. 创建群机器人
- 在企业微信群中 → 群设置 → 群机器人 → 添加机器人
- 获取 Webhook URL

### 2. 配置群机器人

```json5
{
  channels: {
    "wechat-work": {
      groups: {
        "group_id_1": {
          webhook: "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=xxx",
          requireMention: false,
        },
      },
    },
  },
}
```

### 3. 使用群机器人

```bash
# AI 助手会自动识别并使用群机器人
# 用户在群中 @机器人 即可触发
```

---

## 🔄 事件回调配置

### 支持的事件类型
- `enter_agent`：用户进入应用
- `approval`：审批状态变化
- `contact_sync`：通讯录变更
- `external_contact`：外部联系人变更

### 配置事件订阅

```json5
{
  channels: {
    "wechat-work": {
      events: {
        enabled: true,
        callback: {
          url: "https://your-domain.com/webhook/wechat-work",
          token: "your_token",
          encodingAESKey: "your_encoding_aes_key",
        },
      },
    },
  },
}
```

---

## 📚 通讯录同步

### 启用通讯录同步
1. 管理工具 → 通讯录同步 → 开启
2. 获取 Secret

### 配置通讯录权限

```json5
{
  channels: {
    "wechat-work": {
      contactSync: {
        enabled: true,
        secret: "your_contact_secret",
        autoSync: true,  // 自动同步
        syncInterval: 3600,  // 同步间隔（秒）
      },
    },
  },
}
```

---

## 🎯 高级功能

### 1. 多应用支持

```json5
{
  channels: {
    "wechat-work-app1": {
      agentId: 1000001,
      secret: "secret_1",
    },
    "wechat-work-app2": {
      agentId: 1000002,
      secret: "secret_2",
    },
  },
}
```

### 2. 消息加密

企业微信要求消息加密，OpenClaw 会自动处理：
- 消息解密（接收）
- 消息加密（发送）
- 签名验证

### 3. 敏感信息过滤

```json5
{
  channels: {
    "wechat-work": {
      contentFilter: {
        enabled: true,
        patterns: ["password", "token", "secret"],
        action: "mask",  // mask | reject
      },
    },
  },
}
```

---

## 🔧 常见问题

### 1. 回调验证失败
**原因**：URL 不可达或 Token 不匹配  
**解决**：
```bash
# 检查回调 URL 是否可访问
curl https://your-domain.com/webhook/wechat-work

# 确认 Token 配置正确
openclaw doctor --channel wechat-work
```

### 2. 消息发送失败
**原因**：应用权限不足或用户 ID 错误  
**解决**：
```bash
# 检查应用权限
openclaw channels status wechat-work

# 验证用户 ID
openclaw wechat-work users list
```

### 3. 群机器人不响应
**原因**：未配置 Webhook 或 `requireMention` 设置错误  
**解决**：
```json5
{
  channels: {
    "wechat-work": {
      groups: {
        "group_id": {
          webhook: "your_webhook_url",
          requireMention: false,  // 改为 false 测试
        },
      },
    },
  },
}
```

---

## 📖 相关资源

- [企业微信官方文档](https://developer.work.weixin.qq.com/document/)
- [企业微信 API 参考](https://developer.work.weixin.qq.com/document/path/90664)
- [OpenClaw 官方文档](https://docs.openclaw.ai/)

---

## 💡 最佳实践

1. **生产环境建议**
   - 使用 HTTPS 回调 URL
   - 启用消息加密
   - 定期同步通讯录

2. **安全建议**
   - 使用 `allowlist` 限制访问用户
   - 定期更换 Secret
   - 监控异常消息

3. **性能优化**
   - 合理设置 `syncInterval`
   - 使用群机器人减少 API 调用
   - 启用消息缓存

---

**下一步** → [飞书配置指南](feishu.md)
