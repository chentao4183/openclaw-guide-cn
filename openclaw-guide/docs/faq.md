# 常见问题 (FAQ)

---

## 安装相关

### Q: Node.js 版本不对怎么办？

**A:** OpenClaw 需要 Node.js 22+

```bash
# 检查版本
node --version

# macOS 升级
brew install node@22

# Ubuntu/Debian
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs

# Windows
# 去 https://nodejs.org 下载最新 LTS 版本
```

---

### Q: 安装脚本执行失败？

**A:** 尝试手动安装：

```bash
npm install -g openclaw@latest
```

如果还是很慢，尝试国内镜像：

```bash
npm install -g openclaw@latest --registry=https://registry.npmmirror.com
```

---

### Q: Windows 上安装报错？

**A:** 建议：
1. 使用 **WSL2**（Windows Subsystem for Linux）
2. 或使用 **PowerShell 7+**（不是 Windows PowerShell 5.1）

---

## 配置相关

### Q: 配置文件在哪里？

**A:** `~/.openclaw/openclaw.json`

```bash
# 查看配置
cat ~/.openclaw/openclaw.json

# 编辑配置
nano ~/.openclaw/openclaw.json
# 或
code ~/.openclaw/openclaw.json
```

---

### Q: 配置改了不生效？

**A:** 大部分配置支持**热重载**，会自动生效。

如果不行，重启 Gateway：

```bash
openclaw gateway restart
```

---

### Q: 配置文件格式错误？

**A:** OpenClaw 使用 **JSON5** 格式（支持注释和尾随逗号）

**常见错误：**
```json5
// ❌ 错误：缺少逗号
{
  channels: {
    whatsapp: { }
    telegram: { }  // ← 这里需要逗号
  }
}

// ✅ 正确
{
  channels: {
    whatsapp: { },
    telegram: { },  // ← 尾随逗号可以
  },
}
```

运行诊断：
```bash
openclaw doctor
```

---

## Gateway 相关

### Q: Gateway 启动失败？

**A:** 按顺序检查：

```bash
# 1. 查看详细日志
openclaw logs --follow

# 2. 运行诊断
openclaw doctor

# 3. 检查端口占用
lsof -i :18789

# 4. 尝试前台运行（看错误信息）
openclaw gateway --port 18789
```

---

### Q: Gateway 一直重启？

**A:** 可能原因：

1. **配置文件错误** → 运行 `openclaw doctor`
2. **端口被占用** → 改端口或杀掉占用进程
3. **权限问题** → 检查 `~/.openclaw` 权限

```bash
# 修复权限
chmod 700 ~/.openclaw
chmod 600 ~/.openclaw/openclaw.json
```

---

### Q: 控制面板打不开？

**A:** 检查：

```bash
# 1. Gateway 是否运行
openclaw gateway status

# 2. 端口是否监听
curl http://127.0.0.1:18789/healthz

# 3. 浏览器访问
# http://127.0.0.1:18789/
```

---

## 渠道相关

### Q: WhatsApp 扫码后无响应？

**A:**

```bash
# 检查连接状态
openclaw channels status

# 重新登录
openclaw channels logout --channel whatsapp
openclaw channels login --channel whatsapp
```

---

### Q: Telegram Bot 不回复？

**A:** 检查清单：

1. ✅ Token 是否正确
2. ✅ `dmPolicy` 不是 `disabled`
3. ✅ `allowFrom` 包含你的用户 ID
4. ✅ Gateway 正在运行

```bash
# 测试 Bot 连接
curl "https://api.telegram.org/bot<你的token>/getMe"
```

---

### Q: 群消息无响应？

**A:** 常见原因：

1. **没有 @ 提及** → 默认需要 @bot
2. **隐私模式** → BotFather 中关闭
3. **群组不在白名单** → 检查 `groups` 配置

---

### Q: 配对码收不到？

**A:**

- 检查 `allowFrom` 配置
- 确认 `dmPolicy` 是 `pairing` 或 `allowlist`
- 查看日志：`openclaw logs --follow`

