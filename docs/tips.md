# 使用技巧与最佳实践

> 提升 OpenClaw 使用效率的实用技巧

![技巧](https://img.shields.io/badge/技巧-最佳实践-blue)

---

## 🚀 性能优化技巧

### 1. 提升响应速度

#### 选择响应快的模型
```json5
{
  agents: {
    defaults: {
      model: "doubao",  // 响应速度较快
      modelConfig: {
        temperature: 0.7,  // 适当降低随机性
        maxTokens: 1024,  // 限制输出长度
      },
    },
  },
}
```

#### 启用消息缓存
```json5
{
  agents: {
    defaults: {
      cache: {
        enabled: true,
        ttl: 3600,  // 缓存 1 小时
        maxSize: 100,  // 最多缓存 100 条
      },
    },
  },
}
```

---

### 2. 降低 API 成本

#### 使用国内模型
```json5
{
  agents: {
    defaults: {
      model: "doubao",  // 豆包性价比高
      // 或
      model: "deepseek",  // DeepSeek 最便宜
    },
  },
}
```

#### 优化提示词
- 简化系统提示词
- 避免重复内容
- 使用简洁指令

#### 限制输出长度
```json5
{
  agents: {
    defaults: {
      modelConfig: {
        maxTokens: 512,  // 限制输出长度
      },
    },
  },
}
```

---

## 🤖 多 Agent 协作技巧

### 3. 场景化 Agent 配置

#### 按场景分配 Agent
```json5
{
  agents: {
    list: [
      {
        id: "home",
        default: true,
        model: "doubao",  // 日常对话用快速模型
        workspace: "~/.openclaw/workspace-home",
      },
      {
        id: "work",
        model: "glm-4",  // 工作用长文本模型
        workspace: "~/.openclaw/workspace-work",
      },
      {
        id: "code",
        model: "deepseek-coder",  // 代码专用
        workspace: "~/.openclaw/workspace-code",
      },
    ],
  },
}
```

#### 按渠道绑定 Agent
```json5
{
  bindings: [
    {
      agentId: "home",
      match: { channel: "whatsapp", accountId: "personal" },
    },
    {
      agentId: "work",
      match: { channel: "wechat-work" },
    },
    {
      agentId: "code",
      match: { channel: "telegram", accountId: "dev_group" },
    },
  ],
}
```

---

### 4. Agent 间协作

#### 使用 Sub-Agents
```bash
# 主 Agent 可以调用 Sub-Agent 处理复杂任务
openclaw subagent spawn --task "分析这段代码的性能瓶颈"
```

#### 配置 Sub-Agent
```json5
{
  agents: {
    defaults: {
      subAgents: {
        enabled: true,
        maxConcurrent: 3,  // 最多 3 个并发
        timeout: 60000,  // 超时 60 秒
      },
    },
  },
}
```

---

## 🛠️ Skills 组合使用

### 5. 常用 Skills 组合

#### 信息收集 + 分析
```bash
# 安装 Skills
clawhub install web-search
clawhub install data-analysis

# 组合使用
# AI 助手会自动调用：先搜索 → 再分析
```

#### 自动化工作流
```json5
{
  skills: {
    enabled: ["weather", "calendar", "email"],
    workflows: [
      {
        trigger: "每天早上 8 点",
        actions: [
          "查询天气",
          "检查日程",
          "发送邮件提醒",
        ],
      },
    ],
  },
}
```

---

### 6. 自定义 Skills 技巧

#### 创建场景化 Skill
```markdown
<!-- skills/my-skill/SKILL.md -->
# 客户服务助手

## 触发词
- 客户服务
- 售后支持
- 投诉处理

## 能力
- 查询订单状态
- 处理退款申请
- 回答常见问题

## 工具
- order-api
- refund-system
```

---

## 🔄 自动化最佳实践

### 7. 定时任务配置

#### 每日报告
```json5
{
  cron: {
    jobs: [
      {
        id: "daily-report",
        schedule: "0 9 * * *",  // 每天 9 点
        task: "生成昨日数据报告",
        channel: "dingtalk",
        group: "report_group",
      },
    ],
  },
}
```

#### 监控预警
```json5
{
  cron: {
    jobs: [
      {
        id: "monitor-alert",
        schedule: "*/5 * * * *",  // 每 5 分钟
        task: "检查系统状态",
        alertOn: ["error", "warning"],
      },
    ],
  },
}
```

---

### 8. 事件驱动自动化

#### 消息触发
```json5
{
  channels: {
    "wechat-work": {
      events: {
        enabled: true,
        triggers: [
          {
            type: "message",
            pattern: "订单号：(\\d+)",
            action: "查询订单状态",
          },
        ],
      },
    },
  },
}
```

#### 审批触发
```json5
{
  channels: {
    "feishu": {
      events: {
        enabled: true,
        triggers: [
          {
            type: "approval",
            status: "approved",
            action: "通知相关人员",
          },
        ],
      },
    },
  },
}
```

---

## 🔐 安全与权限

### 9. 访问控制最佳实践

#### 严格模式（生产环境）
```json5
{
  channels: {
    "wechat-work": {
      dmPolicy: "allowlist",  // 仅允许列表用户
      allowFrom: ["user_id_1", "user_id_2"],
      groupPolicy: "allowlist",
      groups: {
        "allowed_group": { requireMention: true },
      },
    },
  },
}
```

#### 开放模式（测试环境）
```json5
{
  channels: {
    "telegram": {
      dmPolicy: "open",  // 允许所有人
      allowFrom: ["*"],
    },
  },
}
```

---

### 10. 敏感信息保护

#### 过滤敏感词
```json5
{
  agents: {
    defaults: {
      contentFilter: {
        enabled: true,
        patterns: ["password", "token", "secret", "key"],
        action: "mask",  // mask | reject
      },
    },
  },
}
```

#### 日志脱敏
```json5
{
  logging: {
    level: "info",
    sanitize: true,  // 自动脱敏
    excludeFields: ["password", "token"],
  },
}
```

---

## 📊 监控与调试

### 11. 实时监控

#### 启用监控
```json5
{
  monitoring: {
    enabled: true,
    metrics: ["requests", "latency", "errors"],
    interval: 60,  // 每 60 秒上报
  },
}
```

#### 查看监控数据
```bash
# 查看实时状态
openclaw status

# 查看详细指标
openclaw metrics --follow
```

---

### 12. 调试技巧

#### 启用调试模式
```bash
# 启用详细日志
export OPENCLAW_DEBUG=true
openclaw gateway start

# 查看详细日志
openclaw logs --follow --level debug
```

#### 测试工具调用
```bash
# 测试单个工具
openclaw test --tool weather

# 测试模型连接
openclaw test --model doubao
```

---

## 🎯 用户体验优化

### 13. 个性化欢迎消息

```json5
{
  channels: {
    "wechat-work": {
      welcomeMessage: {
        enabled: true,
        text: "你好！我是 AI 助手，可以帮你：\n1. 查询订单\n2. 处理投诉\n3. 回答问题\n\n有什么可以帮助你的？",
      },
    },
  },
}
```

---

### 14. 智能引导

```json5
{
  agents: {
    defaults: {
      guidance: {
        enabled: true,
        suggestions: [
          "你可以问我：今天天气如何？",
          "试试说：帮我查询订单 12345",
          "或者：设置明天 9 点的提醒",
        ],
      },
    },
  },
}
```

---

### 15. 错误处理优化

```json5
{
  agents: {
    defaults: {
      errorHandling: {
        retryAttempts: 3,  // 重试 3 次
        fallbackMessage: "抱歉，我暂时无法处理这个请求。请稍后再试或联系人工客服。",
        notifyAdmin: true,  // 通知管理员
      },
    },
  },
}
```

---

## 💡 更多技巧

- **定期清理会话**：`openclaw session prune`
- **备份配置**：定期备份 `~/.openclaw/` 目录
- **关注更新**：订阅 GitHub Releases
- **参与社区**：加入微信群交流经验

---

**遇到问题？** → [常见问题 FAQ](/docs/faq)  
**分享技巧？** → [加入社区](/docs/community)
