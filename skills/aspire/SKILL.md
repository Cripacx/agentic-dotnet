---
name: aspire
description: 'Aspire skill covering the Aspire CLI, AppHost orchestration, service discovery, integrations, MCP server, VS Code extension, Dev Containers, GitHub Codespaces, templates, dashboard, and deployment. Use when the user asks to create, run, debug, configure, deploy, or troubleshoot an Aspire distributed application.'
---

# Aspire — Polyglot Distributed-App Orchestration

Aspire is a **code-first, polyglot toolchain** for building observable, production-ready distributed applications. It orchestrates containers, executables, and cloud resources from a single AppHost project — regardless of whether the workloads are C#, Python, JavaScript/TypeScript, Go, Java, Rust, Bun, Deno, or PowerShell.

> **Mental model:** The AppHost is a *conductor* — it doesn't play the instruments, it tells every service when to start, how to find each other, and watches for problems.

## 1. Researching Aspire Documentation

The Aspire team ships an **MCP server** that provides documentation tools directly inside your AI assistant.

### Aspire CLI 13.2+ (recommended — has built-in docs search)

If running Aspire CLI **13.2 or later** (`aspire --version`), the MCP server includes docs search tools:

| Tool | Description |
|---|---|
| `list_docs` | Lists all available documentation from aspire.dev |
| `search_docs` | Performs weighted lexical search across indexed documentation |
| `get_doc` | Retrieves a specific document by its slug |

These tools were added in [PR #14028](https://github.com/dotnet/aspire/pull/14028). To update: `aspire update --self --channel daily`.

For more on this approach, see David Pine's post: https://davidpine.dev/posts/aspire-docs-mcp-tools/

### Aspire CLI 13.1 (integration tools only)

On 13.1, the MCP server provides integration lookup but **not** docs search:

| Tool | Description |
|---|---|
| `list_integrations` | Lists available Aspire hosting integrations |
| `get_integration_docs` | Gets documentation for a specific integration package |

For general docs queries on 13.1, use **Context7** as your primary source (see below).

### Fallback: Context7

Use **Context7** (`mcp_context7`) when the Aspire MCP docs tools are unavailable (13.1) or the MCP server isn't running:

**Step 1 — Resolve the library ID** (one-time per session):

Call `mcp_context7_resolve-library-id` with `libraryName: ".NET Aspire"`.

| Rank | Library ID | Use when |
|---|---|---|
| 1 | `/microsoft/aspire.dev` | Primary source. Guides, integrations, CLI reference, deployment. |
| 2 | `/dotnet/aspire` | API internals, source-level implementation details. |
| 3 | `/communitytoolkit/aspire` | Non-Microsoft polyglot integrations (Go, Java, Node.js, Ollama). |

**Step 2 — Query docs:**

```
libraryId: "/microsoft/aspire.dev", query: "Python integration AddPythonApp service discovery"
libraryId: "/communitytoolkit/aspire", query: "Golang Java Node.js community integrations"
```

### Fallback: GitHub search (when Context7 is also unavailable)

Search the official docs repo on GitHub:
- **Docs repo:** `microsoft/aspire.dev` — path: `src/frontend/src/content/docs/`
- **Source repo:** `dotnet/aspire`
- **Samples repo:** `dotnet/aspire-samples`
- **Community integrations:** `CommunityToolkit/Aspire`

---

## 2. Prerequisites & Install

| Requirement | Details |
|---|---|
| **.NET SDK** | 10.0+ (required even for non-.NET workloads — the AppHost is .NET) |
| **Container runtime** | Docker Desktop, Podman, or Rancher Desktop |
| **IDE (optional)** | VS Code + C# Dev Kit, Visual Studio 2022, JetBrains Rider |

```bash
# Linux / macOS
curl -sSL https://aspire.dev/install.sh | bash

# Windows PowerShell
irm https://aspire.dev/install.ps1 | iex

# Verify
aspire --version

## 3. CLI Quick Reference

Valid commands in Aspire CLI 13.1:

| Command | Description | Status |
|---|---|---|
| `aspire new <template>` | Create from template | Stable |
| `aspire init` | Initialize in existing project | Stable |
| `aspire run` | Start all resources locally | Stable |
| `aspire add <integration>` | Add an integration | Stable |
| `aspire publish` | Generate deployment manifests | Preview |
| `aspire config` | Manage configuration settings | Stable |
| `aspire cache` | Manage disk cache | Stable |
| `aspire deploy` | Deploy to defined targets | Preview |
| `aspire do <step>` | Execute a pipeline step | Preview |
| `aspire update` | Update integrations (or `--self` for CLI) | Preview |
| `aspire mcp init` | Configure MCP for AI assistants | Stable |
| `aspire mcp start` | Start the MCP server | Stable |
---