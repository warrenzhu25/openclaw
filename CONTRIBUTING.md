# Contributing to OpenClaw

Welcome to the lobster tank! This comprehensive guide covers everything you need to know about contributing to OpenClaw, the multi-channel AI gateway with extensible messaging integrations.

---

## Table of Contents

1. [Welcome & Overview](#1-welcome--overview)
2. [Quick Start Guide](#2-quick-start-guide)
3. [Development Environment Setup](#3-development-environment-setup)
4. [Architecture Deep Dive](#4-architecture-deep-dive)
5. [Design Patterns Guide](#5-design-patterns-guide)
6. [Component Reference](#6-component-reference)
7. [Plugin Development Guide](#7-plugin-development-guide)
8. [Testing Guide](#8-testing-guide)
9. [Build & CI/CD Guide](#9-build--cicd-guide)
10. [Code Style & Conventions](#10-code-style--conventions)
11. [Troubleshooting Guide](#11-troubleshooting-guide)
12. [API Reference](#12-api-reference)
13. [FAQ & Common Issues](#13-faq--common-issues)
14. [Appendices](#14-appendices)

---

## 1. Welcome & Overview

### What is OpenClaw?

OpenClaw is a multi-channel AI gateway that connects various messaging platforms (Telegram, Discord, WhatsApp, Slack, Signal, iMessage, and more) to AI agents. It provides a unified interface for managing AI-powered conversations across different communication channels.

### Quick Links

- **GitHub:** https://github.com/openclaw/openclaw
- **Vision:** [`VISION.md`](VISION.md)
- **Discord:** https://discord.gg/qkhbAGHRBT
- **X/Twitter:** [@steipete](https://x.com/steipete) / [@openclaw](https://x.com/openclaw)
- **ClawHub:** https://clawhub.ai/ (community hub for OpenClaw skills)
- **Documentation:** https://docs.openclaw.ai/

### Maintainers

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

### Contribution Types

OpenClaw welcomes contributions in many forms:

| Type | Description | Where to Start |
|------|-------------|----------------|
| **Bug Fixes** | Fix issues in existing functionality | Browse [GitHub Issues](https://github.com/openclaw/openclaw/issues) with "bug" label |
| **Features** | Add new capabilities | Start a [GitHub Discussion](https://github.com/openclaw/openclaw/discussions) first |
| **Documentation** | Improve guides, API docs, examples | Check `docs/` directory |
| **Channels** | Add new messaging platform integrations | See [Plugin Development Guide](#7-plugin-development-guide) |
| **Skills** | Create reusable AI skills | Visit [ClawHub](https://clawhub.ai/) |
| **Tests** | Improve test coverage | See [Testing Guide](#8-testing-guide) |
| **Performance** | Optimize token usage, memory, speed | Profile first, then propose changes |

### Current Focus & Roadmap

We are currently prioritizing:

- **Stability**: Fixing edge cases in channel connections (WhatsApp/Telegram)
- **UX**: Improving the onboarding wizard and error messages
- **Skills**: For skill contributions, head to [ClawHub](https://clawhub.ai/)
- **Performance**: Optimizing token usage and compaction logic

Check the [GitHub Issues](https://github.com/openclaw/openclaw/issues) for "good first issue" labels!

---

## 2. Quick Start Guide

### Prerequisites

Before you begin, ensure you have:

| Requirement | Version | Check Command |
|-------------|---------|---------------|
| Node.js | >= 22.12.0 | `node --version` |
| pnpm | 10.23.0 | `pnpm --version` |
| Git | Latest | `git --version` |

### First-Time Setup

```bash
# 1. Fork and clone the repository
git clone https://github.com/YOUR_USERNAME/openclaw.git
cd openclaw

# 2. Install dependencies
pnpm install

# 3. Build the project
pnpm build

# 4. Run checks to verify setup
pnpm check

# 5. Run tests
pnpm test
```

### Making Your First Contribution

```bash
# 1. Create a feature branch
git checkout -b feature/my-contribution

# 2. Make your changes
# ... edit files ...

# 3. Run the full check suite
pnpm build && pnpm check && pnpm test

# 4. Commit your changes
git add <files>
git commit -m "feat: add my contribution"

# 5. Push and create a PR
git push origin feature/my-contribution
```

### Quick Reference Commands

| Command | Description |
|---------|-------------|
| `pnpm install` | Install all dependencies |
| `pnpm build` | Build the project |
| `pnpm check` | Run format check, type check, and lint |
| `pnpm test` | Run unit tests |
| `pnpm test:fast` | Run unit tests (fast mode) |
| `pnpm test:e2e` | Run end-to-end tests |
| `pnpm lint` | Run oxlint linter |
| `pnpm lint:fix` | Fix linting issues automatically |
| `pnpm format` | Format code with oxfmt |
| `pnpm format:check` | Check code formatting |
| `pnpm dev` | Run development server |
| `pnpm gateway:dev` | Run gateway in dev mode |
| `pnpm tui` | Run terminal UI |
| `pnpm openclaw` | Run the CLI |

---

## 3. Development Environment Setup

### System Requirements

#### macOS

```bash
# Install Homebrew if not present
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Node.js via nvm (recommended)
brew install nvm
nvm install 22
nvm use 22

# Install pnpm
npm install -g pnpm@10.23.0

# Optional: Install platform-specific dependencies
# For WhatsApp (requires native modules):
brew install python3

# For voice features:
brew install ffmpeg

# For macOS app development:
brew install xcodegen swiftlint swiftformat
```

#### Linux (Ubuntu/Debian)

```bash
# Install Node.js via nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc
nvm install 22
nvm use 22

# Install pnpm
npm install -g pnpm@10.23.0

# Install build essentials (for native modules)
sudo apt-get update
sudo apt-get install -y build-essential python3 libsecret-1-dev

# Optional: For voice features
sudo apt-get install -y ffmpeg
```

#### Windows

```powershell
# Install Node.js via nvm-windows
# Download from: https://github.com/coreybutler/nvm-windows/releases

nvm install 22
nvm use 22

# Install pnpm
npm install -g pnpm@10.23.0

# Install Windows Build Tools (for native modules)
npm install -g windows-build-tools
```

### IDE Setup

#### VS Code (Recommended)

Install the following extensions:

```json
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "bradlc.vscode-tailwindcss",
    "ms-vscode.vscode-typescript-next",
    "vitest.explorer"
  ]
}
```

Create `.vscode/settings.json`:

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "typescript.tsdk": "node_modules/typescript/lib",
  "typescript.enablePromptUseWorkspaceTsdk": true,
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

#### Debugging Configuration

Create `.vscode/launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Debug OpenClaw CLI",
      "runtimeExecutable": "pnpm",
      "runtimeArgs": ["openclaw"],
      "console": "integratedTerminal",
      "skipFiles": ["<node_internals>/**"]
    },
    {
      "type": "node",
      "request": "launch",
      "name": "Debug Gateway",
      "runtimeExecutable": "pnpm",
      "runtimeArgs": ["gateway:dev"],
      "console": "integratedTerminal",
      "skipFiles": ["<node_internals>/**"]
    },
    {
      "type": "node",
      "request": "launch",
      "name": "Debug Tests",
      "runtimeExecutable": "pnpm",
      "runtimeArgs": ["test:fast", "--", "--reporter=verbose"],
      "console": "integratedTerminal",
      "skipFiles": ["<node_internals>/**"]
    }
  ]
}
```

### Environment Variables

OpenClaw uses environment variables for configuration. Create a `.env` file in the project root:

```bash
# Core Settings
OPENCLAW_PROFILE=dev                    # Profile name (dev, prod, test)
OPENCLAW_LOG_LEVEL=debug                # Log level (debug, info, warn, error)
OPENCLAW_STATE_DIR=~/.openclaw          # State directory

# AI Provider Keys (add as needed)
OPENAI_API_KEY=sk-...                   # OpenAI API key
ANTHROPIC_API_KEY=sk-ant-...            # Anthropic API key
GOOGLE_AI_API_KEY=...                   # Google AI API key

# Channel Tokens (add as needed)
TELEGRAM_BOT_TOKEN=...                  # Telegram bot token
DISCORD_BOT_TOKEN=...                   # Discord bot token
SLACK_BOT_TOKEN=xoxb-...                # Slack bot token
SLACK_APP_TOKEN=xapp-...                # Slack app token

# Development Settings
OPENCLAW_SKIP_CHANNELS=1                # Skip channel initialization (faster dev)
OPENCLAW_LIVE_TEST=1                    # Enable live tests
```

#### Environment Variables Reference

| Variable | Description | Default |
|----------|-------------|---------|
| `OPENCLAW_PROFILE` | Configuration profile name | `default` |
| `OPENCLAW_STATE_DIR` | Directory for state files | `~/.openclaw` |
| `OPENCLAW_LOG_LEVEL` | Logging verbosity | `info` |
| `OPENCLAW_SKIP_CHANNELS` | Skip channel init in dev | `0` |
| `OPENCLAW_CONFIG_PATH` | Custom config file path | `~/.openclaw/config.yaml` |
| `OPENCLAW_GATEWAY_PORT` | Gateway WebSocket port | `18789` |
| `OPENCLAW_PLUGIN_MANIFEST_CACHE_MS` | Plugin manifest cache TTL | `60000` |
| `VITEST` | Indicates test environment | - |

### Configuration Files

#### `tsconfig.json`

The TypeScript configuration uses strict mode with the following key settings:

```json
{
  "compilerOptions": {
    "strict": true,
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "target": "es2023",
    "experimentalDecorators": true,
    "useDefineForClassFields": false,
    "lib": ["DOM", "DOM.Iterable", "ES2023", "ScriptHost"]
  }
}
```

Key points:
- **Strict mode**: All strict type-checking options enabled
- **ESM modules**: Uses NodeNext module resolution
- **Legacy decorators**: Required for Lit-based Control UI
- **ES2023 target**: Modern JavaScript features

#### `.oxlintrc.json`

The linter configuration:

```json
{
  "plugins": ["unicorn", "typescript", "oxc"],
  "categories": {
    "correctness": "error",
    "perf": "error",
    "suspicious": "error"
  },
  "rules": {
    "typescript/no-explicit-any": "error",
    "curly": "error"
  }
}
```

Key rules:
- **No `any`**: Explicit `any` types are forbidden
- **Curly braces**: Required for all control structures
- **Performance**: Performance-related issues are errors

### Workspace Structure

```
openclaw/
├── src/                    # Main source code
│   ├── agents/            # Agent configuration and tools
│   ├── channels/          # Channel plugin system
│   ├── cli/               # CLI commands
│   ├── config/            # Configuration system
│   ├── gateway/           # WebSocket gateway server
│   ├── hooks/             # Internal hook system
│   ├── infra/             # Infrastructure utilities
│   ├── memory/            # Vector search and embeddings
│   ├── plugins/           # Plugin registry and runtime
│   ├── plugin-sdk/        # Public plugin SDK exports
│   └── ...                # Channel-specific modules
├── extensions/            # Built-in and optional plugins
│   ├── discord/           # Discord channel plugin
│   ├── telegram/          # Telegram channel plugin
│   ├── whatsapp/          # WhatsApp channel plugin
│   └── ...                # Other channel plugins
├── skills/                # Bundled AI skills
├── ui/                    # Web UI (Control UI)
├── apps/                  # Native applications
│   ├── macos/             # macOS app (Swift)
│   ├── ios/               # iOS app (Swift)
│   └── android/           # Android app (Kotlin)
├── test/                  # Test utilities and setup
├── docs/                  # Documentation (Mintlify)
├── scripts/               # Build and utility scripts
└── .github/               # GitHub Actions workflows
```

### Running Development Servers

#### Gateway Development

```bash
# Start gateway with channel skip (fastest)
pnpm gateway:dev

# Start gateway with channels enabled
pnpm dev gateway

# Start gateway with reset (clear state)
pnpm gateway:dev:reset

# Watch mode (auto-restart on changes)
pnpm gateway:watch
```

#### TUI Development

```bash
# Start TUI
pnpm tui

# Start TUI in dev profile
pnpm tui:dev
```

#### UI Development

```bash
# Install UI dependencies
pnpm ui:install

# Start UI dev server
pnpm ui:dev

# Build UI for production
pnpm ui:build
```

---

## 4. Architecture Deep Dive

### High-Level System Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              OpenClaw System                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │  Telegram   │  │   Discord   │  │  WhatsApp   │  │   Slack     │  ...   │
│  │  Channel    │  │   Channel   │  │   Channel   │  │   Channel   │        │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘        │
│         │                │                │                │                │
│         └────────────────┴────────────────┴────────────────┘                │
│                                    │                                         │
│                          ┌─────────▼─────────┐                              │
│                          │   Channel Dock    │                              │
│                          │  (Unified Layer)  │                              │
│                          └─────────┬─────────┘                              │
│                                    │                                         │
│  ┌─────────────────────────────────┼─────────────────────────────────────┐  │
│  │                         Gateway Server                                 │  │
│  │                        (WebSocket :18789)                              │  │
│  │  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐              │  │
│  │  │ Control Plane │  │ Message Router│  │ Session Mgmt  │              │  │
│  │  └───────────────┘  └───────────────┘  └───────────────┘              │  │
│  └─────────────────────────────────┬─────────────────────────────────────┘  │
│                                    │                                         │
│                          ┌─────────▼─────────┐                              │
│                          │   Agent Runtime   │                              │
│                          │  ┌─────────────┐  │                              │
│                          │  │   Tools     │  │                              │
│                          │  │   Hooks     │  │                              │
│                          │  │   Sessions  │  │                              │
│                          │  │   Memory    │  │                              │
│                          │  └─────────────┘  │                              │
│                          └─────────┬─────────┘                              │
│                                    │                                         │
│                          ┌─────────▼─────────┐                              │
│                          │   LLM Providers   │                              │
│                          │  (OpenAI, Claude, │                              │
│                          │   Gemini, etc.)   │                              │
│                          └───────────────────┘                              │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Gateway Architecture

The gateway is the central hub of OpenClaw, managing connections between channels, agents, and clients.

#### WebSocket Control Plane

```
┌─────────────────────────────────────────────────────────────────┐
│                    Gateway Server (:18789)                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────────┐    ┌─────────────────────┐             │
│  │   WebSocket Server  │    │    HTTP Server      │             │
│  │   (Control Plane)   │    │  (Webhooks, OAuth)  │             │
│  └──────────┬──────────┘    └──────────┬──────────┘             │
│             │                           │                        │
│  ┌──────────▼───────────────────────────▼──────────┐            │
│  │              Request Router                      │            │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌────────┐ │            │
│  │  │ config  │ │  send   │ │  talk   │ │ skills │ │            │
│  │  │ methods │ │ methods │ │ methods │ │methods │ │            │
│  │  └─────────┘ └─────────┘ └─────────┘ └────────┘ │            │
│  └─────────────────────────────────────────────────┘            │
│                                                                  │
│  ┌─────────────────────┐    ┌─────────────────────┐             │
│  │   Channel Manager   │    │   Plugin Registry   │             │
│  │  (Start/Stop/Status)│    │  (Load/Unload/Hook) │             │
│  └─────────────────────┘    └─────────────────────┘             │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

#### Gateway Methods

| Method | Description | Source |
|--------|-------------|--------|
| `config.get` | Get current configuration | `src/gateway/server-methods/config.ts` |
| `config.set` | Update configuration | `src/gateway/server-methods/config.ts` |
| `send.text` | Send text message | `src/gateway/server-methods/send.ts` |
| `send.media` | Send media message | `src/gateway/server-methods/send.ts` |
| `talk.start` | Start agent conversation | `src/gateway/server-methods/talk.ts` |
| `talk.stop` | Stop agent conversation | `src/gateway/server-methods/talk.ts` |
| `skills.list` | List available skills | `src/gateway/server-methods/skills.ts` |
| `skills.install` | Install a skill | `src/gateway/server-methods/skills.ts` |

### Plugin System Architecture

OpenClaw uses a plugin architecture with Registry, Strategy, and Composition patterns.

```
┌─────────────────────────────────────────────────────────────────┐
│                     Plugin System                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    Plugin Registry                          ││
│  │  ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐   ││
│  │  │  Channels │ │   Tools   │ │   Hooks   │ │ Providers │   ││
│  │  └───────────┘ └───────────┘ └───────────┘ └───────────┘   ││
│  └─────────────────────────────────────────────────────────────┘│
│                              │                                   │
│  ┌───────────────────────────▼─────────────────────────────────┐│
│  │                    Plugin Runtime                           ││
│  │  ┌─────────────────────────────────────────────────────┐   ││
│  │  │                 OpenClawPluginApi                    │   ││
│  │  │  • registerTool()      • registerHook()             │   ││
│  │  │  • registerChannel()   • registerProvider()         │   ││
│  │  │  • registerService()   • registerCommand()          │   ││
│  │  │  • registerHttpHandler() • registerGatewayMethod()  │   ││
│  │  └─────────────────────────────────────────────────────┘   ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                  │
│  Plugin Sources:                                                 │
│  ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐       │
│  │  Bundled  │ │  Global   │ │ Workspace │ │  Config   │       │
│  │extensions/│ │~/.openclaw│ │  ./node_  │ │ plugins:  │       │
│  │           │ │ /plugins  │ │  modules  │ │   [...]   │       │
│  └───────────┘ └───────────┘ └───────────┘ └───────────┘       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

#### Plugin Origin Types

| Origin | Description | Location |
|--------|-------------|----------|
| `bundled` | Built-in plugins | `extensions/` directory |
| `global` | User-installed globally | `~/.openclaw/plugins/` |
| `workspace` | Project-specific plugins | `./node_modules/` |
| `config` | Configured in YAML/JSON | `plugins:` config section |

### Channel Architecture

Channels follow the Adapter pattern, providing a unified interface for different messaging platforms.

```
┌─────────────────────────────────────────────────────────────────┐
│                      Channel Plugin                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ChannelPlugin<ResolvedAccount, Probe, Audit>                   │
│  ├── id: ChannelId                                              │
│  ├── meta: ChannelMeta                                          │
│  ├── capabilities: ChannelCapabilities                          │
│  │                                                               │
│  ├── Adapters:                                                  │
│  │   ├── config: ChannelConfigAdapter<ResolvedAccount>          │
│  │   ├── setup?: ChannelSetupAdapter                            │
│  │   ├── pairing?: ChannelPairingAdapter                        │
│  │   ├── security?: ChannelSecurityAdapter<ResolvedAccount>     │
│  │   ├── groups?: ChannelGroupAdapter                           │
│  │   ├── mentions?: ChannelMentionAdapter                       │
│  │   ├── outbound?: ChannelOutboundAdapter                      │
│  │   ├── status?: ChannelStatusAdapter<ResolvedAccount>         │
│  │   ├── gateway?: ChannelGatewayAdapter<ResolvedAccount>       │
│  │   ├── auth?: ChannelAuthAdapter                              │
│  │   ├── streaming?: ChannelStreamingAdapter                    │
│  │   ├── threading?: ChannelThreadingAdapter                    │
│  │   ├── messaging?: ChannelMessagingAdapter                    │
│  │   ├── directory?: ChannelDirectoryAdapter                    │
│  │   ├── resolver?: ChannelResolverAdapter                      │
│  │   ├── actions?: ChannelMessageActionAdapter                  │
│  │   └── heartbeat?: ChannelHeartbeatAdapter                    │
│  │                                                               │
│  └── agentTools?: ChannelAgentToolFactory | ChannelAgentTool[]  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

#### Channel Capabilities

```typescript
type ChannelCapabilities = {
  chatTypes: Array<"direct" | "group" | "channel" | "thread">;
  polls?: boolean;           // Can send polls
  reactions?: boolean;       // Can react to messages
  edit?: boolean;            // Can edit messages
  unsend?: boolean;          // Can delete/unsend messages
  reply?: boolean;           // Can reply to specific messages
  effects?: boolean;         // Can send message effects
  groupManagement?: boolean; // Can manage group members
  threads?: boolean;         // Supports threaded conversations
  media?: boolean;           // Can send media (images, files)
  nativeCommands?: boolean;  // Platform has slash commands
  blockStreaming?: boolean;  // Can stream responses in blocks
};
```

#### Built-in Channels

| Channel | ID | Capabilities |
|---------|-----|--------------|
| Telegram | `telegram` | direct, group, channel, thread, commands, block streaming |
| Discord | `discord` | direct, channel, thread, polls, reactions, media, commands, threads |
| WhatsApp | `whatsapp` | direct, group, polls, reactions, media |
| Slack | `slack` | direct, channel, thread, reactions, media, commands, threads |
| Signal | `signal` | direct, group, reactions, media |
| iMessage | `imessage` | direct, group, reactions, media |
| IRC | `irc` | direct, group, media, block streaming |
| Google Chat | `googlechat` | direct, group, thread, reactions, media, threads, block streaming |

### Hook System

The hook system implements Observer/Pub-Sub patterns for extensibility.

```
┌─────────────────────────────────────────────────────────────────┐
│                        Hook System                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                     Hook Registry                           ││
│  │  Map<HookName, Array<{ handler, priority, pluginId }>>     ││
│  └─────────────────────────────────────────────────────────────┘│
│                              │                                   │
│  Hook Lifecycle:             ▼                                   │
│  ┌─────────┐    ┌─────────────────┐    ┌─────────────────────┐ │
│  │  Event  │───▶│  Emit to Hook   │───▶│  Execute Handlers   │ │
│  │ Trigger │    │    Registry     │    │  (by priority)      │ │
│  └─────────┘    └─────────────────┘    └─────────────────────┘ │
│                                                                  │
│  Hook Categories:                                                │
│  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐         │
│  │    Agent      │ │   Message     │ │   Session     │         │
│  │ before_agent  │ │ msg_received  │ │ session_start │         │
│  │ llm_input     │ │ msg_sending   │ │ session_end   │         │
│  │ llm_output    │ │ msg_sent      │ │               │         │
│  │ agent_end     │ │               │ │               │         │
│  └───────────────┘ └───────────────┘ └───────────────┘         │
│  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐         │
│  │     Tool      │ │  Compaction   │ │   Gateway     │         │
│  │ before_tool   │ │ before_compact│ │ gateway_start │         │
│  │ after_tool    │ │ after_compact │ │ gateway_stop  │         │
│  │ tool_persist  │ │ before_reset  │ │               │         │
│  └───────────────┘ └───────────────┘ └───────────────┘         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

#### All Hook Types (20 total)

| Hook Name | Category | Description |
|-----------|----------|-------------|
| `before_model_resolve` | Agent | Before model selection for a run |
| `before_prompt_build` | Agent | Before building the prompt |
| `before_agent_start` | Agent | Before agent execution starts (legacy) |
| `llm_input` | Agent | When sending input to LLM |
| `llm_output` | Agent | When receiving output from LLM |
| `agent_end` | Agent | After agent execution completes |
| `before_compaction` | Compaction | Before compacting session history |
| `after_compaction` | Compaction | After compaction completes |
| `before_reset` | Compaction | Before session reset (/new, /reset) |
| `message_received` | Message | When a message is received |
| `message_sending` | Message | Before sending a message |
| `message_sent` | Message | After a message is sent |
| `before_tool_call` | Tool | Before executing a tool |
| `after_tool_call` | Tool | After tool execution |
| `tool_result_persist` | Tool | Before persisting tool result |
| `before_message_write` | Tool | Before writing message to transcript |
| `session_start` | Session | When a session starts |
| `session_end` | Session | When a session ends |
| `gateway_start` | Gateway | When gateway starts |
| `gateway_stop` | Gateway | When gateway stops |

### Agent Runtime

The agent runtime manages AI agent execution, tools, and sessions.

```
┌─────────────────────────────────────────────────────────────────┐
│                       Agent Runtime                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    Session Manager                          ││
│  │  • Session creation/resumption                              ││
│  │  • Message history management                               ││
│  │  • Compaction (context window management)                   ││
│  │  • JSONL transcript persistence                             ││
│  └─────────────────────────────────────────────────────────────┘│
│                              │                                   │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                     Tool Registry                           ││
│  │  • Built-in tools (bash, read, write, search)              ││
│  │  • Channel tools (send, react, poll)                        ││
│  │  • Plugin-registered tools                                  ││
│  │  • Skill-provided tools                                     ││
│  └─────────────────────────────────────────────────────────────┘│
│                              │                                   │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    Provider Bridge                          ││
│  │  • Model resolution (provider + model)                      ││
│  │  • Auth profile management                                  ││
│  │  • API key rotation                                         ││
│  │  • Rate limiting                                            ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Configuration System

Configuration is managed through YAML/JSON files with validation and migrations.

```
┌─────────────────────────────────────────────────────────────────┐
│                   Configuration System                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Config Sources (priority order):                                │
│  1. Environment variables (OPENCLAW_*)                          │
│  2. CLI flags                                                    │
│  3. Config file (~/.openclaw/config.yaml)                       │
│  4. Default values                                               │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    Config Loading                           ││
│  │  YAML/JSON ──▶ Parse ──▶ Validate (Zod) ──▶ Migrate ──▶ Use││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                  │
│  Config Structure:                                               │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  OpenClawConfig                                             ││
│  │  ├── agent: AgentConfig                                     ││
│  │  │   ├── defaultModel: string                               ││
│  │  │   ├── provider: string                                   ││
│  │  │   └── tools: ToolsConfig                                 ││
│  │  ├── channels: ChannelsConfig                               ││
│  │  │   ├── telegram: TelegramConfig                           ││
│  │  │   ├── discord: DiscordConfig                             ││
│  │  │   ├── whatsapp: WhatsAppConfig                           ││
│  │  │   └── ...                                                 ││
│  │  ├── plugins: PluginsConfig                                 ││
│  │  ├── memory: MemoryConfig                                   ││
│  │  └── gateway: GatewayConfig                                 ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Message Flow Diagrams

#### Inbound Message Flow

```
┌──────────────┐
│   Platform   │  (Telegram, Discord, etc.)
│   Webhook    │
└──────┬───────┘
       │ HTTP POST
       ▼
┌──────────────┐
│   Channel    │
│   Adapter    │
└──────┬───────┘
       │ Normalize message
       ▼
┌──────────────┐
│   Security   │  Check allowFrom, DM policy
│    Gate      │
└──────┬───────┘
       │ If allowed
       ▼
┌──────────────┐
│    Hook:     │  message_received
│  Plugins     │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│   Session    │  Create/resume session
│   Manager    │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│    Agent     │  Execute with tools
│   Runtime    │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│    Hook:     │  message_sending
│  Plugins     │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│   Outbound   │  Format and send
│   Adapter    │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│   Platform   │  Deliver to user
│     API      │
└──────────────┘
```

#### Outbound Message Flow

```
┌──────────────┐
│    Agent     │  Tool call: send_message
│   Decision   │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│    Tool      │  Validate parameters
│  Execution   │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│    Hook:     │  message_sending (can modify/block)
│  Plugins     │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│   Outbound   │  Resolve target, chunk text
│   Router     │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│   Channel    │  Platform-specific formatting
│   Adapter    │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│   Platform   │  API call (REST/WebSocket)
│     API      │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│    Hook:     │  message_sent
│  Plugins     │
└──────────────┘
```

---

## 5. Design Patterns Guide

### Plugin Architecture Pattern

OpenClaw uses a plugin architecture that combines Registry, Factory, and Strategy patterns.

#### The Registry Pattern

Plugins register their capabilities with a central registry at startup:

```typescript
// src/plugins/types.ts
export type OpenClawPluginApi = {
  id: string;
  name: string;
  version?: string;
  description?: string;
  source: string;
  config: OpenClawConfig;
  pluginConfig?: Record<string, unknown>;
  runtime: PluginRuntime;
  logger: PluginLogger;

  // Registration methods
  registerTool: (tool: AnyAgentTool | OpenClawPluginToolFactory, opts?: OpenClawPluginToolOptions) => void;
  registerHook: (events: string | string[], handler: InternalHookHandler, opts?: OpenClawPluginHookOptions) => void;
  registerHttpHandler: (handler: OpenClawPluginHttpHandler) => void;
  registerHttpRoute: (params: { path: string; handler: OpenClawPluginHttpRouteHandler }) => void;
  registerChannel: (registration: OpenClawPluginChannelRegistration | ChannelPlugin) => void;
  registerGatewayMethod: (method: string, handler: GatewayRequestHandler) => void;
  registerCli: (registrar: OpenClawPluginCliRegistrar, opts?: { commands?: string[] }) => void;
  registerService: (service: OpenClawPluginService) => void;
  registerProvider: (provider: ProviderPlugin) => void;
  registerCommand: (command: OpenClawPluginCommandDefinition) => void;

  // Utilities
  resolvePath: (input: string) => string;
  on: <K extends PluginHookName>(hookName: K, handler: PluginHookHandlerMap[K], opts?: { priority?: number }) => void;
};
```

**Usage Example:**

```typescript
// extensions/my-plugin/index.ts
import type { OpenClawPluginDefinition } from "openclaw/plugin-sdk";

const plugin: OpenClawPluginDefinition = {
  id: "my-plugin",
  name: "My Plugin",
  version: "1.0.0",

  register: (api) => {
    // Register a tool
    api.registerTool({
      name: "my_tool",
      description: "Does something useful",
      parameters: { type: "object", properties: {} },
      execute: async () => ({ result: "done" }),
    });

    // Register a hook
    api.on("message_received", (event, ctx) => {
      api.logger.info(`Message from ${event.from}: ${event.content}`);
    });
  },
};

export default plugin;
```

### Strategy Pattern for Channels

Each channel implements a common interface with platform-specific strategies:

```typescript
// src/channels/plugins/types.plugin.ts
export type ChannelPlugin<ResolvedAccount = any, Probe = unknown, Audit = unknown> = {
  id: ChannelId;
  meta: ChannelMeta;
  capabilities: ChannelCapabilities;

  // Required adapter
  config: ChannelConfigAdapter<ResolvedAccount>;

  // Optional adapters (strategies)
  setup?: ChannelSetupAdapter;
  pairing?: ChannelPairingAdapter;
  security?: ChannelSecurityAdapter<ResolvedAccount>;
  groups?: ChannelGroupAdapter;
  mentions?: ChannelMentionAdapter;
  outbound?: ChannelOutboundAdapter;
  status?: ChannelStatusAdapter<ResolvedAccount, Probe, Audit>;
  gateway?: ChannelGatewayAdapter<ResolvedAccount>;
  auth?: ChannelAuthAdapter;
  streaming?: ChannelStreamingAdapter;
  threading?: ChannelThreadingAdapter;
  messaging?: ChannelMessagingAdapter;
  directory?: ChannelDirectoryAdapter;
  resolver?: ChannelResolverAdapter;
  actions?: ChannelMessageActionAdapter;
  heartbeat?: ChannelHeartbeatAdapter;

  // Optional tools
  agentTools?: ChannelAgentToolFactory | ChannelAgentTool[];
};
```

**Strategy Example - Outbound Adapter:**

```typescript
// Different channels implement outbound differently
const telegramOutbound: ChannelOutboundAdapter = {
  deliveryMode: "direct",
  textChunkLimit: 4000,

  sendText: async ({ cfg, to, text, replyToId }) => {
    // Telegram-specific sending logic
    const bot = getTelegramBot(cfg);
    const result = await bot.sendMessage(to, text, {
      reply_to_message_id: replyToId,
    });
    return { channel: "telegram", messageId: result.message_id };
  },
};

const whatsappOutbound: ChannelOutboundAdapter = {
  deliveryMode: "gateway",  // Uses gateway for WebSocket connection
  textChunkLimit: 4000,

  sendText: async ({ cfg, to, text }) => {
    // WhatsApp-specific logic via Baileys
    const sock = getWhatsAppSocket(cfg);
    const result = await sock.sendMessage(to, { text });
    return { channel: "whatsapp", messageId: result.key.id };
  },
};
```

### Factory Functions Over Constructors

OpenClaw prefers factory functions over classes for several reasons:

1. **Simpler testing** - Easier to mock and stub
2. **Better tree-shaking** - Unused code can be eliminated
3. **No `this` binding issues** - Clearer scope
4. **Composition over inheritance** - More flexible

**Bad - Class with constructor:**

```typescript
// Avoid this pattern
class ChannelManager {
  private channels: Map<string, Channel>;

  constructor(config: Config) {
    this.channels = new Map();
    this.initializeChannels(config);
  }

  private initializeChannels(config: Config) {
    // ...
  }
}
```

**Good - Factory function:**

```typescript
// Prefer this pattern
type ChannelManager = {
  channels: Map<string, Channel>;
  getChannel: (id: string) => Channel | undefined;
  startChannel: (id: string) => Promise<void>;
};

function createChannelManager(config: Config): ChannelManager {
  const channels = new Map<string, Channel>();

  // Initialize channels
  for (const [id, channelConfig] of Object.entries(config.channels)) {
    channels.set(id, createChannel(id, channelConfig));
  }

  return {
    channels,
    getChannel: (id) => channels.get(id),
    startChannel: async (id) => {
      const channel = channels.get(id);
      if (channel) await channel.start();
    },
  };
}
```

### Context-Based Dependency Injection

Instead of constructor injection or service locators, OpenClaw uses context objects:

```typescript
// Context types carry dependencies
type PluginHookAgentContext = {
  agentId?: string;
  sessionKey?: string;
  sessionId?: string;
  workspaceDir?: string;
  messageProvider?: string;
};

type ChannelGatewayContext<ResolvedAccount = unknown> = {
  cfg: OpenClawConfig;
  accountId: string;
  account: ResolvedAccount;
  runtime: RuntimeEnv;
  abortSignal: AbortSignal;
  log?: ChannelLogSink;
  getStatus: () => ChannelAccountSnapshot;
  setStatus: (next: ChannelAccountSnapshot) => void;
};

// Handlers receive context as parameter
type PluginHookHandler = (
  event: PluginHookEvent,
  ctx: PluginHookContext,
) => Promise<PluginHookResult | void> | PluginHookResult | void;
```

**Benefits:**

1. **Explicit dependencies** - Clear what each function needs
2. **Testable** - Easy to create mock contexts
3. **Type-safe** - TypeScript catches missing dependencies
4. **Scoped** - Context can vary per call

### Discriminated Unions for Events

Events use discriminated unions for type-safe handling:

```typescript
// Each hook has specific event and result types
type PluginHookName =
  | "before_model_resolve"
  | "before_prompt_build"
  | "before_agent_start"
  | "llm_input"
  | "llm_output"
  | "agent_end"
  // ... more hooks

// Event types are specific to each hook
type PluginHookBeforeToolCallEvent = {
  toolName: string;
  params: Record<string, unknown>;
};

type PluginHookAfterToolCallEvent = {
  toolName: string;
  params: Record<string, unknown>;
  result?: unknown;
  error?: string;
  durationMs?: number;
};

// Handler map provides type-safe dispatch
type PluginHookHandlerMap = {
  before_tool_call: (
    event: PluginHookBeforeToolCallEvent,
    ctx: PluginHookToolContext,
  ) => Promise<PluginHookBeforeToolCallResult | void> | PluginHookBeforeToolCallResult | void;

  after_tool_call: (
    event: PluginHookAfterToolCallEvent,
    ctx: PluginHookToolContext,
  ) => Promise<void> | void;

  // ... more handlers
};
```

### Error Handling Patterns

OpenClaw uses custom Error classes with structured data:

```typescript
// Custom error types
class ChannelConfigError extends Error {
  constructor(
    public readonly channel: string,
    public readonly field: string,
    message: string,
  ) {
    super(`[${channel}] Config error in '${field}': ${message}`);
    this.name = "ChannelConfigError";
  }
}

class OutboundDeliveryError extends Error {
  constructor(
    public readonly channel: string,
    public readonly target: string,
    public readonly cause?: Error,
  ) {
    super(`Failed to deliver to ${channel}:${target}`);
    this.name = "OutboundDeliveryError";
  }
}

// Result types for operations that can fail
type OutboundDeliveryResult = {
  channel: ChannelId;
  messageId?: string;
  error?: string;
  retryable?: boolean;
};

// Validation results
type PluginConfigValidation =
  | { ok: true; value?: unknown }
  | { ok: false; errors: string[] };
```

### Composition over Inheritance

OpenClaw avoids deep inheritance hierarchies in favor of composition:

**Bad - Inheritance:**

```typescript
// Avoid this pattern
class BaseChannel {
  protected config: Config;
  connect() { /* ... */ }
}

class TelegramChannel extends BaseChannel {
  private bot: TelegramBot;
  connect() {
    super.connect();
    // Telegram-specific...
  }
}

class WhatsAppChannel extends BaseChannel {
  private socket: WASocket;
  connect() {
    super.connect();
    // WhatsApp-specific...
  }
}
```

**Good - Composition:**

```typescript
// Prefer this pattern - compose behaviors
type ConnectionManager = {
  connect: () => Promise<void>;
  disconnect: () => Promise<void>;
  isConnected: () => boolean;
};

type MessageSender = {
  sendText: (to: string, text: string) => Promise<SendResult>;
  sendMedia: (to: string, media: Media) => Promise<SendResult>;
};

// Compose into channel
function createTelegramChannel(config: TelegramConfig): ChannelPlugin {
  const connection = createTelegramConnection(config);
  const sender = createTelegramSender(config);

  return {
    id: "telegram",
    meta: { /* ... */ },
    capabilities: { /* ... */ },
    config: createTelegramConfigAdapter(config),
    gateway: {
      startAccount: () => connection.connect(),
      stopAccount: () => connection.disconnect(),
    },
    outbound: {
      deliveryMode: "direct",
      sendText: sender.sendText,
      sendMedia: sender.sendMedia,
    },
  };
}
```

---

## 6. Component Reference

### `src/agents/` - Agent System

The agents module handles AI agent configuration, authentication, and tool execution.

#### Directory Structure

```
src/agents/
├── auth-profiles/         # Authentication profile management
│   ├── index.ts          # Profile registry
│   └── types.ts          # Credential types
├── schema/               # Schema utilities for tools
│   └── typebox.ts        # TypeBox helpers
├── tools/                # Built-in agent tools
│   ├── common.ts         # Shared tool utilities
│   ├── discord-actions.ts # Discord-specific tools
│   └── slack-actions.ts  # Slack-specific tools
├── identity.ts           # Agent identity management
├── model-scan.ts         # Model discovery
├── pi-embedded-runner.ts # Embedded agent runner
├── sandbox.ts            # Sandboxing support
├── sandbox-paths.ts      # Sandbox path resolution
├── skills-install.ts     # Skill installation
└── pi-tool-definition-adapter.ts # Tool definition adapters
```

#### Key Types

```typescript
// src/agents/tools/common.ts
export type AnyAgentTool = AgentTool<TSchema, unknown>;

// Tool creation utilities
export function createActionGate(params: {
  action: string;
  channel?: string;
}): { allowed: boolean; reason?: string };

export function jsonResult<T>(data: T): AgentToolResult<T>;

export function readStringParam(
  args: Record<string, unknown>,
  key: string,
): string | undefined;

export function readNumberParam(
  args: Record<string, unknown>,
  key: string,
): number | undefined;
```

#### Auth Profiles

```typescript
// src/agents/auth-profiles/types.ts
export type AuthProfileCredential =
  | { kind: "api_key"; apiKey: string }
  | { kind: "oauth"; accessToken: string; refreshToken?: string; expiresAt?: number }
  | { kind: "token"; token: string }
  | { kind: "custom"; data: Record<string, unknown> };

export type OAuthCredential = {
  accessToken: string;
  refreshToken?: string;
  expiresAt?: number;
  scope?: string;
};
```

### `src/gateway/` - Gateway Server

The gateway provides WebSocket and HTTP interfaces for clients.

#### Directory Structure

```
src/gateway/
├── server-methods/       # RPC method handlers
│   ├── config.ts        # Configuration methods
│   ├── send.ts          # Message sending methods
│   ├── skills.ts        # Skills management
│   ├── talk.ts          # Agent conversation methods
│   ├── types.ts         # Handler types
│   ├── web.ts           # Web-related methods
│   └── wizard.ts        # Setup wizard methods
├── protocol/            # Protocol definitions
├── call.ts              # Gateway RPC client
├── client.ts            # Gateway client implementation
├── control-ui.ts        # Control UI serving
├── server.ts            # Main server implementation
├── server-bridge.ts     # Bridge to channel adapters
└── server-channels.ts   # Channel management
```

#### Gateway Request Handler

```typescript
// src/gateway/server-methods/types.ts
export type GatewayRequestHandler = (
  params: Record<string, unknown>,
  opts: GatewayRequestHandlerOptions,
) => Promise<unknown> | unknown;

export type GatewayRequestHandlerOptions = {
  cfg: OpenClawConfig;
  runtime: RuntimeEnv;
  respond: RespondFn;
  clientName?: string;
  clientMode?: string;
};

export type RespondFn = (data: unknown) => void;
```

#### Registering Gateway Methods

```typescript
// In a plugin
api.registerGatewayMethod("myPlugin.doSomething", async (params, opts) => {
  const { cfg, runtime, respond } = opts;

  // Validate params
  const input = params.input as string;
  if (!input) {
    throw new Error("Missing required parameter: input");
  }

  // Process and respond
  const result = await processInput(input, cfg);
  return { success: true, result };
});
```

### `src/channels/` - Channel System

The channels module implements the adapter pattern for messaging platforms.

#### Directory Structure

```
src/channels/
├── plugins/              # Channel plugin infrastructure
│   ├── types.ts         # Re-exports
│   ├── types.core.ts    # Core channel types
│   ├── types.plugin.ts  # ChannelPlugin interface
│   ├── types.adapters.ts # Adapter interfaces
│   ├── onboarding-types.ts # Onboarding types
│   ├── onboarding/      # Platform onboarding wizards
│   ├── normalize/       # Target normalization
│   ├── status-issues/   # Status issue collectors
│   └── ...
├── dock.ts              # ChannelDock unified interface
├── registry.ts          # Channel metadata registry
├── chat-type.ts         # Chat type definitions
├── allowlists/          # Allowlist resolution
├── ack-reactions.ts     # Acknowledgment reactions
├── mention-gating.ts    # Mention-based gating
├── typing.ts            # Typing indicator support
├── reply-prefix.ts      # Reply prefix handling
├── command-gating.ts    # Command access control
├── location.ts          # Location message handling
├── logging.ts           # Channel-specific logging
└── session.ts           # Session tracking
```

#### ChannelDock

The ChannelDock provides a lightweight, unified interface for channels:

```typescript
// src/channels/dock.ts
export type ChannelDock = {
  id: ChannelId;
  capabilities: ChannelCapabilities;
  commands?: ChannelCommandAdapter;
  outbound?: {
    textChunkLimit?: number;
  };
  streaming?: ChannelDockStreaming;
  elevated?: ChannelElevatedAdapter;
  config?: {
    resolveAllowFrom?: (params: {
      cfg: OpenClawConfig;
      accountId?: string | null;
    }) => Array<string | number> | undefined;
    formatAllowFrom?: (params: {
      cfg: OpenClawConfig;
      accountId?: string | null;
      allowFrom: Array<string | number>;
    }) => string[];
  };
  groups?: ChannelGroupAdapter;
  mentions?: ChannelMentionAdapter;
  threading?: ChannelThreadingAdapter;
  agentPrompt?: ChannelAgentPromptAdapter;
};

// Get a channel dock
export function getChannelDock(id: ChannelId): ChannelDock | undefined;

// List all channel docks
export function listChannelDocks(): ChannelDock[];
```

### `src/config/` - Configuration System

The config module handles configuration loading, validation, and migrations.

#### Directory Structure

```
src/config/
├── config.ts            # Main config type and loading
├── types.ts             # Configuration type definitions
├── types.tools.ts       # Tool-related config types
├── group-policy.ts      # Group policy resolution
├── zod-schema.core.ts   # Core Zod schemas
├── zod-schema.providers-core.ts # Provider schemas
├── zod-schema.providers-whatsapp.ts # WhatsApp schemas
├── zod-schema.agent-runtime.ts # Agent runtime schemas
└── migrations/          # Config migration scripts
```

#### OpenClawConfig Type

```typescript
// src/config/config.ts
export type OpenClawConfig = {
  // Agent configuration
  agent?: {
    defaultModel?: string;
    provider?: string;
    tools?: ToolsConfig;
    memory?: MemoryConfig;
  };

  // Channel configurations
  channels?: {
    telegram?: TelegramConfig;
    discord?: DiscordConfig;
    whatsapp?: WhatsAppConfig;
    slack?: SlackConfig;
    signal?: SignalConfig;
    imessage?: IMessageConfig;
    irc?: IrcConfig;
    googlechat?: GoogleChatConfig;
    // ... more channels
  };

  // Plugin configuration
  plugins?: {
    enabled?: string[];
    disabled?: string[];
    config?: Record<string, unknown>;
  };

  // Gateway configuration
  gateway?: {
    port?: number;
    host?: string;
    cors?: CorsConfig;
  };
};
```

### `src/plugins/` - Plugin System

The plugins module manages plugin discovery, loading, and lifecycle.

#### Directory Structure

```
src/plugins/
├── types.ts             # Plugin type definitions
├── registry.ts          # Plugin registry
├── runtime.ts           # Plugin runtime management
├── runtime/             # Runtime utilities
│   └── types.ts         # Runtime types
├── config-schema.ts     # Config schema utilities
├── http-path.ts         # HTTP path normalization
├── http-registry.ts     # HTTP handler registry
└── voice-call.plugin.ts # Example bundled plugin
```

#### Plugin Definition

```typescript
// src/plugins/types.ts
export type OpenClawPluginDefinition = {
  id?: string;
  name?: string;
  description?: string;
  version?: string;
  kind?: PluginKind;
  configSchema?: OpenClawPluginConfigSchema;
  register?: (api: OpenClawPluginApi) => void | Promise<void>;
  activate?: (api: OpenClawPluginApi) => void | Promise<void>;
};

// Plugin can also be a simple function
export type OpenClawPluginModule =
  | OpenClawPluginDefinition
  | ((api: OpenClawPluginApi) => void | Promise<void>);
```

### `src/hooks/` - Hook System

The hooks module implements the internal event system.

#### Directory Structure

```
src/hooks/
├── types.ts             # Hook type definitions
├── internal-hooks.ts    # Internal hook handlers
└── bundled/             # Bundled hook implementations
```

#### Hook Registration

```typescript
// Using the api.on() method
api.on("message_received", (event, ctx) => {
  console.log(`Received: ${event.content} from ${event.from}`);
}, { priority: 100 });

// Using registerHook for more control
api.registerHook(
  ["message_sending", "message_sent"],
  async (event, ctx) => {
    // Handle both events
  },
  {
    name: "message-logger",
    description: "Logs all outgoing messages",
  }
);
```

### `src/cli/` - CLI Commands

The CLI module provides command-line interface commands.

#### Directory Structure

```
src/cli/
├── index.ts             # CLI entry point
├── commands/            # Individual commands
└── options/             # Option parsing utilities
```

### `src/memory/` - Memory System

The memory module handles vector search, embeddings, and full-text search.

#### Features

- Vector similarity search (SQLite-vec)
- Full-text search (FTS5)
- Embedding generation
- Memory compaction

### `src/infra/` - Infrastructure

The infra module provides cross-cutting infrastructure utilities.

#### Key Utilities

```typescript
// Error handling
export { formatErrorMessage } from "./errors.js";

// HTTP utilities
export {
  readJsonBodyWithLimit,
  readRequestBodyWithLimit,
  installRequestBodyLimitGuard,
} from "./http-body.js";

// Network security
export { fetchWithSsrFGuard, isPrivateIpAddress } from "./net/ssrf.js";

// Deduplication
export { createDedupeCache, type DedupeCache } from "./dedupe.js";

// Diagnostics
export { emitDiagnosticEvent, onDiagnosticEvent } from "./diagnostic-events.js";

// Device pairing
export { approveDevicePairing, listDevicePairing, rejectDevicePairing } from "./device-pairing.js";
```

### `src/browser/` - Browser Automation

The browser module provides Playwright-based automation for web interfaces.

### `extensions/` - Built-in Extensions

Each extension is a self-contained plugin:

| Extension | Description |
|-----------|-------------|
| `bluebubbles` | BlueBubbles (iMessage bridge) integration |
| `copilot-proxy` | Copilot compatibility proxy |
| `device-pair` | Device pairing protocol |
| `diagnostics-otel` | OpenTelemetry diagnostics |
| `discord` | Discord channel plugin |
| `feishu` | Feishu/Lark integration |
| `googlechat` | Google Chat integration |
| `imessage` | iMessage integration |
| `irc` | IRC protocol support |
| `line` | LINE messaging integration |
| `llm-task` | LLM task execution |
| `lobster` | Lobster protocol support |
| `matrix` | Matrix protocol support |
| `mattermost` | Mattermost integration |
| `memory-core` | Core memory functionality |
| `memory-lancedb` | LanceDB memory backend |
| `msteams` | Microsoft Teams integration |
| `nextcloud-talk` | Nextcloud Talk integration |
| `nostr` | Nostr protocol support |
| `open-prose` | Open Prose integration |
| `phone-control` | Phone control features |
| `signal` | Signal integration |
| `slack` | Slack integration |
| `talk-voice` | Voice conversation support |
| `telegram` | Telegram integration |
| `thread-ownership` | Thread ownership management |
| `tlon` | Tlon/Urbit integration |
| `twitch` | Twitch integration |
| `voice-call` | Voice calling support |
| `whatsapp` | WhatsApp integration |
| `zalo` | Zalo integration |

### `skills/` - Built-in Skills

Skills are reusable AI capabilities. For community skills, visit [ClawHub](https://clawhub.ai/).

---

## 7. Plugin Development Guide

### Plugin Structure

A typical plugin has this structure:

```
my-plugin/
├── package.json          # Plugin manifest
├── src/
│   ├── index.ts         # Main entry point
│   └── ...              # Additional modules
├── test/
│   └── index.test.ts    # Plugin tests
└── README.md            # Documentation
```

### Package.json Configuration

```json
{
  "name": "openclaw-plugin-example",
  "version": "1.0.0",
  "type": "module",
  "main": "dist/index.js",
  "openclaw": {
    "extensions": ["./dist/index.js"],
    "compatibleWith": ">=2024.1.0"
  },
  "peerDependencies": {
    "openclaw": "*"
  }
}
```

### OpenClawPluginApi Reference

The `OpenClawPluginApi` object is passed to your plugin's `register` or `activate` function:

```typescript
type OpenClawPluginApi = {
  // Identity
  id: string;                          // Plugin ID
  name: string;                        // Plugin display name
  version?: string;                    // Plugin version
  description?: string;                // Plugin description
  source: string;                      // Plugin source path

  // Configuration
  config: OpenClawConfig;              // Current OpenClaw config
  pluginConfig?: Record<string, unknown>; // Plugin-specific config

  // Runtime
  runtime: PluginRuntime;              // Runtime environment
  logger: PluginLogger;                // Logging interface

  // Registration methods
  registerTool(tool, opts?): void;     // Register an agent tool
  registerHook(events, handler, opts?): void; // Register hook handler
  registerHttpHandler(handler): void;  // Register HTTP middleware
  registerHttpRoute(params): void;     // Register HTTP route
  registerChannel(registration): void; // Register channel plugin
  registerGatewayMethod(method, handler): void; // Register gateway method
  registerCli(registrar, opts?): void; // Register CLI commands
  registerService(service): void;      // Register background service
  registerProvider(provider): void;    // Register LLM provider
  registerCommand(command): void;      // Register chat command

  // Utilities
  resolvePath(input): string;          // Resolve relative paths
  on<K>(hookName, handler, opts?): void; // Type-safe hook registration
};
```

### Creating a Channel Plugin

Here's a complete example of a channel plugin:

```typescript
// my-channel/src/index.ts
import type {
  OpenClawPluginDefinition,
  ChannelPlugin,
  ChannelCapabilities,
  ChannelMeta,
  ChannelConfigAdapter,
  ChannelOutboundAdapter,
  OpenClawConfig,
} from "openclaw/plugin-sdk";

// Define your account type
type MyChannelAccount = {
  apiKey: string;
  webhookUrl?: string;
  allowFrom?: string[];
};

// Define capabilities
const capabilities: ChannelCapabilities = {
  chatTypes: ["direct", "group"],
  media: true,
  reactions: false,
  polls: false,
};

// Define metadata
const meta: ChannelMeta = {
  id: "mychannel",
  label: "My Channel",
  selectionLabel: "My Channel",
  docsPath: "/channels/mychannel",
  blurb: "Connect to My Channel messaging platform",
};

// Config adapter
const configAdapter: ChannelConfigAdapter<MyChannelAccount> = {
  listAccountIds: (cfg: OpenClawConfig) => {
    const myChannel = cfg.channels?.mychannel as Record<string, unknown> | undefined;
    if (!myChannel?.accounts) return ["default"];
    return Object.keys(myChannel.accounts);
  },

  resolveAccount: (cfg: OpenClawConfig, accountId?: string | null): MyChannelAccount => {
    const myChannel = cfg.channels?.mychannel as Record<string, unknown> | undefined;
    const accounts = myChannel?.accounts as Record<string, MyChannelAccount> | undefined;
    const id = accountId || "default";
    return accounts?.[id] || { apiKey: "" };
  },

  isConfigured: async (account: MyChannelAccount) => {
    return Boolean(account.apiKey);
  },
};

// Outbound adapter
const outboundAdapter: ChannelOutboundAdapter = {
  deliveryMode: "direct",
  textChunkLimit: 4000,

  sendText: async ({ cfg, to, text, accountId }) => {
    const account = configAdapter.resolveAccount(cfg, accountId);

    // Your platform's API call
    const response = await fetch("https://api.mychannel.com/send", {
      method: "POST",
      headers: {
        "Authorization": `Bearer ${account.apiKey}`,
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ to, text }),
    });

    const result = await response.json();
    return {
      channel: "mychannel",
      messageId: result.id,
    };
  },

  sendMedia: async ({ cfg, to, text, mediaUrl, accountId }) => {
    const account = configAdapter.resolveAccount(cfg, accountId);

    const response = await fetch("https://api.mychannel.com/send", {
      method: "POST",
      headers: {
        "Authorization": `Bearer ${account.apiKey}`,
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ to, text, mediaUrl }),
    });

    const result = await response.json();
    return {
      channel: "mychannel",
      messageId: result.id,
    };
  },
};

// Create the channel plugin
const myChannelPlugin: ChannelPlugin<MyChannelAccount> = {
  id: "mychannel",
  meta,
  capabilities,
  config: configAdapter,
  outbound: outboundAdapter,
};

// Export as OpenClaw plugin
const plugin: OpenClawPluginDefinition = {
  id: "mychannel",
  name: "My Channel",
  version: "1.0.0",

  register: (api) => {
    api.registerChannel(myChannelPlugin);

    api.logger.info("My Channel plugin registered");
  },
};

export default plugin;
```

### Creating Tools

```typescript
import type { AnyAgentTool } from "openclaw/plugin-sdk";
import { Type } from "@sinclair/typebox";

// Simple tool
const myTool: AnyAgentTool = {
  name: "my_tool",
  description: "Does something useful",
  parameters: Type.Object({
    input: Type.String({ description: "The input to process" }),
    count: Type.Optional(Type.Number({ description: "Number of times" })),
  }),
  execute: async (args) => {
    const { input, count = 1 } = args;
    const result = input.repeat(count);
    return { result };
  },
};

// Tool factory for dynamic tools
const myToolFactory: OpenClawPluginToolFactory = (ctx) => {
  if (!ctx.config?.myPlugin?.enabled) {
    return null; // Don't provide tool if disabled
  }

  return {
    name: "context_aware_tool",
    description: "A tool that adapts to context",
    parameters: Type.Object({
      query: Type.String(),
    }),
    execute: async (args) => {
      // Can access ctx.agentId, ctx.sessionKey, etc.
      return { answer: `Processed in agent ${ctx.agentId}` };
    },
  };
};

// Register in plugin
api.registerTool(myTool);
api.registerTool(myToolFactory);
```

### Creating Hooks

```typescript
// Type-safe hook with api.on()
api.on("before_agent_start", (event, ctx) => {
  // Access event.prompt, event.messages
  return {
    systemPrompt: "Additional system context...",
    prependContext: "User prefers formal language.",
  };
}, { priority: 50 });

// Message interception
api.on("message_sending", (event, ctx) => {
  // Modify or block outgoing messages
  if (event.content.includes("secret")) {
    return {
      content: "[REDACTED]",
    };
  }
  // Return nothing to pass through unchanged
}, { priority: 100 });

// Tool interception
api.on("before_tool_call", (event, ctx) => {
  // Block certain tools
  if (event.toolName === "dangerous_tool") {
    return {
      block: true,
      blockReason: "This tool is disabled by policy",
    };
  }

  // Modify parameters
  return {
    params: {
      ...event.params,
      audit: true,
    },
  };
});

// Session tracking
api.on("session_start", (event, ctx) => {
  api.logger.info(`Session started: ${event.sessionId}`);
});

api.on("session_end", (event, ctx) => {
  api.logger.info(`Session ended: ${event.sessionId}, messages: ${event.messageCount}`);
});
```

### Plugin Configuration Schemas

```typescript
import { z } from "zod";

// Define config schema with Zod
const configSchema = {
  safeParse: (value: unknown) => {
    const schema = z.object({
      apiKey: z.string(),
      endpoint: z.string().url().optional(),
      maxRetries: z.number().int().min(1).max(10).default(3),
    });

    const result = schema.safeParse(value);
    if (result.success) {
      return { success: true, data: result.data };
    }
    return {
      success: false,
      error: {
        issues: result.error.issues.map(i => ({
          path: i.path.map(String),
          message: i.message,
        })),
      },
    };
  },

  // UI hints for configuration UI
  uiHints: {
    apiKey: {
      label: "API Key",
      help: "Your My Channel API key",
      sensitive: true,
      placeholder: "mc_...",
    },
    endpoint: {
      label: "Custom Endpoint",
      help: "Override the default API endpoint",
      advanced: true,
    },
    maxRetries: {
      label: "Max Retries",
      help: "Number of retry attempts for failed requests",
    },
  },
};

// Use in plugin definition
const plugin: OpenClawPluginDefinition = {
  id: "my-plugin",
  configSchema,
  register: (api) => {
    // api.pluginConfig is validated against configSchema
    const { apiKey, endpoint, maxRetries } = api.pluginConfig as {
      apiKey: string;
      endpoint?: string;
      maxRetries: number;
    };
  },
};
```

### Testing Plugins

```typescript
// my-plugin/test/index.test.ts
import { describe, it, expect, vi, beforeEach } from "vitest";
import { createTestRegistry } from "../../../src/test-utils/channel-plugins.js";

describe("My Channel Plugin", () => {
  let mockApi: OpenClawPluginApi;

  beforeEach(() => {
    mockApi = {
      id: "my-plugin",
      name: "My Plugin",
      source: "test",
      config: {},
      logger: {
        info: vi.fn(),
        warn: vi.fn(),
        error: vi.fn(),
      },
      registerTool: vi.fn(),
      registerHook: vi.fn(),
      registerChannel: vi.fn(),
      // ... other mocked methods
    } as unknown as OpenClawPluginApi;
  });

  it("registers the channel", async () => {
    const plugin = await import("../src/index.js");
    await plugin.default.register?.(mockApi);

    expect(mockApi.registerChannel).toHaveBeenCalledWith(
      expect.objectContaining({
        id: "mychannel",
      })
    );
  });

  it("sends text messages", async () => {
    const plugin = await import("../src/index.js");
    const channel = plugin.myChannelPlugin;

    const result = await channel.outbound?.sendText?.({
      cfg: { channels: { mychannel: { apiKey: "test-key" } } },
      to: "user123",
      text: "Hello!",
    });

    expect(result?.channel).toBe("mychannel");
    expect(result?.messageId).toBeDefined();
  });
});
```

---

## 8. Testing Guide

### Test Infrastructure Overview

OpenClaw uses **Vitest 4.0.18** as its test runner with multiple configurations:

| Config | File | Purpose |
|--------|------|---------|
| Base | `vitest.config.ts` | Shared settings |
| Unit | `vitest.unit.config.ts` | Unit tests |
| E2E | `vitest.e2e.config.ts` | End-to-end tests |
| Live | `vitest.live.config.ts` | Live API tests |
| Extensions | `vitest.extensions.config.ts` | Extension tests |
| Gateway | `vitest.gateway.config.ts` | Gateway-specific tests |

### Test File Naming Conventions

| Pattern | Type | Location |
|---------|------|----------|
| `*.test.ts` | Unit tests | Same directory as source |
| `*.e2e.test.ts` | End-to-end tests | Same directory as source |
| `*.live.test.ts` | Live API tests | Same directory as source |
| `test/**/*.test.ts` | Integration tests | `test/` directory |

### Test Isolation

OpenClaw provides isolation utilities to prevent test pollution:

```typescript
// test/test-env.ts
import { withIsolatedTestHome } from "./test-env.js";

// In test/setup.ts - applied globally
const testEnv = withIsolatedTestHome();
afterAll(() => testEnv.cleanup());
```

The `withIsolatedTestHome()` function:
- Creates a temporary `HOME` directory
- Isolates state files per test
- Cleans up after all tests complete

### Writing Unit Tests

```typescript
// src/my-module.test.ts
import { describe, it, expect, vi, beforeEach, afterEach } from "vitest";
import { myFunction, MyClass } from "./my-module.js";

describe("myFunction", () => {
  it("returns expected result", () => {
    const result = myFunction("input");
    expect(result).toBe("expected");
  });

  it("handles edge cases", () => {
    expect(myFunction("")).toBe("");
    expect(myFunction(null as any)).toBeUndefined();
  });
});

describe("MyClass", () => {
  let instance: MyClass;

  beforeEach(() => {
    instance = new MyClass({ option: "value" });
  });

  afterEach(() => {
    instance.cleanup();
  });

  it("initializes correctly", () => {
    expect(instance.isReady).toBe(true);
  });
});
```

### Mocking Patterns

#### Mocking the Plugin Registry

```typescript
import { setActivePluginRegistry } from "../src/plugins/runtime.js";
import { createTestRegistry } from "../src/test-utils/channel-plugins.js";

beforeEach(() => {
  const registry = createTestRegistry([
    {
      pluginId: "discord",
      plugin: createStubPlugin({ id: "discord", label: "Discord" }),
      source: "test",
    },
  ]);
  setActivePluginRegistry(registry);
});
```

#### Mocking External Services

```typescript
import { vi } from "vitest";

// Mock fetch
vi.mock("node:fetch", () => ({
  default: vi.fn(),
}));

// Use in test
import fetch from "node:fetch";

it("calls the API", async () => {
  vi.mocked(fetch).mockResolvedValueOnce({
    ok: true,
    json: async () => ({ result: "success" }),
  } as Response);

  await myApiCall();

  expect(fetch).toHaveBeenCalledWith(
    "https://api.example.com/endpoint",
    expect.objectContaining({ method: "POST" })
  );
});
```

#### Mocking Environment Variables

```typescript
import { vi, beforeEach, afterEach } from "vitest";

beforeEach(() => {
  vi.stubEnv("MY_API_KEY", "test-key");
  vi.stubEnv("NODE_ENV", "test");
});

afterEach(() => {
  vi.unstubAllEnvs();
});
```

### Writing E2E Tests

E2E tests use Docker for isolation:

```typescript
// src/feature.e2e.test.ts
import { describe, it, expect, beforeAll, afterAll } from "vitest";

describe("Feature E2E", () => {
  let container: Container;

  beforeAll(async () => {
    container = await startDockerContainer({
      image: "openclaw-test",
      env: { OPENCLAW_PROFILE: "test" },
    });
  });

  afterAll(async () => {
    await container.stop();
  });

  it("completes the workflow", async () => {
    const result = await container.exec("openclaw", ["status"]);
    expect(result.exitCode).toBe(0);
  });
});
```

### Running Tests

```bash
# Run all unit tests (parallel)
pnpm test

# Run unit tests (fast, single process)
pnpm test:fast

# Run with watch mode
pnpm test:watch

# Run E2E tests
pnpm test:e2e

# Run live tests (requires API keys)
OPENCLAW_LIVE_TEST=1 pnpm test:live

# Run with coverage
pnpm test:coverage

# Run specific test file
pnpm test:fast src/my-module.test.ts

# Run tests matching pattern
pnpm test:fast -t "should handle"

# Run Docker-based tests
pnpm test:docker:all

# Individual Docker tests
pnpm test:docker:live-models
pnpm test:docker:gateway-network
pnpm test:docker:plugins
```

### Coverage Requirements

OpenClaw maintains these coverage thresholds:

| Metric | Threshold |
|--------|-----------|
| Lines | 70% |
| Functions | 70% |
| Branches | 55% |
| Statements | 70% |

Coverage excludes:
- Test files (`*.test.ts`)
- CLI and commands (`src/cli/`, `src/commands/`)
- Channel surfaces (integration-tested)
- UI and TUI code
- Extension code (separate coverage)

### Test Configuration Details

From `vitest.config.ts`:

```typescript
export default defineConfig({
  test: {
    testTimeout: 120_000,        // 2 minute timeout
    hookTimeout: 120_000,        // 2 minute hook timeout
    unstubEnvs: true,            // Auto-unstub env vars
    unstubGlobals: true,         // Auto-unstub globals
    pool: "forks",               // Process isolation
    maxWorkers: isCI ? 3 : 16,   // Worker count
    include: [
      "src/**/*.test.ts",
      "extensions/**/*.test.ts",
      "test/**/*.test.ts",
    ],
    setupFiles: ["test/setup.ts"],
    exclude: [
      "dist/**",
      "**/node_modules/**",
      "**/*.live.test.ts",
      "**/*.e2e.test.ts",
    ],
  },
});
```

---

## 9. Build & CI/CD Guide

### Build System

OpenClaw uses **TypeScript 5.9.3** with **tsdown** for bundling:

```bash
# Full build
pnpm build

# Build components
pnpm build                    # Main build
pnpm build:plugin-sdk:dts     # Plugin SDK type definitions
pnpm canvas:a2ui:bundle       # Canvas A2UI bundle
```

#### Build Steps

1. **Canvas A2UI Bundle**: `scripts/bundle-a2ui.sh`
2. **TypeScript Compilation**: `tsdown`
3. **Plugin SDK DTS**: `tsc -p tsconfig.plugin-sdk.dts.json`
4. **Post-build Scripts**:
   - `write-plugin-sdk-entry-dts.ts`
   - `canvas-a2ui-copy.ts`
   - `copy-hook-metadata.ts`
   - `copy-export-html-templates.ts`
   - `write-build-info.ts`
   - `write-cli-compat.ts`

### Linting

OpenClaw uses **oxlint** (Rust-based linter):

```bash
# Run linter
pnpm lint

# Run linter with type-aware rules
pnpm lint --type-aware

# Fix auto-fixable issues
pnpm lint:fix

# Lint Swift code (macOS/iOS)
pnpm lint:swift

# Lint documentation
pnpm lint:docs
```

### Formatting

OpenClaw uses **oxfmt** for code formatting:

```bash
# Format all code
pnpm format

# Check formatting (CI)
pnpm format:check

# Format docs
pnpm format:docs

# Format Swift code
pnpm format:swift
```

### CI Pipeline Overview

The CI pipeline (`.github/workflows/ci.yml`) consists of several jobs:

```
┌─────────────────────────────────────────────────────────────────┐
│                      CI Pipeline                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌────────────┐                                                 │
│  │ docs-scope │  Detect docs-only changes                       │
│  └─────┬──────┘                                                 │
│        │                                                         │
│  ┌─────▼───────────┐                                            │
│  │ changed-scope   │  Detect affected areas                     │
│  └─────┬───────────┘                                            │
│        │                                                         │
│  ┌─────▼──────┐  ┌─────────────┐  ┌───────────────┐            │
│  │   check    │  │ check-docs  │  │   secrets     │            │
│  │ (lint,fmt) │  │ (if docs)   │  │ (detect-sec)  │            │
│  └─────┬──────┘  └─────────────┘  └───────────────┘            │
│        │                                                         │
│  ┌─────▼───────────┐                                            │
│  │ build-artifacts │  Build dist once                           │
│  └─────┬───────────┘                                            │
│        │                                                         │
│  ┌─────┴─────────────────────────────────────────┐              │
│  │                                               │              │
│  ▼                                               ▼              │
│  ┌────────────┐  ┌────────────────┐  ┌─────────────────┐       │
│  │   checks   │  │ checks-windows │  │      macos      │       │
│  │ (Linux)    │  │                │  │ (if macos code) │       │
│  │ - test     │  │ - lint         │  │ - TS tests      │       │
│  │ - protocol │  │ - test         │  │ - Swift lint    │       │
│  │ - bun test │  │ - protocol     │  │ - Swift build   │       │
│  └────────────┘  └────────────────┘  │ - Swift test    │       │
│                                      └─────────────────┘       │
│                                                                  │
│  ┌────────────────┐  ┌───────────────┐                         │
│  │    android     │  │ release-check │                         │
│  │ (if android)   │  │ (push only)   │                         │
│  │ - test         │  │               │                         │
│  │ - build        │  │               │                         │
│  └────────────────┘  └───────────────┘                         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### CI Jobs Reference

| Job | Trigger | Description |
|-----|---------|-------------|
| `docs-scope` | All | Detect docs-only changes |
| `changed-scope` | Non-docs | Detect which areas changed |
| `check` | Non-docs | Types, lint, format |
| `check-docs` | Docs changed | Doc format, lint, links |
| `secrets` | All | Secret detection scan |
| `build-artifacts` | Non-docs | Build and upload dist |
| `release-check` | Push to main | Validate npm pack contents |
| `checks` | Non-docs, Node | Run tests (Linux) |
| `checks-windows` | Non-docs, Node | Run tests (Windows) |
| `macos` | PR, macOS code | TS + Swift tests |
| `android` | Android code | Android build/test |

### Pre-commit Hooks

OpenClaw uses Git hooks for pre-commit validation:

```bash
# Hooks are set up automatically via:
git config core.hooksPath git-hooks

# Manual setup (if needed)
pnpm prepare
```

Pre-commit checks:
- Format check
- Type check
- Lint
- (Optional) Test affected files

### Release Process

1. **Version Bump**: Update `package.json` version
2. **Changelog**: Update `CHANGELOG.md`
3. **Build**: `pnpm build`
4. **Check**: `pnpm check && pnpm test`
5. **Release Check**: `pnpm release:check`
6. **Tag**: `git tag v2024.x.x`
7. **Push**: `git push && git push --tags`

---

## 10. Code Style & Conventions

### TypeScript Conventions

#### Strict Mode

All TypeScript code uses strict mode:

```typescript
// tsconfig.json
{
  "compilerOptions": {
    "strict": true,
    // No any types allowed
    // No implicit any
    // Strict null checks
    // Strict function types
  }
}
```

#### No `any` Type

The `typescript/no-explicit-any` rule is enforced:

```typescript
// Bad
function process(data: any) { ... }

// Good
function process(data: unknown) {
  if (typeof data === "string") {
    // Now TypeScript knows it's a string
  }
}

// Good - use generics for flexibility
function process<T extends Record<string, unknown>>(data: T) { ... }
```

When `any` is absolutely necessary, use the disable comment:

```typescript
// oxlint-disable-next-line typescript/no-explicit-any
const legacyData: any = getLegacyData();
```

### File Organization

#### Maximum Lines of Code

Files should not exceed **500 lines of code**. Run the check:

```bash
pnpm check:loc
```

If a file grows too large:
1. Extract related functions into a separate module
2. Create sub-modules for distinct concerns
3. Use barrel exports (`index.ts`) to maintain API

#### Barrel Exports

Use `index.ts` files to re-export public API:

```typescript
// src/channels/plugins/index.ts
export * from "./types.js";
export * from "./types.core.js";
export * from "./types.plugin.js";
export * from "./types.adapters.js";
```

### Naming Conventions

#### Case Styles

| Element | Convention | Example |
|---------|------------|---------|
| Files | kebab-case | `my-module.ts` |
| Classes | PascalCase | `ChannelManager` |
| Types/Interfaces | PascalCase | `ChannelConfig` |
| Functions | camelCase | `getChannelDock()` |
| Variables | camelCase | `channelId` |
| Constants | UPPER_SNAKE | `DEFAULT_PORT` |
| Env Variables | UPPER_SNAKE | `OPENCLAW_LOG_LEVEL` |

#### OpenClaw vs openclaw

- **Code identifiers**: `OpenClaw` (PascalCase)
- **Package name**: `openclaw` (lowercase)
- **Config/env**: `OPENCLAW_*` or `openclaw.*`
- **File paths**: `openclaw/` (lowercase)

```typescript
// Types use OpenClaw
type OpenClawConfig = { ... };
type OpenClawPluginApi = { ... };

// Package imports use openclaw
import { ... } from "openclaw/plugin-sdk";

// Environment variables use OPENCLAW_
process.env.OPENCLAW_LOG_LEVEL
```

### Import Organization

Organize imports in this order:

```typescript
// 1. Node.js built-ins
import path from "node:path";
import { readFile } from "node:fs/promises";

// 2. External packages
import { z } from "zod";
import { Type } from "@sinclair/typebox";

// 3. Internal absolute imports
import type { OpenClawConfig } from "../config/config.js";
import { getChannelDock } from "../channels/dock.js";

// 4. Relative imports
import { myHelper } from "./helpers.js";
import type { MyType } from "./types.js";
```

### Documentation Comments

Use JSDoc for public APIs:

```typescript
/**
 * Resolves the channel dock for a given channel ID.
 *
 * @param id - The channel identifier (e.g., "telegram", "discord")
 * @returns The channel dock if found, undefined otherwise
 *
 * @example
 * ```typescript
 * const dock = getChannelDock("telegram");
 * if (dock) {
 *   console.log(dock.capabilities);
 * }
 * ```
 */
export function getChannelDock(id: ChannelId): ChannelDock | undefined {
  // ...
}

/**
 * Configuration for a channel plugin.
 *
 * @template ResolvedAccount - The resolved account type for this channel
 * @template Probe - The probe result type (default: unknown)
 * @template Audit - The audit result type (default: unknown)
 */
export type ChannelPlugin<
  ResolvedAccount = any,
  Probe = unknown,
  Audit = unknown
> = {
  /** Unique channel identifier */
  id: ChannelId;
  /** Channel metadata for display */
  meta: ChannelMeta;
  // ...
};
```

### Control UI Decorators

The Control UI uses Lit with **legacy** decorators:

```typescript
// Control UI components use legacy decorators
import { LitElement, html } from "lit";
import { customElement, property, state } from "lit/decorators.js";

@customElement("my-component")
class MyComponent extends LitElement {
  // Use @state() for internal reactive state
  @state() private count = 0;

  // Use @property() for external properties
  @property({ type: String }) name = "";
  @property({ type: Number }) value = 0;

  render() {
    return html`<div>${this.name}: ${this.count}</div>`;
  }
}
```

**Important**: Do not use standard decorators with `accessor` fields - the Rollup build doesn't support them.

### ESM Module Requirements

OpenClaw is an ESM-only package:

```typescript
// Always use .js extension in imports (even for .ts files)
import { helper } from "./helper.js";

// Use import type for type-only imports
import type { MyType } from "./types.js";

// Node.js built-ins use node: prefix
import path from "node:path";
import { readFile } from "node:fs/promises";

// package.json must have
{
  "type": "module"
}
```

### Error Messages

Write clear, actionable error messages:

```typescript
// Bad
throw new Error("Invalid config");

// Good
throw new Error(
  `Invalid Telegram config: 'botToken' is required. ` +
  `Set it in config.yaml or via TELEGRAM_BOT_TOKEN environment variable.`
);

// Good - with error code
class ConfigError extends Error {
  constructor(
    public readonly code: string,
    message: string,
  ) {
    super(message);
    this.name = "ConfigError";
  }
}

throw new ConfigError(
  "TELEGRAM_BOT_TOKEN_MISSING",
  "Telegram bot token is required for this operation"
);
```

---

## 11. Troubleshooting Guide

### Common Development Issues

#### Issue: `pnpm install` fails with native module errors

**Symptoms:**
- Build errors during `npm rebuild`
- Missing node-gyp or Python

**Solution:**

```bash
# macOS
xcode-select --install
brew install python3

# Linux
sudo apt-get install -y build-essential python3

# Windows
npm install -g windows-build-tools
```

#### Issue: Tests fail with `ENOSPC` (Linux)

**Symptoms:**
- "System limit for number of file watchers reached"

**Solution:**

```bash
# Increase inotify limit
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

#### Issue: TypeScript errors about missing types

**Symptoms:**
- "Cannot find module 'openclaw/plugin-sdk'"

**Solution:**

```bash
# Rebuild the project
pnpm build

# Check tsconfig paths
cat tsconfig.json | grep "openclaw/plugin-sdk"
```

#### Issue: Tests hang or timeout

**Symptoms:**
- Tests don't complete
- Timeout errors after 2 minutes

**Solution:**

```bash
# Run with verbose output
pnpm test:fast --reporter=verbose

# Check for leaked async operations
# Add this to your test:
afterEach(() => {
  vi.useRealTimers();
});
```

### Debugging Techniques

#### Node.js Inspector

```bash
# Start with inspector
node --inspect scripts/run-node.mjs gateway

# Or with break on start
node --inspect-brk scripts/run-node.mjs gateway

# Then open chrome://inspect in Chrome
```

#### Verbose Logging

```bash
# Enable debug logging
OPENCLAW_LOG_LEVEL=debug pnpm dev

# Enable channel-specific logging
OPENCLAW_TELEGRAM_DEBUG=1 pnpm dev
OPENCLAW_DISCORD_DEBUG=1 pnpm dev
```

#### Test Debugging

```bash
# Run single test with debugging
node --inspect-brk ./node_modules/vitest/vitest.mjs run src/my-test.test.ts

# Use Vitest UI
pnpm test:watch --ui
```

### Platform-Specific Issues

#### macOS

**Issue: Playwright fails to launch browser**

```bash
# Install browser dependencies
npx playwright install-deps

# Or install browsers
npx playwright install
```

**Issue: node-pty build fails**

```bash
# Ensure Xcode command line tools
xcode-select --install

# Rebuild native modules
pnpm rebuild
```

#### Linux

**Issue: WhatsApp QR code not displaying**

```bash
# Install terminal with image support (e.g., kitty, iTerm2)
# Or use the web UI for QR scanning
```

**Issue: Permission denied for state files**

```bash
# Check ownership
ls -la ~/.openclaw

# Fix permissions
chmod -R 755 ~/.openclaw
```

#### Windows

**Issue: Line ending issues**

```bash
# Configure Git
git config --global core.autocrlf input

# Fix existing files
git rm --cached -r .
git reset --hard
```

**Issue: Path too long errors**

```powershell
# Enable long paths in Git
git config --system core.longpaths true

# Enable in Windows (requires admin)
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" `
  -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
```

### Gateway Troubleshooting

#### Issue: Cannot connect to gateway

**Check:**
1. Is the gateway running?
   ```bash
   curl http://localhost:18789/health
   ```

2. Is the port in use?
   ```bash
   lsof -i :18789
   ```

3. Check firewall rules

**Solution:**

```bash
# Use a different port
OPENCLAW_GATEWAY_PORT=18790 pnpm gateway:dev

# Or kill existing process
kill $(lsof -t -i:18789)
```

#### Issue: Channel authentication fails

**Telegram:**
```bash
# Verify bot token
curl "https://api.telegram.org/bot<TOKEN>/getMe"
```

**Discord:**
```bash
# Check bot permissions
# Ensure bot has required intents enabled in Discord Developer Portal
```

**WhatsApp:**
```bash
# Clear auth state and re-login
rm -rf ~/.openclaw/whatsapp-auth
pnpm openclaw whatsapp login
```

### Plugin Development Issues

#### Issue: Plugin not loading

**Check:**
1. Is `openclaw.extensions` configured in package.json?
2. Is the path correct?
3. Does the export default a valid plugin?

```bash
# Debug plugin loading
OPENCLAW_PLUGIN_DEBUG=1 pnpm gateway:dev
```

#### Issue: Hook not firing

**Check:**
1. Is the hook name correct?
2. Is the plugin registered before the event occurs?
3. Check hook priority (lower runs first)

```typescript
// Debug hook registration
api.on("message_received", (event, ctx) => {
  console.log("Hook fired!", event);
}, { priority: 0 }); // Priority 0 runs first
```

### Test Failures

#### CI Passes, Local Fails

**Common causes:**
1. Environment variable differences
2. Timezone issues
3. Different Node.js version

```bash
# Match CI environment
NODE_ENV=test CI=true pnpm test
```

#### Local Passes, CI Fails

**Common causes:**
1. File system case sensitivity (CI is case-sensitive)
2. Missing dependencies (CI uses `--frozen-lockfile`)
3. Platform-specific code

```bash
# Test with frozen lockfile
rm -rf node_modules
pnpm install --frozen-lockfile
pnpm test
```

### Memory and Performance Issues

#### Issue: Out of memory during tests

```bash
# Increase heap size
NODE_OPTIONS="--max-old-space-size=4096" pnpm test

# Or reduce parallelism
pnpm test --maxWorkers=2
```

#### Issue: Slow test execution

```bash
# Profile test execution
pnpm test:fast --reporter=verbose

# Check for slow tests
node scripts/vitest-slowest.mjs
```

---

## 12. API Reference

### Plugin SDK API

The plugin SDK is exported from `openclaw/plugin-sdk`:

```typescript
import type {
  // Core plugin types
  OpenClawPluginApi,
  OpenClawPluginDefinition,
  OpenClawPluginModule,
  OpenClawPluginService,
  OpenClawPluginServiceContext,

  // Channel types
  ChannelPlugin,
  ChannelDock,
  ChannelCapabilities,
  ChannelMeta,
  ChannelId,

  // Adapter types
  ChannelConfigAdapter,
  ChannelOutboundAdapter,
  ChannelSetupAdapter,
  ChannelStatusAdapter,
  ChannelGatewayAdapter,
  ChannelGroupAdapter,
  ChannelMentionAdapter,
  ChannelThreadingAdapter,
  ChannelMessagingAdapter,
  ChannelDirectoryAdapter,
  ChannelSecurityAdapter,
  ChannelAuthAdapter,
  ChannelPairingAdapter,
  ChannelHeartbeatAdapter,
  ChannelStreamingAdapter,
  ChannelMessageActionAdapter,
  ChannelResolverAdapter,
  ChannelElevatedAdapter,
  ChannelCommandAdapter,

  // Config types
  OpenClawConfig,

  // Tool types
  AnyAgentTool,

  // Provider types
  ProviderAuthContext,
  ProviderAuthResult,

  // Gateway types
  GatewayRequestHandler,
  GatewayRequestHandlerOptions,
  RespondFn,

  // Runtime types
  PluginRuntime,
  RuntimeLogger,
  RuntimeEnv,

  // Utility types
  ChatType,
  ReplyPayload,
  HookEntry,
} from "openclaw/plugin-sdk";
```

### Hook Event Types

#### Agent Hooks

```typescript
// before_model_resolve
type PluginHookBeforeModelResolveEvent = {
  prompt: string;
};
type PluginHookBeforeModelResolveResult = {
  modelOverride?: string;
  providerOverride?: string;
};

// before_prompt_build
type PluginHookBeforePromptBuildEvent = {
  prompt: string;
  messages: unknown[];
};
type PluginHookBeforePromptBuildResult = {
  systemPrompt?: string;
  prependContext?: string;
};

// before_agent_start (legacy)
type PluginHookBeforeAgentStartEvent = {
  prompt: string;
  messages?: unknown[];
};
type PluginHookBeforeAgentStartResult =
  PluginHookBeforePromptBuildResult & PluginHookBeforeModelResolveResult;

// llm_input
type PluginHookLlmInputEvent = {
  runId: string;
  sessionId: string;
  provider: string;
  model: string;
  systemPrompt?: string;
  prompt: string;
  historyMessages: unknown[];
  imagesCount: number;
};

// llm_output
type PluginHookLlmOutputEvent = {
  runId: string;
  sessionId: string;
  provider: string;
  model: string;
  assistantTexts: string[];
  lastAssistant?: unknown;
  usage?: {
    input?: number;
    output?: number;
    cacheRead?: number;
    cacheWrite?: number;
    total?: number;
  };
};

// agent_end
type PluginHookAgentEndEvent = {
  messages: unknown[];
  success: boolean;
  error?: string;
  durationMs?: number;
};
```

#### Compaction Hooks

```typescript
// before_compaction
type PluginHookBeforeCompactionEvent = {
  messageCount: number;
  compactingCount?: number;
  tokenCount?: number;
  messages?: unknown[];
  sessionFile?: string;
};

// after_compaction
type PluginHookAfterCompactionEvent = {
  messageCount: number;
  tokenCount?: number;
  compactedCount: number;
  sessionFile?: string;
};

// before_reset
type PluginHookBeforeResetEvent = {
  sessionFile?: string;
  messages?: unknown[];
  reason?: string;
};
```

#### Message Hooks

```typescript
// message_received
type PluginHookMessageReceivedEvent = {
  from: string;
  content: string;
  timestamp?: number;
  metadata?: Record<string, unknown>;
};

// message_sending
type PluginHookMessageSendingEvent = {
  to: string;
  content: string;
  metadata?: Record<string, unknown>;
};
type PluginHookMessageSendingResult = {
  content?: string;
  cancel?: boolean;
};

// message_sent
type PluginHookMessageSentEvent = {
  to: string;
  content: string;
  success: boolean;
  error?: string;
};
```

#### Tool Hooks

```typescript
// before_tool_call
type PluginHookBeforeToolCallEvent = {
  toolName: string;
  params: Record<string, unknown>;
};
type PluginHookBeforeToolCallResult = {
  params?: Record<string, unknown>;
  block?: boolean;
  blockReason?: string;
};

// after_tool_call
type PluginHookAfterToolCallEvent = {
  toolName: string;
  params: Record<string, unknown>;
  result?: unknown;
  error?: string;
  durationMs?: number;
};

// tool_result_persist
type PluginHookToolResultPersistEvent = {
  toolName?: string;
  toolCallId?: string;
  message: AgentMessage;
  isSynthetic?: boolean;
};
type PluginHookToolResultPersistResult = {
  message?: AgentMessage;
};

// before_message_write
type PluginHookBeforeMessageWriteEvent = {
  message: AgentMessage;
  sessionKey?: string;
  agentId?: string;
};
type PluginHookBeforeMessageWriteResult = {
  block?: boolean;
  message?: AgentMessage;
};
```

#### Session Hooks

```typescript
// session_start
type PluginHookSessionStartEvent = {
  sessionId: string;
  resumedFrom?: string;
};

// session_end
type PluginHookSessionEndEvent = {
  sessionId: string;
  messageCount: number;
  durationMs?: number;
};
```

#### Gateway Hooks

```typescript
// gateway_start
type PluginHookGatewayStartEvent = {
  port: number;
};

// gateway_stop
type PluginHookGatewayStopEvent = {
  reason?: string;
};
```

### Configuration Schema Reference

#### Channel Configuration Example

```yaml
# config.yaml
channels:
  telegram:
    accounts:
      default:
        botToken: ${TELEGRAM_BOT_TOKEN}
        allowFrom:
          - "123456789"
          - "@username"
        dm:
          policy: allowlist
        groups:
          allowUnmentioned: false
          toolPolicy: restricted

  discord:
    accounts:
      main:
        botToken: ${DISCORD_BOT_TOKEN}
        applicationId: "123456789"
        allowFrom:
          - "user:123456789"
        channels:
          - id: "987654321"
            enabled: true

  whatsapp:
    accounts:
      default:
        allowFrom:
          - "+1234567890"
        groups:
          enabled: true
          allowUnmentioned: true
```

### CLI Commands Reference

| Command | Description |
|---------|-------------|
| `openclaw` | Start interactive mode |
| `openclaw gateway` | Start the gateway server |
| `openclaw tui` | Start terminal UI |
| `openclaw agent` | Run agent in various modes |
| `openclaw status` | Show system status |
| `openclaw config` | Manage configuration |
| `openclaw channels` | Manage channels |
| `openclaw skills` | Manage skills |
| `openclaw doctor` | Run diagnostics |
| `openclaw login` | Authenticate with providers |

---

## 13. FAQ & Common Issues

### General Questions

**Q: How do I add a new messaging channel?**

A: Create a channel plugin following the [Plugin Development Guide](#7-plugin-development-guide). Implement the `ChannelPlugin` interface with at least `id`, `meta`, `capabilities`, and `config` adapter.

**Q: How do I create a new skill?**

A: Skills are published to [ClawHub](https://clawhub.ai/). Follow the skill template and publish guidelines there.

**Q: How do I run tests for a specific file?**

A: Use `pnpm test:fast src/path/to/file.test.ts` or `pnpm test:fast -t "test name pattern"`.

### Development Questions

**Q: Why is my plugin not loading?**

A: Check:
1. `openclaw.extensions` in package.json points to the correct file
2. The file exports a default plugin definition
3. Run with `OPENCLAW_PLUGIN_DEBUG=1` for detailed logs

**Q: How do I test my plugin with live channels?**

A:
1. Set up environment variables for the channel
2. Run `pnpm gateway:dev` (without `OPENCLAW_SKIP_CHANNELS`)
3. Use the TUI or web UI to interact

**Q: How do I add API keys for testing?**

A: Create a `.env` file in the project root:
```bash
OPENAI_API_KEY=sk-...
TELEGRAM_BOT_TOKEN=...
```

### Architecture Questions

**Q: Why does OpenClaw use factory functions instead of classes?**

A: Factory functions provide:
- Simpler testing (no `new` keyword, no `this` binding)
- Better tree-shaking
- Clearer composition patterns
- Avoid inheritance complexity

**Q: Why are context objects passed instead of using dependency injection?**

A: Context objects:
- Make dependencies explicit
- Are easier to type
- Work well with async operations
- Don't require a DI framework

**Q: Why does the hook system use priorities?**

A: Priorities allow:
- Plugins to run before/after others
- Core functionality to have first/last say
- Predictable execution order

### Contribution Questions

**Q: How long does PR review take?**

A: Typically 1-5 business days, depending on complexity. Keep PRs focused to speed up review.

**Q: How do I become a maintainer?**

A: Email contributing@openclaw.ai with:
- Links to your PRs on OpenClaw
- Links to open source projects you maintain
- Your GitHub, Discord, and X/Twitter handles
- A brief intro about your background and interests
- How much time you can commit

See the [Maintainers](#maintainers) section for the full process.

**Q: Can I use AI tools to write code?**

A: Yes! AI-assisted PRs are welcome. Please:
- Mark the PR as AI-assisted
- Note the testing level
- Include prompts/session logs if helpful
- Confirm you understand the code

---

## 14. Appendices

### Glossary

| Term | Definition |
|------|------------|
| **Agent** | An AI-powered assistant that processes messages and executes tools |
| **Channel** | A messaging platform integration (Telegram, Discord, etc.) |
| **Dock** | A lightweight channel interface for shared code paths |
| **Gateway** | The WebSocket server that manages connections and routing |
| **Hook** | An event handler that can intercept and modify behavior |
| **Node** | A connected client or service in the OpenClaw network |
| **Plugin** | An extension that adds functionality (channels, tools, hooks) |
| **Skill** | A reusable AI capability that can be installed from ClawHub |
| **Session** | A conversation context with message history |
| **Tool** | A capability the agent can use (bash, read, write, send) |

### File Path Quick Reference

| Path | Description |
|------|-------------|
| `src/plugins/types.ts` | Core plugin types and API |
| `src/channels/plugins/types.ts` | Channel type exports |
| `src/channels/plugins/types.plugin.ts` | ChannelPlugin interface |
| `src/channels/plugins/types.core.ts` | Core channel types |
| `src/channels/plugins/types.adapters.ts` | Adapter interfaces |
| `src/channels/dock.ts` | ChannelDock and utilities |
| `src/config/config.ts` | OpenClawConfig type |
| `src/plugin-sdk/index.ts` | Public SDK exports |
| `src/gateway/server.ts` | Gateway server |
| `src/gateway/server-methods/` | Gateway RPC handlers |
| `vitest.config.ts` | Test configuration |
| `.github/workflows/ci.yml` | CI pipeline |

### Environment Variables Reference

| Variable | Description | Default |
|----------|-------------|---------|
| `OPENCLAW_PROFILE` | Configuration profile | `default` |
| `OPENCLAW_STATE_DIR` | State directory | `~/.openclaw` |
| `OPENCLAW_CONFIG_PATH` | Config file path | `~/.openclaw/config.yaml` |
| `OPENCLAW_LOG_LEVEL` | Log verbosity | `info` |
| `OPENCLAW_GATEWAY_PORT` | Gateway port | `18789` |
| `OPENCLAW_SKIP_CHANNELS` | Skip channel init | `0` |
| `OPENCLAW_LIVE_TEST` | Enable live tests | `0` |
| `OPENCLAW_PLUGIN_DEBUG` | Plugin debug logs | `0` |
| `OPENCLAW_PLUGIN_MANIFEST_CACHE_MS` | Manifest cache TTL | `60000` |
| `VITEST` | Test environment flag | - |
| `CI` | CI environment flag | - |

### External Resources

- **Documentation**: https://docs.openclaw.ai/
- **Discord Community**: https://discord.gg/qkhbAGHRBT
- **ClawHub (Skills)**: https://clawhub.ai/
- **GitHub Issues**: https://github.com/openclaw/openclaw/issues
- **GitHub Discussions**: https://github.com/openclaw/openclaw/discussions

---

## How to Contribute

1. **Bugs & small fixes** → Open a PR!
2. **New features / architecture** → Start a [GitHub Discussion](https://github.com/openclaw/openclaw/discussions) or ask in Discord first
3. **Questions** → Discord #setup-help

## Before You PR

- Test locally with your OpenClaw instance
- Run tests: `pnpm build && pnpm check && pnpm test`
- Ensure CI checks pass
- Keep PRs focused (one thing per PR; do not mix unrelated concerns)
- Describe what & why

## AI/Vibe-Coded PRs Welcome!

Built with Codex, Claude, or other AI tools? **Awesome - just mark it!**

Please include in your PR:

- [ ] Mark as AI-assisted in the PR title or description
- [ ] Note the degree of testing (untested / lightly tested / fully tested)
- [ ] Include prompts or session logs if possible (super helpful!)
- [ ] Confirm you understand what the code does

AI PRs are first-class citizens here. We just want transparency so reviewers know what to look for.

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

---

## Additional Resources

### Complete Configuration Reference

This section provides exhaustive configuration examples for all supported channels.

#### Telegram Configuration

```yaml
# Full Telegram configuration reference
channels:
  telegram:
    # Global settings (apply to all accounts unless overridden)
    replyToMode: "off"  # "off" | "first" | "all"
    markdown:
      enabled: true
      tableMode: "ascii"  # "ascii" | "simple" | "none"

    accounts:
      # Default account (used when accountId not specified)
      default:
        # Bot token - required
        # Can be set via env: TELEGRAM_BOT_TOKEN
        botToken: "${TELEGRAM_BOT_TOKEN}"

        # Webhook configuration (optional, for production)
        webhook:
          url: "https://your-domain.com/webhook/telegram"
          secretToken: "${TELEGRAM_WEBHOOK_SECRET}"

        # Allowlist configuration
        allowFrom:
          - "123456789"           # Telegram user ID
          - "@username"           # Telegram username
          - "-1001234567890"      # Group/channel ID

        # DM (direct message) policy
        dm:
          policy: "allowlist"     # "allowlist" | "open" | "closed"
          # If open, anyone can message; use with caution

        # Group configuration
        groups:
          enabled: true
          allowUnmentioned: false  # Respond without @mention?
          toolPolicy: "full"       # "full" | "restricted" | "none"
          toolsBySender:
            "123456789": "full"    # Per-user tool access

        # Channel (broadcast) configuration
        channels:
          enabled: false

        # Threading configuration
        threading:
          enabled: true
          autoThread: false       # Auto-create threads?

        # Rate limiting
        rateLimit:
          messagesPerMinute: 20
          burstLimit: 5

      # Additional account example
      work-bot:
        botToken: "${WORK_TELEGRAM_BOT_TOKEN}"
        allowFrom:
          - "@work_user1"
          - "@work_user2"
        dm:
          policy: "allowlist"
```

#### Discord Configuration

```yaml
# Full Discord configuration reference
channels:
  discord:
    replyToMode: "off"

    accounts:
      default:
        # Bot token - required
        botToken: "${DISCORD_BOT_TOKEN}"

        # Application ID - required for slash commands
        applicationId: "123456789012345678"

        # Public key - for webhook verification
        publicKey: "${DISCORD_PUBLIC_KEY}"

        # DM allowlist
        allowFrom:
          - "user:123456789012345678"  # Discord user ID
          - "pk:system_id"              # PluralKit system ID

        dm:
          policy: "allowlist"

        # Server (guild) configuration
        guilds:
          "987654321098765432":         # Guild ID
            enabled: true
            channels:
              - id: "111111111111111111"
                enabled: true
                toolPolicy: "full"
              - id: "222222222222222222"
                enabled: true
                toolPolicy: "restricted"
            roles:
              admin: "333333333333333333"
              moderator: "444444444444444444"

        # Interaction configuration
        interactions:
          slashCommands: true
          messageCommands: true
          userCommands: false

        # Presence configuration
        presence:
          status: "online"              # "online" | "idle" | "dnd" | "invisible"
          activity:
            type: "playing"             # "playing" | "streaming" | "listening" | "watching"
            name: "with AI"

        # Rate limiting
        rateLimit:
          messagesPerMinute: 50
          burstLimit: 10
```

#### WhatsApp Configuration

```yaml
# Full WhatsApp configuration reference
channels:
  whatsapp:
    accounts:
      default:
        # Session management
        authDir: "${HOME}/.openclaw/whatsapp-auth"

        # Allowlist - phone numbers in E.164 format
        allowFrom:
          - "+12025551234"
          - "+442071234567"
          - "*"                         # Allow all (use with caution)

        # Group configuration
        groups:
          enabled: true
          allowUnmentioned: false
          toolPolicy: "restricted"
          autoJoin: false

        # Media handling
        media:
          enabled: true
          maxSizeBytes: 16777216        # 16MB
          allowedTypes:
            - "image/*"
            - "video/*"
            - "audio/*"
            - "application/pdf"

        # Message formatting
        formatting:
          useMarkdown: true
          linkPreview: true

        # Connection settings
        connection:
          printQRInTerminal: true
          browser: ["OpenClaw", "Chrome", "1.0.0"]
          syncFullHistory: false
          markOnlineOnConnect: true
```

#### Slack Configuration

```yaml
# Full Slack configuration reference
channels:
  slack:
    accounts:
      default:
        # Bot token (xoxb-...) - required
        botToken: "${SLACK_BOT_TOKEN}"

        # App token (xapp-...) - required for Socket Mode
        appToken: "${SLACK_APP_TOKEN}"

        # Signing secret - for request verification
        signingSecret: "${SLACK_SIGNING_SECRET}"

        # Allowlist - Slack user IDs
        allowFrom:
          - "U01234567"
          - "U89ABCDEF"

        dm:
          policy: "allowlist"

        # Channel configuration
        channels:
          "C01234567":                  # Channel ID
            enabled: true
            toolPolicy: "full"
            threadOnly: false           # Only respond in threads?
          "C89ABCDEF":
            enabled: true
            toolPolicy: "restricted"
            threadOnly: true

        # App Home configuration
        appHome:
          enabled: true
          showMessages: true

        # Event subscriptions
        events:
          message: true
          appMention: true
          reaction: true
          fileShare: false

        # Socket Mode (recommended)
        socketMode:
          enabled: true
```

#### Signal Configuration

```yaml
# Full Signal configuration reference
channels:
  signal:
    accounts:
      default:
        # Signal number in E.164 format
        signalNumber: "+12025551234"

        # signal-cli path (if not in PATH)
        cliPath: "/usr/local/bin/signal-cli"

        # Data directory
        dataDir: "${HOME}/.local/share/signal-cli"

        # Allowlist
        allowFrom:
          - "+12025559876"
          - "+442071234567"

        # Group configuration
        groups:
          enabled: true
          allowUnmentioned: false

        # Attachment handling
        attachments:
          enabled: true
          maxSizeBytes: 104857600       # 100MB
          downloadPath: "${HOME}/.openclaw/signal-attachments"

        # Daemon mode
        daemon:
          enabled: true
          port: 7583
```

#### iMessage Configuration

```yaml
# Full iMessage configuration reference
channels:
  imessage:
    accounts:
      default:
        # Service type
        service: "imessage"             # "imessage" | "sms" | "auto"

        # Database path (macOS Messages.app database)
        dbPath: "${HOME}/Library/Messages/chat.db"

        # Allowlist - phone numbers or Apple IDs
        allowFrom:
          - "+12025551234"
          - "user@icloud.com"

        # Group configuration
        groups:
          enabled: true
          allowUnmentioned: false

        # SMS fallback
        smsFallback:
          enabled: false
          allowedNumbers: []
```

#### Google Chat Configuration

```yaml
# Full Google Chat configuration reference
channels:
  googlechat:
    accounts:
      default:
        # Service account credentials
        credentialsPath: "${HOME}/.openclaw/google-chat-credentials.json"

        # Or use environment variable
        # credentials: "${GOOGLE_CHAT_CREDENTIALS_JSON}"

        # Webhook mode (simpler setup)
        webhook:
          enabled: false
          url: "${GOOGLE_CHAT_WEBHOOK_URL}"

        # DM configuration
        dm:
          allowFrom:
            - "users/123456789"
            - "user@example.com"
          policy: "allowlist"

        # Space configuration
        spaces:
          "spaces/AAAA1111":
            enabled: true
            toolPolicy: "full"
          "spaces/BBBB2222":
            enabled: true
            toolPolicy: "restricted"
```

#### IRC Configuration

```yaml
# Full IRC configuration reference
channels:
  irc:
    accounts:
      default:
        # Server connection
        server: "irc.libera.chat"
        port: 6697
        secure: true                    # Use TLS

        # Authentication
        nick: "openclawbot"
        user: "openclaw"
        realName: "OpenClaw AI Bot"
        password: "${IRC_PASSWORD}"

        # NickServ identification
        nickserv:
          enabled: true
          password: "${IRC_NICKSERV_PASSWORD}"

        # SASL authentication
        sasl:
          enabled: true
          mechanism: "PLAIN"
          username: "openclawbot"
          password: "${IRC_SASL_PASSWORD}"

        # Allowlist - IRC nicknames
        allowFrom:
          - "operator"
          - "trusted_user"

        # Channel configuration
        channels:
          "#openclaw":
            enabled: true
            key: ""                     # Channel key/password
            toolPolicy: "restricted"
          "#dev":
            enabled: true
            toolPolicy: "full"

        # Rate limiting
        rateLimit:
          messagesPerSecond: 2
          burstLimit: 5
```

### Advanced Testing Scenarios

This section covers complex testing patterns for various scenarios.

#### Testing Async Operations

```typescript
import { describe, it, expect, vi, beforeEach, afterEach } from "vitest";

describe("Async Operations", () => {
  beforeEach(() => {
    vi.useFakeTimers();
  });

  afterEach(() => {
    vi.useRealTimers();
  });

  it("handles timeouts correctly", async () => {
    const timeoutPromise = new Promise((_, reject) => {
      setTimeout(() => reject(new Error("Timeout")), 5000);
    });

    const operationPromise = performAsyncOperation();

    // Advance timers
    vi.advanceTimersByTime(5000);

    await expect(Promise.race([operationPromise, timeoutPromise]))
      .rejects.toThrow("Timeout");
  });

  it("retries on failure", async () => {
    const mockFn = vi.fn()
      .mockRejectedValueOnce(new Error("First failure"))
      .mockRejectedValueOnce(new Error("Second failure"))
      .mockResolvedValueOnce({ success: true });

    const result = await retryOperation(mockFn, { maxRetries: 3 });

    expect(mockFn).toHaveBeenCalledTimes(3);
    expect(result).toEqual({ success: true });
  });

  it("debounces rapid calls", async () => {
    const callback = vi.fn();
    const debouncedFn = debounce(callback, 100);

    // Rapid calls
    debouncedFn("a");
    debouncedFn("b");
    debouncedFn("c");

    // Only the last call should execute
    vi.advanceTimersByTime(100);

    expect(callback).toHaveBeenCalledTimes(1);
    expect(callback).toHaveBeenCalledWith("c");
  });
});
```

#### Testing WebSocket Connections

```typescript
import { describe, it, expect, vi, beforeEach, afterEach } from "vitest";
import { WebSocket, Server } from "ws";

describe("WebSocket Gateway", () => {
  let server: Server;
  let client: WebSocket;

  beforeEach(async () => {
    server = new Server({ port: 0 });
    const address = server.address() as { port: number };
    client = new WebSocket(`ws://localhost:${address.port}`);
    await new Promise(resolve => client.on("open", resolve));
  });

  afterEach(async () => {
    client.close();
    await new Promise(resolve => server.close(resolve));
  });

  it("handles RPC requests", async () => {
    server.on("connection", (ws) => {
      ws.on("message", (data) => {
        const request = JSON.parse(data.toString());
        ws.send(JSON.stringify({
          id: request.id,
          result: { status: "ok" },
        }));
      });
    });

    const response = await sendRpcRequest(client, "status.get", {});

    expect(response).toEqual({ status: "ok" });
  });

  it("handles connection drops", async () => {
    const reconnectSpy = vi.fn();
    const manager = createConnectionManager({
      url: `ws://localhost:${(server.address() as { port: number }).port}`,
      onReconnect: reconnectSpy,
    });

    await manager.connect();
    server.close();

    // Wait for reconnect attempt
    await vi.waitFor(() => {
      expect(reconnectSpy).toHaveBeenCalled();
    });
  });
});
```

#### Testing Plugin Registration

```typescript
import { describe, it, expect, vi, beforeEach } from "vitest";
import type { OpenClawPluginApi } from "openclaw/plugin-sdk";

describe("Plugin Registration", () => {
  let api: OpenClawPluginApi;
  let registeredTools: Map<string, unknown>;
  let registeredHooks: Map<string, unknown[]>;

  beforeEach(() => {
    registeredTools = new Map();
    registeredHooks = new Map();

    api = {
      id: "test-plugin",
      name: "Test Plugin",
      source: "test",
      config: {},
      pluginConfig: {},
      runtime: {
        stateDir: "/tmp/test",
        workspaceDir: "/tmp/workspace",
      },
      logger: {
        info: vi.fn(),
        warn: vi.fn(),
        error: vi.fn(),
        debug: vi.fn(),
      },
      registerTool: vi.fn((tool, opts) => {
        const name = opts?.name || (typeof tool === "function" ? "factory" : tool.name);
        registeredTools.set(name, tool);
      }),
      registerHook: vi.fn((events, handler, opts) => {
        const eventList = Array.isArray(events) ? events : [events];
        for (const event of eventList) {
          if (!registeredHooks.has(event)) {
            registeredHooks.set(event, []);
          }
          registeredHooks.get(event)!.push({ handler, opts });
        }
      }),
      registerChannel: vi.fn(),
      registerGatewayMethod: vi.fn(),
      registerCli: vi.fn(),
      registerService: vi.fn(),
      registerProvider: vi.fn(),
      registerCommand: vi.fn(),
      registerHttpHandler: vi.fn(),
      registerHttpRoute: vi.fn(),
      resolvePath: vi.fn((p) => p),
      on: vi.fn((hookName, handler, opts) => {
        if (!registeredHooks.has(hookName)) {
          registeredHooks.set(hookName, []);
        }
        registeredHooks.get(hookName)!.push({ handler, opts });
      }),
    } as unknown as OpenClawPluginApi;
  });

  it("registers tools with correct names", async () => {
    const plugin = {
      id: "my-plugin",
      register: (api: OpenClawPluginApi) => {
        api.registerTool({
          name: "my_tool",
          description: "A test tool",
          parameters: { type: "object", properties: {} },
          execute: async () => ({ result: "ok" }),
        });
      },
    };

    await plugin.register(api);

    expect(api.registerTool).toHaveBeenCalled();
    expect(registeredTools.has("my_tool")).toBe(true);
  });

  it("registers hooks with correct priorities", async () => {
    const plugin = {
      id: "my-plugin",
      register: (api: OpenClawPluginApi) => {
        api.on("message_received", () => {}, { priority: 100 });
        api.on("message_received", () => {}, { priority: 50 });
        api.on("message_received", () => {}, { priority: 200 });
      },
    };

    await plugin.register(api);

    const hooks = registeredHooks.get("message_received")!;
    expect(hooks).toHaveLength(3);
    expect(hooks.map(h => h.opts?.priority)).toEqual([100, 50, 200]);
  });
});
```

#### Testing Channel Adapters

```typescript
import { describe, it, expect, vi, beforeEach } from "vitest";
import type { ChannelPlugin, ChannelOutboundAdapter } from "openclaw/plugin-sdk";

describe("Channel Adapter", () => {
  let mockFetch: ReturnType<typeof vi.fn>;

  beforeEach(() => {
    mockFetch = vi.fn();
    vi.stubGlobal("fetch", mockFetch);
  });

  afterEach(() => {
    vi.unstubAllGlobals();
  });

  describe("outbound adapter", () => {
    const outbound: ChannelOutboundAdapter = {
      deliveryMode: "direct",
      textChunkLimit: 4000,

      sendText: async ({ to, text }) => {
        const response = await fetch("https://api.example.com/send", {
          method: "POST",
          body: JSON.stringify({ to, text }),
        });
        const data = await response.json();
        return { channel: "example", messageId: data.id };
      },
    };

    it("sends text messages", async () => {
      mockFetch.mockResolvedValueOnce({
        ok: true,
        json: async () => ({ id: "msg_123" }),
      });

      const result = await outbound.sendText!({
        cfg: {},
        to: "user123",
        text: "Hello!",
      } as any);

      expect(result.messageId).toBe("msg_123");
      expect(mockFetch).toHaveBeenCalledWith(
        "https://api.example.com/send",
        expect.objectContaining({ method: "POST" })
      );
    });

    it("handles API errors", async () => {
      mockFetch.mockResolvedValueOnce({
        ok: false,
        status: 500,
        statusText: "Internal Server Error",
      });

      await expect(outbound.sendText!({
        cfg: {},
        to: "user123",
        text: "Hello!",
      } as any)).rejects.toThrow();
    });

    it("chunks long messages", async () => {
      const longText = "A".repeat(10000);
      const chunks = chunkText(longText, outbound.textChunkLimit!);

      expect(chunks.length).toBeGreaterThan(1);
      expect(chunks.every(c => c.length <= 4000)).toBe(true);
    });
  });
});
```

### Migration Guides

#### Migrating from v2023 to v2024

**Breaking Changes:**

1. **Config structure changes**
   - `channels.telegram.token` → `channels.telegram.accounts.default.botToken`
   - `channels.discord.token` → `channels.discord.accounts.default.botToken`

2. **Plugin API changes**
   - `registerTool(name, handler)` → `registerTool(tool, opts?)`
   - Hook signatures updated with context parameter

3. **Environment variable changes**
   - `CLAWDBOT_*` → `OPENCLAW_*`

**Migration Steps:**

```bash
# 1. Backup your config
cp ~/.openclaw/config.yaml ~/.openclaw/config.yaml.backup

# 2. Run the migration tool
openclaw migrate --from 2023 --to 2024

# 3. Verify the migration
openclaw doctor
```

**Manual config migration:**

```yaml
# Old (v2023)
channels:
  telegram:
    token: "bot123:ABC..."
    allowFrom:
      - "123456789"

# New (v2024)
channels:
  telegram:
    accounts:
      default:
        botToken: "bot123:ABC..."
        allowFrom:
          - "123456789"
```

**Plugin migration:**

```typescript
// Old (v2023)
api.registerTool("my_tool", async (args) => {
  return { result: "ok" };
});

// New (v2024)
api.registerTool({
  name: "my_tool",
  description: "My tool description",
  parameters: Type.Object({}),
  execute: async (args) => {
    return { result: "ok" };
  },
});
```

#### Migrating Plugins to TypeScript

If you have JavaScript plugins, here's how to migrate:

```typescript
// Before: my-plugin.js
module.exports = function(api) {
  api.registerTool("my_tool", async (args) => {
    return { result: args.input.toUpperCase() };
  });
};

// After: my-plugin.ts
import type { OpenClawPluginDefinition } from "openclaw/plugin-sdk";
import { Type } from "@sinclair/typebox";

const plugin: OpenClawPluginDefinition = {
  id: "my-plugin",
  name: "My Plugin",
  version: "2.0.0",

  register: (api) => {
    api.registerTool({
      name: "my_tool",
      description: "Converts input to uppercase",
      parameters: Type.Object({
        input: Type.String({ description: "Text to convert" }),
      }),
      execute: async (args) => {
        return { result: args.input.toUpperCase() };
      },
    });
  },
};

export default plugin;
```

### Performance Optimization Guide

#### Memory Management

```typescript
// Use WeakMap for caching objects
const cache = new WeakMap<object, ProcessedData>();

function processWithCache(obj: object): ProcessedData {
  if (cache.has(obj)) {
    return cache.get(obj)!;
  }
  const result = expensiveProcess(obj);
  cache.set(obj, result);
  return result;
}

// Clear large data structures when done
function processLargeDataset(data: LargeData[]): void {
  const processor = createProcessor();
  try {
    for (const item of data) {
      processor.process(item);
    }
  } finally {
    // Explicitly clear to help GC
    processor.clear();
  }
}
```

#### Connection Pooling

```typescript
// Reuse connections instead of creating new ones
const connectionPool = new Map<string, Connection>();

async function getConnection(endpoint: string): Promise<Connection> {
  if (connectionPool.has(endpoint)) {
    const conn = connectionPool.get(endpoint)!;
    if (conn.isAlive()) {
      return conn;
    }
    connectionPool.delete(endpoint);
  }

  const conn = await createConnection(endpoint);
  connectionPool.set(endpoint, conn);
  return conn;
}

// Clean up idle connections periodically
setInterval(() => {
  for (const [endpoint, conn] of connectionPool) {
    if (conn.idleTime > 60000) {
      conn.close();
      connectionPool.delete(endpoint);
    }
  }
}, 30000);
```

#### Batch Processing

```typescript
// Process in batches to avoid overwhelming resources
async function processInBatches<T, R>(
  items: T[],
  processor: (item: T) => Promise<R>,
  batchSize = 10,
): Promise<R[]> {
  const results: R[] = [];

  for (let i = 0; i < items.length; i += batchSize) {
    const batch = items.slice(i, i + batchSize);
    const batchResults = await Promise.all(batch.map(processor));
    results.push(...batchResults);

    // Small delay between batches
    if (i + batchSize < items.length) {
      await sleep(100);
    }
  }

  return results;
}
```

### Security Best Practices

#### Input Validation

```typescript
import { z } from "zod";

// Define strict schemas for user input
const MessageInputSchema = z.object({
  to: z.string().min(1).max(100),
  text: z.string().min(1).max(10000),
  attachments: z.array(z.object({
    url: z.string().url(),
    type: z.enum(["image", "video", "audio", "document"]),
  })).max(10).optional(),
});

function handleMessage(input: unknown) {
  const result = MessageInputSchema.safeParse(input);
  if (!result.success) {
    throw new ValidationError(result.error.issues);
  }
  return processMessage(result.data);
}
```

#### Secret Management

```typescript
// Never log sensitive data
function logRequest(req: Request) {
  const sanitized = {
    ...req,
    headers: {
      ...req.headers,
      authorization: req.headers.authorization ? "[REDACTED]" : undefined,
    },
    body: req.body?.apiKey ? { ...req.body, apiKey: "[REDACTED]" } : req.body,
  };
  logger.info("Request:", sanitized);
}

// Use secure comparison for tokens
import { timingSafeEqual } from "node:crypto";

function verifyToken(provided: string, expected: string): boolean {
  if (provided.length !== expected.length) {
    return false;
  }
  return timingSafeEqual(
    Buffer.from(provided),
    Buffer.from(expected),
  );
}
```

#### Rate Limiting

```typescript
// Implement rate limiting for public endpoints
const rateLimiter = new Map<string, { count: number; resetAt: number }>();

function checkRateLimit(clientId: string, limit: number, windowMs: number): boolean {
  const now = Date.now();
  const record = rateLimiter.get(clientId);

  if (!record || record.resetAt < now) {
    rateLimiter.set(clientId, { count: 1, resetAt: now + windowMs });
    return true;
  }

  if (record.count >= limit) {
    return false;
  }

  record.count++;
  return true;
}
```

### Debugging Deep Dives

#### Tracing Message Flow

```typescript
// Add trace IDs to follow messages through the system
function createTraceContext(): TraceContext {
  return {
    traceId: crypto.randomUUID(),
    spanId: crypto.randomUUID().slice(0, 16),
    startTime: Date.now(),
  };
}

// Log with trace context
function traceLog(ctx: TraceContext, event: string, data?: unknown) {
  console.log(JSON.stringify({
    traceId: ctx.traceId,
    spanId: ctx.spanId,
    elapsed: Date.now() - ctx.startTime,
    event,
    data,
  }));
}

// Usage
async function handleIncomingMessage(msg: Message) {
  const trace = createTraceContext();
  traceLog(trace, "message.received", { from: msg.from });

  try {
    const session = await getSession(msg.sessionKey);
    traceLog(trace, "session.loaded", { sessionId: session.id });

    const response = await runAgent(session, msg.content);
    traceLog(trace, "agent.completed", { responseLength: response.length });

    await sendReply(msg.from, response);
    traceLog(trace, "reply.sent");
  } catch (error) {
    traceLog(trace, "error", { message: error.message, stack: error.stack });
    throw error;
  }
}
```

#### Memory Leak Detection

```typescript
// Monitor memory usage over time
let lastHeapUsed = 0;

setInterval(() => {
  const usage = process.memoryUsage();
  const heapGrowth = usage.heapUsed - lastHeapUsed;

  if (heapGrowth > 50 * 1024 * 1024) { // 50MB growth
    console.warn("Significant heap growth detected:", {
      heapUsed: formatBytes(usage.heapUsed),
      growth: formatBytes(heapGrowth),
      rss: formatBytes(usage.rss),
    });

    // Optionally trigger garbage collection (requires --expose-gc flag)
    if (global.gc) {
      global.gc();
      console.log("Manual GC triggered");
    }
  }

  lastHeapUsed = usage.heapUsed;
}, 60000);

function formatBytes(bytes: number): string {
  return `${(bytes / 1024 / 1024).toFixed(2)} MB`;
}
```

#### Event Loop Monitoring

```typescript
// Detect event loop blocking
let lastCheck = Date.now();
const THRESHOLD_MS = 100;

setInterval(() => {
  const now = Date.now();
  const delay = now - lastCheck - 1000; // Expected 1000ms interval

  if (delay > THRESHOLD_MS) {
    console.warn(`Event loop blocked for ${delay}ms`);

    // Log current async hooks if available
    if (typeof async_hooks !== "undefined") {
      // Log active handles
    }
  }

  lastCheck = now;
}, 1000).unref();
```

### Integration Patterns

#### Webhook Integration

```typescript
import express from "express";
import { createHmac } from "node:crypto";

const app = express();

// Verify webhook signatures
function verifyWebhookSignature(
  payload: Buffer,
  signature: string,
  secret: string,
): boolean {
  const expected = createHmac("sha256", secret)
    .update(payload)
    .digest("hex");
  return `sha256=${expected}` === signature;
}

// Webhook endpoint
app.post("/webhook/:channel", express.raw({ type: "*/*" }), async (req, res) => {
  const { channel } = req.params;
  const signature = req.headers["x-signature"] as string;
  const secret = getWebhookSecret(channel);

  if (!verifyWebhookSignature(req.body, signature, secret)) {
    return res.status(401).json({ error: "Invalid signature" });
  }

  try {
    const event = JSON.parse(req.body.toString());
    await processWebhookEvent(channel, event);
    res.status(200).json({ ok: true });
  } catch (error) {
    console.error("Webhook error:", error);
    res.status(500).json({ error: "Processing failed" });
  }
});
```

#### Queue-Based Processing

```typescript
// Simple in-memory queue for message processing
class MessageQueue {
  private queue: QueueItem[] = [];
  private processing = false;
  private concurrency: number;

  constructor(concurrency = 5) {
    this.concurrency = concurrency;
  }

  async enqueue(message: Message): Promise<void> {
    this.queue.push({
      message,
      addedAt: Date.now(),
    });
    this.process();
  }

  private async process(): Promise<void> {
    if (this.processing) return;
    this.processing = true;

    try {
      while (this.queue.length > 0) {
        const batch = this.queue.splice(0, this.concurrency);
        await Promise.all(batch.map(item => this.processItem(item)));
      }
    } finally {
      this.processing = false;
    }
  }

  private async processItem(item: QueueItem): Promise<void> {
    const queueTime = Date.now() - item.addedAt;
    if (queueTime > 30000) {
      console.warn(`Message queued for ${queueTime}ms`);
    }
    await handleMessage(item.message);
  }
}
```

---

*Last updated: 2024*
