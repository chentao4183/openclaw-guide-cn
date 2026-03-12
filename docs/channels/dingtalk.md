# 钉钉配置指南

> 对接钉钉开放平台，支持机器人消息、事件回调与组织架构相关能力，适配企业协作与通知场景

![钉钉](https://img.shields.io/badge/钉钉-DingTalk-blue)
![难度](https://img.shields.io/badge/难度-⭐⭐-yellow)

---

## 📋 准备工作

### 前置要求
- 钉钉管理员权限
- 钉钉应用 AppKey 和 AppSecret
- 公网可访问的服务器（用于接收回调）

### 获取必要信息

1. **创建钉钉应用**
   - 登录 [钉钉开放平台](https://open.dingtalk.com/)
   - 应用开发 → 企业内部开发 → 创建应用
   - 获取 AppKey 和 AppSecret

2. **配置权限**
   - 应用详情 → 权限管理
   - 添加必要权限：
     - `Contact.User.Read`（通讯录用户信息读权限）
     - `Message.Robot.Send`（机器人消息发送）

3. **配置回调地址**
   - 应用详情 → 开发管理 → 修改
   - 服务器出口 IP：`your_server_ip`
   - 消息接收地址：`https://your-domain.com/webhook/dingtalk`

---

## ⚙️ 配置 OpenClaw

### 1. 基础配置

编辑 `~/.openclaw/openclaw.json`：

```json5
{
  channels: {
    "dingtalk": {
      enabled: true,
      
      // 应用信息
      appKey: "dingxxxxxxxxx",
      appSecret: "your_app_secret",
      
      // 消息策略
      dmPolicy: "pairing",  // pairing | allowlist | open
      allowFrom: ["user_id_1", "user_id_2"],
      
      // 群组配置
      groupPolicy: "allowlist",
      groups: {
        "*": {
          requireMention: true,
        },
      },
      
      // 回调配置
      webhook: {
        enabled: true,
        path: "/webhook/dingtalk",
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

## 🔐 钉钉登录

### 扫码配对

```bash
# 启动钉钉渠道
openclaw channels login --channel dingtalk

# 查看配对请求
openclaw pairing list dingtalk

# 批准配对
openclaw pairing approve dingtalk <CODE>
```

---

## 📨 消息接收与发送

### 接收消息

OpenClaw 会自动处理以下消息类型：
- 文本消息
- 图片消息
- 文件消息
- 语音消息
- 链接消息
- OA 消息

### 发送消息

```bash
# 通过 API 发送消息
curl -X POST http://localhost:18789/api/send \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "dingtalk",
    "to": "user_id",
    "message": "你好，这是钉钉测试消息"
  }'
```

---

## 🤖 群机器人配置

### 1. 创建群机器人
- 在钉钉群中 → 群设置 → 智能群助手 → 添加机器人
- 选择"自定义"机器人
- 获取 Webhook URL 和加签密钥

### 2. 配置群机器人

```json5
{
  channels: {
    "dingtalk": {
      groups: {
        "chat_id_1": {
          webhook: "https://oapi.dingtalk.com/robot/send?access_token=xxx",
          secret: "your_secret",  // 加签密钥
          requireMention: false,
        },
      },
    },
  },
}
```

### 3. 安全设置

钉钉机器人支持三种安全设置：
1. **自定义关键词**：消息必须包含关键词
2. **加签**：使用密钥签名（推荐）
3. **IP 地址（段）**：限制来源 IP

```json5
{
  channels: {
    "dingtalk": {
      groups: {
        "chat_id": {
          security: {
            type: "sign",  // keyword | sign | ip
            secret: "your_secret",
            // keywords: ["关键词1", "关键词2"],  // type=keyword 时
            // ips: ["192.168.1.1"],  // type=ip 时
          },
        },
      },
    },
  },
}
```

---

## 🔄 事件回调配置

### 支持的事件类型

#### 通讯录事件
- `user_add_org`：用户加入企业
- `user_modify_org`：用户信息变更
- `user_leave_org`：用户离开企业
- `org_admin_add`：管理员添加
- `org_admin_remove`：管理员移除

#### 群组事件
- `chat_add_user`：用户加入群
- `chat_remove_user`：用户退出群
- `chat_update_title`：群名称变更

#### 审批事件
- `bpms_instance_change`：审批实例变更
- `bpms_task_change`：审批任务变更

### 配置事件订阅

```json5
{
  channels: {
    "dingtalk": {
      events: {
        enabled: true,
        subscriptions: [
          "user_add_org",
          "user_modify_org",
          "bpms_instance_change",
        ],
      },
    },
  },
}
```

---

## 🏢 组织架构集成

### 1. 部门管理

```json5
{
  channels: {
    "dingtalk": {
      department: {
        enabled: true,
        autoSync: true,  // 自动同步
        syncInterval: 3600,  // 同步间隔（秒）
      },
    },
  },
}
```

### 2. 用户管理

```bash
# 同步用户信息
openclaw dingtalk sync users

# 查看用户列表
openclaw dingtalk users list

# 查看部门结构
openclaw dingtalk departments tree
```

---

## 🚀 高级功能

### 1. 审批流程集成

```json5
{
  channels: {
    "dingtalk": {
      approval: {
        enabled: true,
        processes: {
          "process_code_1": {
            autoApprove: false,
            notifyUsers: ["manager_id"],
          },
        },
      },
    },
  },
}
```

### 2. 工作通知

```json5
{
  channels: {
    "dingtalk": {
      workNotification: {
        enabled: true,
        agentId: "your_agent_id",
      },
    },
  },
}
```

### 3. 日志报表

```json5
{
  channels: {
    "dingtalk": {
      report: {
        enabled: true,
        templates: ["template_id_1", "template_id_2"],
      },
    },
  },
}
```

---

## 🔧 常见问题

### 1. 回调验证失败
**原因**：服务器出口 IP 未配置或 URL 不可达  
**解决**：
```bash
# 确认 IP 白名单配置
openclaw doctor --channel dingtalk

# 检查 URL 可访问性
curl https://your-domain.com/webhook/dingtalk
```

### 2. 消息发送失败
**原因**：权限不足或 access_token 过期  
**解决**：
```bash
# 检查权限配置
openclaw channels status dingtalk

# 刷新 access_token
openclaw dingtalk token refresh
```

### 3. 群机器人不响应
**原因**：安全设置不匹配  
**解决**：
```json5
{
  channels: {
    "dingtalk": {
      groups: {
        "chat_id": {
          security: {
            type: "sign",
            secret: "your_secret",  // 确保密钥正确
          },
        },
      },
    },
  },
}
```

---

## 📖 相关资源

- [钉钉开放平台文档](https://open.dingtalk.com/document/)
- [钉钉 API 参考](https://open.dingtalk.com/document/orgapp/api-overview)
- [群机器人开发指南](https://open.dingtalk.com/document/robots/custom-robot-access)

---

## 💡 最佳实践

1. **消息设计**
   - 使用 Markdown 格式提升可读性
   - 合理使用 @ 提及
   - 控制消息频率避免打扰

2. **权限管理**
   - 最小权限原则
   - 定期审查权限配置
   - 使用部门权限控制

3. **性能优化**
   - 缓存 access_token
   - 异步处理耗时操作
   - 批量接口提升效率

---

**下一步** → [微信公众号配置指南](wechat-official.md)
