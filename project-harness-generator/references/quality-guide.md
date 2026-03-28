# quality.md generation guide

Inspired by Anthropic's Generator-Evaluator pattern: agents grade their own work too generously. Pre-defined, concrete evaluation criteria counteract this bias.

## Template

```markdown
# Quality Standards

## Evaluation Axes

All work is evaluated on four axes. Each has a concrete checklist and a pass/fail threshold.

### 1. Feature Completeness

Evaluated against PROJECT-SPEC.md acceptance criteria.

- [ ] All specified acceptance criteria are implemented
- [ ] Happy path works end-to-end
- [ ] Key error paths are handled
- [ ] If there's UI, users can complete the core task
- [ ] Data is correctly saved and retrieved

**Pass threshold:** ≥ 90% of acceptance criteria met.

### 2. Code Quality

Evaluated against conventions.md.

- [ ] Naming rules followed
- [ ] Function length / parameter limits respected
- [ ] No duplicate code (extract if repeated 3+ times)
- [ ] Error handling follows the project's strategy
- [ ] No unnecessary comments (code is self-explanatory)
- [ ] No hardcoded values (use constants or config)

**Pass threshold:** All items met.

### 3. API Compliance

Evaluated against api-design.md.

- [ ] Unified response format used
- [ ] HTTP status codes are correct
- [ ] URL naming rules followed
- [ ] Error responses include proper error codes
- [ ] Validation happens at request DTO level
- [ ] Pagination applied to list APIs where needed

**Pass threshold:** All items met.

### 4. Test Coverage

- [ ] Unit tests exist for core business logic
- [ ] Integration tests exist for API endpoints
- [ ] Tests actually run and pass
- [ ] Edge cases tested (empty values, boundaries, unauthorized access)

**Pass threshold:** Core logic tests exist + all pass.

---

## Sprint Contract Template

Before starting work, the generator and evaluator (or developer and reviewer) agree on "what done looks like."

```yaml
sprint: {number}
features:
  - name: "{feature name}"
    description: "{one-line summary}"
    done_criteria:
      - "{specific, testable condition 1}"
      - "{specific, testable condition 2}"
      - "{specific, testable condition 3}"
    verification: "{manual / unit_test / integration_test / e2e}"
```

### Writing rules for done criteria

1. **Start with a verb.** Good: "Login API returns JWT for valid credentials." Bad: "Login feature."

2. **Specify the verification method.** manual = browser/API client check. unit_test = unit test. integration_test = integration test. e2e = Playwright or similar.

3. **One verification point per criterion.** "Login, see dashboard, edit profile" → split into three.

---

## Self-Verification Process

After completing work, verify in this order:

1. **Build:** Does it compile/build?
2. **Tests:** Do all tests pass?
3. **Lint:** No lint/format warnings?
4. **Functionality:** Walk through acceptance criteria one by one.
5. **Checklist:** Check each of the 4 evaluation axes above.

---

## Progress Tracking (claude-progress.md)

For context resets — so the next agent session can pick up where the previous one left off.

```markdown
# Progress

## Last updated: {date/time}

## Completed
- [x] {completed task 1}
- [x] {completed task 2}

## In Progress
- {current task}
- Status: {progress / blockers}

## Next Up
- [ ] {next task 1}
- [ ] {next task 2}

## Known Issues
- {discovered bugs or problems}

## Decisions Made
- {technical decisions and their reasoning}
```
```

## Principles

1. **Decompose "does it work?" into concrete criteria.** Abstract evaluation is why agents are too generous with themselves. 4 axes × concrete checklists prevent this.

2. **Sprint contracts are written BEFORE work begins.** Post-hoc criteria get adjusted to fit what was already built.

3. **Progress tracking exists for context resets.** When an agent hits a context window limit and resets, this file lets the next session pick up quickly.

4. **Automate what you can.** Build → tests → lint in sequence as automated verification. Manual checks come last.
