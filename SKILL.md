# chip-models — Model Configuration for OpenClaw

Public reference for model aliases and configuration. No secrets, just structure.

## Quick Reference

| Alias | Full Model ID | Purpose |
|-------|--------------|---------|
| `kimi` | `kimi-coding/k2p5` | Primary — complex coding, architecture |
| `sonnet` | `anthropic/claude-sonnet-4-6` | Claude Sonnet 4.6 — reasoning, general tasks |
| `mm` | `minimax/MiniMax-M2.5-highspeed` | Fast responses, quick tasks |
| `Minimax` | `minimax/MiniMax-M2.5` | Full MiniMax M2.5 |
| `codex` | `openai-codex/gpt-5.3-codex` | OpenAI Codex — code generation |
| `opus` | `anthropic/claude-opus-4-6` | Claude Opus 4.6 — most capable |

## Configuration Structure

### Default / Primary Model

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "kimi-coding/k2p5",
        "fallbacks": [
          "minimax/MiniMax-M2.5",
          "openai-codex/gpt-5.3-codex"
        ]
      }
    }
  }
}
```

### Model Aliases

```json
{
  "agents": {
    "defaults": {
      "models": {
        "kimi-coding/k2p5": { "alias": "kimi" },
        "openai-codex/gpt-5.3-codex": { "alias": "codex" },
        "minimax/MiniMax-M2.5-highspeed": { "alias": "mm" },
        "minimax/MiniMax-M2.5": { "alias": "Minimax" },
        "anthropic/claude-sonnet-4-6": { "alias": "sonnet" }
      }
    }
  }
}
```

## Provider Configurations (Templates)

### Anthropic (Claude)

```json
{
  "models": {
    "providers": {
      "anthropic": {
        "baseUrl": "https://api.anthropic.com",
        "api": "anthropic-messages",
        "models": [
          {
            "id": "claude-opus-4-6",
            "name": "Claude Opus 4.6",
            "reasoning": true,
            "input": ["text", "image"],
            "cost": { "input": 0.005, "output": 0.025 },
            "contextWindow": 800000,
            "maxTokens": 64000
          },
          {
            "id": "claude-sonnet-4-5",
            "name": "Claude Sonnet 4.5",
            "reasoning": false,
            "input": ["text", "image"],
            "cost": { "input": 0.003, "output": 0.015 },
            "contextWindow": 200000,
            "maxTokens": 64000
          },
          {
            "id": "claude-sonnet-4-6",
            "name": "Claude Sonnet 4.6",
            "maxTokens": 8192
          }
        ]
      }
    }
  }
}
```

### Kimi (Moonshot AI)

```json
{
  "models": {
    "providers": {
      "kimi-coding": {
        "baseUrl": "https://api.kimi.com/coding/v1",
        "apiKey": "${KIMI_API_KEY}",
        "api": "openai-completions",
        "models": [
          {
            "id": "kimi-for-coding",
            "name": "Kimi 2.5 Coding",
            "reasoning": false,
            "input": ["text"],
            "cost": { "input": 0, "output": 0 },
            "contextWindow": 200000,
            "maxTokens": 128000
          }
        ]
      }
    }
  }
}
```

### MiniMax

```json
{
  "models": {
    "providers": {
      "minimax": {
        "baseUrl": "https://api.minimax.io/anthropic",
        "apiKey": "${MINIMAX_API_KEY}",
        "api": "anthropic-messages",
        "models": [
          {
            "id": "MiniMax-M2.5",
            "name": "MiniMax M2.5",
            "reasoning": true,
            "input": ["text"],
            "cost": { "input": 0, "output": 0 },
            "contextWindow": 204800,
            "maxTokens": 16384
          },
          {
            "id": "MiniMax-M2.5-highspeed",
            "name": "MiniMax M2.5 Highspeed",
            "reasoning": true,
            "input": ["text"],
            "cost": { "input": 0, "output": 0 },
            "contextWindow": 204800,
            "maxTokens": 16384
          }
        ]
      }
    }
  }
}
```

### OpenAI Codex

```json
{
  "models": {
    "providers": {
      "openai-codex": {
        "baseUrl": "https://api.openai.com",
        "apiKey": "${OPENAI_API_KEY}",
        "api": "openai-completions",
        "models": [
          {
            "id": "gpt-5.3-codex",
            "name": "GPT-5.3 Codex",
            "reasoning": true,
            "input": ["text"],
            "cost": { "input": 0, "output": 0 },
            "contextWindow": 128000,
            "maxTokens": 4096
          }
        ]
      }
    }
  }
}
```

### Z.ai (GLM)

```json
{
  "models": {
    "providers": {
      "zai": {
        "baseUrl": "https://api.z.ai/api/coding/paas/v4",
        "apiKey": "${ZAI_API_KEY}",
        "api": "openai-completions",
        "models": [
          {
            "id": "glm-4.7-flashx",
            "name": "GLM-4.7-FlashX",
            "reasoning": false,
            "contextWindow": 200000,
            "maxTokens": 128000
          },
          {
            "id": "glm-5",
            "name": "GLM-5",
            "contextWindow": 200000,
            "maxTokens": 8192
          }
        ]
      }
    }
  }
}
```

## Usage

### Switch models in session

```
Use kimi for this task
Model: sonnet
/kimi analyze this code
```

### Set as default in config

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "kimi-coding/k2p5"
      }
    }
  }
}
```

## Model Comparison

| Model | Context | Speed | Best For |
|-------|---------|-------|----------|
| Kimi K2.5 | 256K | Medium | Complex coding, architecture |
| Claude Opus 4.6 | 800K | Medium | Deep reasoning, long context |
| Claude Sonnet 4.6 | 195K | Medium | General purpose, balanced |
| MiniMax M2.5 | 200K | Fast | Quick responses, high throughput |
| GPT-5.3 Codex | 128K | Medium | Code generation, OpenAI ecosystem |

## License

MIT — use as reference for your own OpenClaw setup.
