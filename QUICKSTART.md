# nanobot 快速开始指南

nanobot 是一个超轻量级的个人 AI 助手，基于 OpenClaw 开发，代码量减少 99%。

## 支持的功能

### 1. 聊天平台集成
支持多种即时通讯平台：

| 平台 | 说明 | 需要配置 |
|------|------|----------|
| **Telegram** | 推荐，Bot token | Bot Token |
| **Discord** | Bot token + Message Content Intent | Bot Token |
| **WhatsApp** | QR 码登录 | 无 |
| **Feishu (飞书)** | WebSocket 长连接 | App ID + App Secret |
| **QQ** | 私聊支持 | App ID + App Secret |
| **Slack** | Socket Mode | Bot Token + App-Level Token |
| **Email** | IMAP/SMTP | 邮箱账号密码 |
| **DingTalk** | Stream 模式 | App Key + App Secret |
| **Wecom (企业微信)** | WebSocket 长连接 | Bot ID + Secret |
| **Matrix** | E2EE 支持 | 账号凭证 |
| **Mochat** | Socket.IO | Claw Token |

### 2. LLM 提供商
支持多种大语言模型提供商：

| 提供商 | 说明 | 获取方式 |
|--------|------|----------|
| **OpenRouter** | 推荐，全球可用 | [openrouter.ai](https://openrouter.ai/keys) |
| **Anthropic** | Claude 直接接入 | [console.anthropic.com](https://console.anthropic.com) |
| **OpenAI** | GPT 模型 | [platform.openai.com](https://platform.openai.com) |
| **DeepSeek** | DeepSeek 模型 | [platform.deepseek.com](https://platform.deepseek.com) |
| **Groq** | LLM + 语音转录(Whisper) | [console.groq.com](https://console.groq.com) |
| **MiniMax** | MiniMax/Kimi | [platform.minimaxi.com](https://platform.minimaxi.com) |
| **Xiaomi MiMo** | MiMo-V2-Pro (免费) | [xiaomimimo.com](https://xiaomimimo.com) |
| **Gemini** | Google Gemini | [aistudio.google.com](https://aistudio.google.com) |
| **Dashscope** | 阿里云 Qwen | [dashscope.console.aliyun.com](https://dashscope.console.aliyun.com) |
| **Moonshot** | Moonshot/Kimi | [platform.moonshot.cn](https://platform.moonshot.cn) |
| **Zhipu** | 智谱 GLM | [open.bigmodel.cn](https://open.bigmodel.cn) |
| **Ollama** | 本地模型 | 本地安装 Ollama |
| **vLLM** | 本地/自托管 OpenAI 兼容 | 自托管服务 |
| **OpenAI Codex** | OAuth 登录 | `nanobot provider login openai-codex` |
| **Azure OpenAI** | Azure 云服务 | Azure Portal |
| **VolcEngine** | 火山引擎 | [volcengine.com](https://www.volcengine.com) |
| **SiliconFlow** | 硅基流动 | [siliconflow.cn](https://siliconflow.cn) |
| **AiHubMix** | API 网关 | [aihubmix.com](https://aihubmix.com) |

### 3. 工具能力

- **Web 搜索**：支持 Brave、Tavily、Jina、SearXNG、DuckDuckGo
- **MCP (Model Context Protocol)**：支持连接外部工具服务器
- **文件系统**：读写、编辑文件
- **Shell 执行**：运行命令行命令
- **Web 获取**：获取网页内容

### 4. 内置技能 (Skills)

| 技能 | 说明 | 启用方式 |
|------|------|----------|
| **memory** | 双层记忆系统 | 默认启用 |
| **cron** | 定时任务和提醒 | 默认启用 |
| **github** | GitHub 操作 (需要安装 `gh` CLI) | 默认启用 |
| **weather** | 天气查询 (无需 API Key) | 默认启用 |
| **summarize** | 内容总结 | 默认启用 |
| **tmux** | Tmux 会话管理 | 默认启用 |
| **skill-creator** | 技能创建工具 | 默认启用 |
| **clawhub** | 公共技能市场 | 默认启用 |

### 5. 高级功能

- **心跳任务 (Heartbeat)**：定期自动执行任务并推送结果
- **定时任务 (Cron)**：支持一次性提醒和循环任务
- **多实例运行**：支持同时运行多个 nanobot 实例
- **记忆系统**：长期记忆 + 历史记录自动摘要

---

## 安装

```bash
# 从源码安装 (最新功能)
git clone https://github.com/HKUDS/nanobot.git
cd nanobot
pip install -e .

# 或从 PyPI 安装 (稳定版)
pip install nanobot-ai
```

---

## 快速启动

### 1. 初始化

```bash
nanobot onboard
```

### 2. 配置

配置文件位于 `~/.nanobot/config.json`

**设置 API Key (以 OpenRouter 为例)**：
```json
{
  "providers": {
    "openrouter": {
      "apiKey": "sk-or-v1-xxx"
    }
  }
}
```

**设置默认模型**：
```json
{
  "agents": {
    "defaults": {
      "model": "anthropic/claude-sonnet-4-6",
      "provider": "openrouter"
    }
  }
}
```

### 3. 启动

**CLI 交互模式**：
```bash
nanobot agent
```

**启动网关 (连接聊天平台)**：
```bash
nanobot gateway
```

---

## 聊天平台配置

### Telegram (推荐)

1. 在 Telegram 搜索 `@BotFather`，发送 `/newbot` 创建机器人
2. 复制 Bot Token
3. 配置 `~/.nanobot/config.json`：

```json
{
  "channels": {
    "telegram": {
      "enabled": true,
      "token": "YOUR_BOT_TOKEN",
      "allowFrom": ["YOUR_USER_ID"]
    }
  }
}
```

4. 启动：
```bash
nanobot gateway
```

### Discord

1. 在 [Discord Developer Portal](https://discord.com/developers/applications) 创建应用
2. 添加 Bot，启用 **MESSAGE CONTENT INTENT**
3. 获取 Bot Token 和你的 User ID
4. 配置：

```json
{
  "channels": {
    "discord": {
      "enabled": true,
      "token": "YOUR_BOT_TOKEN",
      "allowFrom": ["YOUR_USER_ID"],
      "groupPolicy": "mention"
    }
  }
}
```

5. 启动：
```bash
nanobot gateway
```

### Feishu (飞书)

1. 在[飞书开放平台](https://open.feishu.cn/app)创建应用
2. 启用 Bot 能力，添加权限 `im:message`, `im:message.p2p_msg:readonly`
3. 添加事件 `im.message.receive_v1`
4. 获取 App ID 和 App Secret
5. 配置：

```json
{
  "channels": {
    "feishu": {
      "enabled": true,
      "appId": "cli_xxx",
      "appSecret": "xxx",
      "allowFrom": ["ou_YOUR_OPEN_ID"],
      "groupPolicy": "mention"
    }
  }
}
```

6. 启动：
```bash
nanobot gateway
```

---

## 功能开启

### Web 搜索

在 `config.json` 中配置：

```json
{
  "tools": {
    "web": {
      "search": {
        "provider": "brave",
        "apiKey": "YOUR_API_KEY",
        "maxResults": 5
      }
    }
  }
}
```

**支持的搜索提供商**：
- `brave` (默认)
- `tavily`
- `jina` (有免费额度)
- `searxng` (自托管)
- `duckduckgo` (免费，无需配置)

**使用代理**：
```json
{
  "tools": {
    "web": {
      "proxy": "http://127.0.0.1:7890"
    }
  }
}
```

### MCP (Model Context Protocol)

添加 MCP 服务器到配置：

```json
{
  "tools": {
    "mcpServers": {
      "filesystem": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/dir"]
      },
      "remote-mcp": {
        "url": "https://example.com/mcp/",
        "headers": {
          "Authorization": "Bearer xxxxx"
        }
      }
    }
  }
}
```

### 定时任务 (Cron)

使用 `cron` 工具调度任务：

```python
# 提醒
cron(action="add", message="Time to take a break!", every_seconds=1200)

# 循环任务
cron(action="add", message="Check GitHub stars", every_seconds=600)

# 定时任务 (cron 表达式)
cron(action="add", message="Morning standup", cron_expr="0 9 * * 1-5", tz="America/New_York")

# 列出任务
cron(action="list")

# 删除任务
cron(action="remove", job_id="abc123")
```

### 心跳任务 (Heartbeat)

编辑工作区的 `HEARTBEAT.md` 文件：

```markdown
## Periodic Tasks

- [ ] Check weather forecast and send a summary
- [ ] Scan inbox for urgent emails
```

网关会每 30 分钟检查并执行任务。

### 记忆系统

- `memory/MEMORY.md` — 长期事实（用户偏好、项目背景）
- `memory/HISTORY.md` — 事件日志（可搜索的历史记录）

### ClawHub 技能市场

安装公共技能：

```bash
npx --yes clawhub@latest search "web scraping"
npx --yes clawhub@latest install <slug> --workdir ~/.nanobot/workspace
```

---

## CLI 命令参考

| 命令 | 说明 |
|------|------|
| `nanobot onboard` | 初始化配置和工作区 |
| `nanobot agent -m "..."` | 与 Agent 对话 |
| `nanobot agent` | 交互式对话模式 |
| `nanobot gateway` | 启动网关（连接聊天平台）|
| `nanobot status` | 查看状态 |
| `nanobot provider login openai-codex` | OAuth 登录 |
| `nanobot channels login` | WhatsApp QR 登录 |
| `nanobot channels status` | 查看频道状态 |

---

## 完整配置示例

```json
{
  "agents": {
    "defaults": {
      "workspace": "~/.nanobot/workspace",
      "model": "anthropic/claude-sonnet-4-6",
      "provider": "openrouter",
      "max_tokens": 8192,
      "temperature": 0.1,
      "max_tool_iterations": 40,
      "reasoning_effort": "medium"
    }
  },
  "providers": {
    "openrouter": {
      "apiKey": "sk-or-v1-xxx"
    }
  },
  "channels": {
    "telegram": {
      "enabled": true,
      "token": "YOUR_BOT_TOKEN",
      "allowFrom": ["YOUR_USER_ID"]
    }
  },
  "tools": {
    "web": {
      "search": {
        "provider": "brave",
        "apiKey": "YOUR_BRAVE_API_KEY",
        "maxResults": 5
      },
      "proxy": null
    },
    "exec": {
      "timeout": 60,
      "path_append": ""
    },
    "restrict_to_workspace": false,
    "mcpServers": {}
  },
  "gateway": {
    "host": "0.0.0.0",
    "port": 18790,
    "heartbeat": {
      "enabled": true,
      "interval_s": 1800
    }
  }
}
```

---

## Docker 部署

```bash
# 初始化配置
docker run -v ~/.nanobot:/root/.nanobot --rm nanobot onboard

# 编辑配置
vim ~/.nanobot/config.json

# 启动网关
docker run -v ~/.nanobot:/root/.nanobot -p 18790:18790 nanobot gateway
```

或使用 Docker Compose：

```bash
docker compose up -d nanobot-gateway
```

---

## 多实例运行

```bash
# 初始化多个实例
nanobot onboard --config ~/.nanobot-telegram/config.json --workspace ~/.nanobot-telegram/workspace
nanobot onboard --config ~/.nanobot-discord/config.json --workspace ~/.nanobot-discord/workspace

# 运行不同实例
nanobot gateway --config ~/.nanobot-telegram/config.json
nanobot gateway --config ~/.nanobot-discord/config.json --port 18791
```

---

## 安全建议

- 生产环境设置 `"restrictToWorkspace": true` 限制工具访问范围
- 使用 `allowFrom` 白名单控制访问权限
- 不要将 API Key 提交到代码仓库
