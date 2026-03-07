# OpenClaw 中文安装指南

> 让 AI 助手连接你的聊天应用 — 10 分钟快速上手

## 🎯 OpenClaw 是什么？

OpenClaw 是一个**自托管网关**，将 WhatsApp、Telegram、Discord、iMessage 等聊天应用连接到 AI 助手（如 Claude、ChatGPT）。

**核心特点：**
- 🔌 多渠道支持 — 一个 Gateway 服务多个平台
- 🤖 Agent 原生 — 支持工具调用、会话管理
- 🔒 自托管 — 数据完全掌控在自己手中
- 🆓 开源免费 — MIT 许可证

## 🚀 5 分钟快速安装

### Windows (PowerShell)
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

### macOS / Linux
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

### 首次设置
```bash
# 运行引导向导
openclaw onboard --install-daemon

# 检查状态
openclaw gateway status

# 打开控制面板
openclaw dashboard
```

## 📖 完整文档

| 章节 | 说明 |
|------|------|
| [快速开始](./docs/quick-start.md) | 各平台详细安装步骤 |
| [渠道配置](./docs/channels/README.md) | WhatsApp、Telegram、Discord 配置 |
| [进阶功能](./docs/advanced/README.md) | 多 Agent、Skills、定时任务 |
| [故障排查](./docs/troubleshooting.md) | 常见问题解答 |

## 🌟 选择你的安装方式

| 方式 | 适合人群 | 难度 |
|------|----------|------|
| [快速安装](./docs/quick-start.md#快速安装) | 新手推荐 | ⭐ |
| [NPM 安装](./docs/quick-start.md#npm-安装) | 开发者 | ⭐⭐ |
| [Docker 部署](./docs/quick-start.md#docker-部署) | 运维人员 | ⭐⭐⭐ |

## 🔗 资源链接

- 📚 [官方文档](https://docs.openclaw.ai/)
- 🐙 [GitHub 仓库](https://github.com/openclaw/openclaw)
- 🛒 [技能市场 ClawHub](https://clawhub.com)
- 💬 [社区 Discord](https://discord.com/invite/clawd)

---

**开始你的 OpenClaw 之旅** → [快速开始](./docs/quick-start.md)
