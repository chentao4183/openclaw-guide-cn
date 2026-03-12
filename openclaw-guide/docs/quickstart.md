# 5 分钟快速开始

> 最快的方式跑起来 OpenClaw

---

## 📋 前置检查

```bash
# 检查 Node.js 版本（需要 22+）
node --version

# 如果版本太低或未安装：
# macOS: brew install node@22
# Ubuntu: curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash - && sudo apt-get install -y nodejs
# Windows: 去 https://nodejs.org 下载安装
```

---

## 🚀 三步启动

### 第一步：安装（1 分钟）

**macOS / Linux：**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**Windows PowerShell：**
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

---

### 第二步：配置（3 分钟）

```bash
openclaw onboard --install-daemon
```

**向导会问你：**

1. **选择 AI 模型提供商**
   - OpenAI（推荐新手）
   - Anthropic Claude
   - 其他支持的提供商

2. **输入 API Key**
   - OpenAI: 去 https://platform.openai.com/api-keys 获取
   - Anthropic: 去 https://console.anthropic.com/ 获取

3. **选择 Gateway 配置**
   - 默认即可（本地 + Token 认证）

4. **是否安装为系统服务**
   - 选 `Yes`（开机自动启动）

5. **是否安装推荐 Skills**
   - 选 `Yes`（可选，增加 AI 能力）

---

### 第三步：验证（1 分钟）

```bash
# 检查 Gateway 状态
openclaw gateway status

# 打开控制面板
openclaw dashboard
```

浏览器会自动打开 `http://127.0.0.1:18789/`

---

## ✅ 成功标志

看到这个界面就成功了：

```
🦞 OpenClaw Gateway
Status: Running
Port: 18789
```

在控制面板的聊天框里输入 "你好" 试试！

---

## 🎯 下一步

### 想在 WhatsApp/Telegram 上用？

1. [连接 WhatsApp](./channels/whatsapp.md)
2. [连接 Telegram](./channels/telegram.md)
3. [连接 Discord](./channels/discord.md)

### 想了解更多？

- [配置文件详解](./configuration.md)
- [安全设置](./security.md)
- [常见问题](./faq.md)

---

## ❓ 遇到问题？

### 安装失败

```bash
# 手动安装
npm install -g openclaw@latest
```

### Gateway 启动失败

```bash
# 查看详细日志
openclaw logs --follow

# 运行诊断
openclaw doctor
```

### 控制面板打不开

```bash
# 检查端口
lsof -i :18789

# 重启 Gateway
openclaw gateway restart
```

---

## 💡 小贴士

- **API Key 很重要**：不要泄露，存在配置文件里
- **首次启动较慢**：需要下载依赖，耐心等待
- **推荐浏览器**：Chrome / Firefox / Safari 最新版

---

_预计耗时：5 分钟_
