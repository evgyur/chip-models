# chip-models

Public OpenClaw model reference: aliases, provider notes, and copy-paste config blocks without secrets.

## Scope

This repository documents a public OpenClaw setup. It intentionally excludes API keys, OAuth exports, cookies, and host-specific private values.

## Current Aliases

| Alias | Full ID | Use Case |
|-------|---------|----------|
| `kimi` | `kimi-coding/kimi-code` | Main Kimi coding/thinking model |
| `sonnet` | `anthropic/claude-sonnet-4-6` | General reasoning |
| `opus` | `anthropic/claude-opus-4-6` | Deep Anthropic tasks |
| `mmfast` | `minimax/MiniMax-M2.5-highspeed` | Fast replies |
| `Minimax` | `minimax/MiniMax-M2.5` | Full MiniMax |
| `codex` | `openai-codex/gpt-5.3-codex` | Codex coding model |
| `gpti` | `openai-codex/gpt-5.3-chat-latest` | GPT-5.3 Instant |
| `gptm` | `openai-codex/gpt-5.4-mini` | GPT-5.4 Mini |
| `gptt` | `openai-codex/gpt-5.5` | GPT-5.5 Thinking |

## OpenAI Note

`codex`, `gpti`, `gptm`, and `gptt` are documented here for the OAuth-backed `openai-codex` flow in OpenClaw, using ChatGPT subscription access rather than pay-as-you-go API billing.

## Kimi Setup Summary

For current OpenClaw builds, the public working Kimi setup is:

- provider: `kimi-coding`
- model ref: `kimi-coding/kimi-code`
- wire API: `anthropic-messages`
- wire endpoint: `POST https://api.kimi.com/coding/v1/messages`

OpenClaw treats **Moonshot** and **Kimi Coding** as different providers:

- use `kimi-coding/kimi-code` for Kimi Coding
- use `moonshot/kimi-k2.5` (or another `moonshot/...` model) for Moonshot API
- do **not** mix them as `kimi/...`

Do not configure Kimi Coding in OpenClaw as:

- `api: "openai-completions"`
- model id `kimi-for-coding`
- `POST https://api.kimi.com/coding/v1/chat/completions`

That older shape can look plausible, but in live testing it commonly degrades into:

- `HTTP 404` / `model_not_found`
- `selected model unavailable`
- OpenClaw fallback to another provider even though the Kimi key itself is valid

Recommended alias setup:

```json
{
  "agents": {
    "defaults": {
      "models": {
        "kimi-coding/kimi-code": { "alias": "kimi" },
        "openai-codex/gpt-5.5": { "alias": "gptt" }
      }
    }
  }
}
```

Then you can switch with:

```text
/model kimi
/model gptt
```

Use [`SKILL.md`](./SKILL.md) as the canonical reference for:

- working provider blocks
- broken anti-pattern examples
- migration from older Kimi configs
- troubleshooting probes and expected error signatures

## Files

- `SKILL.md` contains the actual public skill/reference text.
- `.gitignore` keeps local secret material out of git.

## Usage

Copy the relevant JSON blocks from `SKILL.md` into `~/.openclaw/openclaw.json`, then fill placeholders locally.
