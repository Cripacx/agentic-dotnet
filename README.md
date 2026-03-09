# agentic-dotnet

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.3-blue?style=flat-square)](plugin.json)

> A curated collection of GitHub Copilot skills and agents for C# and .NET development — covering testing, documentation, architecture, and agentic workflows.

## Overview

**agentic-dotnet** is a [GitHub Copilot plugin](https://githubnext.com/projects/copilot-extensions/) that extends Copilot with domain-specific skills and agents tailored for .NET engineers. Whether you're writing unit tests, upgrading a legacy project, designing an MCP server, or running a multi-step agentic workflow, these skills guide Copilot with precise, opinionated instructions.

> [!NOTE]
> Many skills originate from the [awesome-copilot](https://github.com/github/awesome-copilot) repository and the [obra/superpowers](https://github.com/obra/superpowers) repository, adopted and extended for .NET-specific workflows.

## What's Included

### C# & .NET

| Skill | Description |
| --- | --- |
| `aspire` | .NET Aspire AppHost orchestration, service discovery, integrations, dashboard, and deployment |
| `csharp-async` | Best practices for C# async/await programming |
| `csharp-docs` | XML documentation comments following C# documentation conventions |
| `csharp-mcp-server-generator` | Generate a complete MCP server project in C# with tools and prompts |
| `dotnet-best-practices` | Ensure .NET/C# code meets established best practices |
| `dotnet-design-pattern-review` | Review code for design pattern usage and suggest improvements |
| `dotnet-upgrade` | Comprehensive .NET framework upgrade analysis and execution |
| `editorconfig` | Generate a best-practice `.editorconfig` based on project analysis |
| `ef-core` | Entity Framework Core best practices |
| `update-packages` | Update all outdated NuGet packages with restore verification |

### Testing

| Skill | Description |
| --- | --- |
| `csharp-mstest` | MSTest 3.x/4.x unit testing with modern assertion APIs and data-driven tests |
| `csharp-nunit` | NUnit unit testing best practices including data-driven tests |
| `csharp-tunit` | TUnit unit testing best practices including data-driven tests |
| `csharp-xunit` | xUnit unit testing best practices including data-driven tests |

### Agentic Workflows

| Skill | Description |
| --- | --- |
| `brainstorming` | Explore user intent and requirements before any implementation |
| `create-implementation-plan` | Write a structured implementation plan for features or refactoring |
| `create-specification` | Create a specification file optimized for AI consumption |
| `dispatching-parallel-agents` | Coordinate multiple independent tasks using parallel agents |
| `executing-plans` | Execute a written implementation plan with review checkpoints between batches |
| `finishing-a-development-branch` | Guide completion of development work once implementation is done |
| `receiving-code-review` | Evaluate code review feedback with technical rigor before acting |
| `requesting-code-review` | Dispatch a code-reviewer subagent to verify work before merging |
| `skill-creator` | Create or update a skill that extends Copilot capabilities |
| `using-superpowers` | Discover and invoke available skills at the start of any session |
| `verification-before-completion` | Run verification commands and confirm output before claiming work is done |
| `writing-plans` | Write comprehensive plans before touching code |
| `writing-skills` | Create, edit, or verify skills applying TDD principles |

### GitHub Integration

| Skill | Description |
| --- | --- |
| `create-github-issue-feature-from-specification` | Create a GitHub Issue for a feature request from a specification file |
| `create-github-issues-feature-from-implementation-plan` | Create GitHub Issues from implementation plan phases |

### Agents

| Agent | Description |
| --- | --- |
| `code-reviewer` | Reviews a completed project step against the original plan and coding standards |

## Installation

### 1. Add the marketplace

Via the Copilot CLI in a terminal:

```sh
copilot plugin marketplace add cripacx/agentic-dotnet
```

Or from within the Copilot CLI prompt:

```
/plugin marketplace add cripacx/agentic-dotnet
```

### 2. Install the plugin

```sh
copilot plugin install agentic-dotnet@agentic-dotnet
```

Or from within the Copilot CLI prompt:

```
/plugin install agentic-dotnet@agentic-dotnet
```

### 3. Register the skills path in VS Code

Open **Settings** and add the following to `Chat: Agent Skills Locations`:

```
~/.copilot/installed-plugins/agentic-dotnet/agentic-dotnet/skills
```

Alternatively, add it to your `.vscode/settings.json`:

```json
{
  "chat.agentSkillsLocations": {
    "~/.copilot/installed-plugins/agentic-dotnet/agentic-dotnet/skills": true
  }
}
```

> [!TIP]
> After registering the skills path, restart VS Code or reload the window to make the skills available in Copilot Chat.

## Usage

Once installed, skills are automatically picked up by GitHub Copilot based on context. You can also invoke a skill explicitly in Copilot Chat by describing what you want to do — for example:

- *"Write xUnit tests for this service"* → invokes `csharp-xunit`
- *"Create an implementation plan for this feature"* → invokes `create-implementation-plan`
- *"Review this code for design patterns"* → invokes `dotnet-design-pattern-review`
- *"Update all outdated NuGet packages"* → invokes `update-packages`

To get an overview of all available skills and how to invoke them, start your session with:

```
Use the using-superpowers skill
```

## Acknowledgements

Skills in this collection are sourced from and inspired by:

- [awesome-copilot](https://github.com/github/awesome-copilot) — community-maintained Copilot instructions and skills
- [obra/superpowers](https://github.com/obra/superpowers) — agentic workflow skills and agents
