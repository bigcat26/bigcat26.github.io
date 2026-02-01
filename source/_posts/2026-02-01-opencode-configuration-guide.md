---
title: Opencode 配置技巧
date: 2026-02-01
# top_img: /img/opencode.jpg
cover: /img/opencode.jpg
tags: 
  - opencode
---
# OpenCode 配置技巧

## 什么是 OpenCode？

OpenCode 是一款功能强大的 AI 代码助手，它可以帮助开发者更高效地编写代码、解决问题、理解代码库等。通过合理的配置，OpenCode 可以成为你开发过程中的得力助手。

## Oh-My-OpenCode 插件安装

### 安装步骤

#### 使用 pnpm 安装

```bash
# MacOS
pnpm add -g oh-my-opencode

# Linux
pnpm add -g oh-my-opencode-linux-x64
```

安装后，可执行文件路径在：`~/.local/share/pnpm/oh-my-opencode`

#### 配置插件

```bash
oh-my-opencode install
```

## MCP 配置

### 官方文档

MCP配置文档：https://opencode.ai/docs/mcp-servers/

### 配置示例

opencode的mcp配置和cursor/windsurf不一样，需要指定一个`type`字段，指定`local`、`remote`，还有一个`enabled`字段，用来控制是否启用该服务。另外，并没有`args`字段，而是直接在`command`中指定参数，`environment`字段对应`env`字段。

以下是一个配置 Vikunja 任务管理的 MCP 服务示例：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "plugin": [
    "oh-my-opencode@latest"
  ],
  "mcp": {
    "vikunja": {
      "type": "local",
      "command": ["npx", "-y", "@aimbitgmbh/vikunja-mcp"],
      "environment": {
        "VIKUNJA_URL": "https://host:port/api/v1",
        "VIKUNJA_API_TOKEN": "token"
      },
      "enabled": true
    }
  }
}
```

## 模型配置

官方文档: https://opencode.ai/docs/providers/

- 最近的更新支持siliconflow了，国内选siliconflow (China)
- 应该是优先读取项目目录下的配置文件，如`opencode.json`，然后再读取全局配置文件`~/.config/opencode/opencode.json`

### 自定义模型

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "myprovider": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "My AI ProviderDisplay Name",
      "options": {
        "baseURL": "https://api.myprovider.com/v1"
      },
      "models": {
        "my-model-name": {
          "name": "My Model Display Name"
        }
      }
    },
    "llama.cpp": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "llama-server (local)",
      "options": {
        "baseURL": "http://127.0.0.1:8080/v1"
      },
      "models": {
        "qwen3-coder:a3b": {
          "name": "Qwen3-Coder: a3b-30b (local)",
          "limit": {
            "context": 128000,
            "output": 65536
          }
        }
      }
    },
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama (local)",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      },
      "models": {
        "llama2": {
          "name": "Llama 2"
        }
      }
    }
  }
}
```
