# Lark MCP - 飞书全场景 Function Call AI 助手 🚀

[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue)](https://www.python.org/)
[![Node.js Version](https://img.shields.io/badge/nodejs-18%2B-blue)](https://nodejs.org/zh-cn/)

一个基于飞书(Lark)的全场景MCP Function Call AI Agent，实现大模型通过飞书进行函数调用和消息处理。


**无需配置飞书机器人，你的飞书账号即是AI助手。**


**只需定义函数和注释，你的飞书机器人会自动根据场景调用。**


## 项目概述 🌟

Lark MCP是一个现代化的Python应用程序，能够:

- 📊 连接飞书API，监听并记录消息
- 🤖 提供自定义函数供大模型调用
- 🔄 实现基于MCP (Model Context Protocol) 的函数调用框架
- 💾 使用SQLAlchemy将消息存储到MySQL数据库

## 效果图🧸

<div align="center">
  <img src="static/resource/back_end.png" width="600" alt="后台日志">
  <br>
  <em>图1: 后台日志</em>
</div>


<div align="center">
  <img src="static/resource/front_end_1.png" width="600" alt="聊天数据库查询">
  <br>
  <em>图2: 聊天数据库查询</em>
</div>

<div align="center">
  <img src="static/resource/front_end_2.png" width="600" alt="天气查询"> 
  <br>
  <em>图3: 天气查询</em>
</div>

<div align="center">
  <img src="static/resource/functions.png" width="600" alt="注册函数"> 
  <br>
  <em>图4: 简单注册函数，只需定义函数和注释</em>
</div>

## ✨ 功能特点

- **函数注册机制**: 简单直观的函数注册装饰器
- **消息自动处理**: 记录所有接收到的消息（私聊和群聊）
- **异步处理**: 采用async/await模式进行异步通信
- **数据持久化**: 使用SQLAlchemy将消息存储在MySQL数据库中
- **灵活配置**: 通过环境变量进行配置
- **容器化部署**: 支持Docker快速部署
- **智能函数调用**: AI会根据用户输入的文字自动分析并调用最匹配的函数，开发者只需添加函数及其注释描述

## 📦 当前支持的函数

项目目前内置了以下函数供大模型调用:

| 函数名 | 描述 |
|-------|------|
| `tell_joke` | 讲一个随机笑话 |
| `get_time` | 获取当前时间 |
| `roll_dice` | 掷一个指定面数的骰子 |
| `make_todo_list` | 从逗号分隔的任务创建待办清单 |
| `translate_to_chinese` | 将英文单词翻译成中文 |
| `countdown` | 创建一个从指定数字开始的倒计时 |
| `random_color` | 生成一个随机的十六进制颜色代码 |
| `fortune` | 抽取一个随机运势 |
| `count_daily_speakers` | 获取今天发言的人数统计 |
| `get_top_speaker_today` | 获取今天发言最多的用户 |
| `send_message` | 给指定用户发送消息 |
| `list_tools` | 列出所有可用的工具及其描述 |


你可以通过在飞书中输入触发指令后跟要执行的操作来调用这些功能，例如: `/run 讲个笑话`

## 📂 项目结构

```
project/
├── app/                    # 应用程序模块
│   ├── api/                # API相关模块
│   │   ├── auth.py         # 认证模块
│   │   └── lark_client.py  # 飞书客户端
│   ├── config/             # 配置模块
│   │   └── settings.py     # 应用配置
│   ├── core/               # 核心业务逻辑
│   │   ├── mcp_server.py   # MCP服务器
│   │   └── message_service.py  # 消息处理服务
│   ├── db/                 # 数据库相关
│   │   ├── models.py       # 数据模型
│   │   └── session.py      # 数据库会话管理
│   └── utils/              # 工具函数
├── builder/                # 请求构建器
├── static/                 # 静态资源
│   ├── resource/           # 图片资源
│   ├── proto_pb2.py        # 协议定义
│   └── lark_decrypt.js     # 飞书解密工具
├── .env                    # 环境变量
├── main.py                 # 应用入口
└── Dockerfile              # Docker配置
```

## 🛠️ 自定义函数开发

在 `app/core/mcp_server.py` 文件中，您可以使用 `@register_tool` 装饰器添加您自己的自定义函数:

```python
@register_tool(name="tell_joke", description="讲一个随机笑话")
def tell_joke() -> str:
    jokes = [
        "为什么程序员都喜欢黑色？因为他们不喜欢 bug 光。",
        "Python 和蛇有什么共同点？一旦缠上你就放不下了。",
        "为什么 Java 开发者很少被邀去派对？因为他们总是抛出异常。",
    ]
    return random.choice(jokes)

@register_tool(name="send_message", description="给指定用户发送消息 {user:用户名称 content:消息内容}")
def send_message(user: str, content: str) -> str:
    """给指定用户发送私信"""
    lark_client = LarkClient(get_auth())
    # ... 实现逻辑 ...
    return f"成功向 {user} 发送了私信: '{content}'"
```

**重要**: 只需添加函数和对应的描述，AI会根据用户的文字自动分析并调用最匹配的函数，无需手动实现函数匹配逻辑。

## 🔧 环境要求

- Python 3.10+
- Node.js 20+
- MySQL数据库

## 📦 安装方法

### 使用本地环境

1. 安装依赖:
   ```bash
   pip install -r requirements.txt
   ```

2. Windows用户注意:
   Windows系统需要额外安装以下依赖:
   ```bash
   pip install win-inet-pton==1.1.0
   ```

### 使用Docker

方法一：单独构建镜像
```bash
# 构建镜像
docker build -t feishuapp .

# 运行容器
docker run -it feishuapp bash
```

方法二：使用Docker Compose（推荐）
```bash
# 启动所有服务（应用和数据库）
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止所有服务
docker-compose down
```

使用Docker Compose可以一键启动整个应用环境，包括MySQL数据库和应用服务，更加方便和高效。

## 🛠️ 配置说明

复制`.env.example`文件命名为`.env`文件，包含以下配置:
```
# 飞书API设置 - 只需配置LARK_COOKIE即可，无需配置飞书机器人
LARK_APP_ID=your_app_id
LARK_APP_SECRET=your_app_secret
LARK_COOKIE=your_lark_cookie   # 只需配置此项，无需飞书机器人配置

# 数据库设置
DB_HOST=localhost
DB_PORT=3306
DB_NAME=lark_messages
DB_USER=username
DB_PASSWORD=password

# OpenAI API配置
OPENAI_API_KEY=your_openai_api_key
OPENAI_API_BASE=https://api.openai.com/v1
OPENAI_MODEL=gpt-4-turbo-preview

# 函数调用触发设置
FUNCTION_TRIGGER_FLAG=/run   # 可自定义触发函数调用的前缀
```

## 🚀 使用指南

### 运行应用程序

方法一：直接运行
```bash
python main.py
```

方法二：使用Docker Compose
```bash
docker-compose up -d
```

应用程序将:
1. 初始化MCP服务器
2. 连接到飞书API并使用你的飞书账号作为AI助手
3. 监听传入的消息
4. 处理并执行大模型通过飞书发起的函数调用
5. 将消息存储在MySQL数据库中


## 🗄️ 数据库结构

应用程序将消息存储在`messages`表中，该表具有以下结构:

| 列名           | 类型           | 描述                      |
|----------------|---------------|---------------------------|
| id             | INT (PK)      | 主键                      |
| user_name      | VARCHAR(255)  | 消息发送者的名称           |
| user_id        | VARCHAR(255)  | 发送者的飞书用户ID         |
| content        | TEXT          | 消息内容                  |
| is_group_chat  | BOOLEAN       | 消息是否来自群聊           |
| group_name     | VARCHAR(255)  | 群聊名称（如适用）         |
| chat_id        | VARCHAR(255)  | 聊天ID                    |
| message_time   | DATETIME      | 消息发送时间               |
| created_at     | DATETIME      | 记录创建时间               |

## 🤝 贡献指南

欢迎贡献！请随时提交Pull Request。

1. Fork这个仓库
2. 创建您的特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交您的更改 (`git commit -m '添加一些很棒的特性'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 打开Pull Request

## 🐛 问题与支持

如果您遇到任何问题或有疑问，请[提交issue](https://github.com/cv-cat/LarkAgentX/issues)或访问我们的[讨论论坛](https://github.com/cv-cat/LarkAgentX/discussions)。

