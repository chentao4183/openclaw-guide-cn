# OpenClaw 知识库

> **维护者**: OpenClaw安装指导大师
> **更新时间**: 2026-03-07
> **版本**: OpenClaw v1.0+

---

## 📚 核心概念

### 什么是OpenClaw?
OpenClaw是一个自托管的多渠道网关，连接WhatsApp、Telegram、Discord等聊天应用到AI代理。
- **自托管**: 运行在你自己的机器上，数据完全掌控
- **多渠道**: 一个Gateway同时支持多个聊天平台
- **AI原生**: 专为AI代理设计,支持工具调用、会话、记忆等
- **开源**: MIT许可证, 社区驱动

### 架构组件
```
聊天应用 → Gateway → AI代理 (Pi)
           ↓
        Web Dashboard / Control UI
```

### 系统要求
- **Node.js**: >=22 (必需)
- **操作系统**: macOS, Linux, Windows (WSL2)
- **推荐**: Brave Search API密钥 (用于网页搜索)

- **磁盘空间**: ~500MB
- **内存**: 512MB+

---

## 🚀 快速开始

### 安装
```bash
# Linux/macOS
curl -fsSL https://openclaw.ai/install.sh | bash

# Windows (PowerShell)
iwr -useb https://openclaw.ai/install.ps1 | iex
```

### 运行向导
```bash
openclaw onboard
```

选择"快速开始"模式，设置:
- 模型提供商 (推荐Anthropic)
- Gateway端口 (默认18789)
- 认证 (自动生成token)
- 皂箱模式 (默认启用)

```

### 选择渠道
**推荐顺序** (从简单到复杂):
1. **WebChat** - 最简单,无需配置
2. **Telegram** - 创建Bot,获取token
3. **Discord** - 创建Bot,获取token
4. **WhatsApp** - 扫描二维码登录

---

## 🔧 渠道配置速查

### Telegram (最简单)
1. 创建Bot: [@BotFather](https://t.me/botfather)
2. 获取Token: 复制API token
3. 配置: 向导中粘贴token
4. 启动: `openclaw gateway`

### WhatsApp (需二维码)
1. 登录: `openclaw channels login`
2. 扫码: 用WhatsApp扫描二维码
3. 配置: 自动写入配置文件
4. 启动: `openclaw gateway`

### Discord (简单)
1. 创建Bot: [Discord Developer Portal](https://discord.com/developers)
2. 获取Token: 复制Bot token
3. 配置: 向导中粘贴token
4. 启动: `openclaw gateway`

---

## 📊 模型配置

### 推荐模型提供商
1. **Anthropic** (推荐)
   - 性能优秀
   - API密钥简单
   - 支持Claude 3.5 Sonnet, Claude 4
   
2. **OpenAI**
   - GPT-4, GPT-4o
   - 需要OpenAI API密钥
   
3. **国内模型** (中国用户)
   - GLM (智谱AI)
   - Moonshot (Kimi)
   - MiniMax

### 配置示例
```json
{
  "agents": {
    "defaults": {
      "model": "anthropic/claude-3-5-sonnet-20241022"
      "thinking": "medium"
    }
  }
}
```

---

## 🛡️ 安全配置

### 基本安全
- **Token认证**: Gateway默认启用token认证
- **配对模式**: 私信需要审批
- **允许列表**: 可配置允许的发送者

### 配置示例
```json
{
  "gateway": {
    "auth": {
      "token": "your-secure-token"
    }
  },
  "channels": {
    "whatsapp": {
    "dmPolicy": "allowlist",
    "allowFrom": ["+8613800138888"]
  }
}
```

---

## 📝 常用命令

### 基本操作
```bash
# 安装
openclaw install

# 运行向导
openclaw onboard

# 启动Gateway
openclaw gateway start

# 查看状态
openclaw status

# 健康检查
openclaw health

# 查看配置
openclaw configure

# 更新
openclaw update
```

### 渠道操作
```bash
# WhatsApp登录
openclaw channels login

# 配对审批
openclaw pairing list whatsapp
openclaw pairing approve whatsapp <code>

# 发送测试消息
openclaw message send --target +1234567890 --message "Test"
```

---

## 🔍 故障排查

### 问题1: 找不到openclaw命令
**解决**:
```bash
# 检查PATH
echo $PATH
# 添加npm全局bin目录到PATH
export PATH="$(npm prefix -g)/bin:$PATH"
```

### 问题2: Gateway启动失败
**解决**:
```bash
# 检查端口占用
netstat -ano | grep 18789
# 更换端口
openclaw gateway --port 18790
```

### 问题3: WhatsApp无法登录
**解决**:
1. 检查网络连接
2. 重新运行 `openclaw channels login`
3. 使用备用手机号码
```

---

## 📚 进阶主题

### 多账户
- 支持多个WhatsApp/Telegram账号
- 每个账号独立会话
- 配置路由规则

### 群组支持
- Telegram群组完整支持
- Discord服务器支持
- WhatsApp群组支持

### 远程访问
- SSH隧道
- Tailscale
- 远程Gateway

### 自动化
- Cron定时任务
- Webhook集成
- 自动健康检查

---

## 🔗 有用链接

- **官方文档**: https://docs.openclaw.ai
- **GitHub**: https://github.com/openclaw/openclaw
- **社区**: https://discord.gg/clawd
- **Skills市场**: https://clawhub.com

---

## 📞 获取帮助

- **文档**: https://docs.openclaw.ai
- **Discord社区**: https://discord.gg/clawd
- **GitHub Issues**: https://github.com/openclaw/openclaw/issues
- **FAQ**: 查看 `knowledge-base.md` 中的常见问题

