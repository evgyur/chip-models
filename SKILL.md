# chip-models — Model Configuration for OpenClaw

Public reference for model aliases and configuration. No secrets, just structure.

## Quick Reference

| Alias | Full Model ID | Provider | Purpose |
|-------|---------------|----------|---------|
| `kimi` | `kimi-coding/k2p5` | Kimi | Complex coding, architecture |
| `sonnet` | `anthropic/claude-sonnet-4-6` | Anthropic | Reasoning, general tasks |
| `mm` | `minimax/MiniMax-M2.5-highspeed` | MiniMax | Fast responses, quick tasks |
| `mmfast` | `minimax/MiniMax-M2.5-highspeed` | MiniMax | Fast responses (alias) |
| `Minimax` | `minimax/MiniMax-M2.5` | MiniMax | Full MiniMax M2.5 |
| `codex` | `openai-codex/gpt-5.3-codex` | OpenAI | Code generation |
| `opus` | `anthropic/claude-opus-4-6` | Anthropic | Most capable |

## Full Configuration (Copy to openclaw.json)

### 1. Agents → Defaults (Aliases + Fallbacks)

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
      },
      "models": {
        "kimi-coding/k2p5": { "alias": "kimi" },
        "anthropic/claude-sonnet-4-6": { "alias": "sonnet" },
        "anthropic/claude-opus-4-6": { "alias": "opus" },
        "minimax/MiniMax-M2.5-highspeed": { "alias": "mmfast" },
        "minimax/MiniMax-M2.5": { "alias": "Minimax" },
        "openai-codex/gpt-5.3-codex": { "alias": "codex" }
      }
    }
  }
}
```

### 2. Models → Providers (Full Configs)

#### Anthropic (Claude)

```json
{
  "models": {
    "providers": {
      "anthropic": {
        "baseUrl": "https://api.anthropic.com",
        "apiKey": "sk-ant-api03-...",
        "api": "anthropic-messages",
        "models": [
          {
            "id": "claude-opus-4-6",
            "name": "Claude Opus 4.6",
            "reasoning": true,
            "input": ["text", "image"],
            "cost": { "input": 0.005, "output": 0.025, "cacheRead": 0.0005, "cacheWrite": 0.00625 },
            "contextWindow": 800000,
            "maxTokens": 64000
          },
          {
            "id": "claude-sonnet-4-6",
            "name": "Claude Sonnet 4.6",
            "reasoning": false,
            "input": ["text", "image"],
            "cost": { "input": 0.003, "output": 0.015 },
            "contextWindow": 200000,
            "maxTokens": 8192
          },
          {
            "id": "claude-sonnet-4-5",
            "name": "Claude Sonnet 4.5",
            "maxTokens": 64000
          }
        ]
      }
    }
  },
  "auth": {
    "profiles": {
      "anthropic:default": {
        "provider": "anthropic",
        "mode": "api_key"
      }
    }
  }
}
```

**Endpoint:** `POST https://api.anthropic.com/v1/messages`  
**Auth:** `x-api-key` header with API key  
**Format:** Anthropic Messages API (not OpenAI-compatible)

---

#### Kimi (Moonshot AI)

```json
{
  "models": {
    "providers": {
      "kimi-coding": {
        "baseUrl": "https://api.kimi.com/coding/v1",
        "apiKey": "sk-kimi-...",
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
  },
  "auth": {
    "profiles": {
      "kimi-coding:default": {
        "provider": "kimi-coding",
        "mode": "api_key"
      }
    }
  }
}
```

**Endpoint:** `POST https://api.kimi.com/coding/v1/chat/completions`  
**Auth:** `Authorization: Bearer <api_key>` header  
**Format:** OpenAI Completions API compatible  
**Get key:** https://kimi.com → API → Coding

---

#### MiniMax

```json
{
  "models": {
    "providers": {
      "minimax": {
        "baseUrl": "https://api.minimax.io/anthropic",
        "apiKey": "sk-cp-...",
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
  },
  "auth": {
    "profiles": {
      "minimax:default": {
        "provider": "minimax",
        "mode": "api_key"
      }
    }
  }
}
```

**Endpoint:** `POST https://api.minimax.io/v1/text/chatcompletion_v2`  
**Auth:** Header `Authorization: Bearer <api_key>`  
**Format:** Anthropic Messages API compatible  
**Get key:** https://platform.minimax.io

---

#### OpenAI Codex

```json
{
  "models": {
    "providers": {
      "openai-codex": {
        "baseUrl": "https://api.openai.com",
        "apiKey": "sk-...",
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
  },
  "auth": {
    "profiles": {
      "openai-codex:default": {
        "provider": "openai-codex",
        "mode": "oauth"
      }
    }
  }
}
```

