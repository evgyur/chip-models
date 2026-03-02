# chip-models — Model Configuration for OpenClaw

Public reference for model aliases and configuration. No secrets, just structure.

## Quick Reference

| Alias | Full Model ID | Provider | Purpose |
|-------|---------------|----------|---------|
| `kimi` | `kimi-coding/k2p5` | Kimi Coding | Complex coding, architecture |
| `codex` | `openai-codex/gpt-5.3-codex` | OpenAI Codex | Code generation |
| `sonnet` | `anthropic/claude-sonnet-4-6` | Anthropic | Reasoning, general tasks |
| `opus` | `anthropic/claude-opus-4-6` | Anthropic | Most capable |
| `mmfast` | `minimax/MiniMax-M2.5-highspeed` | MiniMax | Fast responses |
| `Minimax` | `minimax/MiniMax-M2.5` | MiniMax | Full MiniMax M2.5 |

---

## Recommended Config (Copy to openclaw.json)

Use only `alias` keys in `agents.defaults.models` (no custom keys like `alias2`).

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "kimi-coding/k2p5",
        "fallbacks": [
          "anthropic/claude-sonnet-4-6",
          "anthropic/claude-opus-4-6",
          "minimax/MiniMax-M2.5-highspeed",
          "minimax/MiniMax-M2.5",
          "openai-codex/gpt-5.3-codex"
        ]
      },
      "models": {
        "kimi-coding/k2p5": { "alias": "kimi" },
        "openai-codex/gpt-5.3-codex": { "alias": "codex" },
        "anthropic/claude-sonnet-4-6": { "alias": "sonnet" },
        "anthropic/claude-opus-4-6": { "alias": "opus" },
        "minimax/MiniMax-M2.5-highspeed": { "alias": "mmfast" },
        "minimax/MiniMax-M2.5": { "alias": "Minimax" }
      }
    }
  }
}
```

---

## Provider/Auth Notes (Important)

### Kimi Coding (`kimi-coding`)

- Model ref: `kimi-coding/k2p5`
- In OpenClaw this provider is Anthropic-compatible.
- If you define provider block manually, use:
  - `baseUrl: "https://api.kimi.com/coding/"`
  - `api: "anthropic-messages"`
- Wire endpoint (effective): `POST https://api.kimi.com/coding/v1/messages`
- Auth: API key (`KIMI_API_KEY` or auth profile)

### OpenAI Codex (`openai-codex`)

- Model ref: `openai-codex/gpt-5.3-codex`
- Recommended auth mode in OpenClaw: OAuth (`openai-codex:default`)
- Do **not** document this as a custom `api.openai.com/v1/chat/completions` provider in OpenClaw config; use built-in `openai-codex` auth flow.
- Optional per-model transport override:

```json
{
  "agents": {
    "defaults": {
      "models": {
        "openai-codex/gpt-5.3-codex": {
          "params": { "transport": "auto" }
        }
      }
    }
  }
}
```

(Allowed values: `auto`, `websocket`, `sse`)

---

## Kimi Multi-Key Rotation (Correct Pattern)

Use multiple auth profiles for the **same** provider (`kimi-coding`) and order them in `auth.order`.

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
        "primary": "kimi-coding/k2p5",
        "fallbacks": ["minimax/MiniMax-M2.5"]
      }
    }
  }
}
```

---

## API Quick Ref

| Provider | Model ref example | Auth mode | Notes |
|----------|-------------------|-----------|-------|
| Kimi Coding | `kimi-coding/k2p5` | API key | Anthropic-compatible (`/coding/v1/messages`) |
| OpenAI Codex | `openai-codex/gpt-5.3-codex` | OAuth | Built-in provider flow in OpenClaw |

---

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
```

---

## License

MIT — use as reference for your own OpenClaw setup.
