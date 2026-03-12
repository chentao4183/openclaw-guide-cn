# 常见问题 FAQ

> 快速解决你的疑问

![FAQ](https://img.shields.io/badge/FAQ-常见问题-blue)
![更新](https://img.shields.io/badge/更新-2026.03-lightgrey)

---

## 📦 安装相关

### Q1: 安装时提示 "Node.js 版本过低"

**A**: OpenClaw 需要 Node.js 22 或更高版本。

**解决方法**：
```bash
# 检查当前版本
node --version

# 如果版本低于 22，请升级 Node.js
# macOS/Linux
nvm install 22
nvm use 22

# Windows
# 下载并安装 Node.js 22+: https://nodejs.org/
```

---

### Q2: Windows PowerShell 提示"无法加载文件，因为在此系统上禁止运行脚本"

**A**: 这是 PowerShell 执行策略限制。

**解决方法**：
```powershell
# 以管理员身份运行 PowerShell，执行：
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# 然后重新运行安装命令
iwr -useb https://openclaw.ai/install.ps1 | iex
```

---

### Q3: 安装后运行 `openclaw` 提示"命令未找到"

**A**: 可能是 PATH 环境变量未更新。

**解决方法**：
```bash
# macOS/Linux
source ~/.bashrc  # 或 ~/.zshrc

# Windows
# 重新打开 PowerShell 窗口

# 或者使用完整路径
~/.openclaw/bin/openclaw  # macOS/Linux
%USERPROFILE%\.openclaw\bin\openclaw.cmd  # Windows
```

---

## ⚙️ 配置相关

### Q4: 配置文件在哪里？

**A**: 默认配置文件位置：
- **macOS/Linux**: `~/.openclaw/openclaw.json`
- **Windows**: `C:\Users\你的用户名\.openclaw\openclaw.json`

**查看配置**：
```bash
openclaw config get
```

---

### Q5: 如何验证配置是否正确？

**A**: 使用 `doctor` 命令：
```bash
# 检查配置
openclaw doctor

# 自动修复问题
openclaw doctor --fix
```

---

### Q6: 配置文件支持注释吗？

**A**: 支持！OpenClaw 使用 JSON5 格式，支持注释和尾随逗号：

```json5
{
  // 这是注释
  agents: {
    defaults: {
      model: "doubao",
    },  // 尾随逗号是允许的
  },
}
```

---

## 🚀 Gateway 相关

### Q7: Gateway 无法启动

**A**: 可能的原因和解决方法：

**1. 端口被占用**
```bash
# 检查端口占用
# macOS/Linux
lsof -i :18789

# Windows
netstat -ano | findstr :18789

# 修改端口（在配置文件中）
{
  gateway: {
    port: 18790,  // 改为其他端口
  },
}
```

**2. 配置文件错误**
```bash
# 验证配置
openclaw doctor

# 查看详细错误
openclaw logs --follow
```

**3. 权限问题**
```bash
# macOS/Linux
sudo openclaw gateway start

# Windows（以管理员身份运行）
```

---

### Q8: 如何查看 Gateway 日志？

**A**:
```bash
# 实时查看日志
openclaw logs --follow

# 查看最近 100 行
openclaw logs --lines 100

# 日志文件位置
# macOS/Linux: ~/.openclaw/logs/
# Windows: %USERPROFILE%\.openclaw\logs\
```

---

### Q9: Gateway 自动重启

**A**: 使用守护进程模式：
```bash
# 安装为系统服务
openclaw onboard --install-daemon

# 查看状态
openclaw gateway status

# 手动启动/停止/重启
openclaw gateway start
openclaw gateway stop
openclaw gateway restart
```

---

## 📱 渠道相关

### Q10: WhatsApp/Telegram 扫码后无反应

**A**: 可能的原因：
1. **网络问题**：确保能访问对应平台
2. **配对未批准**：检查配对请求
   ```bash
   openclaw pairing list whatsapp
   openclaw pairing approve whatsapp <CODE>
   ```
3. **配置错误**：运行 `openclaw doctor --channel whatsapp`

---

### Q11: 企业微信消息发送失败

**A**: 常见原因：
1. **权限不足**：检查应用权限配置
2. **用户 ID 错误**：确认用户 ID 正确
3. **回调 URL 不可达**：确保服务器可公网访问

**调试方法**：
```bash
# 检查渠道状态
openclaw channels status wechat-work

# 查看详细日志
openclaw logs --follow --channel wechat-work
```

---

### Q12: 飞书卡片回调无响应

**A**: 检查以下几点：
1. **回调 URL 配置正确**
2. **事件订阅已启用**
3. **防火墙允许回调请求**

**配置示例**：
```json5
{
  channels: {
    "feishu": {
      card: {
        enabled: true,
        callback: {
          url: "https://your-domain.com/webhook/feishu/card",
        },
      },
    },
  },
}
```

---

## 🤖 模型相关

### Q13: 如何切换模型？

**A**: 修改配置文件：
```json5
{
  agents: {
    defaults: {
      model: "glm-4",  // 改为你想用的模型
      modelConfig: {
        provider: "zhipu",
        apiKey: "your_api_key",
      },
    },
  },
}
```

---

### Q14: 模型响应很慢

**A**: 可能的原因：
1. **网络问题**：使用国内模型（豆包、智谱等）
2. **模型负载高**：切换到其他模型
3. **请求内容过长**：减少输入长度

**优化建议**：
```json5
{
  agents: {
    defaults: {
      model: "doubao",  // 响应较快的模型
      modelConfig: {
        temperature: 0.7,  // 降低随机性
        maxTokens: 2048,  // 限制输出长度
      },
    },
  },
}
```

---

### Q15: API Key 配置错误

**A**: 检查以下几点：
1. **Key 格式正确**：无多余空格或换行
2. **Key 未过期**：登录对应平台检查
3. **权限足够**：确保有必要的 API 权限

**验证方法**：
```bash
# 测试模型连接
openclaw test --model doubao

# 查看配置
openclaw config get agents.defaults.modelConfig
```

---

## 🔧 故障排查

### Q16: 如何重置 OpenClaw？

**A**:
```bash
# 备份配置
cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.backup

# 重置会话
openclaw session reset

# 完全重置（慎用！）
rm -rf ~/.openclaw/
openclaw onboard
```

---

### Q17: 出现"Unknown error"

**A**: 查看详细日志：
```bash
# 启用调试模式
export OPENCLAW_DEBUG=true  # macOS/Linux
$env:OPENCLAW_DEBUG="true"  # Windows PowerShell

openclaw gateway start

# 查看日志
openclaw logs --follow
```

---

### Q18: CPU/内存占用过高

**A**: 优化建议：
1. **减少并发会话**
2. **降低模型 maxTokens**
3. **禁用不必要的 Skills**

```json5
{
  agents: {
    defaults: {
      maxConcurrentSessions: 5,  // 限制并发
      modelConfig: {
        maxTokens: 1024,  // 减少输出长度
      },
    },
  },
}
```

---

## 💬 社区相关

### Q19: 如何加入微信群？

**A**:
1. 访问 [社区页面](/docs/community)
2. 扫描微信群二维码
3. 添加时备注：`行业 + 学习目的`

---

### Q20: 在哪里提问最合适？

**A**: 根据问题类型选择：
- **安装配置问题**：微信群（响应快）
- **Bug 报告**：[GitHub Issues](https://github.com/openclaw/openclaw/issues)
- **功能建议**：[GitHub Discussions](https://github.com/openclaw/openclaw/discussions)
- **文档问题**：[文档仓库](https://github.com/openclaw/openclaw-docs)

---

## 📚 更多资源

- [使用技巧](/docs/tips)
- [故障排查指南](/docs/troubleshooting)
- [社区讨论](https://github.com/openclaw/openclaw/discussions)

---

**没找到答案？**  
→ [加入微信群提问](/docs/community)  
→ [提交 GitHub Issue](https://github.com/openclaw/openclaw/issues)
