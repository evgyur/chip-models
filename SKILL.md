---
name: chip-models
description: Use when documenting or configuring public OpenClaw model aliases and provider notes without exposing secrets
---

# chip-models

Public reference for OpenClaw model aliases and provider/auth notes. Keep secrets out of git; use placeholders or local-only auth files.

## Contract

**Primary class:** Verifier
**Secondary class:** Governor

**Use when:** documenting, auditing, or preparing safe model alias/provider/auth guidance for OpenClaw without exposing secrets.

**Do not use when:** the user asks to make a live config change and the exact config subtree has not been verified yet.

**Output contract:**
- alias or provider mapping used
- whether the statement is documentation-only or live-config applicable
- relevant provider/auth caveat
- explicit note when a config/schema check is still required before editing

**Verification gate:** do not present a model/provider detail as live-config truth unless it is verified against the actual runtime/config path.

## Quick Reference

| Alias | Full Model ID | Provider | Purpose |
|-------|---------------|----------|---------|
| `kimi` | `kimi-coding/kimi-code` | Kimi Coding | Main Kimi coding/thinking model |
| `codex` | `openai-codex/gpt-5.3-codex` | OpenAI Codex | Code generation |
| `gpti` | `openai-codex/gpt-5.3-chat-latest` | OpenAI Codex | GPT-5.3 Instant |
| `gptm` | `openai-codex/gpt-5.4-mini` | OpenAI Codex | GPT-5.4 Mini |
| `gptt` | `openai-codex/gpt-5.5` | OpenAI Codex | GPT-5.5 Thinking |
| `sonnet` | `anthropic/claude-sonnet-4-6` | Anthropic | General reasoning |
| `opus` | `anthropic/claude-opus-4-6` | Anthropic | Most capable |
| `mmfast` | `minimax/MiniMax-M2.7-highspeed` | MiniMax | Fast responses |
| `Minimax` | `minimax/MiniMax-M2.7` | MiniMax | Full MiniMax M2.7 |
| `trinity` | `openrouter/arcee-ai/trinity-large-thinking` | OpenRouter | Optional Trinity shortcut |

## Recommended Config

Use only `alias` keys in `agents.defaults.models` unless a model explicitly needs `params`.

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "kimi-coding/kimi-code",
        "fallbacks": [
          "openai-codex/gpt-5.5",
          "anthropic/claude-sonnet-4-6",
          "anthropic/claude-opus-4-6",
          "minimax/MiniMax-M2.5-highspeed",
          "minimax/MiniMax-M2.5",
          "openai-codex/gpt-5.3-codex"
        ]
      },
      "models": {
        "kimi-coding/kimi-code": { "alias": "kimi" },
        "openai-codex/gpt-5.3-codex": { "alias": "codex" },
        "openai-codex/gpt-5.3-chat-latest": { "alias": "gpti" },
        "openai-codex/gpt-5.4-mini": {
          "alias": "gptm",
          "params": { "transport": "auto" }
        },
        "openai-codex/gpt-5.5": {
          "alias": "gptt",
          "params": { "transport": "auto" }
        },
        "anthropic/claude-sonnet-4-6": { "alias": "sonnet" },
        "anthropic/claude-opus-4-6": { "alias": "opus" },
        "minimax/MiniMax-M2.7-highspeed": { "alias": "mmfast" },
        "minimax/MiniMax-M2.7": { "alias": "Minimax" },
        "openrouter/arcee-ai/trinity-large-thinking": { "alias": "trinity" }
      }
    }
  }
}
```

## Provider/Auth Notes

### Kimi Coding (`kimi-coding`)

- Model ref: `kimi-coding/kimi-code`
- In OpenClaw this provider is Anthropic-compatible.
- If you define the provider block manually, use:
  - `baseUrl: "https://api.kimi.com/coding/v1"`
  - `api: "anthropic-messages"`
- Wire endpoint: `POST https://api.kimi.com/coding/v1/messages`
- Auth: API key via local-only config or auth profile

Important distinction:

- `kimi-coding/...` = Kimi Coding
- `moonshot/...` = Moonshot API
- `kimi/...` is not the canonical way to reference current OpenClaw Kimi models

### OpenAI Codex (`openai-codex`)

- Model refs: `openai-codex/gpt-5.3-codex`, `openai-codex/gpt-5.3-chat-latest`, `openai-codex/gpt-5.4-mini`, `openai-codex/gpt-5.5`
- Recommended auth mode in OpenClaw: OAuth (`openai-codex:default`)
- This is the ChatGPT subscription flow, not pay-as-you-go API billing.
- Do not document this as a custom `api.openai.com/v1/chat/completions` provider.
- On current OpenClaw builds, define the provider models below if `gpti`, `gptm`, or `gptt` show up as `configured,missing`.

