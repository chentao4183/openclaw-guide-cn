# 模型选择指南

> 如何为 OpenClaw 选择合适的 AI 模型？国内主流模型推荐与配置指南

![模型选择](https://img.shields.io/badge/AI-模型推荐-blue)
![更新时间](https://img.shields.io/badge/更新-2026.03-lightgrey)

---

## 🎯 选择模型的关键因素

### 1. 使用场景
- **通用对话**：豆包/火山、通义千问
- **长文本处理**：智谱 GLM
- **代码生成**：DeepSeek、智谱 GLM
- **企业应用**：通义千问、文心一言

### 2. 成本预算
- **低成本**：DeepSeek、豆包
- **性价比**：智谱 GLM、通义千问
- **企业级**：文心一言、火山引擎

### 3. 性能需求
- **响应速度**：豆包、通义千问
- **准确度**：智谱 GLM、文心一言
- **稳定性**：火山引擎、通义千问

---

## 🇨🇳 国内主流模型推荐

### 1. 豆包 / 火山引擎 ⭐⭐⭐⭐⭐

**推荐指数：★★★★★**  
**适合场景：通用对话、日常任务**

#### 特点
- ✅ 响应速度快
- ✅ 价格实惠
- ✅ 中文理解强
- ✅ 稳定性高

#### 配置示例

```json5
{
  agents: {
    defaults: {
      model: "doubao",
      modelConfig: {
        provider: "volcengine",
        model: "doubao-pro-32k",
        apiKey: "your_volcengine_api_key",
        endpoint: "https://ark.cn-beijing.volces.com/api/v3",
      },
    },
  },
}
```

#### 获取方式
1. 访问 [火山引擎控制台](https://console.volcengine.com/ark)
2. 创建推理接入点
3. 获取 API Key 和 Endpoint

---

### 2. 智谱 GLM ⭐⭐⭐⭐⭐

**推荐指数：★★★★★**  
**适合场景：长文本处理、代码生成**

#### 特点
- ✅ 长文本支持（128K）
- ✅ 代码能力强
- ✅ 逻辑推理优秀
- ✅ API 稳定

#### 配置示例

```json5
{
  agents: {
    defaults: {
      model: "glm-4",
      modelConfig: {
        provider: "zhipu",
        model: "glm-4",
        apiKey: "your_zhipu_api_key",
      },
    },
  },
}
```

#### 获取方式
1. 访问 [智谱 AI 开放平台](https://open.bigmodel.cn/)
2. 注册并获取 API Key
3. 选择模型（GLM-4 / GLM-3-Turbo）

---

### 3. 通义千问 ⭐⭐⭐⭐

**推荐指数：★★★★☆**  
**适合场景：企业应用、多模态处理**

#### 特点
- ✅ 企业级支持
- ✅ 多模态能力（文本、图像、视频）
- ✅ 阿里云生态集成
- ✅ 长文本支持

#### 配置示例

```json5
{
  agents: {
    defaults: {
      model: "qwen",
      modelConfig: {
        provider: "aliyun",
        model: "qwen-max",
        apiKey: "your_aliyun_api_key",
      },
    },
  },
}
```

#### 获取方式
1. 访问 [阿里云百炼平台](https://bailian.console.aliyun.com/)
2. 开通 DashScope 服务
3. 获取 API Key

---

### 4. DeepSeek ⭐⭐⭐⭐

**推荐指数：★★★★☆**  
**适合场景：代码生成、高性价比**

#### 特点
- ✅ 价格极低
- ✅ 代码能力强
- ✅ 开源版本可用
- ✅ 社区活跃

#### 配置示例

```json5
{
  agents: {
    defaults: {
      model: "deepseek",
      modelConfig: {
        provider: "deepseek",
        model: "deepseek-chat",
        apiKey: "your_deepseek_api_key",
      },
    },
  },
}
```

#### 获取方式
1. 访问 [DeepSeek 官网](https://www.deepseek.com/)
2. 注册并获取 API Key
3. 选择模型（DeepSeek Chat / DeepSeek Coder）

---

### 5. 文心一言 ⭐⭐⭐⭐

**推荐指数：★★★★☆**  
**适合场景：企业应用、中文理解**

#### 特点
- ✅ 百度生态支持
- ✅ 中文理解强
- ✅ 企业级 SLA
- ✅ 知识图谱集成

#### 配置示例

```json5
{
  agents: {
    defaults: {
      model: "ernie",
      modelConfig: {
        provider: "baidu",
        model: "ernie-4.0",
        apiKey: "your_baidu_api_key",
        secretKey: "your_baidu_secret_key",
      },
    },
  },
}
```

#### 获取方式
1. 访问 [百度智能云](https://console.bce.baidu.com/qianfan/)
2. 开通千帆大模型平台
3. 获取 API Key 和 Secret Key

---

## 🌍 国际模型推荐

### 1. Claude 3.5 Sonnet ⭐⭐⭐⭐⭐

**推荐指数：★★★★★**  
**适合场景：复杂推理、长文本、代码**

#### 配置示例

```json5
{
  agents: {
    defaults: {
      model: "claude-3-5-sonnet",
      modelConfig: {
        provider: "anthropic",
        model: "claude-3-5-sonnet-20241022",
        apiKey: "your_anthropic_api_key",
      },
    },
  },
}
```

---

### 2. GPT-4 / GPT-4o ⭐⭐⭐⭐⭐

**推荐指数：★★★★★**  
**适合场景：通用对话、多模态、复杂任务**

#### 配置示例

```json5
{
  agents: {
    defaults: {
      model: "gpt-4",
      modelConfig: {
        provider: "openai",
        model: "gpt-4o",
        apiKey: "your_openai_api_key",
      },
    },
  },
}
```

---

## 📊 模型对比表

| 模型 | 价格 | 速度 | 长文本 | 代码能力 | 中文理解 | 推荐场景 |
|------|------|------|--------|----------|----------|----------|
| 豆包 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 通用对话 |
| 智谱 GLM | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 长文本、代码 |
| 通义千问 | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 企业应用 |
| DeepSeek | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 代码、性价比 |
| 文心一言 | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 企业应用 |
| Claude 3.5 | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 复杂推理 |
| GPT-4 | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 通用、多模态 |

---

## 🎯 选择建议

### 新手推荐
**首选：豆包/火山引擎**
- 价格实惠，响应快
- 中文理解强
- 配置简单

### 开发者推荐
**首选：智谱 GLM 或 DeepSeek**
- 代码能力强
- 长文本支持
- 性价比高

### 企业用户推荐
**首选：通义千问或文心一言**
- 企业级支持
- 稳定性高
- 生态集成

### 高级用户推荐
**首选：Claude 3.5 Sonnet**
- 推理能力强
- 长文本支持
- 代码能力优秀

---

## 🔧 配置多模型

### 1. 默认模型 + 备用模型

```json5
{
  agents: {
    defaults: {
      model: "doubao",  // 默认使用豆包
      fallbackModel: "glm-4",  // 备用模型
      modelConfig: {
        // 主模型配置
      },
      fallbackModelConfig: {
        // 备用模型配置
      },
    },
  },
}
```

### 2. 不同 Agent 使用不同模型

```json5
{
  agents: {
    list: [
      {
        id: "home",
        default: true,
        model: "doubao",  // 日常对话用豆包
        workspace: "~/.openclaw/workspace-home",
      },
      {
        id: "work",
        model: "glm-4",  // 工作场景用 GLM
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

---

## 💡 使用建议

### 1. 本地先跑通
```bash
# 完成基础配置
openclaw onboard

# 测试模型连接
openclaw test --model doubao
```

### 2. 按需选择
- 通用场景首选豆包/火山
- 长文本推荐智谱 GLM
- 代码任务用 DeepSeek

### 3. 按量扩容
- 先验证业务效果
- 再考虑充值
- 降低试错成本

---

## 🎁 社区专属优惠

通过以下社区专属链接注册或充值，可获得额外初始额度或首充优惠，同时支持 OpenClaw 开源项目持续维护。

### 豆包/火山引擎
- [注册链接](https://console.volcengine.com/ark)
- 新用户赠送额度

### 智谱 GLM
- [注册链接](https://open.bigmodel.cn/)
- 新用户赠送 tokens

### 通义千问
- [注册链接](https://bailian.console.aliyun.com/)
- 首充优惠

---

## 📖 相关资源

- [OpenClaw 模型配置文档](https://docs.openclaw.ai/configuration/models)
- [各模型官方文档](https://docs.openclaw.ai/providers)
- [模型性能测试报告](https://docs.openclaw.ai/benchmarks)

---

**下一步** → [高级功能配置](advanced/README.md)
