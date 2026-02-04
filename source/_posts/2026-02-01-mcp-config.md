---
title: 常用的 MCP 配置记录
date: 2026-02-01
top_img: /img/mcp-market.jpg
cover: /img/mcp-market.jpg
tags: 
  - MCP
---
# 常用的 MCP 配置记录

## 什么是 MCP？

MCP (Model Context Protocol) 是一种为 AI 助手提供扩展功能的协议，通过这些服务，AI 可以执行更复杂的任务，如文件操作、浏览器自动化、时间处理等。

## MCP 服务器市场

MCP 服务器市场提供了丰富的扩展服务，以下是一些主要的 MCP 服务器来源：

- **windsurf内置MCP市场**
- [Modelscope MCP市场](https://www.modelscope.cn/mcp)
- [MCP Server Finder](https://www.mcpserverfinder.com/)

## 常用 MCP 服务

### Playwright

Playwright 是一个强大的浏览器自动化工具，用于执行网页操作、截图、表单填写等任务。它支持多种浏览器（Chromium、Firefox、WebKit），可以模拟真实用户的浏览器行为，非常适合自动化测试和数据采集。

### Excel

Excel MCP 服务可以直接操作 Excel 文件，包括读取、写入、格式化等功能。特别值得注意的是，配置中设置了 `EXCEL_MCP_PAGING_CELLS_LIMIT` 为 4000，这样可以处理更大规模的表格数据，避免因数据量过大而导致的性能问题。

### Memory

Memory MCP 服务提供了知识图谱和记忆存储功能，让 AI 可以存储和检索实体、关系等结构化信息。这对于需要长期记忆和关联分析的任务非常有用，比如项目管理、知识整理等场景。

### Time

Time MCP 服务专门处理时间相关的操作，配置中将本地时区设置为 `Asia/Shanghai`，确保所有时间计算和转换都基于北京时间。这对于需要处理跨时区时间、日程安排等任务非常方便。

### OpenRPC

OpenRPC MCP 服务允许 AI 调用 JSON-RPC 方法，这为与各种支持 JSON-RPC 协议的服务进行交互提供了可能。通过它，可以远程调用各种 API 服务，执行复杂的后端操作。

### maven

Maven MCP 服务用于管理和查询 Maven 依赖，用于检查依赖版本、获取最新版本信息等。这对于 Java 项目的依赖管理非常有帮助，可以确保使用的依赖都是最新且安全的版本。

### Joplin

Joplin MCP 服务连接到 Joplin 笔记应用，让 AI 可以读取、创建、更新笔记。这对于快速记录想法、管理知识库、跟踪任务进展非常有用。

### Fetch

Fetch MCP 服务用于执行 HTTP 请求，让 AI 可以获取网络资源、调用 API 等。这对于需要实时获取网络数据、集成外部服务的任务非常有用。

### Filesystem

Filesystem MCP 服务提供了文件系统操作功能，让 AI 可以读写文件、创建目录等。这对于需要处理本地文件、进行数据持久化的任务非常有用。

### GitHub

GitHub MCP 服务连接到 GitHub，让 AI 可以执行仓库操作、查看代码、创建 PR 等。这对于开发者来说非常有用，可以直接在 AI 助手界面中管理 GitHub 相关任务。

### Git

Git MCP 服务提供了 Git 版本控制操作功能，让 AI 可以执行提交、分支管理、合并等 Git 命令。这对于需要版本控制、团队协作的开发任务非常有用。

### Vikunja

Vikunja 是一个开源的项目管理工具，类似于 Trello 或 Todoist，支持任务管理、看板和项目组织。通过 MCP 与 Vikunja 实例的集成，可以实现任务的同步和管理，让 AI 能够直接操作和更新项目任务。配置中需要设置 Vikunja 实例的 URL 和 API 令牌，以便建立安全连接。

### Sequential Thinking

Sequential Thinking MCP 服务提供了逻辑推理和顺序思考能力，让 AI 可以进行更复杂的逻辑分析和问题解决。这对于需要逐步推理、分析复杂问题的任务非常有用。

### Kanboard

Kanboard MCP 服务连接到 Kanboard 项目管理系统，让 AI 可以执行项目管理相关的操作，包括创建和管理任务、项目、评论、标签等。它提供了 60+ 个 Kanboard API 端点的访问能力，涵盖项目、任务、分类、列、看板、评论、用户、链接、子任务、标签和文件等多个方面。配置中需要设置 Kanboard 实例的 URL、用户名和 API 令牌。官方 URL：[https://github.com/hoducha/kanboard-mcp](https://github.com/hoducha/kanboard-mcp)

## MCP 配置

以下是完整的 MCP 配置文件：

```json
{
    "mcpServers": {
        "Playwright": {
            "command": "npx",
            "args": [
                "-y",
                "@executeautomation/playwright-mcp-server"
            ],
            "env": {}
        },
        "Excel": {
            "command": "npx",
            "args": [
                "--yes",
                "@negokaz/excel-mcp-server"
            ],
            "env": {
                "EXCEL_MCP_PAGING_CELLS_LIMIT": "4000"
            }
        },
        "Memory": {
            "command": "npx",
            "args": [
                "-y",
                "@modelcontextprotocol/server-memory"
            ],
            "env": {
                "MEMORY_FILE_PATH": "/path/to/memory.json"
            }
        },
        "Time": {
            "command": "uvx",
            "args": [
                "mcp-server-time",
                "--local-timezone=Asia/Shanghai"
            ],
            "env": {}
        },
        "OpenRPC": {
            "command": "npx",
            "args": [
                "-y",
                "openrpc-mcp-server"
            ],
            "env": {}
        },
        "maven": {
            "command": "npx",
            "args": [
                "mcp-maven-deps"
            ],
            "env": {}
        },
        "Joplin": {
            "command": "uvx",
            "args": [
                "--from",
                "joplin-mcp",
                "joplin-mcp-server"
            ],
            "env": {
                "JOPLIN_TOKEN": "token"
            }
        },
        "Vikunja": {
            "command": "npx",
            "args": [
                "-y",
                "@aimbitgmbh/vikunja-mcp"
            ],
            "env": {
                "VIKUNJA_URL": "<URL>",
                "VIKUNJA_API_TOKEN": "<API_TOKEN>"
            }
        },
        "fetch": {
            "args": [
                "mcp-server-fetch"
            ],
            "command": "uvx",
            "env": {}
        },
        "filesystem": {
            "args": [
                "-y",
                "@modelcontextprotocol/server-filesystem",
                ""
            ],
            "command": "npx",
            "env": {}
        },
        "github-mcp-server": {
            "args": [
                "run",
                "-i",
                "--rm",
                "-e",
                "GITHUB_PERSONAL_ACCESS_TOKEN",
                "ghcr.io/github/github-mcp-server"
            ],
            "command": "docker",
            "env": {
                "GITHUB_PERSONAL_ACCESS_TOKEN": "token"
            }
        },
        "sequential-thinking": {
            "args": [
                "-y",
                "@modelcontextprotocol/server-sequential-thinking"
            ],
            "command": "npx",
            "env": {}
        },
        "git": {
            "args": [
                "mcp-server-git"
            ],
            "command": "uvx",
            "env": {}
        },
        "docker": {
            "command": "uvx",
            "args": [
                "docker-mcp"
            ]
        },
        "duckduckgo": {
            "command": "uvx",
            "args": [
                "duckduckgo-mcp-server"
            ],
            "env": {}
        },
        "kanboard": {
            "command": "uvx",
            "args": ["kanboard-mcp"],
            "env": {
                "KANBOARD_URL": "https://your-kanboard.com/jsonrpc.php",
                "KANBOARD_USERNAME": "jsonrpc",
                "KANBOARD_API_TOKEN": "your_api_token_here"
            }
        }
    }
}
```
