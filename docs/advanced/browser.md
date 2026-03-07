# 浏览器工具

自动化浏览和操作网页。

---

## 什么是浏览器工具？

浏览器工具让 AI 能够：
- 🌐 打开和浏览网页
- 📸 截取网页截图
- 🔍 提取网页内容
- 🖱️ 自动化操作（点击、输入）

---

## 启动浏览器

### 检查状态

```bash
openclaw browser status
```

### 启动浏览器

```bash
openclaw browser start
```

### 打开网页

```bash
openclaw browser open https://example.com
```

---

## 在对话中使用

直接告诉 AI：

```
"打开 GitHub 查看 openclaw 仓库"
"帮我截取这个网页的截图"
"在这个表单中填入信息"
```

---

## 浏览器操作

### 导航

```bash
# 打开 URL
openclaw browser open https://docs.openclaw.ai

# 后退
openclaw browser back

# 前进
openclaw browser forward

# 刷新
openclaw browser reload
```

### 截图

```bash
# 截取当前页面
openclaw browser screenshot

# 截取完整页面
openclaw browser screenshot --full-page

# 保存到文件
openclaw browser screenshot --output screenshot.png
```

### 快照

获取页面结构（用于自动化）：

```bash
# 获取页面快照
openclaw browser snapshot
```

返回页面的可交互元素列表。

### 自动化操作

```bash
# 点击元素
openclaw browser click --selector "#submit-button"

# 输入文本
openclaw browser type --selector "#search-input" --text "OpenClaw"

# 按键
openclaw browser press --key Enter
```

---

## 使用场景

### 场景 1：网页内容提取

```
"打开这篇博客文章，提取主要内容"
```

AI 会：
1. 打开网页
2. 解析内容
3. 返回结构化信息

### 场景 2：自动化表单

```
"帮我在这个网站注册账号"
```

AI 会：
1. 定位表单
2. 填写信息
3. 提交表单

### 场景 3：网页监控

```
"每天检查这个网站的价格变化"
```

结合 Cron 实现自动化监控。

---

## 浏览器配置

### 配置文件

```json5
{
  browser: {
    enabled: true,
    headless: true,  // 无头模式
    timeout: 30000,  // 超时时间（毫秒）
  },
}
```

### 代理设置

```json5
{
  browser: {
    proxy: {
      server: "http://proxy.example.com:8080",
      username: "user",
      password: "pass",
    },
  },
}
```

---

## Canvas 画布

在节点上展示网页：

```bash
# 在节点上打开网页
openclaw canvas present --node <node-id> --url https://example.com

# 执行 JavaScript
openclaw canvas eval --node <node-id> --script "document.title"

# 截取画布
openclaw canvas snapshot --node <node-id>
```

---

## 安全考虑

### 沙箱模式

浏览器工具在沙箱中运行，隔离风险：

```json5
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",
      },
    },
  },
}
```

### 权限控制

限制浏览器访问：

```json5
{
  browser: {
    allowedDomains: ["docs.openclaw.ai", "github.com"],
  },
}
```

---

## 故障排查

### Q: 浏览器启动失败

**A:**
1. 确认系统有足够的资源
2. 检查是否安装了浏览器
3. 查看 Gateway 日志

### Q: 页面加载超时

**A:**
1. 增加超时时间
2. 检查网络连接
3. 尝试无头模式

### Q: 截图失败

**A:**
1. 确认页面已加载完成
2. 检查存储空间
3. 查看错误日志

---

## 下一步

- [安全配置](./security.md)
- [定时任务](./cron.md)
