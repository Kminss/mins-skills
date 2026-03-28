# conventions.md generation guide

## Template

```markdown
# Coding Conventions

## General Principles

- {2–3 project-wide principles}
- Example: "Explicit is better than implicit", "A little duplication beats the wrong abstraction"

## Naming Rules

### Files & Directories

| Target | Convention | Example |
|--------|-----------|---------|
| Directories | {kebab-case / camelCase} | order-service |
| Source files | {PascalCase / snake_case} | OrderService.java |
| Test files | {convention} | OrderServiceTest.java |

### Code

| Target | Convention | Example |
|--------|-----------|---------|
| Classes/Types | PascalCase | OrderService, UserDto |
| Functions | {camelCase / snake_case} | createOrder / create_order |
| Variables | {camelCase / snake_case} | orderCount / order_count |
| Constants | UPPER_SNAKE_CASE | MAX_RETRY_COUNT |
| DB tables | snake_case | user_orders |
| DB columns | snake_case | created_at |
| API paths | kebab-case, plural nouns | /api/v1/order-items |

## Code Structure

### Functions

- One function, one job.
- Max length: {recommended, e.g., 40 lines}.
- Max parameters: {recommended, e.g., 4}. Beyond that, wrap in an object.
- Always specify return types (use type hints in dynamic languages).

### Error Handling

- {Project's error handling strategy}
- Example: "Business exceptions extend a custom BusinessException class"
- Example: "External API calls are always wrapped in try-catch with a fallback"

## Git Conventions

### Branch Strategy

- main: production
- develop: integration
- feature/{name}: feature work
- fix/{issue}: bug fixes

### Commit Messages

Follow Conventional Commits:
- `feat(order): add order cancellation API`
- `fix(auth): handle expired refresh tokens`
- `refactor(payment): extract gateway interface`

## Environment Variables

- Never commit `.env` files.
- Maintain `.env.example` with all required keys and descriptions.
- Naming: UPPER_SNAKE_CASE with purpose prefix (DB_HOST, JWT_SECRET, AWS_REGION).

## Stack-Specific References

For detailed conventions specific to this project's stack,
install the relevant community skills:

Install:
  npx skills add anthropics/skills

Recommended:
- {skill name} — {one-line why}
```

## Principles

1. **Prefer mechanically verifiable rules.** Rules that a linter or formatter can enforce are the most effective.

2. **Include the "why" for each rule.** Agents that understand the reasoning make better judgment calls in edge cases.

3. **Always include examples.** "Good example" vs "Bad example" side-by-side is the most effective format.

4. **Stack-specific details go to community skills.** Keep this file focused on project-level decisions. Detailed framework rules (Lombok usage, Pydantic patterns, React hook rules) are better maintained as separate, purpose-built skills.
