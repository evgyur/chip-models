# chip-models

OpenClaw model configuration reference — aliases, providers, and templates.

## Purpose

This repo contains a public reference for configuring AI models in OpenClaw. No API keys, no secrets — just the structure you need to set up your own model aliases and fallbacks.

## What's Inside

- **Model aliases** — short names like `kimi`, `sonnet`, `mm` for quick switching
- **Provider configs** — JSON templates for Anthropic, Kimi, MiniMax, OpenAI, Z.ai
- **Fallback chains** — how to set up primary + fallback models

## Quick Start

Copy the relevant JSON blocks from `SKILL.md` into your `~/.openclaw/openclaw.json` and add your API keys.

## Model Aliases Reference

| Alias | Full ID | Use Case |
|-------|---------|----------|
| `kimi` | `kimi-coding/k2p5` | Complex coding, architecture |
| `sonnet` | `anthropic/claude-sonnet-4-6` | Reasoning, general tasks |
| `mm` | `minimax/MiniMax-M2.5-highspeed` | Fast responses |
| `Minimax` | `minimax/MiniMax-M2.5` | Full MiniMax |
| `codex` | `openai-codex/gpt-5.3-codex` | Code generation |
| `opus` | `anthropic/claude-opus-4-6` | Most capable |

## See Also

- [OpenClaw Documentation](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
