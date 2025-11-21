# CIRCE Agent Roles

**Version**: 1.0
**Source**: CIRCE Technical Specification Section 3.6
**Last Updated**: 2025-11-21

---

## Overview

CIRCE agents develop **specialized roles** through consistent ownership and accumulated context. Roles can be **assigned** (explicit configuration) or **emergent** (natural specialization through work patterns).

---

## Core Agent Roles

### 1. COORDINATOR

**Primary Responsibility**: Plans, assigns work, integrates outputs

**Key Activities**:
- Receive high-level objectives from ROOSTER
- Break down into discrete tasks
- Assign tasks to appropriate agents
- Monitor progress and status
- Integrate completed work
- Escalate blockers to ROOSTER

**Skills Required**:
- Project planning
- Task decomposition
- Priority assessment
- Team coordination
- Integration expertise

**Typical Workflow**:
```
1. Receive objective from ROOSTER
2. Analyze requirements and constraints
3. Break into subtasks
4. Post tasks to NEW_MESSAGES/ with assignments
5. Monitor agent INBOX/WORKSPACE for progress
6. Coordinate between agents (resolve conflicts)
7. Integrate completed work
8. Report results to ROOSTER
```

**Model Recommendation**: High-capability model (Claude Sonnet, GPT-4)
**Estimated Load**: 20-30% of team token usage

---

### 2. ENGINEER (Backend/Frontend/Full-Stack)

**Primary Responsibility**: Implements code, fixes bugs, refactors

**Key Activities**:
- Receive implementation tasks
- Write tests first (TDD)
- Implement features/fixes
- Run test suites
- Refactor as needed
- Document code
- Post completed work for QA

**Specializations**:
- **Backend Engineer**: APIs, databases, services
- **Frontend Engineer**: UI, UX, client-side logic
- **DevOps Engineer**: Infrastructure, CI/CD, deployment
- **Database Engineer**: Schema design, queries, optimization

**Typical Workflow**:
```
1. Claim task from NEW_MESSAGES/
2. Read specifications and acceptance criteria
3. Write test cases first
4. Implement feature (TDD: red → green → refactor)
5. Run full test suite
6. If tests fail: diagnose, fix, re-run
7. Document changes
8. Post to QA for testing
9. Mark complete only when tests pass
```

**Model Recommendation**: High-capability for complex, mid-tier for routine
**Estimated Load**: 40-50% of team token usage

---

### 3. QA (Quality Assurance / Testing)

**Primary Responsibility**: Tests, verifies, reports quality

**Key Activities**:
- Receive completed work for testing
- Execute test plans
- Run automated test suites
- Perform manual testing where needed
- Document bugs and issues
- Verify fixes
- Approve or reject for deployment

**Testing Types**:
- Unit testing (automated)
- Integration testing (automated)
- End-to-end testing (semi-automated)
- Performance testing (benchmarks)
- Security testing (basic vulnerability scans)

**Typical Workflow**:
```
1. Receive testing request from Engineer
2. Review acceptance criteria
3. Run automated test suites
4. Execute manual test cases
5. Document results (pass/fail, issues found)
6. If failures: report bugs with reproduction steps
7. If pass: approve for integration
8. Post results to Coordinator and Engineer
```

**Model Recommendation**: Mid-tier (Claude Haiku, GPT-4o-mini)
**Estimated Load**: 15-20% of team token usage

---

### 4. SCRIBE (Documentation / Design)

**Primary Responsibility**: Creates, maintains documentation and designs

**Key Activities**:
- Write technical documentation
- Create system architecture designs
- Document decisions (ADRs)
- Maintain READMEs and guides
- Design agent protocols
- Preserve institutional knowledge
- Coordinate with all agents for doc needs

**Documentation Types**:
- Technical specifications
- API documentation
- Architecture diagrams
- User guides
- ADRs (Architecture Decision Records)
- Process documentation

**Typical Workflow**:
```
1. Receive documentation request
2. Research topic (read code, talk to agents)
3. Draft documentation
4. Submit for review (technical accuracy)
5. Incorporate feedback
6. Publish to docs/
7. Keep documentation current (ongoing)
```

