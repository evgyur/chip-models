# chip-models

Public OpenClaw model reference: aliases, provider notes, and copy-paste config blocks without secrets.

## Scope

This repository documents a public OpenClaw setup. It intentionally excludes API keys, OAuth exports, cookies, and host-specific private values.

## Current Aliases

| Alias | Full ID | Use Case |
|-------|---------|----------|
| `kimi` | `kimi-coding/k2p5` | Complex coding and architecture |
| `sonnet` | `anthropic/claude-sonnet-4-6` | General reasoning |
| `opus` | `anthropic/claude-opus-4-6` | Deep Anthropic tasks |
| `mmfast` | `minimax/MiniMax-M2.5-highspeed` | Fast replies |
| `Minimax` | `minimax/MiniMax-M2.5` | Full MiniMax |
| `codex` | `openai-codex/gpt-5.3-codex` | Codex coding model |
| `gpti` | `openai-codex/gpt-5.3-chat-latest` | GPT-5.3 Instant |
| `gptm` | `openai-codex/gpt-5.4-mini` | GPT-5.4 Mini |
| `gptt` | `openai-codex/gpt-5.4` | GPT-5.4 Thinking |

## OpenAI Note

`codex`, `gpti`, `gptm`, and `gptt` are documented here for the OAuth-backed `openai-codex` flow in OpenClaw, using ChatGPT subscription access rather than pay-as-you-go API billing.

## Files

- `SKILL.md` contains the actual public skill/reference text.
- `.gitignore` keeps local secret material out of git.

## Usage

Copy the relevant JSON blocks from `SKILL.md` into `~/.openclaw/openclaw.json`, then fill placeholders locally.
