# DeepRolePlay: 基于多智能体架构的深度角色扮演系统

[English](README_en.md) | 中文

## 项目概述

DeepRolePlay 是一个突破性的多智能体角色扮演系统，通过 Agent 协作机制彻底解决传统大语言模型的角色遗忘问题。

DeepRolePlay 采用多智能体分工架构：**记忆闪回智能体** + **情景更新智能体** + **主对话模型**，让 AI 告别角色遗忘，实现真正连贯的角色扮演。

## 🚀 解决角色扮演核心痛点

### 😤 你是否遇到过这些问题？
- 🤖 **AI 突然忘记角色设定**：明明是法师却拿起了剑
- 📖 **剧情前后不一致**：昨天的重要情节今天完全不记得
- 💸 **Token 消耗巨大**：长对话费用飞涨，体验中断

### ✅ DeepRolePlay 的解决方案
- 🧠 **永不遗忘**：Agent 自动维护角色记忆，设定永久保持
- 🔄 **剧情连贯**：智能情景更新，千万轮对话依然逻辑清晰  
- 💰 **成本可控**：情景压缩技术，长对话费用降低 80%
- 📚 **智能联网**：集成 Wikipedia 百科，免费自动补全角色背景和故事设定
- ⚡ **即插即用**：5分钟集成，SillyTavern 等平台直接使用

### ⚖️ 权衡与缺点

为了实现以上效果，本项目存在以下代价，请在使用前知悉：

- ⏱️ **响应时间增加**：Agent 工作流需要额外处理时间，整体耗时可能增加 2-3 倍
- 💸 **初期 Token 消耗**：对话前几轮需要建立情景状态，Token 消耗可能略高于直接调用
- 🔧 **系统复杂度**：相比直接调用 LLM，需要额外的服务部署和维护

**适用场景**：如果你追求长期连贯的大型角色扮演体验，不介意稍慢的响应速度，那么这些代价是值得的。

## 🎯 如何使用

### 超简单集成
1. **启动服务**：运行 `uv run python main.py`，系统在 6666 端口启动
2. **更换接口**：在 SillyTavern、OpenWebUI 等平台中：
   - 将 `base_url` 改为 `http://localhost:6666/v1`
   - API Key 保持不变（直接透传给后端模型）
3. **开始使用**：立即享受无遗忘的角色扮演体验！

### 兼容性说明
- ✅ **完全兼容 OpenAI API 格式**：所有支持 OpenAI 的工具都能直接使用
- ✅ **支持主流模型**：OpenAI GPT、DeepSeek、Claude、本地 Ollama 等
- ✅ **双重配置**：Agent 和转发目标可使用不同模型，成本优化灵活

## Agent 工作原理

传统单一模型的问题：**角色遗忘** → **剧情断裂** → **体验崩坏**

DeepRolePlay 的 Agent 解决方案：
- 🔍 **记忆闪回智能体**：智能检索历史对话和外部知识
- 📝 **情景更新智能体**：实时维护角色状态和剧情连贯性  
- 🎭 **主对话模型**：基于完整上下文生成角色回应

## 工作流程

```
用户请求 -> HTTP代理服务
           |
           v
    触发工作流执行
           |
    +------+------+
    |             |
    v             v
记忆闪回      情景更新
智能体        智能体
    |             |
    +------+------+
           |
           v
    注入更新的情景
           |
           v
    转发至目标LLM
           |
           v
    返回增强响应
```

## 使用步骤

### 环境要求

- Python 3.12
- UV 虚拟环境管理器（推荐）

### 1. 安装项目

```bash
git clone https://github.com/yourusername/deepRolePlay.git
cd deepRolePlay
uv venv --python 3.12
uv pip install -r requirements.txt
```

### 2. 配置服务

编辑 `config/config.yaml` 文件，**推荐使用 DeepSeek（性价比最高）**：

```yaml
# API代理配置 - 转发目标
proxy:
  target_url: "https://api.deepseek.com/v1"   # 推荐 DeepSeek，成本低性能好
  api_key: "your-deepseek-api-key"            # DeepSeek API Key
  timeout: 30                                 # 请求超时时间（秒）

# 智能体配置 - Agent 使用的模型  
agent:
  model: "deepseek-chat"                      # 推荐 DeepSeek Chat，经济实惠
  base_url: "https://api.deepseek.com/v1"    # 可与代理目标不同
  api_key: "your-deepseek-api-key"            # 可使用相同或不同的 API Key
  temperature: 0.1                            # 生成温度（0-1）
  max_iterations: 25                          # 最大迭代次数

# 情景管理
scenario:
  file_path: "./scenarios/scenario.txt"      # 情景文件路径
  update_enabled: true                        # 是否启用自动更新

# 服务器配置
server:
  host: "0.0.0.0"
  port: 6666
```

### 3. 启动服务

```bash
uv run python main.py
```

### 4. 接入使用

将你的 AI 应用（SillyTavern、OpenWebUI 等）的 API 端点改为：
```
http://localhost:6666/v1
```

系统将自动：
1. 拦截对话请求
2. 执行智能体工作流
3. 更新情景状态
4. 将增强的上下文注入请求
5. 返回更准确的角色扮演响应

## 支持的模型

### 🔌 全面兼容 OpenAI 格式 API
本项目采用标准 OpenAI API 格式，支持所有兼容的服务商：

- **🌟 DeepSeek**（强烈推荐）：性价比最高，角色扮演效果出色
- **💻 本地 Ollama**：完全私有化部署，数据安全

### ⚠️ 不推荐 OpenAI 官方 API
虽然完全兼容 OpenAI 格式，但**不建议使用 OpenAI 官方服务**：
- 🔒 **过度安全策略**：对角色扮演内容限制严格，影响体验


## 参考文献

本项目的设计理念受到以下研究的启发：

- [Building effective agents](https://www.anthropic.com/research/building-effective-agents) - Anthropic
- [LangGraph Documentation](https://python.langchain.com/docs/langgraph) - LangChain

## 许可证

MIT License
