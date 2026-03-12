# 微信小程序配置指南

> 对接微信小程序，支持轻量入口与业务页面承载，可从对话跳转至任务处理与信息查询

![微信小程序](https://img.shields.io/badge/微信小程序-Mini%20Program-green)
![难度](https://img.shields.io/badge/难度-⭐⭐⭐-orange)

---

## 📋 准备工作

### 前置要求
- 微信小程序管理员权限
- 小程序 AppID 和 AppSecret
- 公网可访问的服务器
- 已备案的域名

### 获取必要信息

1. **获取小程序信息**
   - 登录 [微信小程序后台](https://mp.weixin.qq.com/)
   - 开发 → 开发管理 → 开发设置 → 获取 AppID 和 AppSecret

2. **配置服务器域名**
   - 开发 → 开发管理 → 开发设置 → 服务器域名
   - request 合法域名：`https://your-domain.com`

3. **配置业务域名**
   - 开发 → 开发管理 → 开发设置 → 业务域名
   - 添加你的域名并上传验证文件

---

## ⚙️ 配置 OpenClaw

### 1. 基础配置

编辑 `~/.openclaw/openclaw.json`：

```json5
{
  channels: {
    "wechat-miniprogram": {
      enabled: true,
      
      // 小程序信息
      appId: "wxabcdef123456",
      appSecret: "your_app_secret",
      
      // 消息策略
      dmPolicy: "open",  // open | allowlist
      allowFrom: ["*"],
      
      // 会话管理
      session: {
        enabled: true,
        timeout: 86400,  // 24 小时
      },
    },
  },
}
```

### 2. 配置说明

#### dmPolicy（私聊策略）
- `open`：允许所有用户（小程序场景推荐）
- `allowlist`：仅允许 `allowFrom` 列表中的用户

---

## 📱 小程序端集成

### 1. 客服消息按钮

在小程序页面中添加客服按钮：

```xml
<!-- 小程序页面 XML -->
<button open-type="contact" bindcontact="handleContact">
  联系客服
</button>
```

### 2. 客服消息处理

```javascript
// 小程序页面 JS
Page({
  handleContact(e) {
    console.log('客服消息', e.detail)
  },
})
```

### 3. 发送消息到 OpenClaw

```javascript
// 小程序端代码
wx.request({
  url: 'https://your-domain.com/api/chat',
  method: 'POST',
  data: {
    openid: 'user_openid',
    message: '你好，这是来自小程序的消息',
  },
  success(res) {
    console.log('消息发送成功', res.data)
  },
})
```

---

## 🔄 消息接收与发送

### 接收消息

OpenClaw 会自动处理以下消息类型：
- 文本消息
- 图片消息
- 小程序卡片
- 事件消息（进入会话等）

### 发送消息

#### 客服消息（48 小时内有效）

```bash
curl -X POST http://localhost:18789/api/send \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "wechat-miniprogram",
    "to": "openid",
    "message": "你好，这是小程序客服消息"
  }'
```

#### 发送小程序卡片

```json5
{
  "channel": "wechat-miniprogram",
  "to": "openid",
  "msgType": "miniprogrampage",
  "miniprogram": {
    "title": "商品详情",
    "pagePath": "/pages/goods/detail?id=123",
    "thumbMediaId": "media_id",
  },
}
```

---

## 🔗 从对话跳转到小程序页面

### 1. 文本链接跳转

AI 助手返回的文本中包含链接：

```
点击查看商品详情：/pages/goods/detail?id=123
```

### 2. 小程序卡片跳转

```json5
{
  "channel": "wechat-miniprogram",
  "to": "openid",
  "miniprogram": {
    "title": "订单详情",
    "pagePath": "/pages/order/detail?orderId=456",
    "thumbMediaId": "media_id",
  },
}
```

### 3. 业务场景示例

```javascript
// 用户在小程序中查询订单
// AI 助手返回订单状态 + 跳转链接

// OpenClaw 自动生成响应
{
  "message": "您的订单 #456 已发货，预计明天送达",
  "link": "/pages/order/detail?orderId=456",
}
```

---

## 🎯 高级功能

### 1. 统一服务消息

```json5
{
  channels: {
    "wechat-miniprogram": {
      uniformMessage: {
        enabled: true,
        appId: "your_official_account_appid",  // 关联的公众号
      },
    },
  },
}
```

### 2. 订阅消息

```bash
# 发送订阅消息
openclaw wechat-miniprogram subscribe send \
  --openid user_openid \
  --templateId template_id \
  --data '{"thing1": {"value": "订单已发货"}, "date2": {"value": "2024-01-01"}}'
```

### 3. 数据分析

```json5
{
  channels: {
    "wechat-miniprogram": {
      analytics: {
        enabled: true,
        trackUser: true,
        trackMessage: true,
      },
    },
  },
}
```

---

## 🌐 小程序页面设计建议

### 1. 客服入口

- 在个人中心、订单详情等关键页面添加客服按钮
- 使用醒目的图标和文案

### 2. 消息列表

- 设计简洁的消息列表页面
- 显示对话历史和 AI 助手回复

### 3. 业务跳转

- 在 AI 助手回复中嵌入业务链接
- 使用小程序卡片提升视觉效果

---

## 🔧 常见问题

### 1. 客服消息发送失败
**原因**：48 小时限制或用户未进入会话  
**解决**：
```bash
# 检查用户会话状态
openclaw wechat-miniprogram session status --openid openid

# 使用订阅消息（需用户授权）
openclaw wechat-miniprogram subscribe send --openid openid
```

### 2. 小程序跳转失败
**原因**：页面路径错误或参数格式不对  
**解决**：
```bash
# 检查页面路径
openclaw wechat-miniprogram pages list

# 验证跳转参数
openclaw doctor --channel wechat-miniprogram
```

### 3. 消息加密失败
**原因**：EncodingAESKey 配置错误  
**解决**：
```bash
# 检查配置
openclaw channels status wechat-miniprogram

# 重新生成 EncodingAESKey
openclaw wechat-miniprogram key regenerate
```

---

## 📖 相关资源

- [微信小程序开发文档](https://developers.weixin.qq.com/miniprogram/dev/framework/)
- [客服消息 API](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/customer-message/customerServiceMessage.send.html)
- [订阅消息 API](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/subscribe-message/subscribeMessage.send.html)

---

## 💡 最佳实践

1. **用户体验**
   - 及时响应用户消息
   - 使用业务跳转提升效率
   - 控制消息频率避免打扰

2. **业务集成**
   - 将 AI 助手与业务流程结合
   - 使用订阅消息主动通知
   - 设计清晰的对话流程

3. **性能优化**
   - 缓存 access_token
   - 异步处理耗时操作
   - 合理使用小程序卡片

---

**下一步** → [模型选择指南](../models.md)
