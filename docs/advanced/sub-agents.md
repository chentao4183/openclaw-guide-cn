# Sub-Agents 子 Agent

在后台运行独立的 Agent 任务。

---

## 什么是 Sub-Agents？

Sub-Agents 允许你：
- 在后台执行长时间任务
- 使用不同的模型或配置
- 并行处理多个任务
- 任务完成后自动通知

---

## 基本用法

### 创建子 Agent 任务

在对话中告诉 AI：

```
"在后台帮我分析这个文档"
"启动一个子任务来爬取这个网站"
"用另一个 Agent 处理这个请求"
```

或者使用命令：

```bash
/subagents spawn <agent-id> <task>
```

---

## 配置 Sub-Agents

### Agent 列表

```json5
{
  agents: {
    list: [
      { id: "main", default: true },
      { id: "research", model: "claude-3-5-sonnet-20241022" },
      { id: "coding", model: "claude-3-5-sonnet-20241022" },
    ],
  },
}
```

### 允许的 Agent

限制可用的子 Agent：

```json5
{
  agents: {
    spawnAllowlist: ["research", "coding"],
  },
}
```

---

## 使用场景

### 场景 1：后台研究

```
"启动一个研究任务，调查 OpenClaw 的最新功能"
```

AI 会：
1. 创建子 Agent
2. 在后台执行研究
3. 完成后通知你结果

### 场景 2：代码生成

```
"用 coding Agent 生成一个 Python 爬虫"
```

### 场景 3：并行任务

```
"同时分析这三个文档"
```

---

## 管理子 Agent

### 查看运行中的任务

```bash
# 列出所有会话
openclaw sessions list

# 查看特定会话
openclaw sessions history <session-key>
```

### 发送消息到子 Agent

```bash
openclaw sessions send --session-key <key> --message "进度如何？"
```

---

## 子 Agent 特点

### 隔离性

- 独立的会话历史
- 独立的工作目录
- 独立的配置

### 自动通知

任务完成后自动通知主会话：

```
[子 Agent 完成] 研究任务已完成，发现以下内容...
```

### 超时控制

```json5
{
  agents: {
    defaults: {
      spawnTimeout: 300000,  // 5 分钟
    },
  },
}
```

---

## 会话管理

### 会话类型

| 类型 | 说明 |
|------|------|
| `main` | 主会话 |
| `isolated` | 隔离子会话 |
| `subagent` | 子 Agent 会话 |

### 会话作用域

```json5
{
  session: {
    dmScope: "per-channel-peer",
  },
}
```

---

## 最佳实践

### 1. 合理分配任务

- 简单任务 → 主会话
- 长时间任务 → 子 Agent
- 需要不同配置 → 子 Agent

### 2. 设置超时

避免任务无限运行：

```json5
{
  agents: {
    defaults: {
      spawnTimeout: 600000,  // 10 分钟
    },
  },
}
```

### 3. 监控资源

子 Agent 会消耗额外的 API 配额和资源。

---

## 故障排查

### Q: 子 Agent 无响应

**A:**
1. 检查 Gateway 日志
2. 确认 Agent 配置正确
3. 检查是否达到超时限制

### Q: 任务结果未返回

**A:**
1. 查看会话历史
2. 检查通知配置
3. 查看 Gateway 日志

---

## 下一步

- [浏览器工具](./browser.md)
- [定时任务](./cron.md)