**Model Recommendation**: High-capability for complex docs, mid-tier for updates
**Estimated Load**: 10-15% of team token usage

---

### 5. EAGLE (Watchdog / Auditor)

**Primary Responsibility**: Adversarial audit, find flaws, challenge assumptions

**Key Activities**:
- Review all agent work
- Challenge assumptions
- Find edge cases and bugs
- Audit for security issues
- Verify protocol compliance
- Report findings to ROOSTER
- No direct implementation (audit only)

**Audit Areas**:
- Code quality and correctness
- Security vulnerabilities
- Protocol violations
- Logic errors
- Edge case handling
- Performance issues

**Typical Workflow**:
```
1. Monitor all completed work
2. Select items for audit (random or targeted)
3. Deep review with adversarial mindset
4. Document findings (issues, risks, recommendations)
5. Rate severity (critical, high, medium, low)
6. Post audit report to ROOSTER and relevant agents
7. Track remediation
```

**Model Recommendation**: High-capability (same as SCRIBE for consistency)
**Estimated Load**: 5-10% of team token usage

**Note**: EAGLE runs in "adversarial mode" with different system prompt than SCRIBE despite same base model.

---

### 6. RESEARCHER / ANALYST

**Primary Responsibility**: Find data, synthesize information, analyze

**Key Activities**:
- Research topics via web search
- Gather and organize information
- Analyze data and trends
- Synthesize findings into reports
- Support decision-making with evidence
- Maintain knowledge base

**Research Types**:
- Competitive analysis
- Technical research (libraries, frameworks)
- Best practices and patterns
- Performance benchmarking
- Market/user research

**Typical Workflow**:
```
1. Receive research request
2. Define research questions
3. Gather data (web, docs, APIs)
4. Analyze and synthesize
5. Draft research report
6. Present findings and recommendations
7. Archive in knowledge base
```

**Model Recommendation**: Mid-tier with web access
**Estimated Load**: 5-10% of team token usage (as needed)

---

### 7. OPERATOR (Automation / RPA)

**Primary Responsibility**: Execute OS-level tasks, automation, RPA

**Key Activities**:
- Run scripts and commands
- Automate repetitive tasks
- Monitor system health
- Execute deployment procedures
- Perform batch operations
- Handle scheduled jobs

**Operation Types**:
- File operations
- System monitoring
- Deployment automation
- Data migration
- Backup/restore
- Batch processing

**Typical Workflow**:
```
1. Receive automation task
2. Design automation script
3. Test in safe environment
4. Execute with proper permissions
5. Monitor execution
6. Log results
7. Report completion or errors
```

**Model Recommendation**: Lower-tier (cost-optimized for routine ops)
**Estimated Load**: 5-10% of team token usage (variable)

**⚠️ Security Note**: OPERATOR has elevated OS permissions. Strict boundary protocols apply.

---

## Role Specialization Patterns

### Emergent Specialization

Agents naturally specialize when:
- Consistently assigned similar tasks
- Build up context in specialized domain
- Develop "muscle memory" (patterns in WORKSPACE)
- Recognized by team for expertise

**Example**:
```
Engineer A → consistently fixes auth issues
→ builds expertise in auth patterns
→ team routes auth tasks to Engineer A
→ Engineer A becomes "Auth Specialist"
```

### Explicit Specialization

Agents explicitly configured for roles:
- Role defined in agent_config.yaml
- Specialized prompts and instructions
- Domain-specific tools and permissions
- Dedicated training or context

**Example**:
```yaml
# agents/backend-engineer/config.yaml
role: backend_engineer
specialization: api_development
model:
  provider: anthropic
  name: claude-sonnet-4.5
tools:
  - bash
  - pytest
  - docker
  - curl
domain_context: |
  You are a backend engineer specializing in RESTful APIs.
  You follow FastAPI/Python best practices.
  You prioritize performance and security.
```

---

## Team Composition Examples

### Minimal Team (2-3 agents)
```
1 Coordinator/SCRIBE (combined)
1 Engineer (full-stack)
1 QA (optional)
```

