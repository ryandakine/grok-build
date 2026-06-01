# Grok Build — Global Setup

**Single source of truth for Grok 4.3 coding agent across ALL ryandakine projects.**

No more per-project duplication. Update once here → propagates everywhere.

## Why This Exists

Your 6 core projects (oregon-trail, poker-coach-pro, MultiSportsBettingPlatformv2, congressional-intel-data, practice-trainer, cimco-v3) were getting heavy Claude-style ceremony. Grok 4.3 is faster, more agentic, and needs less hand-holding. This global setup leans into Grok’s strengths:

- Plan Mode first
- Parallel sub-agents
- Test-first discipline
- Direct GitHub tool usage (create_branch, create_pull_request, etc.)
- Zero bloat, maximum speed

## How to Use in Any Project

### Option 1: Minimal Reference (Recommended)
In your project root, create `GROK.md` with:

```markdown
# Grok Build for [Project Name]

**Extends global setup from ryandakine/grok-build**

See: https://github.com/ryandakine/grok-build/blob/main/GLOBAL_GROK.md

## Project-Specific Overrides & Guardrails
- [Add local rules, taxonomies, or exceptions here]

## Global Rules (from GLOBAL_GROK.md)
[Copy the full content of GLOBAL_GROK.md below this line or keep the reference]
```

### Option 2: Full Copy + Local Additions
Copy the entire GLOBAL_GROK.md into your project’s GROK.md and prepend any project-specific rules at the top.

### Quick Bootstrap (Future)
A `bootstrap.sh` will be added soon to auto-pull the global + create the thin GROK.md wrapper.

## Current Projects Using This Global Setup
- oregon-trail (kaplay-rebuild branch)
- poker-coach-pro (grok-build-upgrade)
- practice-trainer
- congressional-intel-data
- MultiSportsBettingPlatformv2
- cimco-v3

Update this repo when the setup evolves. All projects stay in sync automatically.

## Philosophy
Grok doesn’t need 200-line WORKFLOW.md theater. It needs clear principles, strong guardrails where domain matters, and the freedom to go fast with Plan Mode + parallel agents.

This is the global Grok Build. Let’s cook.