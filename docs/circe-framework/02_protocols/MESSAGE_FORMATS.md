# CIRCE Message Formats

**Version**: 1.0
**Source**: CIRCE Technical Specification Section 7.1 & Appendix A
**Last Updated**: 2025-11-21

---

## Overview

CIRCE uses structured Markdown files as the message format for all inter-agent communication. This provides human-readability, version control compatibility, and zero-dependency simplicity.

---

## Message Schema (Markdown)

### Standard Message Format

```markdown
# MESSAGE
from: coordinator
to: engineer
intent: implement_feature
task_id: FEAT-021
status: new
created_at: 2025-11-06T14:02:11Z

## Summary
Add API endpoint /api/tasks with pagination and auth.

## Acceptance Criteria
- Unit + integration tests written first
- 95% coverage for module
- Pass `./enforce-testing.sh --full`

## Context
[Additional context, links, references]

## Artifacts
[Links to related files, previous messages, etc.]
```

### Required Headers

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `from` | string | Yes | Sender agent ID |
| `to` | string | Yes | Recipient agent ID (or "all" for broadcast) |
| `intent` | string | Yes | Message purpose (see intents below) |
| `task_id` | string | Conditional | Required if related to a task |
| `status` | string | Yes | new, in_progress, completed, blocked, failed |
| `created_at` | ISO 8601 | Yes | Timestamp in UTC |

### Standard Intents

**Task Management**:
- `implement_feature` - Request to build new functionality
- `fix_bug` - Request to resolve issue
- `refactor` - Request to improve code quality
- `review_code` - Request for peer review
- `test` - Request for testing/QA

**Coordination**:
- `status_update` - Share progress on task
- `request_help` - Ask for assistance
- `claim_task` - Declare ownership of work
- `release_task` - Free up task for others
- `coordinate` - Synchronize work between agents

**Communication**:
- `question` - Ask clarifying question
- `answer` - Respond to question
- `escalate` - Request human involvement
- `inform` - Share information

**Quality**:
- `test_results` - Share testing outcomes
- `audit_report` - Share audit findings (EAGLE)
- `approval_request` - Request approval/sign-off

---

## Message Filename Convention

**Format**: `YYYYMMDD_HHMMSS_from-to_intent_taskid.md`

**Examples**:
```
20251106_140211_coordinator-engineer_implement-feature_FEAT-021.md
20251106_153022_engineer-qa_test_FEAT-021.md
20251106_164455_qa-coordinator_test-results_FEAT-021.md
```

**Benefits**:
- Chronological sorting by default
- Clear sender/recipient from filename
- Intent visible without opening file
- Task tracking built into filename

---

## Message Locations

### NEW_MESSAGES/
- **Purpose**: Unclaimed messages and directives
- **Cleared by**: Agents claiming/acknowledging messages
- **Retention**: Move to LOGS/ after acknowledgment

### AGENTS/{agent_id}/INBOX/
- **Purpose**: Messages directed to specific agent
- **Cleared by**: Agent after processing
- **Retention**: Move to WORKSPACE/ or LOGS/ after handling

### AGENTS/{agent_id}/WORKSPACE/
- **Purpose**: Active work, drafts, work-in-progress
- **Cleared by**: Agent upon task completion
- **Retention**: Keep for session, archive on completion

### LOGS/
- **Purpose**: Permanent record of all messages
- **Cleared by**: Never (or by archival policy)
- **Retention**: Indefinite, with optional summarization

---

## Task State Files

Stored in `TASKS/` directory alongside task definitions.

### taskid.status
```yaml
task_id: FEAT-021
status: in_progress
owner: engineer
started_at: 2025-11-06T14:05:00Z
updated_at: 2025-11-06T15:30:00Z
```

### taskid.owner
```yaml
owner: engineer
claimed_at: 2025-11-06T14:05:00Z
```

### taskid.checks
```yaml
tests_passed: false
coverage_threshold_met: false
peer_reviewed: false
ready_for_merge: false
```

---

## Examples

### Example 1: Feature Implementation Request

```markdown
# MESSAGE
from: coordinator
to: engineer
intent: implement_feature
task_id: AUTH-012
status: new
created_at: 2025-11-21T17:00:00Z

## Summary
Implement JWT-based authentication for API endpoints

## Acceptance Criteria
- JWT token generation and validation
- Middleware for protected routes
- Token refresh mechanism
- Unit tests with 90%+ coverage
- Integration tests for auth flow
- Documentation for API consumers

## Context
- Use existing User model from database
- Token expiry: 1 hour (access), 7 days (refresh)
- Follow security best practices (OWASP)

## Artifacts
- Related: /docs/api/authentication.md
- Reference: /examples/jwt-implementation/
```

