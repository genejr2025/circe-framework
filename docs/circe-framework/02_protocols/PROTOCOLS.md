# CIRCE Protocols

**Version**: 1.0
**Source**: CIRCE Technical Specification Section 3.5
**Last Updated**: 2025-11-21

---

## Overview

CIRCE protocols are **enforced behaviors** that raise reliability and quality beyond best-effort prompting. These are not suggestions—they are requirements that agents must follow.

---

## Core Protocols

### 1. Listen-to-User Protocol

**Principle**: Human guidance always takes priority over AI initiative.

**Requirements**:
- **Always address explicit human guidance FIRST**
- Confirm understanding before taking action
- Clarify ambiguities before proceeding
- Never assume human intent—ask questions

**Implementation**:
```
1. Check for human messages in priority order
2. Parse human intent and requirements
3. Confirm understanding: "I understand you want [X]. I will [Y]."
4. Execute only after confirmation or implicit approval
5. Report back with results and next steps
```

**Violations**:
- ❌ Implementing features not requested
- ❌ Changing scope without confirmation
- ❌ Ignoring explicit instructions
- ❌ Proceeding despite unclear requirements

**Example**:
```markdown
Human: "Add authentication to the API"

Agent Response:
"I understand you want to add authentication to the API. Before I proceed:
1. What authentication method? (JWT, OAuth, Basic Auth?)
2. Which endpoints need protection? (All, or specific routes?)
3. User management: Existing database or new implementation?

Please clarify so I can implement exactly what you need."
```

---

### 2. Testing Protocol

**Principle**: Tasks cannot be marked "✅ complete" until automated tests pass.

**Requirements**:
- **Write tests FIRST** (TDD approach)
- All code tasks must have test coverage
- Minimum 90% coverage for new code
- Integration tests for critical paths
- Tests must pass before declaring completion

**Test Gates**:
```bash
# Required test gates
1. Unit tests: pytest -q --maxfail=1
2. Coverage: coverage run -m pytest && coverage report --fail-under=90
3. Linting: ruff check . && mypy --strict
4. Integration tests: pytest tests/integration/
```

**Workflow**:
```
1. Receive task
2. Write test cases FIRST (based on acceptance criteria)
3. Verify tests FAIL (red)
4. Implement feature
5. Run tests until they PASS (green)
6. Refactor if needed (maintain green)
7. Run full test suite
8. If ANY test fails: diagnose, fix, re-run FULL suite
9. Only mark complete when ALL tests pass
```

**Failing Tests = Task Not Complete**:
- If tests fail, task status remains "in_progress"
- Create diagnostic report
- Fix and re-run FULL test suite
- Never skip failing tests

**Example Enforcement Script**:
```bash
#!/usr/bin/env bash
# enforce-testing.sh
set -euo pipefail

echo "[CIRCE] Running full test suite..."
pytest -q --maxfail=1 || { echo "❌ Tests failed"; exit 1; }

echo "[CIRCE] Checking coverage..."
coverage run -m pytest && coverage report --fail-under=90 || { echo "❌ Coverage below 90%"; exit 1; }

echo "[CIRCE] Linting..."
ruff check . || { echo "❌ Linting failed"; exit 1; }
mypy --strict . || { echo "❌ Type checking failed"; exit 1; }

echo "[CIRCE] ✅ All gates passed. Task may be marked complete."
```

---

### 3. Coordination Protocol

**Principle**: Avoid conflicting edits and wasted work through explicit coordination.

**Requirements**:
- **Declare ownership** before starting work
- Check for existing work before claiming tasks
- Post regular status updates
- Coordinate on shared resources
- Release tasks if blocked or unable to complete

**Coordination Steps**:
```
1. Check TASKS/ for similar or conflicting work
2. Claim task explicitly (create taskid.owner file)
3. Post initial status update
4. Work in personal WORKSPACE/
5. Post progress updates (at least hourly for active work)
6. Before modifying shared files, check for locks
7. Post completion message
8. Release ownership
```

**Ownership Files**:
```yaml
# TASKS/AUTH-012.owner
owner: engineer
claimed_at: 2025-11-21T17:00:00Z
estimated_completion: 2025-11-21T20:00:00Z
status_url: /AGENTS/engineer/WORKSPACE/AUTH-012-progress.md
```

**Conflict Avoidance**:
- Never edit files in another agent's WORKSPACE/
- Use NEW_MESSAGES/ for work requests
- Coordinate on shared resources (database, configs, etc.)
- If conflict detected, escalate immediately

**Example**:
```markdown
# Before starting work
Engineer checks: ls TASKS/ | grep -i "auth"
Found: AUTH-012.owner (owned by: different_engineer)

Engineer posts message:
"@coordinator I see AUTH-012 is claimed by different_engineer.
Should I work on AUTH-013 instead, or coordinate with them?"
```

---

### 4. Boundary Protocols

**Principle**: Respect privacy, safety, and scope policies.

**Safety Boundaries**:
- Never execute destructive commands without confirmation
- Never access files outside permitted paths
- Never make external network calls without approval
- Never commit credentials or secrets

**Privacy Boundaries**:
- Only access files within agent workspace
- Don't read other agents' INBOX/ or WORKSPACE/ without permission
- Log all file access for audit trail

**Scope Boundaries**:
- Stay within assigned task scope
- Don't refactor unrelated code
- Don't add unrequested features
- Focus on acceptance criteria

**Permission Model** (from Appendix D):
```yaml
actions:
  os_read_file:
    allowed: true
    paths: ["/workspace/AI_MANAGER/**"]
  os_write_file:
    allowed: true
    paths: ["/workspace/AI_MANAGER/AGENTS/engineer/WORKSPACE/**"]
  os_delete_file:
    allowed: false
  net_http_post:
    allowed: false
gates:
  requires_human_review:
    - "modify_production_config"
    - "delete_data"
    - "external_api_calls"
```

**When to Refuse**:
- Requests for unsafe operations
- Tasks outside scope
- Ambiguous or unclear instructions
- Violations of security policies

**Refusal Template**:
```markdown
I cannot complete this request because [reason].

This violates [protocol/policy].

Suggested alternatives:
1. [Safe alternative 1]
2. [Safe alternative 2]

Please clarify or approve override if this is intentional.
```

---

## Protocol Enforcement

### Automated Enforcement
- Pre-commit hooks for code quality
- CI/CD gates for testing protocol
- File permissions for boundary protocol
- Audit logs for coordination protocol

### Manual Review
- Human spot-checks of agent work
- EAGLE watchdog audit (adversarial review)
- Periodic protocol compliance reports

### Violations
- Log all protocol violations
- Automatic escalation to ROOSTER
- Learning loop: update protocols as needed

---

## Protocol Evolution

Protocols are living documents:
1. **Observe**: Monitor agent behavior and outcomes
2. **Identify**: Find patterns of success and failure
3. **Codify**: Turn successful patterns into protocols
4. **Enforce**: Implement automated checks
5. **Iterate**: Refine based on experience

---

## Summary Table

| Protocol | Enforces | Check Method |
|----------|----------|--------------|
| **Listen-to-User** | Human authority | Human review, escalation patterns |
| **Testing** | Quality gates | Automated test suite, coverage reports |
| **Coordination** | Avoid conflicts | Ownership files, status updates |
| **Boundary** | Safety & scope | Permissions, audit logs, human review |

---

**Status**: Complete
**Next**: See MESSAGE_FORMATS.md for communication schemas
**Related**: TESTING_PROTOCOL.md for detailed testing procedures
