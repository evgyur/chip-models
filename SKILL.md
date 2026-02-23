# chip-models — Model Configuration for Claw Server

## Current Model Stack

### Primary: Kimi K2.5 (kimi-coding)
- **Model ID:** `kimi-coding/k2p5`
- **Alias:** `/kimi`
- **Provider:** `kimi-coding`
- **API:** OpenAI-compatible (`https://api.kimi.com/coding/v1`)
- **API Key:** (see `/home/chip/.openclaw/.env`)
- **Features:** Vision, function calling, 200K context

### Fallback #1: OpenAI Codex
- **Model ID:** `openai-codex/gpt-5.3-codex`
- **Alias:** `/codex`
- **Auth:** OAuth (CLAUDE_CODE_OAUTH_TOKEN in .env)
- **Features:** Latest Codex model

### Fallback #2: MiniMax
- **Model ID:** `minimax/MiniMax-M2.5-highspeed`
- **Aliases:** `/mm`, `/mmfast`
- **Provider:** `minimax`
- **API Key:** (see `/home/chip/.openclaw/.env`)

---

## Configuration Location

**File:** `/home/chip/.openclaw/openclaw.json`

### Key Sections:

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "kimi-coding/k2p5",
        "fallbacks": [
          "openai-codex/gpt-5.3-codex",
          "minimax/MiniMax-M2.5"
        ]
      },
      "models": {
        "kimi-coding/k2p5": { "alias": "kimi" },
        "openai-codex/gpt-5.3-codex": { "alias": "codex" },
        "minimax/MiniMax-M2.5-highspeed": { "alias": "mmfast" }
      },
      "aliases": {
        "mm": "minimax/MiniMax-M2.5-highspeed"
      }
    }
  },
  "providers": {
    "kimi-coding": {
      "baseUrl": "https://api.kimi.com/coding/v1",
      "apiKey": "(see .env file)",
      "api": "openai-completions"
    },
    "minimax": {
      "apiKey": "(see .env file)"
    }
  }
}
```

---

## API Keys Location

**File:** `/home/chip/.openclaw/.env`

Store all API keys here, never commit them to git:
- `KIMI_API_KEY`
- `MINIMAX_API_KEY`
- `CLAUDE_CODE_OAUTH_TOKEN`
- `OPENAI_API_KEY`

---

## Adding New Models

### 1. Add to providers (if new provider):
```json
"providers": {
  "provider-name": {
    "baseUrl": "https://api.provider.com/v1",
    "apiKey": "sk-...",
    "api": "openai-completions"
  }
}
```

### 2. Add to agents.defaults.models:
```json
"agents": {
  "defaults": {
    "models": {
      "provider/model-id": { "alias": "/shortcut" }
    }
  }
}
```

### 3. Restart gateway:
```bash
systemctl restart openclaw-gateway
```

---

## Known Issues & Fixes

### Issue: Wrong model selected for `/kimi` alias

**Date:** 2026-02-23

**Symptom:** Command `/kimi` sets model to `kimi-coding/kimi-for-coding` instead of `kimi-coding/k2p5`.

**Root Cause:** Mismatch between model ID in provider definition and model reference in alias.

In `/home/chip/.openclaw/openclaw.json`:
```json
// WRONG - model ID doesn't match alias reference
"kimi-coding": {
  "models": [{ "id": "kimi-for-coding", ... }]  // ← wrong ID
},
"agents": {
  "defaults": {
    "models": {
      "kimi-coding/k2p5": { "alias": "kimi" }  // ← references k2p5
    }
  }
}
```

**Fix:** Change model ID from `"kimi-for-coding"` to `"k2p5"` to match the alias reference.

```bash
# On server 178.63.60.99:
sed -i 's|"id": "kimi-for-coding"|"id": "k2p5"|g' /home/chip/.openclaw/openclaw.json
systemctl restart openclaw-gateway
```

**Verification:** After fix, `/kimi` should report `Model set to kimi (kimi-coding/k2p5)`.

---

## Troubleshooting

### Check if model is loaded:
```bash
ssh root@178.63.60.99 'python3 -c "
import json
d = json.load(open(\"/home/chip/.openclaw/openclaw.json\"))
models = d.get(\"agents\", {}).get(\"defaults\", {}).get(\"models\", {})
for k, v in models.items():
    if \"alias\" in v:
        print(v[\"alias\"] + \" -> \" + k)
"'
```

### Check gateway status:
```bash
ssh root@178.63.60.99 'systemctl status openclaw-gateway'
```

### Restart gateway:
```bash
ssh root@178.63.60.99 'systemctl restart openclaw-gateway'
```

---

## Current Aliases Summary

| Alias | Model |
|-------|-------|
| `/kimi` | kimi-coding/k2p5 |
| `/codex` | openai-codex/gpt-5.3-codex |
| `/mm` | minimax/MiniMax-M2.5-highspeed |
| `/mmfast` | minimax/MiniMax-M2.5 |
| `/sonnet` | anthropic/claude-sonnet-4-5 |
| `/opus` | anthropic/claude-opus-4-6 |
| `/GLM` | zai/glm-5 |

---

## Server Info

- **Host:** 178.63.60.99
- **User:** chip
- **Config:** `/home/chip/.openclaw/openclaw.json`
- **Env:** `/home/chip/.openclaw/.env`