```json
{
  "models": {
    "providers": {
      "openai-codex": {
        "baseUrl": "https://chatgpt.com/backend-api",
        "api": "openai-codex-responses",
        "models": [
          {
            "id": "gpt-5.3-chat-latest",
            "name": "GPT-5.3 Instant",
            "reasoning": false,
            "input": ["text", "image"],
            "contextWindow": 266240,
            "maxTokens": 266240
          },
          {
            "id": "gpt-5.4-mini",
            "name": "GPT-5.4 Mini",
            "reasoning": true,
            "input": ["text", "image"],
            "contextWindow": 400000,
            "maxTokens": 400000
          },
          {
            "id": "gpt-5.5",
            "name": "GPT-5.5 Thinking",
            "reasoning": true,
            "input": ["text", "image"],
            "contextWindow": 400000,
            "maxTokens": 400000
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

Optional transport override:

```json
{
  "agents": {
    "defaults": {
      "models": {
        "openai-codex/gpt-5.4-mini": {
          "params": { "transport": "auto" }
        },
        "openai-codex/gpt-5.5": {
          "params": { "transport": "auto" }
        }
      }
    }
  }
}
```

Allowed values: `auto`, `websocket`, `sse`

### OpenRouter Trinity (`/trinity`)

Alias: `trinity` -> `openrouter/arcee-ai/trinity-large-thinking`

Use for optional Trinity access without changing the default model.

Documented setup example:

```json
{
  "agents": {
    "defaults": {
      "models": {
        "openrouter/arcee-ai/trinity-large-thinking": {
          "alias": "trinity"
        }
      }
    }
  }
}
```

Auth: provide `OPENROUTER_API_KEY` via local-only gateway env/config, not in git.

### GPT-5.3 Instant (`/gpti`)

Alias: `gpti` -> `openai-codex/gpt-5.3-chat-latest`

Use for general inference tasks when you want GPT-5.3 without Codex tuning.

### GPT-5.4 Mini (`/gptm`)

Alias: `gptm` -> `openai-codex/gpt-5.4-mini`

Use for lower-latency, lower-cost OpenAI Codex workloads when you still want strong tool use and reasoning.

### GPT-5.5 Thinking (`/gptt`)

Alias: `gptt` -> `openai-codex/gpt-5.5`

Use when you want longer reasoning in the same OAuth-backed OpenAI Codex flow.

## Kimi Multi-Key Rotation

Use multiple auth profiles for the same provider (`kimi-coding`) and order them in `auth.order`.

```json
{
  "auth": {
    "profiles": {
      "kimi-coding:key1": { "provider": "kimi-coding", "mode": "api_key" },
      "kimi-coding:key2": { "provider": "kimi-coding", "mode": "api_key" },
      "kimi-coding:key3": { "provider": "kimi-coding", "mode": "api_key" }
    },
    "order": {
      "kimi-coding": [
        "kimi-coding:key1",
        "kimi-coding:key2",
        "kimi-coding:key3"
      ]
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "kimi-coding/kimi-code",
        "fallbacks": ["minimax/MiniMax-M2.5"]
      }
    }
  }
}
```

## API Quick Ref

| Provider | Model ref example | Auth mode | Notes |
|----------|-------------------|-----------|-------|
| Kimi Coding | `kimi-coding/kimi-code` | API key | Anthropic-compatible (`/coding/v1/messages`) |
| OpenAI Codex | `openai-codex/gpt-5.3-codex` | OAuth | ChatGPT subscription flow |
| OpenAI GPT-5.3 | `openai-codex/gpt-5.3-chat-latest` | OAuth | `/gpti` alias |
| OpenAI GPT-5.4 Mini | `openai-codex/gpt-5.4-mini` | OAuth | `/gptm` alias |
| OpenAI GPT-5.5 | `openai-codex/gpt-5.5` | OAuth | `/gptt` alias |
| OpenRouter Trinity | `openrouter/arcee-ai/trinity-large-thinking` | API key | `/trinity` alias |
| Moonshot | `moonshot/kimi-k2.5` | API key | Separate provider line from `kimi-coding` |

## Usage

```bash
openclaw models list
openclaw models status
openclaw models auth login --provider openai-codex
openclaw onboard --auth-choice kimi-code-api-key
```

In chat:

```text
Use kimi for this task
Model: codex
Model: gpti
Model: gptm
Model: gptt
Model: trinity
```

## Claim wording rule

Separate:
- **documented reference** — what this skill documents as the intended alias/provider setup
- **runtime verified** — what has been checked in the actual current config/runtime
- **requires config/schema check** — likely valid, but not yet safe to present as current truth

Avoid collapsing documentation examples into claims about the current live config.

## Safety Checklist

- Keep `auth-profiles.json`, `.env`, `.env.local`, and `*.key` ignored by git.
- Use placeholders only in docs.
- Do not paste exported OAuth state, tokens, or cookies into commits, screenshots, or markdown.
