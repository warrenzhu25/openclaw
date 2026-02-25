# Contributing to OpenClaw

Welcome to the lobster tank!

OpenClaw is a personal AI assistant that connects to multiple messaging channels (WhatsApp, Telegram, Slack, Discord, Signal, iMessage, and more). This guide covers everything you need to contribute effectively.

## Quick Links

- **GitHub:** https://github.com/openclaw/openclaw
- **Vision:** [`VISION.md`](VISION.md)
- **Docs:** https://docs.openclaw.ai
- **Discord:** https://discord.gg/qkhbAGHRBT
- **X/Twitter:** [@steipete](https://x.com/steipete) / [@openclaw](https://x.com/openclaw)

## Maintainers

- **Peter Steinberger** - Benevolent Dictator
  - GitHub: [@steipete](https://github.com/steipete) · X: [@steipete](https://x.com/steipete)

- **Shadow** - Discord subsystem, Discord admin, Clawhub, all community moderation
  - GitHub: [@thewilloftheshadow](https://github.com/thewilloftheshadow) · X: [@4shad0wed](https://x.com/4shad0wed)

- **Vignesh** - Memory (QMD), formal modeling, TUI, IRC, and Lobster
  - GitHub: [@vignesh07](https://github.com/vignesh07) · X: [@\_vgnsh](https://x.com/_vgnsh)

- **Jos** - Telegram, API, Nix mode
  - GitHub: [@joshp123](https://github.com/joshp123) · X: [@jjpcodes](https://x.com/jjpcodes)

- **Ayaan Zaidi** - Telegram subsystem, iOS app
  - GitHub: [@obviyus](https://github.com/obviyus) · X: [@0bviyus](https://x.com/0bviyus)

- **Tyler Yust** - Agents/subagents, cron, BlueBubbles, macOS app
  - GitHub: [@tyler6204](https://github.com/tyler6204) · X: [@tyleryust](https://x.com/tyleryust)

- **Mariano Belinky** - iOS app, Security
  - GitHub: [@mbelinky](https://github.com/mbelinky) · X: [@belimad](https://x.com/belimad)

- **Seb Slight** - Docs, Agent Reliability, Runtime Hardening
  - GitHub: [@sebslight](https://github.com/sebslight) · X: [@sebslig](https://x.com/sebslig)

- **Christoph Nakazawa** - JS Infra
  - GitHub: [@cpojer](https://github.com/cpojer) · X: [@cnakazawa](https://x.com/cnakazawa)

- **Gustavo Madeira Santana** - Multi-agents, CLI, web UI
  - GitHub: [@gumadeiras](https://github.com/gumadeiras) · X: [@gumadeiras](https://x.com/gumadeiras)

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                     Message Sources                              │
│  WhatsApp │ Telegram │ Slack │ Discord │ Signal │ iMessage │... │
└─────────────────────────────┬───────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                  Gateway (Control Plane)                         │
│  src/gateway/     WebSocket + HTTP server                        │
└─────────────────────────────┬───────────────────────────────────┘
                              │
         ┌────────────────────┼────────────────────┐
         ▼                    ▼                    ▼
   ┌──────────┐        ┌──────────┐        ┌──────────┐
   │  Agents  │        │ Plugins  │        │  Memory  │
   │src/agents│        │src/plugins│       │src/memory│
   └──────────┘        │extensions│        └──────────┘
                       └──────────┘
```

### Key Directories

| Directory | Purpose |
|-----------|---------|
| `src/gateway/` | Central control plane - WebSocket + HTTP server handling all message routing |
| `src/agents/` | AI agent execution, tool definitions, model providers, and authentication |
| `src/plugins/` | Plugin runtime, loading, and hook system |
| `src/channels/` | Core channel infrastructure and routing logic |
| `src/config/` | Configuration management, validation, and schema |
| `src/cli/` | Command-line interface (Commander-based) |
| `src/memory/` | Vector search and embeddings for long-term memory |
| `src/hooks/` | Lifecycle event hooks system |
| `extensions/` | 37+ messaging channel plugins (Discord, Telegram, WhatsApp, etc.) |
| `skills/` | 50+ bundled skills (1Password, GitHub, Notion, Slack, etc.) |

### Key Design Patterns

- **Adapter Pattern**: Channels implement a `ChannelPlugin` interface for consistent messaging behavior
- **Hook System**: Lifecycle events (`before-agent-start`, `after-tool-call`, etc.) for extensibility
- **TypeBox Schemas**: Runtime validation with TypeScript type inference
- **Plugin Discovery**: Precedence order: config paths → workspace → global → bundled

---

## Component Deep Dive

### Gateway (`src/gateway/`)

The Gateway is OpenClaw's central control plane - a WebSocket + HTTP server that orchestrates all communication.

**Key Logic:**
- **Server startup** (`server.impl.ts`): Initializes WebSocket handlers, loads plugins, starts channel managers, configures TLS, and exposes health endpoints
- **Authentication** (`auth.ts`, `auth-rate-limit.ts`): Token-based auth with rate limiting to prevent abuse
- **Message routing**: Routes incoming messages from channels to the appropriate agent session
- **Config hot-reload** (`config-reload.ts`): Watches config file changes and applies updates without restart
- **Channel health monitoring** (`channel-health-monitor.ts`): Tracks channel connection states and reports failures

**Key Files:**
- `server.impl.ts` - Main server implementation (~500 LOC of startup orchestration)
- `server-methods.ts` - Core RPC method handlers
- `client.ts` - WebSocket client for connecting to gateway
- `control-ui.ts` - Browser-based control panel serving

### Agents (`src/agents/`)

Agents are the AI execution layer - they run LLM conversations, execute tools, and manage sessions.

**Key Logic:**
- **Agent execution** (`pi-embedded-runner/`): Runs the Pi agent framework for tool-calling conversations
- **Tool definitions** (`tools/`): 40+ built-in tools including `bash_tool`, `browser_tool`, `memory_tool`, `message_tool`, `cron_tool`
- **Auth profiles** (`auth-profiles/`): Manages API keys and OAuth tokens for model providers (OpenAI, Anthropic, Google, etc.)
- **Model discovery**: Auto-discovers available models based on configured credentials
- **Session management**: Maintains conversation history and context per agent/channel/peer

**Key Files:**
- `tools/*.ts` - Tool implementations (each tool ~100-400 LOC)
- `auth-profiles/*.ts` - Credential storage and rotation
- `bash-tools.ts` - Shell execution with sandboxing
- `agent-scope.ts` - Agent workspace and directory resolution

**Tool System:**
Tools are defined with TypeBox schemas for input validation:
```ts
{
  name: "memory_search",
  description: "Search long-term memory",
  parameters: Type.Object({
    query: Type.String(),
    limit: Type.Optional(Type.Number()),
  }),
  execute: async (params, ctx) => { ... }
}
```

### Memory (`src/memory/`, `extensions/memory-*`)

Memory provides long-term context via vector search and embeddings.

**Key Logic:**
- **Embedding providers** (`embeddings.ts`): Supports OpenAI, Gemini, Voyage, and local GGUF models
- **Vector normalization**: Sanitizes and L2-normalizes embeddings for consistent similarity search
- **Batch processing** (`batch-*.ts`): Efficient bulk embedding for large document sets

**Memory Plugins:**
- `memory-core`: Basic file-backed memory with `memory_search` and `memory_get` tools
- `memory-lancedb`: Advanced LanceDB-backed memory with:
  - **Auto-capture**: Hooks into `after-tool-call` to capture important information
  - **Auto-recall**: Hooks into `before-agent-start` to inject relevant memories
  - **Categories**: facts, preferences, relationships, events, other
  - **Importance scoring**: Prioritizes high-value memories in search results

**Key Files:**
- `embeddings.ts` - Embedding provider factory and normalization
- `extensions/memory-lancedb/index.ts` - Full-featured memory plugin
- `extensions/memory-core/index.ts` - Lightweight memory tools

### Channels (`src/channels/`, `extensions/`)

Channels are messaging platform adapters that normalize different APIs into a common interface.

**Key Logic:**
- **Channel Dock** (`dock.ts`): Registry of channel capabilities, adapters, and configuration resolvers
- **Allowlist matching** (`allowlist-match.ts`): Determines which senders can interact with the bot
- **Command gating** (`command-gating.ts`): Controls which commands are available per channel
- **Mention gating** (`mention-gating.ts`): Handles @-mention requirements in group chats

**Channel Plugin Interface:**
```ts
interface ChannelPlugin {
  id: string;
  meta: { label, docsPath, blurb, aliases };
  capabilities: { chatTypes: ["direct", "group"] };
  config: {
    listAccountIds: (cfg) => string[];
    resolveAccount: (cfg, accountId) => AccountConfig;
  };
  outbound: {
    deliveryMode: "direct" | "queued";
    sendText: (params) => Promise<SendResult>;
  };
  // Optional: setup, security, status, gateway, threading, streaming
}
```

**Built-in Channels** (`extensions/`):
- `whatsapp` - WhatsApp Web via Baileys
- `telegram` - Telegram Bot API via Grammy
- `discord` - Discord.js with slash commands
- `slack` - Slack Bolt framework
- `signal` - Signal CLI bridge
- `imessage` - macOS Messages.app integration
- Plus 30+ more (Matrix, MS Teams, IRC, Line, etc.)

### Plugins & Hooks (`src/plugins/`)

Plugins extend OpenClaw at runtime. Hooks provide lifecycle interception points.

**Key Logic:**
- **Discovery** (`discovery.ts`): Scans config paths, workspace, global, and bundled directories
- **Loading** (`loader.ts`): Uses jiti for TypeScript runtime loading
- **Hook runner** (`hooks.ts`): Executes hooks with priority ordering and error handling

**Available Hooks:**
| Hook | Trigger | Use Case |
|------|---------|----------|
| `before-agent-start` | Before LLM call | Inject context, modify prompt |
| `after-tool-call` | After tool execution | Log results, capture memories |
| `before-tool-call` | Before tool execution | Validate, modify params |
| `message-received` | Incoming message | Filter, transform, route |
| `message-sending` | Before send | Modify outgoing messages |
| `gateway-start` | Server startup | Initialize resources |
| `gateway-stop` | Server shutdown | Cleanup |

**Plugin Registration:**
```ts
export default function(api: OpenClawPluginApi) {
  api.registerTool(...);           // Add agent tools
  api.registerChannel(...);        // Add messaging channel
  api.registerGatewayMethod(...);  // Add RPC endpoints
  api.registerCli(...);            // Add CLI commands
  api.registerHook("before-agent-start", ...);
}
```

### Routing (`src/routing/`)

Routing determines which agent handles which conversation.

**Key Logic:**
- **Session keys** (`session-key.ts`): Builds unique keys from `channel:account:peer` for session isolation
- **Route resolution** (`resolve-route.ts`): Matches incoming messages to agents via bindings
- **Bindings**: Configure agent assignment by peer, guild, team, account, or channel

**Matching Priority:**
1. `binding.peer` - Specific user/chat binding
2. `binding.peer.parent` - Thread parent binding
3. `binding.guild+roles` - Discord guild + role combination
4. `binding.guild` - Discord guild
5. `binding.team` - Slack workspace
6. `binding.account` - Channel account
7. `binding.channel` - Entire channel
8. `default` - Default agent

### Config (`src/config/`)

Configuration management with validation and hot-reload support.

**Key Logic:**
- **Schema validation**: TypeBox schemas with JSON Schema generation for UI
- **Migration** (`config.ts`): Handles legacy config format upgrades
- **Environment variables**: Supports `OPENCLAW_*` env var overrides
- **Agent directories** (`agent-dirs.ts`): Resolves per-agent workspace paths

**Config Locations:**
- `~/.openclaw/openclaw.json` - Main config file
- `~/.openclaw/credentials/` - Secure credential storage
- `~/.openclaw/extensions/` - User-installed plugins
- `<workspace>/.openclaw/` - Project-specific overrides

---

## Development Setup

### Prerequisites

- **Node.js 22.12.0+** (check with `node --version`)
- **pnpm** (install with `npm install -g pnpm`)

### Installation

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm build
```

### Running Locally

```bash
# First-time setup
pnpm openclaw onboard --install-daemon

# Development with auto-reload
pnpm gateway:watch

# Run CLI commands
pnpm openclaw <command>
```

---

## How to Contribute

1. **Bugs & small fixes** → Open a PR!
2. **New features / architecture** → Start a [GitHub Discussion](https://github.com/openclaw/openclaw/discussions) or ask in Discord first
3. **Questions** → Discord #setup-help

### Before You PR

Run all quality gates:

```bash
pnpm build    # TypeScript compilation
pnpm check    # Format + typecheck + lint
pnpm test     # Unit tests
```

Guidelines:
- Test locally with your OpenClaw instance
- Keep PRs focused (one thing per PR; do not mix unrelated concerns)
- Describe what & why in your PR description

---

## Testing Guide

OpenClaw has three Vitest suites plus Docker runners.

### Test Commands

| Command | Purpose |
|---------|---------|
| `pnpm test` | All unit/integration tests |
| `pnpm test:fast` | Fast unit tests only |
| `pnpm test:e2e` | End-to-end gateway tests |
| `pnpm test:live` | Live provider tests (needs API keys) |
| `pnpm test:coverage` | Coverage report (70% threshold) |

### Test File Conventions

- `*.test.ts` - Unit tests (colocated with source)
- `*.e2e.test.ts` - End-to-end tests
- `*.live.test.ts` - Live provider tests (real API calls)

### When to Run What

- **Editing logic/tests**: `pnpm test` (and `pnpm test:coverage` if you changed a lot)
- **Touching gateway/WS/pairing**: Add `pnpm test:e2e`
- **Debugging provider issues**: Run a narrowed `pnpm test:live`

Full testing documentation: [docs/help/testing.md](docs/help/testing.md)

---

## Code Style

- **Language**: TypeScript (ESM), strict typing
- **Formatting**: `pnpm format` (oxfmt)
- **Linting**: `pnpm lint` (oxlint with type-aware rules)
- **File size**: Keep files under ~500-700 LOC when feasible
- **Never** use `@ts-nocheck` or disable `no-explicit-any`; fix root causes

Run the full check before committing:

```bash
pnpm check   # format + typecheck + lint
```

---

## Plugin Development

OpenClaw supports three types of plugins:

- **Channel plugins**: Add new messaging channels (e.g., Matrix, Nostr)
- **Provider plugins**: Add model auth flows (OAuth, API keys)
- **Tool plugins**: Add agent tools

### Quick Start

```bash
# List loaded plugins
openclaw plugins list

# Install from npm
openclaw plugins install @openclaw/voice-call

# Install from local path
openclaw plugins install ./my-plugin
```

### Plugin SDK

```ts
import { ... } from "openclaw/plugin-sdk";
```

Full plugin documentation: [docs/tools/plugin.md](https://docs.openclaw.ai/tools/plugin)

---

## PR Guidelines

### What to Include

- **Summary**: Problem you're solving and your approach
- **Security impact**: Note any security considerations
- **Testing evidence**: Logs, screenshots, or test output

### PR Quality Bar

- Do not trust PR code by default - validate it
- Keep types strict (no `any` in implementation)
- Validate external inputs (CLI, env vars, network payloads)
- Understand the system before changing it

---

## Control UI Decorators

The Control UI uses Lit with **legacy** decorators (current Rollup parsing does not support
`accessor` fields required for standard decorators). When adding reactive fields, keep the
legacy style:

```ts
@state() foo = "bar";
@property({ type: Number }) count = 0;
```

The root `tsconfig.json` is configured for legacy decorators (`experimentalDecorators: true`)
with `useDefineForClassFields: false`. Avoid flipping these unless you are also updating the UI
build tooling to support standard decorators.

---

## AI/Vibe-Coded PRs Welcome!

Built with Codex, Claude, or other AI tools? **Awesome - just mark it!**

Please include in your PR:

- [ ] Mark as AI-assisted in the PR title or description
- [ ] Note the degree of testing (untested / lightly tested / fully tested)
- [ ] Include prompts or session logs if possible (super helpful!)
- [ ] Confirm you understand what the code does

AI PRs are first-class citizens here. We just want transparency so reviewers know what to look for.

---

## Current Focus & Roadmap

We are currently prioritizing:

- **Stability**: Fixing edge cases in channel connections (WhatsApp/Telegram).
- **UX**: Improving the onboarding wizard and error messages.
- **Skills**: For skill contributions, head to [ClawHub](https://clawhub.ai/) — the community hub for OpenClaw skills.
- **Performance**: Optimizing token usage and compaction logic.

Check the [GitHub Issues](https://github.com/openclaw/openclaw/issues) for "good first issue" labels!

---

## Getting Help

- **Discord #setup-help**: Quick questions and troubleshooting
- **GitHub Discussions**: Feature requests and design discussions
- **"good first issue" labels**: Great starting points for new contributors

---

## Becoming a Maintainer

We're selectively expanding the maintainer team.
If you're an experienced contributor who wants to help shape OpenClaw's direction — whether through code, docs, or community — we'd like to hear from you.

Being a maintainer is a responsibility, not an honorary title. We expect active, consistent involvement — triaging issues, reviewing PRs, and helping move the project forward.

Still interested? Email contributing@openclaw.ai with:

- Links to your PRs on OpenClaw (if you don't have any, start there first)
- Links to open source projects you maintain or actively contribute to
- Your GitHub, Discord, and X/Twitter handles
- A brief intro: background, experience, and areas of interest
- Languages you speak and where you're based
- How much time you can realistically commit

We welcome people across all skill sets — engineering, documentation, community management, and more.
We review every human-only-written application carefully and add maintainers slowly and deliberately.
Please allow a few weeks for a response.

---

## Report a Vulnerability

We take security reports seriously. Report vulnerabilities directly to the repository where the issue lives:

- **Core CLI and gateway** — [openclaw/openclaw](https://github.com/openclaw/openclaw)
- **macOS desktop app** — [openclaw/openclaw](https://github.com/openclaw/openclaw) (apps/macos)
- **iOS app** — [openclaw/openclaw](https://github.com/openclaw/openclaw) (apps/ios)
- **Android app** — [openclaw/openclaw](https://github.com/openclaw/openclaw) (apps/android)
- **ClawHub** — [openclaw/clawhub](https://github.com/openclaw/clawhub)
- **Trust and threat model** — [openclaw/trust](https://github.com/openclaw/trust)

For issues that don't fit a specific repo, or if you're unsure, email **security@openclaw.ai** and we'll route it.

### Required in Reports

1. **Title**
2. **Severity Assessment**
3. **Impact**
4. **Affected Component**
5. **Technical Reproduction**
6. **Demonstrated Impact**
7. **Environment**
8. **Remediation Advice**

Reports without reproduction steps, demonstrated impact, and remediation advice will be deprioritized. Given the volume of AI-generated scanner findings, we must ensure we're receiving vetted reports from researchers who understand the issues.