### Example 2: Test Results Report

```markdown
# MESSAGE
from: qa
to: engineer
intent: test_results
task_id: AUTH-012
status: completed
created_at: 2025-11-21T19:30:00Z

## Summary
Test results for AUTH-012 (JWT Authentication)

## Results
✅ Unit Tests: 47/47 passed (94% coverage)
✅ Integration Tests: 12/12 passed
✅ Security Tests: 8/8 passed
⚠️  Performance Tests: 4/5 passed (1 timeout at 5000 concurrent users)

## Issues Found
1. Token refresh endpoint times out under heavy load
   - Severity: Medium
   - Recommendation: Add caching layer for token validation

## Approval Status
✅ Ready for deployment with performance optimization noted

## Artifacts
- Test report: /logs/test-results-AUTH-012.html
- Coverage report: /logs/coverage-AUTH-012/
```

### Example 3: Human Escalation

```markdown
# MESSAGE
from: engineer
to: coordinator
intent: escalate
task_id: AUTH-012
status: blocked
created_at: 2025-11-21T18:15:00Z

## Summary
Need human decision on security trade-off

## Issue
Current JWT implementation uses RS256 (RSA) for signatures.
Alternative HS256 (HMAC) would be faster but less secure for distributed systems.

## Options
1. **RS256 (current)**: Slower, more secure, supports distributed validation
2. **HS256**: Faster, shared secret, only auth server can validate

## Request
ROOSTER please advise on security vs performance priority for this use case.

## Context
- Current: 200ms per token validation
- With HS256: Est. 50ms per token validation
- Expected load: 1000-5000 requests/min

## Recommendation
Engineer suggests RS256 for security, with Redis caching for performance.
```

---

## Best Practices

### For Senders
1. **Be specific**: Clear intent and acceptance criteria
2. **Include context**: Link to relevant docs/code
3. **Set expectations**: Timeframes, priorities, blockers
4. **Reference artifacts**: Make it easy to find related work

### For Recipients
1. **Acknowledge promptly**: Claim tasks quickly
2. **Update status**: Regular progress updates
3. **Ask questions**: Escalate blockers early
4. **Document outcomes**: Share results and learnings

### For All Agents
1. **Follow naming convention**: Consistent filenames
2. **Use standard intents**: Don't invent new ones
3. **Archive processed messages**: Keep inboxes clean
4. **Log everything**: Comprehensive audit trail

---

## Message Flow Example

```
1. Coordinator creates task
   → NEW_MESSAGES/20251121_170000_coordinator-all_implement-feature_AUTH-012.md

2. Engineer claims task
   → Moves to AGENTS/engineer/INBOX/
   → Creates TASKS/AUTH-012.status (status: claimed)

3. Engineer works on task
   → Updates in AGENTS/engineer/WORKSPACE/
   → Regular status updates to coordinator

4. Engineer completes implementation
   → NEW_MESSAGES/20251121_183000_engineer-qa_test_AUTH-012.md

5. QA tests implementation
   → Claims message
   → Runs tests
   → Reports results

6. Coordinator integrates work
   → Marks task complete
   → Archives all related messages to LOGS/
```

---

## Appendix: Intent Registry

Complete list of standard message intents and their usage:

| Intent | From | To | Purpose |
|--------|------|-----|---------|
| `implement_feature` | Coordinator | Engineer | Request feature implementation |
| `fix_bug` | Any | Engineer | Request bug fix |
| `test` | Engineer | QA | Request testing |
| `test_results` | QA | Engineer/Coordinator | Share test outcomes |
| `review_code` | Engineer | Engineer/SCRIBE | Request peer review |
| `audit` | SCRIBE | EAGLE | Request audit |
| `audit_report` | EAGLE | SCRIBE/Coordinator | Share audit findings |
| `document` | Any | SCRIBE | Request documentation |
| `escalate` | Any | Coordinator/ROOSTER | Request human decision |
| `status_update` | Any | Coordinator | Share progress |
| `coordinate` | Any | Any | Synchronize work |

---

**Status**: Complete
**Next**: See PROTOCOLS.md for agent behavior protocols