---

## API Key 相关

### Q: OpenAI API Key 在哪里获取？

**A:**

1. 登录 https://platform.openai.com/
2. 点击右上角头像 → View API keys
3. Create new secret key
4. 复制（只显示一次）

---

### Q: Anthropic API Key 在哪里获取？

**A:**

1. 登录 https://console.anthropic.com/
2. API Keys → Create Key
3. 复制

---

### Q: API Key 泄露了怎么办？

**A:** 立即：

1. **撤销旧 Key**（在提供商控制台）
2. **生成新 Key**
3. **更新配置**：`~/.openclaw/openclaw.json`
4. **重启 Gateway**：`openclaw gateway restart`

---

## 性能相关

### Q: 响应很慢怎么办？

**A:** 可能原因：

1. **模型太慢** → 换更快的模型
2. **网络问题** → 检查网络连接
3. **上下文太长** → 清理会话：`/new`

---

### Q: Token 消耗太快？

**A:** 优化方法：

1. **用更便宜的模型**（Sonnet > Opus）
2. **减少上下文** → 定期 `/new`
3. **限制工具** → 禁用不常用工具

---

### Q: 内存占用很高？

**A:** 清理会话：

```bash
# 查看会话
openclaw sessions --json

# 清理旧会话
openclaw sessions cleanup --enforce
```

---

## 安全相关

### Q: 如何限制谁可以使用？

**A:** 配置访问控制：

```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "allowlist",
      allowFrom: ["+你的手机号"],
    },
  },
}
```

---

### Q: 如何防止 Prompt 注入？

**A:** 多层防护：

1. **限制访问** → `dmPolicy: "allowlist"`
2. **禁用危险工具** → `tools.deny: ["exec", "write"]`
3. **启用沙箱** → `sandbox.mode: "all"`
4. **用强模型** → Claude Sonnet / GPT-4

```bash
# 运行安全审计
openclaw security audit
```

---

### Q: 如何在公网部署？

**A:** 推荐 **Tailscale**（不要直接暴露端口）：

```json5
{
  gateway: {
    bind: "loopback",  // 只监听本地
  },
  // 通过 Tailscale Serve 暴露
}
```

详见：[远程访问文档](https://docs.openclaw.ai/gateway/remote)

---

## Docker 相关

### Q: Docker 部署后无法访问？

**A:** 检查：

```bash
# 容器是否运行
docker compose ps

# 查看日志
docker compose logs -f openclaw-gateway

# 检查端口
docker compose port openclaw-gateway 18789
```

---

### Q: Docker 镜像很大？

**A:** 正常，包含：
- Node.js 运行时
- 浏览器（可选）
- 依赖库

**优化**：使用预构建镜像：

```bash
export OPENCLAW_IMAGE="ghcr.io/openclaw/openclaw:latest"
./docker-setup.sh
```

---

## 其他问题

### Q: 如何升级到最新版？

**A:**

```bash
# 重新运行安装脚本
curl -fsSL https://openclaw.ai/install.sh | bash

# 或 npm
npm install -g openclaw@latest

# 重启
openclaw gateway restart
```

---

### Q: 如何完全卸载？

**A:**

```bash
# 运行卸载脚本
openclaw uninstall

# 或手动删除
rm -rf ~/.openclaw
npm uninstall -g openclaw
```

---

### Q: 在哪里寻求帮助？

**A:**

1. **文档**：https://docs.openclaw.ai/
2. **社区**：https://discord.com/invite/clawd
3. **GitHub Issues**：https://github.com/openclaw/openclaw/issues
4. **运行诊断**：`openclaw doctor`

---

## 没找到答案？

1. 查看 [官方文档](https://docs.openclaw.ai/)
2. 搜索 [GitHub Issues](https://github.com/openclaw/openclaw/issues)
3. 加入 [Discord 社区](https://discord.com/invite/clawd)
4. 提交新 Issue（附上 `openclaw doctor` 输出）

---

_持续更新中..._
