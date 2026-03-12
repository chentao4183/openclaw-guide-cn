# 微信公众号配置指南

> 对接微信公众平台，支持订阅消息与客服消息，适配对外服务与运营通知

![微信公众号](https://img.shields.io/badge/微信公众号-Official%20Account-green)
![难度](https://img.shields.io/badge/难度-⭐⭐-yellow)

---

## 📋 准备工作

### 前置要求
- 微信公众号管理员权限
- 公众号 AppID 和 AppSecret
- 公网可访问的服务器（80 或 443 端口）
- 已备案的域名

### 获取必要信息

1. **获取公众号信息**
   - 登录 [微信公众平台](https://mp.weixin.qq.com/)
   - 开发 → 基本配置 → 获取 AppID 和 AppSecret

2. **配置服务器**
   - 开发 → 基本配置 → 服务器配置
   - URL：`https://your-domain.com/webhook/wechat`
   - Token：自定义（用于签名验证）
   - EncodingAESKey：随机生成（消息加密密钥）

3. **配置 IP 白名单**
   - 开发 → 基本配置 → IP 白名单
   - 添加服务器 IP

---

## ⚙️ 配置 OpenClaw

### 1. 基础配置

编辑 `~/.openclaw/openclaw.json`：

```json5
{
  channels: {
    "wechat-official": {
      enabled: true,
      
      // 公众号信息
      appId: "wxabcdef123456",
      appSecret: "your_app_secret",
      
      // 服务器配置
      token: "your_token",
      encodingAESKey: "your_encoding_aes_key",
      
      // 消息策略
      dmPolicy: "open",  // open | allowlist
      allowFrom: ["*"],  // 允许所有用户
      
      // 回调配置
      webhook: {
        enabled: true,
        path: "/webhook/wechat",
      },
    },
  },
}
```

### 2. 配置说明

#### dmPolicy（私聊策略）
- `open`：允许所有用户（公众号场景推荐）
- `allowlist`：仅允许 `allowFrom` 列表中的用户

---

## 🔐 服务器验证

### 验证流程

微信会向配置的 URL 发送 GET 请求进行验证：

```bash
# 启动 OpenClaw 网关
openclaw gateway start

# 微信会自动验证，OpenClaw 会处理签名验证
```

### 验证参数
- `signature`：微信加密签名
- `timestamp`：时间戳
- `nonce`：随机数
- `echostr`：随机字符串

OpenClaw 会自动处理验证逻辑，返回正确的 `echostr`。

---

## 📨 消息接收与发送

### 接收消息

OpenClaw 会自动处理以下消息类型：
- 文本消息
- 图片消息
- 语音消息
- 视频消息
- 地理位置消息
- 链接消息
- 事件消息（关注、取消关注、菜单点击等）

### 发送消息

#### 客服消息（48 小时内有效）

```bash
curl -X POST http://localhost:18789/api/send \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "wechat-official",
    "to": "openid",
    "message": "你好，这是客服消息"
  }'
```

#### 模板消息（需用户关注）

```json5
{
  "channel": "wechat-official",
  "to": "openid",
  "template": {
    "templateId": "your_template_id",
    "data": {
      "first": { "value": "您好，您有新的通知" },
      "keyword1": { "value": "订单号：123456" },
      "keyword2": { "value": "已发货" },
      "remark": { "value": "感谢您的支持" },
    },
  },
}
```

---

## 📢 菜单配置

### 1. 创建自定义菜单

```json5
{
  channels: {
    "wechat-official": {
      menu: {
        button: [
          {
            type: "click",
            name: "今日歌曲",
            key: "V1001_TODAY_MUSIC",
          },
          {
            type: "view",
            name: "搜索",
            url: "https://www.example.com/search",
          },
          {
            name: "菜单",
            subButton: [
              {
                type: "view",
                name: "搜索",
                url: "https://www.example.com/search",
              },
              {
                type: "click",
                name: "赞一下我们",
                key: "V1001_GOOD",
              },
            ],
          },
        ],
      },
    },
  },
}
```

### 2. 同步菜单

```bash
# 创建菜单
openclaw wechat menu create

# 删除菜单
openclaw wechat menu delete

# 查询菜单
openclaw wechat menu get
```

---

## 🔄 事件处理

### 支持的事件类型

#### 关注事件
- `subscribe`：用户关注
- `unsubscribe`：用户取消关注

#### 菜单事件
- `CLICK`：点击菜单拉取消息
- `VIEW`：点击菜单跳转链接

#### 扫码事件
- `SCAN`：用户已关注时的扫码事件
- `subscribe`（带参数）：用户未关注时的扫码事件

### 配置事件处理

```json5
{
  channels: {
    "wechat-official": {
      events: {
        subscribe: {
          reply: "欢迎关注！我是 AI 助手，有什么可以帮助你的？",
        },
        unsubscribe: {
          notify: true,  // 通知管理员
        },
      },
    },
  },
}
```

---

## 🎯 高级功能

### 1. 带参数二维码

```bash
# 创建临时二维码
openclaw wechat qrcode create --scene "user_123" --expire 604800

# 创建永久二维码
openclaw wechat qrcode create --scene "vip_user" --permanent
```

### 2. 用户标签管理

```json5
{
  channels: {
    "wechat-official": {
      tags: {
        enabled: true,
        autoTag: {
          subscribe: "新用户",
          interact: "活跃用户",
        },
      },
    },
  },
}
```

### 3. 素材管理

```bash
# 上传图片素材
openclaw wechat media upload --type image --file image.jpg

# 获取素材列表
openclaw wechat media list --type image
```

---

## 🔧 常见问题

### 1. 服务器配置验证失败
**原因**：Token 不匹配或 URL 不可达  
**解决**：
```bash
# 检查 URL 可访问性
curl https://your-domain.com/webhook/wechat

# 确认 Token 配置正确
openclaw doctor --channel wechat-official
```

### 2. 消息发送失败
**原因**：48 小时限制或 access_token 过期  
**解决**：
```bash
# 检查 access_token
openclaw wechat token status

# 刷新 access_token
openclaw wechat token refresh
```

### 3. 模板消息发送失败
**原因**：模板 ID 错误或用户未关注  
**解决**：
```bash
# 查看可用模板
openclaw wechat template list

# 检查用户关注状态
openclaw wechat user info --openid openid
```

---

## 📖 相关资源

- [微信公众平台开发文档](https://developers.weixin.qq.com/doc/offiaccount/Getting_Started/Overview.html)
- [微信 API 参考](https://developers.weixin.qq.com/doc/offiaccount/Basic_Information/Get_access_token.html)
- [OpenClaw 官方文档](https://docs.openclaw.ai/)

---

## 💡 最佳实践

1. **消息策略**
   - 合理使用客服消息和模板消息
   - 控制消息频率避免骚扰
   - 使用 Markdown 提升可读性

2. **用户体验**
   - 关注后自动欢迎
   - 菜单清晰易懂
   - 及时响应用户消息

3. **安全建议**
   - 启用消息加密
   - 定期更换 EncodingAESKey
   - 监控异常请求

---

**下一步** → [微信小程序配置指南](wechat-miniprogram.md)
