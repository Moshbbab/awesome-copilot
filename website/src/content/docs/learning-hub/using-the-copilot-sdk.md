---
title: 'Embedding Agents with the GitHub Copilot SDK'
description: 'Learn how to use the GitHub Copilot SDK to embed the same agentic engine behind Copilot CLI directly into your own applications and services.'
authors:
  - GitHub Copilot Learning Hub Team
lastUpdated: 2026-08-08
estimatedReadingTime: '7 minutes'
tags:
  - sdk
  - agents
  - automation
  - integrations
relatedArticles:
  - ./building-custom-agents.md
  - ./understanding-mcp-servers.md
  - ./automating-with-hooks.md
prerequisites:
  - Basic understanding of GitHub Copilot agents
  - Familiarity with at least one of Node.js, Python, Go, .NET, Rust, or Java
---

The [GitHub Copilot SDK](https://github.com/github/copilot-sdk) lets you embed the same production-tested agent engine that powers Copilot CLI directly into your own applications. Instead of shelling out to the CLI or building your own orchestration layer, you define agent behavior in code and let the SDK handle planning, tool invocation, file edits, and model calls.

This guide covers what the Copilot SDK is, how it works, and when to reach for it instead of the CLI or IDE experiences.

## What Is the Copilot SDK?

The Copilot SDK is a set of official client libraries — for **Node.js/TypeScript, Python, Go, .NET, Rust, and Java** — that talk to the Copilot CLI running in server mode over JSON-RPC:

```
Your Application
       ↓
  SDK Client
       ↓ JSON-RPC
  Copilot CLI (server mode)
```

The SDK manages the CLI process lifecycle for you. For Node.js, Python, and .NET, the CLI is bundled automatically as a dependency — no separate install required. For Go, Java, and Rust, install the CLI manually or ensure `copilot` is on your `PATH` (Go and Rust also expose application-level CLI bundling options). You can also connect an SDK client to an externally running CLI server instead of letting it manage the process.

The SDK is generally available and follows semantic versioning.

## Why Use the SDK Instead of the CLI?

The CLI is built for interactive and scripted terminal use. The SDK is built for **embedding agentic behavior inside another application** — a backend service, an internal tool, a CI pipeline, or a product feature. Reach for the SDK when you need to:

- Trigger agent runs programmatically from application code rather than a shell command
- Stream tool calls, file edits, and reasoning back into your own UI instead of a terminal
- Approve, deny, or customize individual tool calls with your own permission logic
- Run the same agent engine across multiple languages in a polyglot stack

## Installing an SDK

```bash
# Node.js / TypeScript
npm install @github/copilot-sdk

# Python
pip install github-copilot-sdk

# Go
go get github.com/github/copilot-sdk/go

# .NET
dotnet add package GitHub.Copilot.SDK

# Rust
cargo add github-copilot-sdk
```

The Java SDK is available via Maven Central (`com.github:copilot-sdk-java`).

## Authentication

The SDK supports the same authentication methods available to the CLI, plus one that's SDK-specific:

- **GitHub signed-in user** — reuses stored OAuth credentials from `copilot` CLI login
- **OAuth GitHub App** — pass user tokens from your own GitHub OAuth app
- **Environment variables** — `COPILOT_GITHUB_TOKEN`, `GH_TOKEN`, or `GITHUB_TOKEN`
- **BYOK (Bring Your Own Key)** — configure your own API keys from supported LLM providers (OpenAI, Microsoft Foundry, Anthropic) to use the SDK **without GitHub authentication at all**

BYOK uses key-based authentication only; Microsoft Entra ID, managed identities, and third-party identity providers are not supported.

## Default Tools and Customization

By default, the SDK exposes Copilot's first-party tools, similar to running the CLI with `--allow-all`. Tool execution is still governed by each SDK's permission handler, so your application can approve, deny, or customize tool calls as they happen — useful for building custom approval UIs or enforcing policy before a tool runs.

The SDK also supports the same customization primitives as the CLI:

- **Custom agents** — define specialized agent behavior for your application's domain
- **Skills** — package reusable task guidance for the agent to invoke
- **Custom tools** — extend the agent with your own application-specific tools

## Billing and Models

SDK usage is billed the same way as Copilot CLI usage — each prompt counts toward your usage allowance (unless you're using BYOK). All models available via Copilot CLI are supported in the SDK, and the SDK exposes a method to list available models at runtime so you can select or surface them dynamically.

## Getting Started

1. (Optional) Install the Copilot CLI — bundled automatically for Node.js, Python, and .NET.
2. Install your preferred language SDK using the commands above.
3. Authenticate using a signed-in GitHub account, an OAuth GitHub App token, an environment variable, or BYOK.
4. Follow the [Getting Started Guide](https://github.com/github/copilot-sdk/blob/main/docs/getting-started.md) for a full walkthrough, including connecting to an external CLI server.

For hands-on code samples across every supported language, see the [Copilot SDK Cookbook](https://github.com/github/awesome-copilot/blob/main/cookbook/copilot-sdk).

## Custom Instructions for SDK Development

To speed up development when building against the SDK, this repository includes language-specific custom instructions you can drop into your own project:

- [Node.js / TypeScript](https://github.com/github/awesome-copilot/blob/main/instructions/copilot-sdk-nodejs.instructions.md)
- [Python](https://github.com/github/awesome-copilot/blob/main/instructions/copilot-sdk-python.instructions.md)
- [.NET](https://github.com/github/awesome-copilot/blob/main/instructions/copilot-sdk-csharp.instructions.md)
- [Go](https://github.com/github/awesome-copilot/blob/main/instructions/copilot-sdk-go.instructions.md)
- [Java](https://github.com/github/awesome-copilot/blob/main/instructions/copilot-sdk-java.instructions.md)

## Next Steps

- **Build Custom Agents**: [Building Custom Agents](../building-custom-agents/) — design agent behavior you can reuse across the CLI and the SDK
- **Extend with MCP**: [Understanding MCP Servers](../understanding-mcp-servers/) — connect external tools and data sources to your embedded agent
- **Add Guardrails**: [Automating with Hooks](../automating-with-hooks/) — enforce checks and formatting around autonomous work

## Further Reading

- [GitHub Copilot SDK repository](https://github.com/github/copilot-sdk)
- [Copilot SDK Getting Started Guide](https://github.com/github/copilot-sdk/blob/main/docs/getting-started.md)
- [Copilot SDK Features documentation](https://github.com/github/copilot-sdk/blob/main/docs/features/README.md)
- [Copilot SDK Cookbook](https://github.com/github/awesome-copilot/blob/main/cookbook/copilot-sdk)
- [Copilot SDK CHANGELOG](https://github.com/github/copilot-sdk/blob/main/CHANGELOG.md)

---