**Endpoint:** `POST https://api.openai.com/v1/chat/completions`  
**Auth:** `Authorization: Bearer <api_key>` header  
**Format:** OpenAI Chat Completions API  
**Note:** Requires OpenAI OAuth or API key with Codex access

---

#### Z.ai (GLM)

```json
{
  "models": {
    "providers": {
      "zai": {
        "baseUrl": "https://api.z.ai/api/coding/paas/v4",
        "apiKey": "...",
        "api": "openai-completions",
        "models": [
          {
            "id": "glm-4.7-flashx",
            "name": "GLM-4.7-FlashX",
            "reasoning": false,
            "input": ["text"],
            "cost": { "input": 0, "output": 0 },
            "contextWindow": 200000,
            "maxTokens": 128000
          },
          {
            "id": "glm-5",
            "name": "GLM-5",
            "reasoning": false,
            "contextWindow": 200000,
            "maxTokens": 8192
          }
        ]
      }
    }
  },
  "auth": {
    "profiles": {
      "zai:default": {
        "provider": "zai",
        "mode": "api_key"
      }
    }
  }
}
```

**Endpoint:** `POST https://api.z.ai/api/coding/paas/v4/chat/completions`  
**Auth:** `Authorization: Bearer <api_key>` header  
**Format:** OpenAI Completions API compatible  
**Get key:** https://www.z.ai (Zhipu AI)

---

#### NVIDIA (Kimi via NVIDIA)

```json
{
  "models": {
    "providers": {
      "nvidiabuild": {
        "baseUrl": "https://integrate.api.nvidia.com/v1",
        "apiKey": "nvapi-...",
        "api": "openai-completions",
        "models": [
          {
            "id": "moonshotai/kimi-k2.5",
            "name": "Kimi K2.5 (NVIDIA)",
            "reasoning": false,
            "input": ["text"],
            "cost": { "input": 0, "output": 0 },
            "contextWindow": 128000,
            "maxTokens": 16384
          }
        ]
      }
    }
  }
}
```

**Endpoint:** `POST https://integrate.api.nvidia.com/v1/chat/completions`  
**Auth:** `Authorization: Bearer <api_key>` header  
**Format:** OpenAI Completions API compatible  
**Get key:** https://build.nvidia.com/moonshotai/kimi-k2.5

---

## Multiple Kimi Keys (Fallback Chain)

```json
{
  "models": {
    "providers": {
      "kimi-coding": { "apiKey": "sk-kimi-key1", ... },
      "kimi2": { "apiKey": "sk-kimi-key2", ... },
      "kimi3": { "apiKey": "sk-kimi-key3", ... },
      "kimi4": { "apiKey": "sk-kimi-key4", ... }
    }
  },
  "auth": {
    "profiles": {
      "kimi-coding:default": { "provider": "kimi-coding", "mode": "api_key" },
      "kimi2:default": { "provider": "kimi2", "mode": "api_key" },
      "kimi3:default": { "provider": "kimi3", "mode": "api_key" },
      "kimi4:default": { "provider": "kimi4", "mode": "api_key" }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "kimi-coding/k2p5",
        "fallbacks": [
          "kimi4/kimi-for-coding",
          "kimi-coding/kimi-for-coding",
          "kimi2/kimi-for-coding",
          "kimi3/kimi-for-coding",
          "minimax/MiniMax-M2.5"
        ]
      }
    }
  }
}
```

---

## API Format Quick Ref

| Provider | Endpoint | Auth Header | API Type |
|----------|----------|-------------|----------|
| Anthropic | `api.anthropic.com/v1/messages` | `x-api-key: <key>` | anthropic-messages |
| Kimi | `api.kimi.com/coding/v1/chat/completions` | `Authorization: Bearer <key>` | openai-completions |
| MiniMax | `api.minimax.io/v1/text/chatcompletion_v2` | `Authorization: Bearer <key>` | anthropic-messages |
| OpenAI | `api.openai.com/v1/chat/completions` | `Authorization: Bearer <key>` | openai-completions |
| Z.ai | `api.z.ai/api/coding/paas/v4/chat/completions` | `Authorization: Bearer <key>` | openai-completions |
| NVIDIA | `integrate.api.nvidia.com/v1/chat/completions` | `Authorization: Bearer <key>` | openai-completions |

---

## Usage

### Switch models in session

```
Use kimi for this task
Model: sonnet
/kimi analyze this code
```

### Check available models

```bash
openclaw models list
```

---

## License

MIT — use as reference for your own OpenClaw setup.
