# Agent entry file generation guide

This guide covers generating entry files for **any** AI coding agent: CLAUDE.md, AGENTS.md, GEMINI.md, .cursorrules, .windsurfrules, or others.

All entry files serve the same purpose: a ~100-line table of contents that gives the agent enough context to start working, with pointers to `docs/` for depth.

## Universal template

Adapt the file name, format, and tool-specific sections to the target agent. The core content is the same.

```markdown
# {Project Name}

{One-line description of what this project does.}

## Tech Stack

- Backend: {technology}
- Frontend: {technology}
- Database: {technology}
- Infra: {technology}

## Quick Start

```bash
# Install dependencies
{command}

# Run dev server
{command}

# Run tests
{command}

# Lint
{command}
```

## Project Structure

```
{Directory tree — major directories only, 2–3 levels deep}
```

## Documentation

- `docs/architecture.md` — System architecture, domain map, package layering
- `docs/conventions.md` — Coding conventions, naming, file structure rules
- `docs/api-design.md` — API design principles, response format, error handling
- `docs/quality.md` — QA checklist, evaluation criteria, sprint contract template
- `PROJECT-SPEC.md` — Full feature spec and sprint plan

## Core Constraints

{3–5 rules that MUST be followed. Specific and verifiable.}

1. {Rule 1 — e.g., "All API endpoints follow the response format in docs/api-design.md"}
2. {Rule 2 — e.g., "New tables require a migration file; never modify the DB manually"}
3. {Rule 3 — e.g., "Environment variables go in .env.example with a description"}

## Work Process

- Check PROJECT-SPEC.md acceptance criteria before implementing a feature.
- Self-verify with docs/quality.md checklist after completing work.
- Record progress in claude-progress.md.
```

## Agent-specific adaptations

After generating the universal template, add or adjust these sections based on the target:

### Claude Code (`CLAUDE.md`)
- No extra sections needed beyond the universal template.
- Can reference hooks system if the project uses Claude Code hooks.
- Config file: `.claude/settings.json` (optional).

### OpenAI Codex (`AGENTS.md`)
- Add a **"CI Invariants"** section listing rules enforced by CI:
  ```markdown
  ## CI Invariants
  - Formatting: `{command}` must pass
  - Tests: `{command}` must pass
  - Type check: `{command}` must pass
  ```
- Emphasize **one-command boot** in Quick Start (Codex runs in sandboxes).
- Write core constraints in **English** (Codex performs more reliably with English).
- Config file: `codex.json` (optional).

### Gemini (`GEMINI.md`)
- Structure is the same as the universal template.
- Config directory: `.gemini/` (optional).

### Cursor (`.cursorrules`)
- Format is a single flat file (no markdown headers hierarchy).
- Combine the most critical rules from `conventions.md` and `api-design.md` inline, since Cursor loads the entire file as a single context block.
- Keep it concise — Cursor's rules file works best under ~200 lines.

### Windsurf (`.windsurfrules`)
- Similar to `.cursorrules` in format.
- Single flat file with rules and conventions.

### Other / Unknown agents
- Use the universal template as-is with `{AGENT_NAME}.md` as the file name.
- Most agents that support SKILL.md or markdown-based context files will work with this format.

## Writing tips

1. **Target ~100 lines.** If it's getting longer, move content to `docs/`.

2. **Commands must be copy-pasteable.** Not "start the server appropriately" — the exact command.

3. **Constraints need a "why."** "Separate DTO and Entity" → "Separate DTO and Entity (so API responses don't break when the DB schema changes)."

4. **Project structure tree should reflect reality.** If no code exists yet, mark it as "target structure."

5. **Don't duplicate docs/.** The entry file points to docs/. It does not repeat what's already there.