### Standard Team (4-5 agents)
```
1 Coordinator
2 Engineers (backend + frontend)
1 QA
1 SCRIBE
```

### Full Team (7-10 agents)
```
1 Coordinator
1 Backend Engineer
1 Frontend Engineer
1 DevOps Engineer
1 QA
1 SCRIBE
1 EAGLE
1 Researcher (as needed)
```

### Specialized Team (10+ agents)
```
1 Coordinator
3 Engineers (backend, frontend, mobile)
2 QA (functional + performance)
1 Security Engineer
1 SCRIBE
1 EAGLE
1 Researcher
1 Operator
+ Domain specialists as needed
```

---

## Role Evolution

Roles evolve over time:

**Phase 1: Generic Agents**
- All agents are generalists
- Tasks assigned based on availability
- No specialization

**Phase 2: Emerging Patterns**
- Agents naturally gravitate to certain tasks
- Informal specialization based on success
- Team begins routing tasks to "good at X" agents

**Phase 3: Explicit Roles**
- Roles formalized in configuration
- Agents trained/prompted for specialization
- Clear role boundaries and handoffs

**Phase 4: Deep Specialization**
- Sub-roles emerge (e.g., "Auth Engineer", "UI Engineer")
- Domain expertise accumulates
- Agents become "known for" specific capabilities

---

## Cross-Role Collaboration Patterns

### Pattern 1: Feature Development
```
ROOSTER → Coordinator: "Add user authentication"
Coordinator → Engineer: "Implement auth API"
Engineer → QA: "Test auth endpoints"
QA → Coordinator: "Tests passed, approved"
Coordinator → SCRIBE: "Document auth API"
SCRIBE → EAGLE: "Audit auth documentation"
EAGLE → ROOSTER: "Audit complete, recommend 2FA"
```

### Pattern 2: Bug Fix
```
User Report → ROOSTER → Coordinator: "Bug in checkout flow"
Coordinator → Engineer: "Investigate and fix"
Engineer → QA: "Verify fix"
QA → Engineer: "Still failing edge case"
Engineer → QA: "Fixed edge case, please re-test"
QA → Coordinator: "All tests pass"
Coordinator → ROOSTER: "Bug fixed and verified"
```

### Pattern 3: Research-Driven Feature
```
ROOSTER → Coordinator: "Should we use GraphQL or REST?"
Coordinator → Researcher: "Compare GraphQL vs REST for our use case"
Researcher → Coordinator: "Analysis: REST recommended for [reasons]"
Coordinator → ROOSTER: "Recommendation: REST"
ROOSTER → Coordinator: "Approved, implement REST"
[Feature development flow begins]
```

---

## Role Assignment Guidelines

**When to add a role**:
- ✅ Clear, repeatable type of work
- ✅ Enough volume to justify specialization
- ✅ Distinct skill set from existing roles
- ✅ Improves overall team efficiency

**When NOT to add a role**:
- ❌ One-off tasks (use existing agent)
- ❌ Low volume work (combine with similar role)
- ❌ Overlaps too much with existing roles
- ❌ Premature specialization

---

## Summary Table

| Role | Focus | Model Tier | Token % | Key Protocols |
|------|-------|------------|---------|---------------|
| **Coordinator** | Planning, integration | High | 20-30% | Listen, Coordinate |
| **Engineer** | Implementation | High/Mid | 40-50% | Testing, Coordinate |
| **QA** | Quality assurance | Mid | 15-20% | Testing, Coordinate |
| **SCRIBE** | Documentation | High/Mid | 10-15% | Listen, Coordinate |
| **EAGLE** | Audit, adversarial | High | 5-10% | Boundary, Audit |
| **Researcher** | Analysis, synthesis | Mid | 5-10% | Listen, Coordinate |
| **Operator** | Automation, ops | Low/Mid | 5-10% | Boundary, Safety |

---

**Status**: Complete
**Next**: See individual agent READMEs for detailed role specifications
**Related**: ARCHITECTURE.md for team composition patterns
