# architecture.md generation guide

## Template

```markdown
# Architecture

## System Overview

{One paragraph describing what components exist and how they connect.}

## Domain Map

| Domain | Responsibility | Core Entities |
|--------|---------------|---------------|
| {domain 1} | {what it owns} | {key entities} |
| {domain 2} | {what it owns} | {key entities} |

## Package / Module Layering

### Backend

```
src/
├── domain/           # Business logic, entities, domain services
│   ├── {domain-1}/
│   └── {domain-2}/
├── application/      # Use cases, application services
├── infrastructure/   # DB, external APIs, messaging implementations
├── presentation/     # Controllers, request/response DTOs
└── config/           # Configuration, security, bean registration
```

### Frontend (if applicable)

```
src/
├── app/              # Routing, pages
├── components/
│   ├── common/       # Shared components
│   └── {domain}/     # Domain-specific components
├── hooks/            # Custom hooks
├── services/         # API calls
├── stores/           # State management
└── types/            # Type definitions
```

## Dependency Direction

{Which layers can reference which. This is the single most useful rule for agents writing new code.}

- presentation → application → domain ✅
- domain → infrastructure ❌ (dependency inversion)
- infrastructure → domain ✅ (implements interfaces)

## External Integrations

| Service | Purpose | Method |
|---------|---------|--------|
| {service} | {why} | {REST API / SDK / messaging} |

## Key Data Flows

{Describe 1–2 critical use cases end-to-end.}

**Example: Create Order**
1. Client → `POST /api/orders` (OrderController)
2. OrderController → OrderService.createOrder()
3. OrderService → OrderRepository.save() + PaymentGateway.charge()
4. Result → OrderResponse DTO → client
```

## Principles

1. **A new team member (or agent) should grasp the full picture in 10 minutes.** Focus on the big picture, not code details.

2. **Organize by domain.** Explain in terms of business domains (order, user, payment), not technical layers (controller, service, repository).

3. **Make dependency direction explicit.** This is the primary guide agents use to decide which module can import which.

4. **Adapt to the tech stack.** Spring Boot: domain-based packages + layer separation. FastAPI: router/schema/service/model. Next.js: App Router directory structure.
