# 故障排查

遇到问题？这里有你需要的答案。

---

## 🔧 诊断工具

### openclaw doctor

快速诊断常见问题：

```bash
# 运行诊断
openclaw doctor

# 自动修复
openclaw doctor --fix
```

### 查看日志

```bash
# 实时日志
openclaw logs --follow

# 最近 100 行
openclaw logs --lines 100
```

### Gateway 状态

```bash
# 检查 Gateway 状态
openclaw gateway status

# 重启 Gateway
openclaw gateway restart
```

### 渠道状态

```bash
# 查看所有渠道连接状态
openclaw channels status
```

---

## ❓ 常见问题

### 安装问题

#### Q: 安装脚本报错 "Permission denied"

**A:**
- **macOS/Linux**: 确保有执行权限，或使用 `sudo`
- **Windows**: 以管理员身份运行 PowerShell

#### Q: "command not found: openclaw"

**A:**
1. 重启终端
2. 检查 PATH 环境变量
3. 重新安装：
   ```bash
   npm install -g openclaw@latest
   ```

#### Q: Node.js 版本过低

**A:**
OpenClaw 需要 Node.js 22+
```bash
# 使用 nvm 安装
nvm install 22
nvm use 22
```

---

### Gateway 问题

#### Q: Gateway 无法启动

**A:**
1. 运行诊断：`openclaw doctor`
2. 查看日志：`openclaw logs --follow`
3. 检查端口占用（默认 18789）
4. 尝试重启：`openclaw gateway restart`

#### Q: Gateway 频繁重启

**A:**
1. 检查配置文件语法：`openclaw doctor`
2. 查看错误日志
3. 检查内存是否充足

---

### 渠道问题

#### Q: WhatsApp 显示 "连接失败"

**A:**
1. 重新登录：`openclaw channels login --channel whatsapp`
2. 确保手机 WhatsApp 是最新版本
3. 检查网络连接
4. 查看 Gateway 日志

#### Q: Telegram Bot 不响应

**A:**
1. 确认 `botToken` 正确
2. 检查 `dmPolicy` 设置
3. 确认已完成配对
4. 重启 Gateway

#### Q: Discord Bot 离线

**A:**
1. 确认 `botToken` 正确
2. 确认启用了 **Message Content Intent**
3. 查看 Gateway 日志
4. 重启 Gateway

---

### 配置问题

#### Q: 配置文件修改后不生效

**A:**
1. 大部分配置支持热重载，等待几秒
2. 某些配置需要重启：`openclaw gateway restart`
3. 检查 JSON5 语法

#### Q: "Configuration validation failed"

**A:**
1. 运行：`openclaw doctor`
2. 检查 JSON5 语法（逗号、引号）
3. 查看配置 schema：`openclaw config schema`

---

### 访问控制问题

#### Q: 用户无法触发机器人

**A:**
1. 检查 `dmPolicy` / `groupPolicy`
2. 确认用户在 `allowFrom` 列表中
3. 检查配对状态：`openclaw pairing list <channel>`
4. 群组中尝试 @提及

#### Q: 配对码无效

**A:**
1. 确认配对码未过期（通常 5 分钟）
2. 重新发起配对请求
3. 查看待配对列表

---

## 🆘 获取帮助

### 官方资源
- 📚 [官方文档](https://docs.openclaw.ai/)
- 🐙 [GitHub Issues](https://github.com/openclaw/openclaw/issues)
- 💬 [Discord 社区](https://discord.com/invite/clawd)

### 提问时请提供

1. OpenClaw 版本：`openclaw --version`
2. 操作系统版本
3. 问题描述和复现步骤
4. 相关日志（去除敏感信息）

---

## 📝 常用命令速查

```bash
# 诊断
openclaw doctor

# 状态检查
openclaw gateway status
openclaw channels status

# 日志
openclaw logs --follow

# 重启
openclaw gateway restart

# 配置检查
openclaw config schema

# 安全审计
openclaw security audit
```
