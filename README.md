# project-harness-generator

Generate a complete agent-ready project harness from a short project description.

Give a one-line idea → get `PROJECT-SPEC.md` and a full `docs/` structure (architecture, conventions, API design, QA checklist with sprint contracts) — ready to drop into your repo so coding agents can start working immediately.

Optionally, request an agent entry file for any tool (`CLAUDE.md`, `AGENTS.md`, `GEMINI.md`, `.cursorrules`, etc.) and it will be generated as a ~100-line table of contents pointing to the docs.

## Why?

The same AI model produces wildly different results depending on its working environment. This insight comes from [Anthropic's harness design research](https://www.anthropic.com/engineering/harness-design-long-running-apps) and [OpenAI's harness engineering post](https://openai.com/index/harness-engineering/): **the harness determines the outcome, not the model.**

This skill automates the creation of that harness — the project documents that tell agents *how* to work in your codebase.

## What it generates

**Core outputs (always):**

```
your-project/
├── PROJECT-SPEC.md       # Feature list, data model, sprint plan
└── docs/
    ├── architecture.md   # System overview, domain map, dependency rules
    ├── conventions.md    # Naming, structure, git conventions
    ├── api-design.md     # Response format, status codes, error codes
    └── quality.md        # 4-axis QA checklist, sprint contract template
```

**Agent entry file (on request):**

```
your-project/
├── CLAUDE.md             # For Claude Code
├── AGENTS.md             # For OpenAI Codex
├── GEMINI.md             # For Gemini
├── .cursorrules          # For Cursor
└── .windsurfrules        # For Windsurf
```

You can request one, several, or none — the core docs work with any agent as-is.

## Install

```bash
# Via skills CLI (works with Claude Code, Codex, Gemini CLI, and more)
npx skills add {your-github-username}/project-harness-generator

# Or copy directly
git clone https://github.com/{your-github-username}/project-harness-generator.git
cp -r project-harness-generator ~/.claude/skills/
```

## Usage

```
> Bootstrap a project harness for an e-commerce order management API using Spring Boot + PostgreSQL, deployed on AWS
```

The skill generates the core docs. Then optionally:

```
> Also generate a CLAUDE.md and AGENTS.md for this project
```

Or all at once:

```
> Set up project docs for a real-time chat app with FastAPI + Next.js. Include CLAUDE.md and .cursorrules
```

## Key design decisions

- **Core docs are agent-agnostic.** `PROJECT-SPEC.md` and `docs/` work with any AI tool without modification.
- **Entry files are optional and multi-agent.** Request `CLAUDE.md`, `AGENTS.md`, `GEMINI.md`, `.cursorrules`, or any combination — only when you need them.
- **Entry file = table of contents, not encyclopedia.** ~100 lines, pointing to `docs/`. Details never duplicated.
- **Separate generation from evaluation.** `quality.md` defines concrete QA criteria *before* work begins — inspired by Anthropic's Generator-Evaluator pattern.
- **Stack-specific details are delegated.** Basic conventions are included inline; detailed framework rules are referenced via community skills from [skills.sh](https://skills.sh), keeping the harness lean and maintainable.

## Compatibility

Core docs (`PROJECT-SPEC.md` + `docs/`) work with **any** AI coding agent.

Optional entry files:

| Agent | Entry file | Status |
|-------|-----------|--------|
| Claude Code | `CLAUDE.md` | ✅ Supported |
| OpenAI Codex | `AGENTS.md` | ✅ Supported |
| Gemini | `GEMINI.md` | ✅ Supported |
| Cursor | `.cursorrules` | ✅ Supported |
| Windsurf | `.windsurfrules` | ✅ Supported |
| Others | `{AGENT}.md` | ✅ Generic template |

## Contributing

Issues and PRs welcome. If you find a pattern that makes agents work better in a specific stack, consider contributing it as a reference guide.

## License

MIT
