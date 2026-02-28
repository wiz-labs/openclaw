# OpenClaw Architecture Analysis

## Overview
**Language:** TypeScript | **Production LOC:** 502,459 | **Source Files:** 3,945

OpenClaw is a full-featured multi-channel AI gateway with 40+ channels, 15+ LLM providers, native apps, and enterprise-grade infrastructure.

## Component Breakdown by LOC

| Component | LOC | % | Description |
|-----------|-----|---|-------------|
| Agents (runtime, tools, models) | 71,011 | 14.1% | Pi agent execution, tool invocation, model selection, subagent orchestration |
| Extensions (40 channels) | 77,678 | 15.4% | Discord, Matrix, Teams, IRC, Mattermost, Zalo, Feishu, Google Chat, etc. |
| Commands (CLI) | 37,116 | 7.4% | doctor, onboard, health, status, gateway CLI commands |
| Gateway (HTTP/WS server) | 35,638 | 7.1% | HTTP + WebSocket server, auth (14,875 LOC), rate limiting |
| Infrastructure | 34,627 | 6.9% | Heartbeat runner, device pairing, Tailscale, auto-updater, state migrations |
| Config | 23,560 | 4.7% | Zod schema validation, session store, IO |
| Browser automation | 12,979 | 2.6% | Playwright integration, extension relay, canvas host |
| Telegram | 12,385 | 2.5% | Bot handlers, send, formatting, voice, media |
| Channels abstraction | 11,526 | 2.3% | Unified channel interface (dock.ts), registry, plugin system |
| Memory/RAG | 10,237 | 2.0% | QMD manager, sync operations |
| Plugins | 7,515 | 1.5% | Hook system, discovery, manifest registry, loader |
| Security | 6,593 | 1.3% | Audit, exec approvals (~50K with analysis), command resolution |
| Cron | 6,555 | 1.3% | Server-cron, gateway cron integration |
| Routing | 855 | 0.2% | Route resolution, bindings, session keys |
| Other | ~154,000 | ~30% | TUI, TTS, media, logging, types, utils, shared libs, per-channel code |

## Architecture
- **Pattern:** Monolithic Node.js gateway binding HTTP + WebSocket
- **Routing:** resolve-route.ts maps channels/accounts/peers/guilds to agents
- **Agent:** Pi embedded runner wraps LLM calls + tool invocation
- **Multi-model:** Claude, OpenAI, Gemini, OpenRouter, Bedrock, vLLM, etc.
- **Security:** Exec-approvals with AST analysis (~50K LOC), audit logging, optional Docker sandbox
- **Native apps:** iOS, Android, macOS

## Strengths
- Most feature-complete platform (40+ channels, 15+ providers)
- Enterprise-grade security and audit infrastructure
- Native mobile/desktop apps
- Plugin system for extensibility

## Limitations
- 502K LOC makes customization extremely difficult
- Individual files exceed 20K lines
- Deeply entangled provider-specific code in agent layer
