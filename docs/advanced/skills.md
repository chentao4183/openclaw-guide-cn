# Skills 技能系统

扩展 AI 助手的能力。

---

## 什么是 Skills？

Skills 是 OpenClaw 的插件系统，让 AI 能够：
- 搜索网络
- 控制智能家居
- 读取日历
- 执行自定义脚本
- ...更多可能

---

## Skills 位置

| 位置 | 说明 |
|------|------|
| `workspace/skills/` | 用户自定义技能 |
| `~/.openclaw/skills/` | 全局技能 |
| Bundled | 内置技能 |

---

## 安装技能

### 从 ClawHub 安装

```bash
# 搜索技能
clawhub search weather

# 安装技能
clawhub install weather

# 安装特定版本
clawhub install weather@1.0.0
```

### ClawHub 市场

👉 https://clawhub.com

浏览和发现新技能。

---

## 技能结构

```
my-skill/
├── SKILL.md          # 技能说明（必需）
├── handler.ts        # 处理脚本（可选）
├── handler.js        # 或 JavaScript
└── assets/           # 资源文件
```

### SKILL.md 格式

```markdown
# 技能名称

简短描述技能的功能。

## 使用方法

告诉 AI：
- "查询天气" 会触发此技能
- "明天会下雨吗" 也会触发

## 配置

如需配置，在 TOOLS.md 中添加：
- API_KEY: 你的 API 密钥
```

---

## 创建自定义技能

### 1. 创建目录

```bash
mkdir -p ~/.openclaw/workspace/skills/my-skill
```

### 2. 编写 SKILL.md

```markdown
# 自定义提醒

创建和管理提醒事项。

## 触发词

- "提醒我..."
- "设置提醒"
- "记住..."

## 功能

- 创建一次性提醒
- 创建重复提醒
- 查看所有提醒
```

### 3. 添加处理脚本（可选）

```typescript
// handler.ts
export default async function handler(args: string[]) {
  // 处理逻辑
  return "提醒已创建";
}
```

---

## 内置技能

| 技能 | 说明 |
|------|------|
| `web-search` | 网络搜索 |
| `calendar` | 日历管理 |
| `weather` | 天气查询 |
| `github` | GitHub 操作 |
| `coding-agent` | 代码助手 |

---

## 管理技能

```bash
# 列出已安装技能
clawhub list

# 更新技能
clawhub update weather

# 更新所有技能
clawhub update --all

# 卸载技能
clawhub uninstall weather
```

---

## 技能最佳实践

### 1. 单一职责

每个技能只做一件事：
- ✅ `weather` - 只查天气
- ❌ `weather-and-calendar` - 功能混杂

### 2. 清晰的触发词

```markdown
## 触发词

- "查询天气"
- "今天天气"
- "明天会下雨吗"
```

### 3. 提供示例

```markdown
## 使用示例

用户: "北京今天天气怎么样"
AI: [调用天气技能] 北京今天晴，温度 15-25°C
```

---

## 发布技能

### 1. 准备技能

确保包含：
- `SKILL.md` - 完整文档
- `package.json` - 元数据
- 测试用例

### 2. 发布到 ClawHub

```bash
clawhub publish my-skill
```

### 3. 版本管理

```bash
# 更新版本
clawhub version my-skill minor

# 重新发布
clawhub publish my-skill
```

---

## 下一步

- [定时任务](./cron.md)
- [Sub-Agents](./sub-agents.md)
