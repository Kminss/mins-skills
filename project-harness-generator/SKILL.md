---
name: project-harness-generator
description: >
  Generate a complete agent-ready project harness from a short project description.
  Outputs PROJECT-SPEC.md and docs/ (architecture, conventions, api-design, quality
  checklist with sprint contracts). Optionally generates an agent entry file for any
  tool: CLAUDE.md, AGENTS.md, .cursorrules, GEMINI.md, etc. Use when the user says
  "bootstrap project", "set up project harness", "generate project docs",
  "project scaffolding docs", "initialize project structure", "create dev guidelines",
  "generate CLAUDE.md", "create AGENTS.md", or any request to produce agent working
  documents for a new project.
---

# Project Harness Generator

Generate a full set of agent-ready project documents from a brief project description.

**Core philosophy** — drawn from Anthropic's and OpenAI's harness engineering research:

- **The harness determines the outcome, not the model.** The same model produces wildly different results depending on the environment it works in.
- **If it's not in the repo, it doesn't exist.** Slack threads, mental decisions, Google Docs — agents can't see any of it.
- **The entry file is a table of contents, not an encyclopedia.** Keep it ~100 lines; put details in `docs/`.
- **Separate generation from evaluation.** Agents grade their own work too generously. Define QA criteria upfront.

---

## Workflow

### Step 1 — Gather project info

Support two modes:

**A) Interview mode** — when the user gives a short description.
Ask questions conversationally. Never ask more than 3 at a time.

Required:
- One-line project summary (what are we building?)
- 3–5 core features
- Tech stack (backend, frontend, DB, infra)
- Target users
- Deployment environment (AWS, GCP, on-prem, etc.)

Optional (ask when relevant):
- External APIs or services
- Auth strategy
- Performance requirements
- Pre-decided conventions or constraints
- Agent entry file — ask: "Do you want an entry file for a specific agent? (e.g., CLAUDE.md, AGENTS.md, GEMINI.md, .cursorrules)" Only generate if requested.

**B) Direct input mode** — when the user provides detailed info upfront.
Extract the required items from what's given. Ask only about what's missing.
Do **not** assume an agent entry file is needed unless explicitly requested.

### Step 2 — Generate outputs

Read the corresponding guide in `references/` before generating each file.

**Core outputs (always generated):**

```
<project>/
├── PROJECT-SPEC.md
└── docs/
    ├── architecture.md
    ├── conventions.md
    ├── api-design.md
    └── quality.md
```

#### Output Contract (STRICT)

Each file MUST contain the sections listed below. The `references/` guides provide detailed templates and examples; the sections here are the **enforced minimum**.

**PROJECT-SPEC.md** — read `references/spec-guide.md` first.
- Overview (what, why, who)
- Tech Stack table (layer / technology / rationale)
- Data Model — core entities and relationships
- Features — organized by sprint, each with user story + testable acceptance criteria
- Non-functional Requirements — with concrete numbers
- Out of Scope

**docs/architecture.md** — read `references/architecture-guide.md` first.
- System Overview (one-paragraph summary)
- Domain Map table (domain / responsibility / core entities)
- Package/Module Layering (directory tree per layer)
- Dependency Direction rules (what can import what)
- External Integrations table
- Key Data Flows (1–2 critical use cases end-to-end)

**docs/conventions.md** — read `references/conventions-guide.md` first.
- Naming Rules tables (files, code, DB, API paths)
- Code Structure rules (function length, parameter limits, error handling)
- Git Conventions (branch strategy, commit message format)
- Environment Variables rules
- Stack-specific References section (pointers to community skills)

**docs/api-design.md** — read `references/api-design-guide.md` first.
- Response Format (success, list/paginated, error — with JSON examples)
- HTTP Status Code table
- URL Design rules
- DTO Naming table
- Validation strategy with error response example
- Error Code System (domain-prefixed codes)
- Authentication section

**docs/quality.md** — read `references/quality-guide.md` first.
- 4 Evaluation Axes, each with checklist and pass/fail threshold:
  1. Feature Completeness
  2. Code Quality
  3. API Compliance
  4. Test Coverage
- Sprint Contract Template (YAML format)
- Self-Verification Process (build → test → lint → manual)
- Progress Tracking format (claude-progress.md template)

**Agent entry file (only when requested):**

If the user asks for an entry file, generate it for the specified agent(s). Read `references/entry-file-guide.md` first.

The entry file MUST contain:
- Project one-liner
- Tech Stack
- Quick Start commands (copy-pasteable)
- Project Structure tree
- Documentation links (to each docs/ file)
- Core Constraints (3–5, each with a "why")
- Work Process

| Agent | File | Adaptation |
|-------|------|------------|
| Claude Code | `CLAUDE.md` | Standard template |
| OpenAI Codex | `AGENTS.md` | Add CI Invariants section; emphasize one-command boot |
| Gemini | `GEMINI.md` | Standard template |
| Cursor | `.cursorrules` | Flat format, inline key rules, ≤200 lines |
| Windsurf | `.windsurfrules` | Flat format |
| Other | `{AGENT}.md` | Standard template |

