# 定时任务

使用 Cron 实现自动化。

---

## 什么是 Cron？

Cron 是 OpenClaw 的定时任务系统，可以：
- ⏰ 设置提醒
- 🔄 定期执行任务
- 📊 定时检查和报告

---

## 基本用法

### 创建一次性任务

```bash
# 在特定时间执行
openclaw cron add --name "提醒" --at "2026-03-07T16:00:00+08:00" \
  --session main --system-event "该开会了"
```

### 创建周期性任务

```bash
# 每小时执行
openclaw cron add --name "检查邮件" --every 1h \
  --session main --system-event "检查新邮件"

# 每天早上 9 点执行
openclaw cron add --name "晨报" --cron "0 9 * * *" \
  --session main --system-event "发送晨报"

# 每周一早上执行
openclaw cron add --name "周报" --cron "0 9 * * 1" \
  --session main --system-event "发送周报"
```

---

## Cron 表达式

格式：`分 时 日 月 周`

| 字段 | 范围 | 说明 |
|------|------|------|
| 分 | 0-59 | 分钟 |
| 时 | 0-23 | 小时 |
| 日 | 1-31 | 日期 |
| 月 | 1-12 | 月份 |
| 周 | 0-6 | 星期（0=周日） |

### 示例

| 表达式 | 说明 |
|--------|------|
| `0 9 * * *` | 每天早上 9 点 |
| `0 */2 * * *` | 每 2 小时 |
| `0 9 * * 1` | 每周一早上 9 点 |
| `0 9 1 * *` | 每月 1 号早上 9 点 |
| `0 9,18 * * *` | 每天早上 9 点和下午 6 点 |

---

## 管理任务

```bash
# 列出所有任务
openclaw cron list

# 查看任务详情
openclaw cron show <job-id>

# 立即执行任务
openclaw cron run <job-id>

# 删除任务
openclaw cron remove <job-id>

# 启用/禁用任务
openclaw cron enable <job-id>
openclaw cron disable <job-id>
```

---

## 任务类型

### 系统事件（System Event）

发送消息到主会话：

```bash
openclaw cron add --name "提醒喝水" --every 2h \
  --session main --system-event "该喝水了"
```

### Agent 任务（Agent Turn）

让 Agent 执行任务：

```bash
openclaw cron add --name "每日总结" --cron "0 22 * * *" \
  --session isolated --agent-turn "总结今天的工作"
```

---

## 使用场景

### 场景 1：日常提醒

```bash
# 早上提醒
openclaw cron add --name "起床提醒" --cron "0 7 * * 1-5" \
  --session main --system-event "起床啦！"

# 喝水提醒
openclaw cron add --name "喝水提醒" --every 2h \
  --session main --system-event "该喝水了"

# 睡前提醒
openclaw cron add --name "睡觉提醒" --cron "0 23 * * *" \
  --session main --system-event "该睡觉了"
```

### 场景 2：定期检查

```bash
# 检查邮件
openclaw cron add --name "检查邮件" --every 30m \
  --session isolated --agent-turn "检查是否有新邮件"

# 检查天气
openclaw cron add --name "天气提醒" --cron "0 7 * * *" \
  --session isolated --agent-turn "查询今天天气并提醒"
```

### 场景 3：自动化报告

```bash
# 每日报告
openclaw cron add --name "每日报告" --cron "0 18 * * *" \
  --session isolated --agent-turn "生成今日工作报告"

# 每周报告
openclaw cron add --name "每周报告" --cron "0 18 * * 5" \
  --session isolated --agent-turn "生成本周工作报告"
```

---

## Wake 事件

立即唤醒 Gateway：

```bash
# 立即唤醒
openclaw cron wake --text "紧急检查" --mode now

# 下次心跳时唤醒
openclaw cron wake --text "常规检查" --mode next-heartbeat
```

---

## 配置

在 `openclaw.json` 中配置：

```json5
{
  cron: {
    enabled: true,
    timezone: "Asia/Shanghai",
  },
}
```

---

## 故障排查

### Q: 任务没有执行

**A:**
1. 确认 Gateway 正在运行
2. 检查任务是否启用：`openclaw cron list`
3. 查看 Gateway 日志

### Q: 时区不正确

**A:**
1. 配置时区：`"timezone": "Asia/Shanghai"`
2. 或使用带时区的时间戳

### Q: 任务执行失败

**A:**
1. 查看 Gateway 日志
2. 检查任务配置
3. 手动运行测试：`openclaw cron run <job-id>`

---

## 下一步

- [Sub-Agents](./sub-agents.md)
- [浏览器工具](./browser.md)
