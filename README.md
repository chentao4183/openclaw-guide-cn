# 🦞 OpenClaw - 让 AI 真正为你工作

> **真正能做事的 AI 助手** — 清理收件箱、发送邮件、管理日历，连接企业微信、飞书、钉钉与微信生态

![OpenClaw](https://img.shields.io/badge/OpenClaw-自托管网关-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Platform](https://img.shields.io/badge/企业微信%20%7C%20飞书%20%7C%20钉钉%20%7C%20微信-支持-brightgreen)

---

## 🎯 OpenClaw 能做什么？

### 📱 连接所有主流平台
- **企业微信**：消息收发、群机器人、事件回调、通讯录同步
- **飞书**：消息订阅、交互卡片、流程触发、表单式交互
- **钉钉**：机器人消息、事件回调、组织架构、企业协作
- **微信生态**：公众号订阅消息、小程序业务跳转
- **国际平台**：WhatsApp、Telegram、Discord、iMessage

### 🤖 真正的 AI 智能体
不只是聊天，而是真正能**做事**的助手：

| 应用场景 | 具体能力 |
|---------|---------|
| 📧 邮件管理 | 自动清理收件箱、分类归档、智能回复 |
| 📅 日程管理 | 会议安排、提醒通知、冲突检测 |
| 📊 数据处理 | 报表生成、数据同步、自动化分析 |
| 🔄 工作流自动化 | 跨平台任务编排、定时执行、条件触发 |
| 💬 客户服务 | 7×24 小时自动响应、工单创建、知识库问答 |

### 🔒 完全自主可控
- **自托管**：数据完全掌握在自己手中
- **开源免费**：MIT 许可证，社区驱动
- **灵活配置**：支持多 Agent 路由、权限控制、沙箱隔离

---

## 🚀 5 分钟快速开始

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
# 运行引导向导（自动安装 Node.js 依赖）
openclaw onboard --install-daemon

# 检查状态
openclaw gateway status

# 打开控制面板
openclaw dashboard
```

> ✅ **一行命令完成所有安装**：自动安装 Node.js 及所有依赖，支持 Windows / macOS / Linux

---

## 🌟 为什么选择 OpenClaw？

### ✅ 对比传统方案

| 特性 | OpenClaw | 传统聊天机器人 | 企业 IM 机器人 |
|------|----------|--------------|---------------|
| 真正的 AI 能力 | ✅ 工具调用、任务执行 | ❌ 仅关键词匹配 | ⚠️ 功能受限 |
| 多平台统一 | ✅ 一个网关服务所有平台 | ❌ 需分别开发 | ❌ 单平台 |
| 自主可控 | ✅ 完全开源自托管 | ❌ 依赖第三方 | ⚠️ 供应商锁定 |
| 灵活扩展 | ✅ Skills 技能系统 | ❌ 难以扩展 | ⚠️ 需官方支持 |

### ✅ 真实案例
- **某互联网公司**：用 OpenClaw 自动处理每日 500+ 客户咨询，响应速度提升 80%
- **个人开发者**：自动化管理 3 个 WhatsApp 群 + 2 个 Telegram 频道，节省 2 小时/天
- **电商团队**：飞书机器人自动同步订单状态、库存预警、客服工单

---

## 📖 完整功能指南

### 🚀 [快速开始](/docs/quick-start)
5 分钟快速安装（Windows / macOS / Linux / Docker）

### 📱 [渠道配置](/docs/channels/)
- [企业微信](/docs/channels/wechat-work)
- [飞书](/docs/channels/feishu)
- [钉钉](/docs/channels/dingtalk)
- [微信公众号](/docs/channels/wechat-official)
- [微信小程序](/docs/channels/wechat-miniprogram)
- [WhatsApp](/docs/channels/whatsapp)
- [Telegram](/docs/channels/telegram)
- [Discord](/docs/channels/discord)

### ⚙️ [进阶功能](/docs/advanced/)
- 配置文件详解
- 多 Agent 路由
- Skills 技能系统
- 移动端节点（手机控制）
- 定时任务（Cron）
- 浏览器自动化
- Sub-Agents（后台任务）

### 🤖 [模型选择指南](/docs/models)
国内主流模型推荐与配置：
- 豆包/火山引擎（通用场景首选）
- 智谱 GLM（长文本处理）
- 通义千问（企业应用）
- DeepSeek（性价比之选）

### 🔧 [故障排查](/docs/troubleshooting)
常见问题解答、诊断工具、日志分析

---

## 🛠️ 部署方式选择

| 方式 | 适合场景 | 难度 | 成本 |
|------|---------|------|------|
| [本地快速安装](/docs/quick-start?id=快速安装) | 个人使用、快速验证 | ⭐ | 免费 |
| [NPM 全局安装](/docs/quick-start?id=npm-全局安装) | 开发者、自定义配置 | ⭐⭐ | 免费 |
| [Docker 部署](/docs/quick-start?id=docker-部署) | 生产环境、团队协作 | ⭐⭐⭐ | 免费 |
| [云端一键部署](/docs/cloud-deploy) | 免运维、快速上线 | ⭐ | 按量付费 |

---

## 🔗 资源链接

| 资源 | 链接 | 说明 |
|------|------|------|
| 📚 官方文档 | https://docs.openclaw.ai/ | 英文完整文档 |
| 🐙 GitHub | https://github.com/openclaw/openclaw | 源码与 Issue |
| 🛒 技能市场 | https://clawhub.com | 发现更多 Skills |
| 💬 Discord 社区 | https://discord.com/invite/clawd | 国际社区 |
| 🇨🇳 微信交流群 | 扫码加入 ↓ | 中文互助答疑 |

---

## 💬 加入中文社区

**更快上手，更少踩坑；有问题直接问**

✅ 安装/更新/配对问题互助答疑  
✅ 网关运维、通道接入、插件实践分享  
✅ 关键变更与使用建议同步（少走弯路）  

> 📱 **微信扫码加入**（请务必备注：行业 + 学习目的，提高通过率）

![微信群二维码](/images/wechat-group.jpg)

---

## 🧠 模型资源与开发者特权

通过社区专属链接注册或充值，可获得额外初始额度或首充优惠，同时支持 OpenClaw 开源项目持续维护。

### 推荐顺序
1. **本地跑通**：先完成 Quick Start，确认环境与链路可用
2. **按需选择**：通用场景首选豆包/火山，长文本推荐智谱 GLM
3. **按量扩容**：验证业务效果后再行充值，降低试错成本

详见 [模型选择指南](/docs/models)

---

## 🎉 开始你的 OpenClaw 之旅

**选择适合你的方式**：

👉 [5 分钟快速安装](/docs/quick-start)（推荐新手）  
👉 [查看企业级案例](/docs/cases)（企业用户）  
👉 [探索 Skills 技能市场](https://clawhub.com)（高级玩家）

---

<div align="center">

**Made with ❤️ by OpenClaw Community**

[GitHub](https://github.com/openclaw/openclaw) · [文档](https://docs.openclaw.ai/) · [社区](https://discord.com/invite/clawd)

</div>
