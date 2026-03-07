# 快速开始

选择你的操作系统，开始安装 OpenClaw。

---

## 📋 安装流程概览

```
┌─────────────────────────────────────────────────────────────┐
│                    OpenClaw 安装流程                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1️⃣ 检查环境                                                │
│     └─ 确认 Node.js 22+ 已安装                               │
│                                                             │
│  2️⃣ 运行安装脚本                                             │
│     └─ Windows: PowerShell 一行命令                          │
│     └─ macOS/Linux: curl 一行命令                            │
│                                                             │
│  3️⃣ 首次设置                                                 │
│     └─ openclaw onboard --install-daemon                    │
│                                                             │
│  4️⃣ 配置渠道                                                 │
│     └─ 连接 WhatsApp / Telegram / Discord                   │
│                                                             │
│  5️⃣ 开始使用                                                 │
│     └─ 发送消息给 AI 助手                                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## ⚡ 10 分钟快速上手

### 步骤 1：检查 Node.js 版本

```bash
node --version
# 需要 v22.0.0 或更高版本
```

**如果版本过低或未安装：**

<Tabs>
<Tab label="Windows">

1. 访问 https://nodejs.org/
2. 下载 LTS 版本（推荐）
3. 运行安装程序
4. 重启 PowerShell

</Tab>
<Tab label="macOS">

```bash
# 使用 Homebrew
brew install node@22

# 或使用 nvm
nvm install 22
nvm use 22
```

</Tab>
<Tab label="Linux">

```bash
# Ubuntu/Debian
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs

# CentOS/RHEL
curl -fsSL https://rpm.nodesource.com/setup_22.x | sudo bash -
sudo yum install -y nodejs
```

</Tab>
</Tabs>

---

### 步骤 2：安装 OpenClaw

<Tabs>
<Tab label="Windows">

**打开 PowerShell（管理员模式）**

按 `Win + X`，选择 **Windows PowerShell (管理员)**

**运行安装命令**

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

**预期输出**

```
正在下载 OpenClaw...
正在安装 OpenClaw...
安装完成！
运行 'openclaw --help' 查看帮助
```

</Tab>
<Tab label="macOS">

**打开终端**

按 `Cmd + Space`，输入 **Terminal**

**运行安装命令**

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**预期输出**

```
Downloading OpenClaw...
Installing OpenClaw...
Installation complete!
Run 'openclaw --help' for usage
```

</Tab>
<Tab label="Linux">

**运行安装命令**

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**预期输出**

```
Downloading OpenClaw...
Installing OpenClaw...
Installation complete!
Run 'openclaw --help' for usage
```

</Tab>
</Tabs>

---

### 步骤 3：首次设置

**运行引导向导**

```bash
openclaw onboard --install-daemon
```

**向导会引导你完成：**

```
╔════════════════════════════════════════════════════════════╗
║           欢迎使用 OpenClaw 配置向导                        ║
╠════════════════════════════════════════════════════════════╣
║                                                            ║
║  ✓ 配置工作目录                                            ║
║  ✓ 设置 AI 模型（Claude/GPT）                              ║
║  ✓ 安装系统服务（后台运行）                                 ║
║  ✓ 启动 Gateway                                            ║
║                                                            ║
╚════════════════════════════════════════════════════════════╝
```

---

### 步骤 4：验证安装

**检查 Gateway 状态**

```bash
openclaw gateway status
```

**预期输出**

```
Gateway Status: running
Version: 1.0.0
Uptime: 2 minutes
Dashboard: http://localhost:18789
```

**打开控制面板**

```bash
openclaw dashboard
```

浏览器会自动打开 `http://localhost:18789`

---

## 🎯 下一步

安装完成后，你需要：

### 1. 配置渠道

选择一个你想使用的聊天平台：

| 渠道 | 难度 | 说明 |
|------|------|------|
| [WhatsApp](/docs/channels/whatsapp) | ⭐⭐ | 扫码登录，最简单 |
| [Telegram](/docs/channels/telegram) | ⭐ | 创建 Bot，配置简单 |
| [Discord](/docs/channels/discord) | ⭐⭐ | 创建 Bot 应用 |

### 2. 测试对话

配置完成后，给你的机器人发送消息：

```
你好！
```

AI 会回复你，表示一切正常！

---

## 🐳 Docker 部署

适合熟悉 Docker 的用户或服务器部署。

### 快速启动

```bash
# 设置镜像
export OPENCLAW_IMAGE="ghcr.io/openclaw/openclaw:latest"

# 运行安装脚本
./docker-setup.sh
```

### 常用命令

```bash
# 运行引导向导
docker compose run --rm openclaw-cli onboard

# 启动 Gateway
docker compose up -d openclaw-gateway

# 查看日志
docker compose logs -f openclaw-gateway

# 重启服务
docker compose restart openclaw-gateway

# 获取 Dashboard 链接
docker compose run --rm openclaw-cli dashboard --no-open
```

### 健康检查

```bash
# 存活探针
curl http://127.0.0.1:18789/healthz

# 就绪探针
curl http://127.0.0.1:18789/readyz
```

### Docker Compose 配置示例

```yaml
version: '3.8'
services:
  openclaw-gateway:
    image: ghcr.io/openclaw/openclaw:latest
    ports:
      - "18789:18789"
    volumes:
      - ./data:/root/.openclaw
    restart: unless-stopped
    environment:
      - NODE_ENV=production
```

---

## 📦 NPM 全局安装

适合 Node.js 开发者。

### 安装

```bash
npm install -g openclaw@latest
```

### 验证

```bash
openclaw --version
# 输出: 1.0.0 或更高版本
```

### 首次设置

```bash
openclaw onboard --install-daemon
openclaw gateway status
```

---

## ❓ 常见安装问题

### Q: 安装脚本报错 "Permission denied"

**Windows**: 以管理员身份运行 PowerShell

**macOS/Linux**:
```bash
sudo curl -fsSL https://openclaw.ai/install.sh | bash
```

### Q: "command not found: openclaw"

1. 重启终端
2. 检查 PATH：
   ```bash
   echo $PATH  # macOS/Linux
   echo $env:PATH  # Windows
   ```
3. 重新安装

### Q: Gateway 无法启动

```bash
# 诊断问题
openclaw doctor

# 查看日志
openclaw logs --follow

# 尝试重启
openclaw gateway restart
```

### Q: 端口被占用

```bash
# 检查端口占用
# Windows
netstat -ano | findstr :18789

# macOS/Linux
lsof -i :18789

# 修改配置使用其他端口
# 编辑 ~/.openclaw/openclaw.json
{
  gateway: {
    port: 18790
  }
}
```

---

## 🔗 下一步

- [配置 WhatsApp](/docs/channels/whatsapp)
- [配置 Telegram](/docs/channels/telegram)
- [配置 Discord](/docs/channels/discord)
- [了解进阶功能](/docs/advanced/)
