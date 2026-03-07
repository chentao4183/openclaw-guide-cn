# 快速开始

选择你的操作系统，开始安装 OpenClaw。

---

## Windows 安装

### 系统要求
- Windows 10/11
- PowerShell 5.1+
- 管理员权限（用于安装服务）

### 安装步骤

**1. 打开 PowerShell（管理员模式）**
- 按 `Win + X`，选择 "Windows PowerShell (管理员)"

**2. 运行安装脚本**
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

**3. 首次设置**
```powershell
# 运行引导向导
openclaw onboard --install-daemon

# 检查 Gateway 状态
openclaw gateway status

# 打开控制面板
openclaw dashboard
```

### 验证安装
```powershell
openclaw --version
```

---

## macOS 安装

### 系统要求
- macOS 11 (Big Sur) 或更高版本

### 安装步骤

**1. 打开终端**
- 按 `Cmd + Space`，输入 "Terminal"

**2. 运行安装脚本**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**3. 首次设置**
```bash
# 运行引导向导
openclaw onboard --install-daemon

# 检查 Gateway 状态
openclaw gateway status

# 打开控制面板
openclaw dashboard
```

### 验证安装
```bash
openclaw --version
```

---

## Linux 安装

### 系统要求
- Ubuntu 20.04+ / Debian 11+ / CentOS 8+
- curl, bash

### 安装步骤

**1. 运行安装脚本**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**2. 首次设置**
```bash
# 运行引导向导
openclaw onboard --install-daemon

# 检查 Gateway 状态
openclaw gateway status

# 打开控制面板
openclaw dashboard
```

### 验证安装
```bash
openclaw --version
```

---

## Docker 部署

### 适合人群
- 熟悉 Docker 的用户
- 需要隔离运行环境
- 服务器部署

### 快速启动

**1. 拉取镜像并启动**
```bash
export OPENCLAW_IMAGE="ghcr.io/openclaw/openclaw:latest"
./docker-setup.sh
```

**2. 运行引导向导**
```bash
docker compose run --rm openclaw-cli onboard
```

**3. 启动 Gateway**
```bash
docker compose up -d openclaw-gateway
```

### 常用命令
```bash
# 查看日志
docker compose logs -f openclaw-gateway

# 重启服务
docker compose restart openclaw-gateway

# 获取 Dashboard 链接
docker compose run --rm openclaw-cli dashboard --no-open
```

### 健康检查
```bash
curl http://127.0.0.1:18789/healthz  # 存活探针
curl http://127.0.0.1:18789/readyz   # 就绪探针
```

---

## NPM 全局安装

### 系统要求
- Node.js 22+ (必需)

### 安装步骤

**1. 安装 Node.js**
- 推荐使用 [nvm](https://github.com/nvm-sh/nvm) 管理 Node.js 版本

**2. 全局安装 OpenClaw**
```bash
npm install -g openclaw@latest
```

**3. 首次设置**
```bash
openclaw onboard --install-daemon
openclaw gateway status
```

---

## 下一步

安装完成后：

1. [配置渠道](./channels/README.md) - 连接 WhatsApp / Telegram / Discord
2. [了解配置文件](./advanced/configuration.md) - 自定义设置
3. [探索 Skills](./advanced/skills.md) - 扩展 AI 能力

---

## 遇到问题？

- 运行 `openclaw doctor` 诊断问题
- 查看 [故障排查](./troubleshooting.md)
- 加入 [Discord 社区](https://discord.com/invite/clawd) 获取帮助
