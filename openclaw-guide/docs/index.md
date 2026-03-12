# OpenClaw 安装指导

> 🎯 让技术小白在 10 分钟内跑起来一个可用的 OpenClaw 实例

---

## 📖 这是什么？

**OpenClaw** 是一个开源的 AI 助手网关，可以连接：
- WhatsApp
- Telegram  
- Discord
- iMessage
- 更多...

**一句话概括**：把 AI 助手接到你的聊天软件上，随时随地对话。

---

## 🚀 快速开始（5 分钟）

### 前置要求

- **Node.js 22+** （必需）
- 一个 **API Key**（OpenAI / Anthropic / 其他支持的模型）
- 5 分钟时间

### 第一步：安装

**macOS / Linux：**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**Windows (PowerShell)：**
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

### 第二步：运行向导

```bash
openclaw onboard --install-daemon
```

向导会帮你：
- ✅ 配置 AI 模型（输入 API Key）
- ✅ 设置 Gateway（本地服务）
- ✅ 安装为系统服务（自动启动）

### 第三步：打开控制面板

```bash
openclaw dashboard
```

浏览器会打开 `http://127.0.0.1:18789/`

### 第四步：开始聊天

在控制面板里直接对话，或连接你的聊天软件（见下文）。

---

## ✅ 检查状态

```bash
# 查看 Gateway 是否运行
openclaw gateway status

# 查看日志
openclaw logs --follow

# 健康检查
curl http://127.0.0.1:18789/healthz
```

---

## 📱 连接聊天软件

### WhatsApp（推荐）

1. **配置访问策略**（编辑 `~/.openclaw/openclaw.json`）：
```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "pairing",
      allowFrom: ["+15551234567"],  // 你的手机号
    },
  },
}
```

2. **扫码登录**：
```bash
openclaw channels login --channel whatsapp
```

3. **给机器人发消息**，会收到配对码

4. **批准配对**：
```bash
openclaw pairing list whatsapp
openclaw pairing approve whatsapp <CODE>
```

### Telegram

1. **创建 Bot**：
   - 在 Telegram 搜索 `@BotFather`
   - 发送 `/newbot` 创建机器人
   - 复制得到的 token（格式：`123:abc...`）

2. **配置**（编辑 `~/.openclaw/openclaw.json`）：
```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123:abc...",  // 你的 token
      dmPolicy: "pairing",
    },
  },
}
```

3. **重启 Gateway**：
```bash
openclaw gateway restart
```

4. **给 Bot 发消息**，收到配对码后批准

### Discord

1. **创建 Discord Bot**（需要 Discord 开发者账号）
2. 获取 Bot Token
3. 配置 `channels.discord`
4. 邀请 Bot 到服务器

---

## 🔧 配置文件

**位置**：`~/.openclaw/openclaw.json`

**格式**：JSON5（支持注释和尾随逗号）

**最小配置示例**：
```json5
{
  agents: { 
    defaults: { 
      workspace: "~/.openclaw/workspace" 
    } 
  },
  channels: { 
    whatsapp: { 
      allowFrom: ["+15551234567"] 
    } 
  },
}
```

**修改后自动生效**（大部分配置支持热重载）

---

## 🐳 Docker 部署（可选）

### 快速启动

```bash
export OPENCLAW_IMAGE="ghcr.io/openclaw/openclaw:latest"
./docker-setup.sh
```

### 常用命令

```bash
# 查看日志
docker compose logs -f openclaw-gateway

# 重启
docker compose restart openclaw-gateway

# 获取控制面板链接
docker compose run --rm openclaw-cli dashboard --no-open
```

---

## ❓ 常见问题

### 配置验证失败

```bash
# 诊断问题
openclaw doctor

# 自动修复
openclaw doctor --fix
```

### 渠道连接不上

```bash
# 检查状态
openclaw channels status

# 重新登录
openclaw channels login --channel whatsapp
```

### Gateway 无法启动

```bash
# 查看日志
openclaw logs --follow

# 检查配置
openclaw doctor
```

### 安全检查

```bash
# 运行安全审计
openclaw security audit
```

---

## 📚 进阶功能

- **多 Agent 路由**：不同渠道用不同 AI
- **Skills 技能**：扩展 AI 能力（市场：https://clawhub.com）
- **Nodes 节点**：连接手机控制 AI
- **Cron 定时任务**：自动化提醒
- **沙箱隔离**：安全运行 AI 工具

详见 [进阶功能文档](./advanced.md)

---

## 🆘 获取帮助

- **官方文档**：https://docs.openclaw.ai/
- **GitHub**：https://github.com/openclaw/openclaw
- **社区**：https://discord.com/invite/clawd
- **问题反馈**：GitHub Issues

---

## 📝 下一步

1. ✅ 安装完成 → 打开控制面板试试
2. ✅ 连接聊天软件 → WhatsApp/Telegram/Discord
3. ✅ 配置安全策略 → 限制谁可以访问
4. ✅ 探索进阶功能 → 多 Agent、Skills、Nodes

---

_最后更新：2026-03-07_
