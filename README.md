<p align="center">
  <img src="https://img.shields.io/badge/Power%20Platform-TypeScript-0078D4?style=for-the-badge&logo=microsoft&logoColor=white" alt="Power Platform"/>
  <img src="https://img.shields.io/badge/TypeScript-4.9+-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript"/>
  <img src="https://img.shields.io/badge/Webpack-5-8DD6F9?style=for-the-badge&logo=webpack&logoColor=black" alt="Webpack 5"/>
  <img src="https://img.shields.io/badge/ESLint-8.x-4B32C3?style=for-the-badge&logo=eslint&logoColor=white" alt="ESLint"/>
  <img src="https://img.shields.io/badge/Prettier-2.x-F7B93E?style=for-the-badge&logo=prettier&logoColor=black" alt="Prettier"/>
  <img src="https://img.shields.io/badge/.NET-4.6.2+-512BD4?style=for-the-badge&logo=dotnet&logoColor=white" alt=".NET"/>
</p>

<h1 align="center">Power Platform — TypeScript Web Resources Development</h1>

<p align="center">
  <strong>Enterprise-grade scaffolding framework and development toolkit for Microsoft Dataverse / Dynamics 365 TypeScript web resources</strong>
</p>

<p align="center">
  <em>This is a public showcase of a private production repository. Source code is proprietary and not included.</em>
</p>

---

## Table of Contents

