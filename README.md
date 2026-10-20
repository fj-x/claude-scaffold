# Claude Code Scaffold — Multi-Stack

Production-ready scaffolds for configuring Claude Code as an AI coding agent across multiple technology stacks.

Each scaffold is a self-contained `.claude/` configuration package — **copy it into your project root before the first prompt** and Claude Code starts every session with the right guards, conventions, and context already in place.

## Available Scaffolds

| Directory | Stack | Status |
|-----------|-------|--------|
| `php-symfony/` | PHP 8.3 + Symfony 7.x | ✅ |
| `python/` | Python (TBD) | ⬜ |
| `go/` | Go (TBD) | ⬜ |
| `ts-angular/` | TypeScript + Angular (TBD) | ⬜ |

## What Is a Scaffold?

A scaffold is **Infrastructure as Code for an AI agent** — a minimal set of files (~6 files, ~4KB). It is not application code; it configures **how Claude Code behaves** in your project.

## Quick Start

```bash
# 1. Copy scaffold into your existing project
cp -r php-symfony/.claude your-project/.claude
cp php-symfony/CLAUDE.md your-project/CLAUDE.md
cp php-symfony/.gitignore your-project/.gitignore  # merge with existing

# 2. Commit to git (settings.json is team-shared, deny rules are security)
cd your-project
git add .claude/settings.json .claude/rules/ CLAUDE.md .gitignore
git commit -m "chore: add Claude Code scaffold"

# 3. Start Claude Code from project root
claude
```

## Architecture: 4 Layers

Every scaffold follows the same 4-layer structure. **Order matters** — enforcement first, then behaviour:

### Layer 1: `.gitignore` + Structure
Foundation hygiene. Keeps secrets (`.env`), personal settings (`settings.local.json`), and build artifacts out of git and out of the agent's context.

### Layer 2: `.claude/settings.json` — Enforcement (Layer B)
**Client-side guards** that intercept every tool call BEFORE execution. The model cannot bypass these — they work even under prompt injection.

- **deny** — operations that must never happen (destructive commands, secret access, force-push)
- **allow** — verify-tools that run without approval prompts (tests, linters, read-only git)
- **env** — subprocess environment scrubbing and telemetry controls

> **Why deny is irreversible:** A deny rule at the project level cannot be overridden by a local allow. This prevents insider/injection attacks from silently removing safety guards.

### Layer 3: `CLAUDE.md` — Persistent Behaviour (Layer A)
A text file automatically concatenated into the system prompt at every session start. Survives `/clear` and `/compact` because it's re-read from disk.

Three categories in positional order:
1. **Context** (top) — what this project is, its stack, architecture, BC map
2. **Conventions** (middle) — naming, testing, commit format
3. **Guardrails** (bottom) — "Do NOT" rules with recency bias advantage

### Layer 4: `.claude/rules/*.md` — Conditional Behaviour
Path-specific rules with `globs:` frontmatter. **Lazy-loaded** — the agent only sees rules relevant to files it's currently touching. Zero attention dilution from irrelevant instructions.

```yaml
---
globs: "**/*.php" # loaded only when touching PHP files
---
# PHP conventions here
```
