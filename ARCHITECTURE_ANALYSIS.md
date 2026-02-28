# OpenClaw Architecture Analysis

## Overview
**Language:** TypeScript | **Total LOC:** ~532K (208K production + 304K tests) | **Source Files:** ~3,945

OpenClaw is a full-featured multi-channel AI gateway with 40+ channels, 15+ LLM providers, native apps, and enterprise-grade infrastructure.

**Key insight:** 57% of the repository is test code (304K lines). Actual production code is ~208K LOC.

## Component Breakdown by LOC (Production Code)

| Component | LOC | Description |
|-----------|-----|-------------|
| Extensions (40 channels) | 77,678 | Discord, Matrix, Teams, IRC, Mattermost, Zalo, Feishu, Google Chat, etc. |
| Agents (runtime, tools, models) | ~45K (est.) | Pi agent execution, tool invocation, model selection, subagent orchestration |
| Config + schema | 23,560 | Zod schema validation, session store, IO, help text |
| Browser automation | 12,979 | Playwright integration, extension relay, canvas host |
| Telegram | 12,385 | Bot handlers, send, formatting, voice, media |
| Channels abstraction | 11,526 | Unified channel interface (dock.ts 635 LOC), registry, plugin system |
| Memory/RAG | 10,237 | QMD manager, sync operations |
| Plugins | 7,515 | Hook system, discovery, manifest registry, loader |
| Cron | 6,555 | Server-cron, gateway cron integration |
| TUI | 5,161 | Terminal UI, progress spinners, tables, status rendering |
| Exec-approvals | 4,700 | Shell lexer (heredocs, quoting, escaping), allowlist matching, command analysis |
| Heartbeat | 2,615 | Per-agent scheduling, session selection, scope resolution, transcript pruning |
| State migrations | 2,600 | Session store migration, schema versioning, symlink handling |
| Types/schemas | ~20K | Type definitions across the codebase |
| Other | ~remaining | TTS, media processing, logging, utils, shared libs, WhatsApp, Slack, Signal core |

## Corrected Misconceptions

| Component | Previously Claimed | Actual | Notes |
|-----------|-------------------|--------|-------|
| Heartbeat runner | "~40K LOC" | 2,615 | 1,213-line runner + supporting modules |
| Exec-approvals | "~50K LOC" | 4,700 | 11 files, hand-written shell lexer |
| dock.ts | "21K LOC" | 635 | Lightweight channel metadata registry |
| Total production | "502K" | ~208K | 304K is test suite |

## Architecture
- **Pattern:** Monolithic Node.js gateway binding HTTP + WebSocket
- **Routing:** resolve-route.ts maps channels/accounts/peers/guilds to agents
- **Agent:** Pi embedded runner wraps LLM calls + tool invocation
- **Multi-model:** Claude, OpenAI, Gemini, OpenRouter, Bedrock, vLLM, etc.
- **Security:** Exec-approvals with recursive-descent shell lexer (4.7K LOC), audit logging, optional Docker sandbox
- **Native apps:** iOS, Android, macOS

## Verdict: Justified but Overkill for Personal Agent Stack

The 208K LOC is ~65% justified by genuine feature breadth (10+ channels, multi-model, security, session management). The 304K test suite indicates mature software. However, for a personal agent stack that needs customizability, this codebase is too deep to modify safely.
