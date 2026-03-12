# 飞书配置指南

> 对接飞书开放平台，支持消息收发、事件订阅与交互卡片，用于流程触发与表单式交互

![飞书](https://img.shields.io/badge/飞书-Lark-blue)
![难度](https://img.shields.io/badge/难度-⭐⭐-yellow)

---

## 📋 准备工作

### 前置要求
- 飞书管理员权限
- 飞书应用 App ID 和 App Secret
- 公网可访问的服务器（用于接收事件回调）

### 获取必要信息

1. **创建飞书应用**
   - 登录 [飞书开放平台](https://open.feishu.cn/)
   - 开发者后台 → 创建企业自建应用
   - 获取 App ID 和 App Secret

2. **配置权限**
   - 应用详情 → 权限管理
   - 添加必要权限：
     - `contact:user.base:readonly`（获取用户基本信息）
     - `im:message`（消息相关）
     - `im:message:send_as_bot`（以应用身份发消息）

3. **配置事件订阅**
   - 应用详情 → 事件订阅 → 配置请求网址
   - URL 格式：`https://your-domain.com/webhook/feishu`

---

## ⚙️ 配置 OpenClaw

### 1. 基础配置

编辑 `~/.openclaw/openclaw.json`：

```json5
{
  channels: {
    "feishu": {
      enabled: true,
      
      // 应用信息
      appId: "cli_xxxxxxxxxx",
      appSecret: "your_app_secret",
      
      // 消息策略
      dmPolicy: "pairing",  // pairing | allowlist | open
      allowFrom: ["ou_xxxxxx"],  // Open ID
      
      // 群组配置
      groupPolicy: "allowlist",
      groups: {
        "*": {
          requireMention: true,
        },
      },
      
      // 事件订阅
      event: {
        enabled: true,
        path: "/webhook/feishu",
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

## 🔐 飞书登录

### 扫码配对

```bash
# 启动飞书渠道
openclaw channels login --channel feishu

# 查看配对请求
openclaw pairing list feishu

# 批准配对
openclaw pairing approve feishu <CODE>
```

---

## 📨 消息接收与发送

### 接收消息

OpenClaw 会自动处理以下消息类型：
- 文本消息
- 富文本消息
- 图片消息
- 文件消息
- 语音消息

### 发送消息

```bash
# 通过 API 发送消息
curl -X POST http://localhost:18789/api/send \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "feishu",
    "to": "ou_xxxxxx",
    "message": "你好，这是飞书测试消息"
  }'
```

---

## 🎴 交互卡片

飞书的交互卡片是最强大的功能之一，支持丰富的 UI 和用户交互。

### 1. 创建交互卡片

```json5
{
  "type": "template",
  "data": {
    "template_id": "your_template_id",
    "template_variable": {
      "title": "审批申请",
      "content": "张三提交了请假申请",
    },
  },
}
```

### 2. 配置卡片回调

```json5
{
  channels: {
    "feishu": {
      card: {
        enabled: true,
        callback: {
          url: "https://your-domain.com/webhook/feishu/card",
        },
      },
    },
  },
}
```

### 3. 处理卡片事件

OpenClaw 会自动处理卡片回调事件：
- 按钮点击
- 表单提交
- 选择器变更

---

## 🔄 事件订阅

### 支持的事件类型

#### 消息事件
- `im.message.receive_v1`：接收消息
- `im.message.read_v1`：消息已读

#### 通讯录事件
- `contact.user.created_v3`：用户创建
- `contact.user.updated_v3`：用户更新
- `contact.user.deleted_v3`：用户删除

#### 审批事件
- `approval.approved`：审批通过
- `approval.rejected`：审批拒绝

### 配置事件订阅

```json5
{
  channels: {
    "feishu": {
      events: {
        enabled: true,
        subscriptions: [
          "im.message.receive_v1",
          "contact.user.created_v3",
          "approval.approved",
        ],
      },
    },
  },
}
```

---

## 🚀 高级功能

### 1. 多维表格集成

飞书多维表格是强大的数据管理工具，OpenClaw 可以自动读写：

```json5
{
  channels: {
    "feishu": {
      bitable: {
        enabled: true,
        permissions: {
          "bascnxxxxxxxxxx": ["read", "write"],
        },
      },
    },
  },
}
```

### 2. 文档集成

```json5
{
  channels: {
    "feishu": {
      doc: {
        enabled: true,
        permissions: {
          "doccnxxxxxxxxxx": ["read", "edit"],
        },
      },
    },
  },
}
```

### 3. 日历集成

```json5
{
  channels: {
    "feishu": {
      calendar: {
        enabled: true,
        autoCreate: true,  // 自动创建日程
      },
    },
  },
}
```

---

## 🤖 机器人能力

### 1. 自动回复

```json5
{
  channels: {
    "feishu": {
      bot: {
        autoReply: {
          enabled: true,
          welcomeMessage: "你好！我是 AI 助手，有什么可以帮助你的？",
        },
      },
    },
  },
}
```

### 2. 命令支持

```bash
# 用户在飞书中发送命令
/help  # 获取帮助
/status  # 查看状态
/new  # 新建会话
```

---

## 🔧 常见问题

### 1. 事件订阅验证失败
**原因**：URL 不可达或签名验证失败  
**解决**：
```bash
# 检查 URL 可访问性
curl https://your-domain.com/webhook/feishu

# 验证配置
openclaw doctor --channel feishu
```

### 2. 消息发送失败
**原因**：权限不足或用户 ID 错误  
**解决**：
```bash
# 检查权限配置
openclaw channels status feishu

# 验证用户 ID
openclaw feishu users list
```

### 3. 卡片回调无响应
**原因**：回调 URL 配置错误  
**解决**：
```json5
{
  channels: {
    "feishu": {
      card: {
        enabled: true,
        callback: {
          url: "https://your-domain.com/webhook/feishu/card",  // 确认 URL 正确
        },
      },
    },
  },
}
```

---

## 📖 相关资源

- [飞书开放平台文档](https://open.feishu.cn/document/)
- [飞书 API 参考](https://open.feishu.cn/document/server-docs/api-reference)
- [交互卡片开发指南](https://open.feishu.cn/document/client-docs/bot-v3/card)

---

## 💡 最佳实践

1. **消息设计**
   - 使用交互卡片提升用户体验
   - 合理使用富文本格式
   - 控制消息长度和频率

2. **权限管理**
   - 最小权限原则
   - 定期审查权限配置
   - 使用 `allowlist` 限制访问

3. **性能优化**
   - 缓存 access_token
   - 异步处理耗时操作
   - 合理使用事件订阅

---

**下一步** → [钉钉配置指南](dingtalk.md)
