# 故障排查

遇到问题？这里有你需要的答案。

---

## 🔧 诊断工具

### openclaw doctor

**快速诊断所有问题：**

```bash
# 运行诊断
openclaw doctor

# 自动修复
openclaw doctor --fix
```

**诊断输出示例：**

```
╔════════════════════════════════════════════════════════════╗
║              OpenClaw 系统诊断                              ║
╠════════════════════════════════════════════════════════════╣
║                                                            ║
║  ✓ Node.js 版本        v22.11.0                          ║
║  ✓ OpenClaw 版本       v1.0.0                            ║
║  ✓ Gateway 状态        运行中                             ║
║  ✓ 配置文件            有效                               ║
║  ✓ 工作目录            存在                               ║
║                                                            ║
║  ⚠ WhatsApp            未配置                             ║
║  ✓ Telegram            已连接                             ║
║  ⚠ Discord             未配置                             ║
║                                                            ║
║  建议：                                                    ║
║  - 配置至少一个渠道                                        ║
║                                                            ║
╚════════════════════════════════════════════════════════════╝
```

---

### 查看日志

```bash
# 实时日志
openclaw logs --follow

# 最近 100 行
openclaw logs --lines 100

# 保存到文件
openclaw logs --lines 1000 > openclaw.log

# 过滤特定关键词
openclaw logs --follow | grep -i error
```

**日志级别：**

```
[INFO]     正常信息
[WARN]     警告（不影响运行）
[ERROR]    错误（需要关注）
[DEBUG]    调试信息（详细信息）
```

---

### Gateway 状态

```bash
# 检查 Gateway 状态
openclaw gateway status

# 重启 Gateway
openclaw gateway restart

# 停止 Gateway
openclaw gateway stop

# 启动 Gateway
openclaw gateway start
```

---

### 渠道状态

```bash
# 查看所有渠道连接状态
openclaw channels status
```

**输出示例：**

```
Channel      Status           Account              Last Active
───────────────────────────────────────────────────────────────
whatsapp     connected        +86138****1234       2 min ago
telegram     connected        @my_bot              5 min ago
discord      disconnected     -                    -
```

---

## 📊 问题分类

### 🚨 安装问题

#### Q: 安装脚本报错 "Permission denied"

**Windows:**

```powershell
# 以管理员身份运行 PowerShell
# 按 Win + X → Windows PowerShell (管理员)
```

**macOS/Linux:**

```bash
# 使用 sudo
sudo curl -fsSL https://openclaw.ai/install.sh | bash
```

---

#### Q: "command not found: openclaw"

**解决方案：**

```bash
# 1. 重启终端

# 2. 检查 PATH
echo $PATH  # macOS/Linux
echo $env:PATH  # Windows

# 3. 重新安装
npm install -g openclaw@latest

# 4. 验证
which openclaw  # macOS/Linux
where openclaw  # Windows
```

---

#### Q: Node.js 版本过低

**错误信息：**

```
Error: OpenClaw requires Node.js 22 or higher
Current version: v18.17.0
```

**解决方案：**

<Tabs>
<Tab label="使用 nvm">

```bash
# 安装 nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# 安装 Node.js 22
nvm install 22
nvm use 22

# 验证
node --version
```

</Tab>
<Tab label="Windows">

```powershell
# 使用 Chocolatey
choco install nodejs-lts

# 或从官网下载
# https://nodejs.org/
```

</Tab>
<Tab label="macOS">

```bash
# 使用 Homebrew
brew install node@22

# 验证
node --version
```

</Tab>
</Tabs>

---

### 🔌 Gateway 问题

#### Q: Gateway 无法启动

**诊断步骤：**

```bash
# 1. 查看详细错误
openclaw logs --lines 50

# 2. 检查配置文件
openclaw doctor

# 3. 检查端口占用
# Windows
netstat -ano | findstr :18789

# macOS/Linux
lsof -i :18789

# 4. 尝试前台运行（查看详细错误）
openclaw gateway start --foreground
```

**常见原因：**

| 原因 | 解决方案 |
|------|----------|
| 端口被占用 | 更换端口或关闭占用进程 |
| 配置文件错误 | 运行 `openclaw doctor` |
| 权限不足 | 使用管理员权限 |
| 内存不足 | 释放内存资源 |

---

#### Q: Gateway 频繁重启

**检查日志：**

```bash
openclaw logs --follow | grep -i "restart\|crash\|error"
```

**常见原因：**

1. **配置文件语法错误**
   ```bash
   openclaw doctor
   ```

2. **渠道连接问题**
   ```bash
   openclaw channels status
   ```

3. **内存不足**
   ```bash
   # 检查系统内存
   free -h  # Linux
   top      # macOS
   ```

---

### 📱 渠道问题

#### Q: WhatsApp 显示 "连接失败"

**解决方案：**

```
步骤 1: 检查 WhatsApp 版本
├─ iOS: App Store → 更新
└─ Android: Play Store → 更新

步骤 2: 重新登录
├─ openclaw channels logout --channel whatsapp
└─ openclaw channels login --channel whatsapp

步骤 3: 检查网络
├─ ping web.whatsapp.com
└─ 检查防火墙设置

步骤 4: 重启 Gateway
└─ openclaw gateway restart
```

---

#### Q: Telegram Bot 不响应

**检查清单：**

