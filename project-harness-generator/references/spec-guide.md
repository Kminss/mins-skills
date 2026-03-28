# PROJECT-SPEC.md generation guide

## Template

```markdown
# {Project Name}

## Overview

{Expand the one-liner into 2–3 paragraphs: what we're building, why, and who uses it.}

## Tech Stack

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| Backend | {e.g., Spring Boot 3.x, Java 17} | {why} |
| Frontend | {e.g., Next.js 14, TypeScript} | {why} |
| Database | {e.g., PostgreSQL 15} | {why} |
| Cache | {e.g., Redis 7} | {if needed} |
| Infra | {e.g., AWS EC2, RDS, S3} | {environment} |
| CI/CD | {e.g., GitHub Actions} | {pipeline} |

## Data Model (high-level)

Core entities and relationships only. Column-level details are decided during implementation.

- {Entity A} ↔ {Entity B}: {relationship}
- {Entity B} ↔ {Entity C}: {relationship}

## Features

### Sprint 1: {Theme — foundation + core feature}

#### F1. {Feature name}
**User story:** As a {role}, I want to {action} so that {value}.
**Acceptance criteria:**
- {Specific, testable condition 1}
- {Specific, testable condition 2}

#### F2. {Feature name}
...

### Sprint 2: {Theme}
...

### Sprint 3: {Theme}
...

## Non-functional Requirements

- **Performance:** {concrete numbers, e.g., API response < 200ms at p95}
- **Security:** {auth method, encryption, etc.}
- **Scalability:** {expected traffic, scaling strategy}

## Out of Scope

- {What we're NOT building in this version — 1}
- {What we're NOT building in this version — 2}
```

## Principles

1. **Structure by sprints.** Sprint 1 = foundation (project setup, DB, auth) + 1–2 core features. Subsequent sprints expand incrementally. 3–5 features per sprint.

2. **Acceptance criteria must be testable.** Bad: "Users can log in easily." Good: "POST /auth/login with valid credentials returns a JWT; expired tokens return 401 with a refresh hint."

3. **Keep technical design high-level.** Describe package structure and component relationships. Leave class design, method signatures to implementation. If the planner over-specifies and gets something wrong, errors cascade downstream.

4. **Ambitious but structured scope.** Start from core features, expand outward. "Nice to have" goes in the last sprint or out of scope.