### Step 3 — Self-evaluate (MANDATORY)

After generating all files, verify your own output before presenting to the user. Check every file against this list:

1. **Completeness** — Does each file contain ALL sections listed in the Output Contract above? List any missing sections.
2. **Specificity** — Are there vague phrases like "appropriate method", "good practices", "as needed"? Replace each with a concrete rule or example.
3. **Testability** — Can every acceptance criterion in PROJECT-SPEC.md be verified with a clear pass/fail? Rewrite any that can't.
4. **Consistency** — Do conventions.md rules align with api-design.md patterns? Do architecture.md dependency rules match the directory structure?
5. **Stack fit** — Are naming conventions, directory structures, and test frameworks correct for the specified tech stack?

If any issues are found, fix them before proceeding. Do not present incomplete output to the user.

### Step 4 — Present and iterate

Present the generated files to the user. Incorporate feedback and regenerate as needed.

---

## Generation guides

Each output file has a dedicated guide in `references/`. **Always read the guide before generating the file.**

| Output | Guide | Key principle |
|--------|-------|---------------|
| `PROJECT-SPEC.md` | `references/spec-guide.md` | Act as a Planner: expand a one-liner into features, data model, and sprint plan. Stay high-level — over-specifying implementation cascades errors downstream. |
| `docs/architecture.md` | `references/architecture-guide.md` | System overview a new team member (or agent) can grasp in 10 minutes. Domain map, package layering, dependency direction, data flow for key use cases. |
| `docs/conventions.md` | `references/conventions-guide.md` | Concrete, mechanically verifiable rules. Good/bad examples for each. Stack-specific details are delegated to community skills — include a reference section at the bottom. |
| `docs/api-design.md` | `references/api-design-guide.md` | Unified response format, HTTP status code table, URL naming rules, DTO naming patterns, error code system, validation strategy. |
| `docs/quality.md` | `references/quality-guide.md` | 4-axis evaluation (feature completeness, code quality, API compliance, test coverage) with checklists. Sprint contract template. Progress tracking format for context resets. |
| Agent entry file | `references/entry-file-guide.md` | *(Optional)* ~100-line table of contents pointing to docs/. Adapted per agent: CLAUDE.md, AGENTS.md, GEMINI.md, .cursorrules, etc. |

---

## Tech-stack customization

### Baseline rules (apply directly)

- **Naming**: Follow the language's official style guide (Java → camelCase methods, Python → snake_case functions, JS/TS → camelCase).
- **Directory structure**: Organize by business domain. Yield to framework convention when it's strong.
- **Testing**: Name the stack's standard test framework (JUnit 5, pytest, Vitest, etc.).
- **Error handling**: Specify a single global exception handling pattern.

### Detailed stack rules — delegate to community skills

Do **not** bake detailed stack-specific conventions (Spring Boot DTO patterns, FastAPI Pydantic rules, Next.js App Router structure) into the generated docs. Instead, add a reference section at the bottom of `conventions.md`:

```markdown
## Stack-specific references

For detailed conventions for this project's stack, install the relevant community skills.
Skills work across Claude Code, Codex CLI, Gemini CLI, and other agents.

Install (Claude Code):
  npx skills add anthropics/skills
  npx skills add {recommended-marketplace}

Recommended:
- {skill name} — {one-line reason for this project}
```

When recommending skills, search the major directories:
- `anthropics/skills` — Official Anthropic skills
- `skills.sh` — Vercel's open ecosystem (7,000+ skills)
- `skillsmp.com` — Aggregator (500,000+ skills)

---

## Important guidelines

1. **Avoid over-specification.** Constraining what the agent can already judge well hurts performance. Specify only what truly matters; leave the rest at principle level.

2. **Prefer mechanically verifiable rules.** "Clean code" is vague. "Functions ≤ 40 lines, ≤ 4 parameters" can be enforced by CI.

3. **Every decision goes into a file.** Anything decided in conversation must be reflected in the docs. If it's not in the repo, it doesn't exist for the agent.

4. **Harnesses evolve with models.** The generated docs are a starting point. As the project progresses, remove constraints that are no longer load-bearing and add new ones for emerging needs.

---

## Scope boundary

This skill generates the **foundational documentation set** for a harness-ready project. It produces documents that are decomposable into tasks — features with sprint structure and testable acceptance criteria — but does NOT generate the tasks themselves.

**This skill does NOT generate:**
- Runtime orchestration, task queues, or agent routing logic
- Execution state artifacts (`tasks.json`, work assignment files)
- Actual code, directory scaffolding, or boilerplate files
- CI/CD pipeline configuration
- Dependency installation or build tool setup

Task generation, agent role assignment, and execution loops belong to **separate downstream workflows or skills**. This skill's responsibility ends at producing a document set that makes those downstream steps possible.