- [Executive Summary](#executive-summary)
- [Problem Statement](#problem-statement)
- [Solution Architecture](#solution-architecture)
- [Technical Deep Dive](#technical-deep-dive)
  - [AI-Powered Scaffolding Engine](#ai-powered-scaffolding-engine)
  - [Build Pipeline](#build-pipeline)
  - [Type Safety Layer](#type-safety-layer)
  - [Deployment Automation](#deployment-automation)
  - [Developer Experience](#developer-experience)
- [Generated Project Anatomy](#generated-project-anatomy)
- [Technology Stack](#technology-stack)
- [Parameterization Model](#parameterization-model)
- [Template Architecture](#template-architecture)
- [Build & Deployment Workflow](#build--deployment-workflow)
- [Code Quality Enforcement](#code-quality-enforcement)
- [Localization Support](#localization-support)
- [Local Debugging Strategy](#local-debugging-strategy)
- [Security Considerations](#security-considerations)
- [Repository Metrics](#repository-metrics)

---

## Executive Summary

This repository contains a **GitHub Copilot Agent Skill** and a **comprehensive template engine** purpose-built for scaffolding production-ready Microsoft Power Platform TypeScript web resource projects.

Instead of spending hours wiring up webpack configurations, ESLint rules, deployment scripts, type generation pipelines, and folder conventions from scratch — developers invoke a single command (`/powerplatform-typescript`) and receive a fully operational, 40+ file project in seconds.

The framework enforces:
- **Zero hardcoded values** — every project-specific setting is parameterized
- **Zero absolute paths** — all references are relative and portable
- **Zero vendor lock-in** — no organization-specific references, fully generic

---

## Problem Statement

Setting up a Power Platform TypeScript web resource project from scratch involves significant boilerplate:

| Challenge | Manual Effort | With This Framework |
|-----------|:---:|:---:|
| Webpack configuration (common, dev, prod) | ~2 hours | Instant |
| ESLint + Prettier + TypeScript integration | ~1 hour | Instant |
| XrmDefinitelyTyped type generation pipeline | ~3 hours | Instant |
| spkl deployment configuration | ~1 hour | Instant |
| PowerShell OAuth connection scripts | ~2 hours | Instant |
| Entity-specific form scaffolding | ~30 min/entity | Instant |
| Localization resource file setup | ~1 hour | Instant |
| .gitignore, README, solution files | ~30 min | Instant |
| **Total** | **~11+ hours** | **< 30 seconds** |

Beyond time savings, manual setup introduces inconsistency. Different developers use different folder structures, naming conventions, and build configurations — creating maintenance headaches across large teams.

---

## Solution Architecture

```
┌──────────────────────────────────────────────────────────────────────┐
│                     GitHub Copilot Agent Skill                        │
│                    /powerplatform-typescript                           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│   ┌─────────────┐    ┌──────────────────┐    ┌────────────────────┐  │
│   │  Parameter   │───▶│  Template Engine  │───▶│  Project Output    │  │
│   │  Collection  │    │  (24 .tmpl files) │    │  (40+ files)       │  │
│   │  & Validation│    │                   │    │                    │  │
│   └─────────────┘    └──────────────────┘    └────────────────────┘  │
│         │                     │                        │              │
│   ┌─────▼─────┐        ┌─────▼──────┐          ┌──────▼──────┐      │
│   │ 13 Params  │        │ Token      │          │ Entity-Aware │      │
│   │ Validated  │        │ Replacement│          │ Iteration    │      │
│   │ & Typed    │        │ Engine     │          │ Engine       │      │
│   └───────────┘        └────────────┘          └─────────────┘      │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌──────────────────────────────────────────────────────────────────────┐
│                      Generated Project                                │
├──────────────┬──────────────┬──────────────┬─────────────────────────┤
│  SourceCode/ │  HelperTools/│  Config      │  Deployment             │
│              │              │              │                          │
│  TypeScript  │  .NET 4.6.2+ │  _Config.ps1 │  spkl.json              │
│  Webpack 5   │  Daxif 5.4   │  OAuth       │  deploy-webresources    │
│  ESLint 8    │  XDT 6.3     │  Entities    │  download-webresources  │
│  Prettier 2  │  spkl 1.0    │  Paths       │  npm run deploy         │
└──────────────┴──────────────┴──────────────┴─────────────────────────┘
```

---

## Technical Deep Dive

### AI-Powered Scaffolding Engine

The core of this repository is a **GitHub Copilot Skill** — a structured YAML+Markdown definition (`SKILL.md`) that instructs the AI agent on how to:

1. **Collect & Validate** 13 typed parameters via interactive prompts
2. **Iterate** over entity lists to generate per-entity form scripts and configuration entries
3. **Expand 24 parameterized templates** with token replacement (`{{PLACEHOLDER}}` → actual values)
4. **Enforce naming conventions** (PascalCase for entity folders, lowercase for logical names)
5. **Output a complete, buildable project** with no further manual wiring required

The skill definition leverages:
- **Progressive loading** — Copilot reads the skill description (~100 tokens) during discovery, loads full instructions (~5,000 tokens) only when invoked
- **Asset references** — Templates are loaded on-demand from the `assets/` directory
- **Validation rules** — Publisher prefix, URL format, entity names, and npm package name constraints

### Build Pipeline

The generated project employs a **three-tier Webpack configuration**:

| Config | Purpose | Key Features |
|--------|---------|-------------|
| `webpack.common.js` | Shared base | Dynamic entry discovery via glob, library namespace export, source maps, ESLint plugin, CopyPlugin for resx/img |
| `webpack.dev.js` | Development | Merges common + disables minification |
| `webpack.prod.js` | Production | Merges common + enables full optimization |

**Dynamic Entry Resolution:**

The webpack configuration uses `glob.sync()` to automatically discover all TypeScript files under `src/ts/` — excluding the `utils/` folder (shared libraries are not entry points). Each discovered file becomes a webpack entry with:

- A **library export** under a configurable global namespace (e.g., `window.Contoso.AccountForm`)
- A **source map** for debugging (output to a separate `maps/` directory)
- **Publisher-prefixed output paths** matching Dataverse web resource naming conventions

This approach means adding a new entity form script requires zero webpack configuration changes — simply create the file and webpack picks it up automatically.

### Type Safety Layer

Type-safe development is achieved through **XrmDefinitelyTyped (XDT)**, which connects to a live Dataverse environment and generates:

| Generated Artifact | Location | Purpose |
|-------------------|----------|---------|
| `Form/{Entity}/Main/{FormName}.d.ts` | `src/typings/XRM/Form/` | Strongly-typed form controls, tabs, attributes |
| `Web/{entity}.d.ts` | `src/typings/XRM/Web/` | Entity schema with all columns, relationships |
| `xrm.d.ts` | `src/typings/XRM/` | Core XRM API type overrides |
| `metadata.d.ts` | `src/typings/XRM/` | SDK metadata interfaces |
| `dg.xrmquery.web.d.ts` | `src/typings/XRM/` | Web API query builder types |
| `_internal/sdk.d.ts` | `src/typings/XRM/_internal/` | EntityReference, Money, OptionSet interfaces |

The generation pipeline:

```
npm run gen
    │
    ▼
GenerateTypeScript.ps1
    │
    ├── Loads _InitDaxif.ps1 (OAuth connection via ADAL)
    │       │
    │       ├── Reads _Config.ps1 (environment URL, entities, paths)
    │       ├── Initializes Daxif DLL from HelperTools build output
    │       └── Constructs connection string with token cache
    │
    ├── Builds FSharp lists from entity array
    └── Invokes DG.Daxif.Solution.GenerateTypeScriptContext()
            │
            └── Outputs .d.ts files to src/typings/XRM/
```

### Deployment Automation

Deployment follows a **two-stage pipeline**:

```
npm run deploy
    │
    ├── Stage 1: webpack --config webpack.prod.js
    │       │
    │       ├── Compiles .ts → .js (minified)
    │       ├── Generates source maps
    │       ├── Copies .resx and .svg assets
    │       └── Outputs to webresources/{prefix}_/
    │
    └── Stage 2: deploy-webresources.bat
            │
            ├── Discovers spkl.exe from NuGet package cache
            ├── Reads spkl.json (solution name, autodetect mode)
            └── Publishes to Dataverse via spkl CLI
                    │
                    ├── OAuth interactive login (cached tokens)
                    ├── Compares local vs server web resources
                    └── Uploads changed files only
```

**spkl.json Configuration:**
- `autodetect: "yes"` — Automatically registers new web resources in the target solution
- `deleteaction: "no"` — Prevents accidental deletion of server-side resources
- `root` — Points to the webpack output directory via relative path

### Developer Experience

| Feature | Implementation |
|---------|---------------|
| **Watch Mode** | `npm run watch` — Webpack recompiles on every `.ts` file change |
| **Source Maps** | Separate `maps/` directory for clean debugging in Fiddler |
| **Auto-Format** | `npm run format` — Prettier formats all TypeScript files |
| **Auto-Lint** | `npm run lint` — ESLint with `--fix` auto-corrects violations |
| **Lint-on-Build** | `ESLintPlugin` in webpack catches errors at compile time |
| **Type Intellisense** | Generated `.d.ts` files provide full autocomplete in VS Code |
| **Localization** | `.resx` files with LCID-based naming (1033=EN, 1043=NL, etc.) |

---

## Generated Project Anatomy

```
{ProjectName}/
│
├── .gitignore                               # Comprehensive ignore rules
├── README.md                                # Auto-generated project documentation
│
├── SourceCode/
│   ├── package.json                         # npm configuration
│   │   ├── 7 npm scripts (dev, dist, watch, deploy, format, lint, gen)
│   │   └── 16 devDependencies (TypeScript, Webpack, ESLint, Prettier, etc.)
│   │
│   ├── tsconfig.json                        # TypeScript compiler options
│   │   ├── Target: ES6 modules
│   │   ├── Strict mode enabled
│   │   └── Custom typeRoots → src/typings/
│   │
│   ├── webpack.common.js                    # Shared webpack configuration
│   │   ├── Dynamic entry point discovery (glob pattern)
│   │   ├── Library namespace exports (configurable)
│   │   ├── Source map generation
│   │   ├── CleanWebpackPlugin (output hygiene)
│   │   ├── ESLintPlugin (compile-time linting)
│   │   └── CopyPlugin (resx + svg asset copying)
│   │
│   ├── webpack.dev.js                       # Development: unminified + source maps
│   ├── webpack.prod.js                      # Production: minified + optimized
│   ├── .eslintrc.cjs                        # ESLint: TS recommended + Prettier integration
│   ├── .eslintignore                        # Webpack configs excluded from linting
│   ├── .prettierrc.cjs                      # Prettier: semicolons, trailing commas, 4-space tabs
│   │
│   └── src/
│       ├── ts/
│       │   ├── forms/                       # Entity form event handlers
│       │   │   ├── {Entity}/                # One folder per Dataverse entity
│       │   │   │   └── {Entity}.ts          # OnLoad, OnSave, field change handlers
│       │   │   └── ...                      # Auto-generated for each entity in config
│       │   │
│       │   └── utils/                       # Shared utility library (excluded from entry points)
│       │       └── General.ts               # ToggleFields(), GuidRemoveBrackets(), etc.
│       │
│       ├── resx/                            # Localization resource files
│       │   ├── {Prefix}.1033.resx           # English (US)
│       │   └── {Prefix}.1043.resx           # Dutch (NL) — additional LCIDs as needed
│       │
│       ├── img/                             # SVG image web resources
│       └── typings/                         # Auto-generated XRM type definitions
│           └── XRM/                         # Output from XrmDefinitelyTyped
│               ├── Form/{entity}/Main/      # Strongly-typed form definitions
│               ├── Web/                     # Entity schema types
│               ├── _internal/               # SDK types (EntityReference, Money, etc.)
│               ├── xrm.d.ts                 # Core XRM API types
│               ├── metadata.d.ts            # Metadata namespace
│               └── dg.xrmquery.web.d.ts     # Web API query builder types
│
└── HelperTools/
    ├── HelperTools.sln                      # Visual Studio solution file
    │
    └── HelperTools/
        ├── HelperTools.csproj               # .NET project with NuGet dependencies
        │   ├── Delegate.Daxif 5.4.0         # Dataverse automation framework
        │   ├── Delegate.XrmDefinitelyTyped 6.3.0  # Type generation engine
        │   ├── FSharp.Core 6.0.2            # Daxif dependency
        │   └── spkl 1.0.640                 # Sparkle XRM deployment tool
        │
        ├── _Config.ps1                      # Central configuration
        │   ├── Environment (URL, OAuth, connection method)
        │   ├── Entities (array for type generation)
        │   └── Paths (relative: solution root, tools, output)
        │
        ├── App.config                       # .NET runtime configuration
        ├── JustToCompile.cs                 # Build target (required for NuGet restore)
        │
        ├── Daxif/
        │   ├── GenerateTypeScript.ps1       # XDT invocation script
        │   │   ├── Loads config via _InitDaxif.ps1
        │   │   ├── Builds FSharp entity lists
        │   │   └── Calls DG.Daxif.Solution.GenerateTypeScriptContext()
        │   │
        │   └── _InitDaxif.ps1               # Connection initialization
        │       ├── Loads _Config.ps1
        │       ├── Imports Daxif, ADAL, XRM Tooling DLLs
        │       ├── Handles OAuth token caching (TokenCache.dat)
        │       └── Creates DG.Daxif.Environment instance
        │
        ├── spkl/
        │   ├── deploy-webresources.bat      # Publish to Dataverse
        │   │   └── Auto-discovers spkl.exe from NuGet cache
        │   └── download-webresources.bat    # Pull from Dataverse
        │
        ├── spkl.json                        # Deployment configuration
        │   ├── Solution target (configurable)
        │   ├── Autodetect mode
        │   └── Root path (relative to webresources output)
        │
        └── XrmDefinitelyTyped/
            └── XrmDefinitelyTyped.exe.config  # XDT standalone config
                ├── Environment URL
                ├── OAuth credentials
                ├── Entity whitelist
                └── Output directory (relative)
```

---

## Technology Stack

| Layer | Technology | Version | Purpose |
|-------|-----------|---------|---------|
| **Language** | TypeScript | 4.9+ | Strongly-typed JavaScript superset |
| **Bundler** | Webpack | 5.75+ | Module bundling, tree shaking, code splitting |
| **Loader** | ts-loader | 9.4+ | TypeScript-to-JavaScript compilation |
| **Linter** | ESLint | 8.29+ | Static analysis and code quality rules |
| **Formatter** | Prettier | 2.8+ | Opinionated code formatting |
| **Type Generation** | XrmDefinitelyTyped | 6.3.0 | Generates `.d.ts` from Dataverse metadata |
| **Build Framework** | Daxif | 5.4.0 | Dataverse automation, context generation |
| **Deployment** | spkl (Sparkle XRM) | 1.0.640 | Web resource publishing to Power Platform |
| **Runtime** | .NET Framework | 4.6.2+ | Hosts Daxif, XDT, and spkl executables |
| **Package Manager** | npm | 8+ | Dependency management and script runner |
| **Auth** | ADAL / MSAL | Via Daxif | OAuth 2.0 with token caching |
| **AI** | GitHub Copilot Skill | — | Intelligent project scaffolding |

---

## Parameterization Model

The framework accepts **13 configurable parameters** that propagate across all 24 template files. No value is ever hardcoded.

| Parameter | Scope | Propagates To |
|-----------|-------|---------------|
| `PROJECT_NAME` | Global | `package.json` name, README title |
| `PROJECT_DESCRIPTION` | Global | `package.json` description |
| `PUBLISHER_PREFIX` | Build + Deploy | webpack output path, Fiddler rules, web resource naming |
| `CRM_URL` | Connection | `_Config.ps1`, `XrmDefinitelyTyped.exe.config` |
| `SOLUTION_NAME` | Deploy | `spkl.json` solution target |
| `ENTITIES` | Scaffold + Config | Form script generation, `_Config.ps1` array, XDT config |
| `OAUTH_APP_ID` | Auth | `_Config.ps1`, `XrmDefinitelyTyped.exe.config` |
| `OAUTH_RETURN_URL` | Auth | `_Config.ps1`, `XrmDefinitelyTyped.exe.config` |
| `AUTHOR_NAME` | Metadata | `package.json` author |
| `TARGET_FRAMEWORK` | Build | `HelperTools.csproj`, `_Config.ps1` tools path |
| `DOTNET_FRAMEWORK_VERSION` | Runtime | `App.config` supportedRuntime |
| `RESX_FILE_PREFIX` | Localization | `.resx` file naming |
| `LIBRARY_NAMESPACE` | Build | webpack `library.name` (global window object) |

**Validation rules enforced at input time:**
- `PUBLISHER_PREFIX` — lowercase alpha only, no underscores
- `CRM_URL` — must begin with `https://`, no trailing slash
- `ENTITIES` — comma-separated lowercase logical names
- `PROJECT_NAME` — valid npm package name (lowercase, hyphens allowed)

---

## Template Architecture

The skill ships with **24 parameterized template files** in the `assets/` directory:

```
assets/
├── package.json.tmpl              # npm configuration with 16 devDependencies
├── tsconfig.json.tmpl             # TypeScript ES6 strict mode configuration
├── webpack.common.js.tmpl         # Dynamic entry discovery + library exports
├── webpack.dev.js.tmpl            # Development merge configuration
├── webpack.prod.js.tmpl           # Production merge configuration
├── eslintrc.cjs.tmpl              # ESLint + Prettier + TypeScript rules
├── eslintignore.tmpl              # Webpack configs excluded
├── prettierrc.cjs.tmpl            # Formatting rules (semicolons, trailing commas)
├── form-script.ts.tmpl            # Per-entity OnLoad handler scaffold
├── general-utils.ts.tmpl          # Shared utility functions
├── resx.tmpl                      # ResX localization template with XSD schema
├── gitignore.tmpl                 # 70+ ignore patterns (VS, Node, .NET, OS)
├── README.md.tmpl                 # Auto-generated project documentation
├── Config.ps1.tmpl                # PowerShell environment + entity configuration
├── GenerateTypeScript.ps1.tmpl    # XDT invocation with FSharp list construction
├── InitDaxif.ps1.tmpl             # OAuth ADAL connection with token caching
├── HelperTools.csproj.tmpl        # .NET project with 4 NuGet packages
├── HelperTools.sln.tmpl           # Visual Studio solution structure
├── App.config.tmpl                # .NET runtime version configuration
├── JustToCompile.cs.tmpl          # Minimal C# build target
├── spkl.json.tmpl                 # Deployment solution + autodetect config
├── deploy-webresources.bat.tmpl   # NuGet-cache-aware spkl invocation
├── download-webresources.bat.tmpl # Pull web resources from Dataverse
└── XrmDefinitelyTyped.exe.config.tmpl  # XDT standalone OAuth + entity config
```

Each template uses `{{PLACEHOLDER}}` tokens that are replaced during scaffolding. The entity parameter triggers an **iteration engine** that generates one form script folder + file per entity and populates configuration arrays.

---

## Build & Deployment Workflow

```
                        Developer Machine
                              │
         ┌────────────────────┼────────────────────┐
         │                    │                     │
    ┌────▼────┐         ┌────▼────┐          ┌─────▼─────┐
    │  Author  │         │  Build  │          │  Deploy   │
    │   .ts    │         │ Webpack │          │  spkl     │
    │  files   │         │         │          │           │
    └────┬────┘         └────┬────┘          └─────┬─────┘
         │                    │                     │
         │    ┌───────────────┼───────────────┐     │
         │    │               │               │     │
         ▼    ▼               ▼               ▼     ▼
    ┌─────────────┐   ┌──────────────┐  ┌──────────────┐
    │ src/ts/     │   │ webresources/│  │  Dataverse    │
    │ forms/      │──▶│ {prefix}_/   │─▶│  Environment  │
    │ utils/      │   │   js/        │  │              │
    └─────────────┘   │   resx/      │  │  Solution:   │
    ┌─────────────┐   │   img/       │  │  {name}      │
    │ src/resx/   │──▶│              │  │              │
    │ src/img/    │──▶│   maps/      │  └──────────────┘
    └─────────────┘   └──────────────┘         ▲
                                                │
    ┌─────────────┐                      ┌──────┴──────┐
    │ Dataverse   │─────────────────────▶│ src/typings/│
    │ Metadata    │    npm run gen       │ XRM/        │
    └─────────────┘                      └─────────────┘
```

| Command | Pipeline Stage | Output |
|---------|---------------|--------|
| `npm run gen` | Type Generation | `src/typings/XRM/**/*.d.ts` |
| `npm run dev` | Dev Build | `webresources/{prefix}_/js/*.js` + source maps |
| `npm run dist` | Prod Build | `webresources/{prefix}_/js/*.js` (minified) |
| `npm run watch` | Continuous Build | Auto-recompile on file change |
| `npm run deploy` | Prod Build + Publish | Build → spkl → Dataverse |
| `npm run format` | Code Quality | Auto-format `.ts` files |
| `npm run lint` | Code Quality | Auto-fix ESLint violations |

---

## Code Quality Enforcement

### ESLint Configuration

```
Parser:     @typescript-eslint/parser
Extends:    @typescript-eslint/recommended + prettier + prettier/recommended
Plugins:    @typescript-eslint, prettier
Rules:
  ├── prettier/prettier: error          # Format violations are build errors
  └── @typescript-eslint/no-explicit-any: off  # Allows Xrm API flexibility
```

### Prettier Configuration

```
Semicolons:      required
Trailing commas: all (ES5+)
Quotes:          double
Print width:     800 (accommodates Dataverse column-heavy lines)
Tab width:       4 spaces
Line endings:    auto (cross-platform)
```

### Build-Time Integration

ESLint runs as a **webpack plugin** — lint errors fail the build:

```
ESLintPlugin({
    fix: true,                    // Auto-fix on build
    extensions: ["ts", "tsx"],    // TypeScript files only
    lintDirtyModulesOnly: true,   // Only lint changed files (perf)
    failOnError: true             // Break build on violations
})
```

---

## Localization Support

The framework generates `.resx` files with Microsoft's standard ResX 2.0 XML schema:

| File | LCID | Language |
|------|------|----------|
| `{Prefix}.1033.resx` | 1033 | English (US) |
| `{Prefix}.1043.resx` | 1043 | Dutch (NL) |

Resource files are automatically copied to the `webresources/{prefix}_/resx/` output during webpack build. Additional languages can be added by creating new `.resx` files with the appropriate LCID.

Resources are consumed at runtime via the Dataverse API:
```typescript
Xrm.Utility.getResourceString("{prefix}_/resx/{Prefix}", "ResourceKey");
```

---

## Local Debugging Strategy

The generated project supports **live local debugging** via Fiddler Classic AutoResponder rules:

```
┌──────────────┐        ┌──────────────┐        ┌──────────────┐
│   Browser    │───────▶│   Fiddler    │───────▶│  Dataverse   │
│              │        │  Proxy       │        │  Server      │
│              │◀───────│              │◀───────│              │
└──────────────┘        └──────┬───────┘        └──────────────┘
                               │
                    ┌──────────▼──────────┐
                    │   AutoResponder     │
                    │                     │
                    │  webresources/      │
                    │  {prefix}_/js/*     │──▶ Local webpack output
                    │  {prefix}_/resx/*   │──▶ Local resx files
                    │  {prefix}_/img/*    │──▶ Local SVG assets
                    └─────────────────────┘
```

**AutoResponder regex patterns** are auto-generated in the project README, configured with the correct publisher prefix — enabling instant local override without modifying any server-side resources.

---

## Security Considerations

| Aspect | Implementation |
|--------|---------------|
| **Credential Storage** | OAuth tokens cached in `TokenCache.dat` (gitignored) |
| **No Plaintext Secrets** | ADAL interactive prompt, no stored passwords |
| **Connection Strings** | Built dynamically in PowerShell, never persisted |
| **User-Specific Config** | `UserSpecific.config` for XDT overrides (gitignored) |
| **Deployment Auth** | Interactive OAuth via spkl CLI |
| **Environment Isolation** | Config-driven environment switching (Dev/UAT/Prod) |

---

## Repository Metrics

| Metric | Value |
|--------|-------|
| Template files | 24 |
| Parameters | 13 |
| Generated files per project | 40+ |
| npm scripts | 7 |
| NuGet dependencies | 4 |
| npm devDependencies | 16 |
| Supported .NET frameworks | net462, net472, net48 |
| Localization LCIDs | Unlimited (2 included) |
| Setup time (manual) | ~11 hours |
| Setup time (with skill) | < 30 seconds |

---

## Author

**Kaustubh Tendulkar**

- GitHub: [@kaustubhtendulkar](https://github.com/kaustubhtendulkar)

---

<p align="center">
  <sub>This is a public showcase of a private production repository.<br/>Source code, templates, and skill definitions are proprietary.<br/>For access inquiries, please contact the author.</sub>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-Production-brightgreen?style=flat-square" alt="Status"/>
  <img src="https://img.shields.io/badge/License-Proprietary-red?style=flat-square" alt="License"/>
  <img src="https://img.shields.io/badge/Copilot%20Skill-Enabled-blue?style=flat-square&logo=github" alt="Copilot"/>
</p>