```bash
# ✓ Token 正确
cat ~/.openclaw/openclaw.json | grep botToken

# ✓ Gateway 运行中
openclaw gateway status

# ✓ Bot 配置正确
openclaw channels status

# ✓ 已完成配对
openclaw pairing list telegram

# ✓ 查看日志
openclaw logs --follow | grep telegram
```

**最常见原因：**

1. **Token 错误** - 重新从 @BotFather 获取
2. **未完成配对** - 运行 `openclaw pairing approve`
3. **dmPolicy 限制** - 检查 `allowFrom` 配置

---

#### Q: Discord Bot 离线

**检查步骤：**

```
1. Token 正确？
   └─ Discord Developer Portal → Bot → Reset Token

2. Message Content Intent 启用？
   └─ Discord Developer Portal → Bot → 
      启用 "Message Content Intent"

3. Gateway 运行中？
   └─ openclaw gateway status

4. 查看日志
   └─ openclaw logs --follow | grep discord
```

---

### ⚙️ 配置问题

#### Q: 配置文件修改后不生效

**解决方案：**

```bash
# 1. 等待几秒（支持热重载）

# 2. 检查语法
openclaw doctor

# 3. 重启 Gateway
openclaw gateway restart

# 4. 查看当前配置
openclaw config get
```

---

#### Q: "Configuration validation failed"

**错误示例：**

```
Error: Configuration validation failed
  - channels.whatsapp.dmPolicy: must be one of [pairing, allowlist, open, disabled]
  - agents.defaults.model: invalid model name
```

**解决方案：**

```bash
# 1. 查看配置 schema
openclaw config schema

# 2. 检查 JSON5 语法
# 常见错误：
# - 缺少逗号
# - 多余的逗号（最后一个字段后）
# - 引号不匹配

# 3. 使用 JSON 验证工具
cat ~/.openclaw/openclaw.json | python -m json.tool
```

---

### 🔐 访问控制问题

#### Q: 用户无法触发机器人

**诊断流程：**

```
1. 检查 dmPolicy
   └─ pairing: 需要批准配对
   └─ allowlist: 需要在白名单中
   └─ open: 允许所有人

2. 检查 allowFrom
   └─ 电话号码格式: +8613800138000
   └─ 确保包含国家代码

3. 检查配对状态
   └─ openclaw pairing list <channel>

4. 群组: 检查 groupPolicy
   └─ 尝试 @提及 机器人
```

---

#### Q: 配对码无效或过期

**解决方案：**

```bash
# 配对码通常 5 分钟后过期

# 1. 让用户重新发送消息获取新配对码

# 2. 查看当前待配对列表
openclaw pairing list <channel>

# 3. 批准配对
openclaw pairing approve <channel> <CODE>
```

---

## 🛠️ 高级调试

### 启用调试日志

```bash
# 设置日志级别
export LOG_LEVEL=debug

# 或在配置文件中
{
  logging: {
    level: "debug",
  },
}

# 重启 Gateway
openclaw gateway restart
```

### 手动测试渠道连接

```bash
# 测试 Telegram
curl "https://api.telegram.org/bot<YOUR_TOKEN>/getMe"

# 测试 Discord
curl -H "Authorization: Bot <YOUR_TOKEN>" \
  https://discord.com/api/users/@me
```

### 检查网络连接

```bash
# 测试 OpenClaw 服务
curl http://localhost:18789/healthz

# 测试外部 API
ping api.openai.com
ping api.anthropic.com
```

---

## 📞 获取帮助

### 官方资源

| 资源 | 链接 |
|------|------|
| 📚 官方文档 | https://docs.openclaw.ai/ |
| 🐙 GitHub Issues | https://github.com/openclaw/openclaw/issues |
| 💬 Discord 社区 | https://discord.com/invite/clawd |

### 提问时请提供

```markdown
**环境信息：**
- 操作系统: Windows 11 / macOS 14 / Ubuntu 22.04
- Node.js 版本: v22.11.0
- OpenClaw 版本: v1.0.0

**问题描述：**
[详细描述问题]

**复现步骤：**
1. 运行命令 xxx
2. 看到错误 xxx

**日志输出：**
```
[粘贴相关日志，去除敏感信息]
```

**配置文件：**
```json5
[粘贴配置，去除 Token 等敏感信息]
```
```

---

## 📝 常用命令速查

```bash
# ========== 诊断 ==========
openclaw doctor                    # 系统诊断
openclaw doctor --fix              # 自动修复

# ========== 状态 ==========
openclaw gateway status            # Gateway 状态
openclaw channels status           # 渠道状态
openclaw --version                 # 版本信息

# ========== 日志 ==========
openclaw logs --follow             # 实时日志
openclaw logs --lines 100          # 最近日志

# ========== 重启 ==========
openclaw gateway restart           # 重启 Gateway
openclaw gateway stop              # 停止 Gateway
openclaw gateway start             # 启动 Gateway

# ========== 配置 ==========
openclaw config schema             # 查看配置格式
openclaw config get                # 获取当前配置

# ========== 渠道 ==========
openclaw channels login --channel <channel>   # 登录渠道
openclaw channels logout --channel <channel>  # 登出渠道
openclaw pairing list <channel>               # 查看配对
openclaw pairing approve <channel> <code>     # 批准配对

# ========== 安全 ==========
openclaw security audit            # 安全审计
openclaw security audit --fix      # 自动修复
```

---

## 🔗 相关文档

- [快速开始](/docs/quick-start)
- [WhatsApp 配置](/docs/channels/whatsapp)
- [Telegram 配置](/docs/channels/telegram)
- [Discord 配置](/docs/channels/discord)
