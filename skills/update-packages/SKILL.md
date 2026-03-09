---
name: update-packages
description: Update all outdated NuGet packages in the solution using dotnet-outdated. Handles tool installation, discovery, and safe update workflow with restore verification.
---

# update-packages

## Purpose
Use this skill to update all outdated NuGet packages across the entire solution using the `dotnet-outdated` tool. It ensures tools are installed, versions verified, and `dotnet restore` confirms compatibility after updates.

## Prerequisites

- .NET SDK installed and `dotnet` CLI available on `PATH`.
- Internet access to reach NuGet feeds (including those listed in `nuget.config`).
- Git working tree should be on a dedicated branch before running updates (never update directly on `develop`).

## Keywords
nuget, packages, outdated, update, upgrade, bump, versions, dotnet-outdated

## Workflow

### 0. Git Preparation (mandatory before any update)
- Checkout `develop` and pull the latest state.
- Create a dedicated branch, e.g. `chore/update-packages-<date>`.
- Run all updates on this branch only.

### 1. Install / restore dotnet-outdated tool
Run once per machine (or verify it is already installed):
```powershell
dotnet tool install --global dotnet-outdated-tool
```
If already installed, update the tool itself to the latest version:
```powershell
dotnet tool update --global dotnet-outdated-tool
```

### 2. Inspect outdated packages (dry run)
Navigate to the solution root and list all outdated packages without applying changes:
```powershell
dotnet outdated backend.slnx
```

> **Pinned packages – do not upgrade beyond the listed major version regardless of instructions:**
> | Package | Max allowed version |
> |---------|-------------------|
> | `MassTransit.*` | `8.x.x` |
Review the output. Note packages that are:
- **patch** updates – safe, apply unconditionally.
- **minor** updates – generally safe, but review changelog for breaking changes.
- **major** updates – require manual review; apply one-by-one and verify.

### 3. Apply updates

> **Default behavior: patch and minor updates only.**  
> Major version upgrades **must never be applied unless the user explicitly requests them**.

#### Default – Update patch and minor versions (always safe to run)
```powershell
dotnet outdated backend.slnx --upgrade --version-lock Major
```

#### Explicit instruction only – Update a specific package including a major bump
Only run this when the user has explicitly named the package and the target major version:
```powershell
dotnet outdated backend.slnx --upgrade --include <PackageName>
```

#### Explicit instruction only – Update all packages including major versions
Only run this when the user has explicitly requested a full upgrade across all major versions:
```powershell
dotnet outdated backend.slnx --upgrade
```

> **Important:** This project uses **Central Package Management** (`Directory.Packages.props`). `dotnet-outdated` respects CPM and updates versions in that file. Verify that `Directory.Packages.props` was modified after the upgrade step.

### 4. Verify restore
After upgrades are applied, run a full restore to confirm no version conflicts:
```powershell
dotnet restore backend.slnx
```
If restore fails:
- Identify the conflicting package from the error output.
- Revert that specific version entry in `Directory.Packages.props` to its previous value.
- Re-run `dotnet restore` until clean.

### 5. Build verification
After a successful restore, confirm the solution still builds:
```powershell
dotnet build backend.slnx --no-restore
```
Fix any compilation errors introduced by API changes in updated packages before proceeding.

### 6. Commit
Use a Conventional Commit message, e.g.:
```
chore(deps): update NuGet packages to latest versions
```
If updating a single notable package, include it in the scope:
```
chore(deps): update MassTransit to 8.x
```

## Rules
- **Never** update packages directly on `main`.
- **Always** run `dotnet restore` and `dotnet build` after updates.
- **Do not** manually edit `Directory.Packages.props` to bump versions during this workflow – let `dotnet-outdated --upgrade` handle it.
- **Major version upgrades are forbidden by default.** Only apply them when the user has explicitly given the instruction, naming the package or confirming a full major upgrade.
- When a major upgrade is explicitly requested, apply and validate **one package at a time** to isolate issues.
- If a package update causes build or test failures, revert that package's version and document it in the PR description.

## Done Criteria
- All targeted packages updated in `Directory.Packages.props`.
- `dotnet restore` completes without errors.
- `dotnet build` completes without errors.
- Changes committed on a feature branch with a compliant Conventional Commit message.
- PR opened targeting `develop` with a summary of updated packages.