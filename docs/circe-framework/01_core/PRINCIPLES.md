# CIRCE Design Principles

**Version**: 1.0
**Source**: CIRCE Technical Specification Section 3.1
**Last Updated**: 2025-11-21

---

## Core Principles

### 1. Persistence-First

**Principle**: Default to writing plans, results, and tests to disk.

**Rationale**:
- Volatile memory = knowledge loss
- Session boundaries shouldn't reset progress
- Audit trails require durable artifacts
- Human oversight needs reviewable artifacts

**Implementation**:
- Every decision → Markdown file
- Every plan → Workspace document
- Every result → Logged outcome
- Every test → Reproducible script

**Anti-Pattern**:
- ❌ Relying on conversation context alone
- ❌ Ephemeral state in memory
- ❌ Undocumented decisions
- ❌ Lost work between sessions

---

### 2. Protocol Over Prompting

**Principle**: Enforce habits (listen, test, coordinate) rather than rely on best-effort prompting.

**Rationale**:
- Prompts can be ignored or misinterpreted
- Protocols are enforceable and measurable
- Consistency comes from structure, not luck
- Quality gates prevent bad outputs

**Implementation**:
- Listen-to-User Protocol (always check human guidance first)
- Testing Protocol (no completion without passing tests)
- Coordination Protocol (explicit ownership and status)
- Boundary Protocols (safety and scope limits)

**Anti-Pattern**:
- ❌ "Please remember to test" (hope-based)
- ❌ "Try to coordinate" (optional)
- ❌ "Be careful with..." (vague guidance)
- ❌ Relying on model's goodwill

---

### 3. Human-Legible by Design

**Principle**: Logs and artifacts are text/markdown; no black-box hidden state.

**Rationale**:
- Humans need to audit AI work
- Debugging requires readable artifacts
- Trust comes from transparency
- Version control works with text

**Implementation**:
- Markdown for all messages
- YAML for structured data
- Plain text logs
- No binary formats unless necessary
- Git-friendly formats

**Anti-Pattern**:
- ❌ Binary message formats
- ❌ Encrypted/encoded logs
- ❌ Hidden internal state
- ❌ Black-box artifacts

---

### 4. Asynchronous Autonomy

**Principle**: Agents run independently, coordinating via files; no central brain required.

**Rationale**:
- Central orchestrator = single point of failure
- Synchronous coordination = bottleneck
- Agents should work in parallel
- Scalability requires independence

**Implementation**:
- Each agent polls their INBOX independently
- File-based message passing (no API calls)
- Ownership files prevent conflicts
- No required "coordination agent"

**Anti-Pattern**:
- ❌ Central controller bottleneck
- ❌ Synchronous API calls between agents
- ❌ Tight coupling
- ❌ Waiting for central approval for every action

---

### 5. Model-Agnostic

**Principle**: Mix proprietary and local models; isolate model choice behind agent interfaces.

**Rationale**:
- Different tasks have different model requirements
- Cost optimization (expensive vs cheap models)
- On-premise vs cloud flexibility
- Technology evolution (new models emerge)

**Implementation**:
- Abstract model interface (ModelProvider)
- Per-agent model configuration
- Easy switching (config change, not code change)
- Support for OpenAI, Anthropic, Ollama, HuggingFace, etc.

**Anti-Pattern**:
- ❌ Hard-coded model dependencies
- ❌ API-specific code throughout
- ❌ Assumption of single model provider
- ❌ Tight coupling to specific AI service

---

### 6. On-Premise Friendly

**Principle**: Minimal infra (shared filesystem), straightforward Docker packaging.

**Rationale**:
- Enterprise requires on-prem options
- Air-gapped environments need local deployment
- Cloud vendor lock-in is risk
- Simple deployment lowers barrier to entry

**Implementation**:
- Filesystem as only required infrastructure
- Optional: NFS, local disk, cloud mount
- Docker Compose for easy start
- Support for local models (Ollama)
- No required external services

**Anti-Pattern**:
- ❌ Requiring cloud-only services
- ❌ Complex infrastructure dependencies
- ❌ Vendor lock-in
- ❌ Network-dependent architecture

---

## Design Philosophy

### Simplicity Over Cleverness

> **"The best tool is the one you understand."**

- Files are simple
- Markdown is universal
- Polling is predictable
- No magic, no hidden complexity

### Boring Technology

> **"Choose boring technology for production."**

- Filesystem: 50+ years of stability
- Text files: Universal compatibility
- File watching: Well-understood patterns
- Docker: Industry standard

### Optimize for Debugging

> **"When it breaks, can you figure out why?"**

- All state is visible (files)
- All history is retained (logs)
- All decisions are documented (markdown)
- All messages are traceable (filenames)

### Human Authority

> **"AI assists, humans decide."**

- ROOSTER (human) is final authority
- Protocols enforce "Listen-to-User" first
- Escalation is easy and encouraged
- Humans can override any agent decision

---

## Principle Trade-offs

### We Choose... Over...

| We Prioritize | Over | Because |
|---------------|------|---------|
| **Simplicity** | Performance | Easier to debug and maintain |
| **Transparency** | Efficiency | Trust requires visibility |
| **Durability** | Speed | Lost work is expensive |
| **Flexibility** | Optimization | Requirements change |
| **Boring tech** | Latest trends | Stability matters in production |
| **Human control** | AI autonomy | Safety and alignment |

---

## Anti-Principles (What CIRCE is NOT)

### NOT Fully Autonomous AGI
- CIRCE is human-guided AI teams
- Humans provide direction and oversight
- AI executes within bounds
- Not aiming for self-directed superintelligence

### NOT Black-Box Magic
- All artifacts are inspectable
- All decisions are documented
- No hidden state or secret sauce
- Transparency is core

### NOT Cloud-Only SaaS
- On-premise deployment is first-class
- Air-gapped environments supported
- No vendor lock-in
- Your data stays on your infrastructure

### NOT Rigid Workflow Engine
- Agents adapt to work patterns
- Protocols are guidelines, not straitjackets
- Human judgment can override
- Flexibility within structure

---

## Principle Evolution

These principles are not dogma:
- **Observe**: What works in practice?
- **Question**: Do principles serve us?
- **Adapt**: Evolve based on evidence
- **Document**: Track changes and rationale

**Example Evolution**:
```
Principle: "Always use Markdown"
Observation: Binary artifacts (images, PDFs) have legitimate uses
Evolution: "Human-legible by default, with exceptions for binary artifacts"
Rationale: Pragmatic flexibility while maintaining transparency
```

---

## Measuring Adherence

### Good Indicators:
- ✅ Can explain any agent decision by reading files
- ✅ New team members onboard by reading docs
- ✅ Failures are diagnosed quickly (logs are clear)
- ✅ Agents work in parallel without conflicts
- ✅ Costs are predictable (model-agnostic allows optimization)
- ✅ Deployments work on various infrastructure

### Warning Signs:
- ⚠️  Can't explain how agent reached conclusion
- ⚠️  Onboarding requires "tribal knowledge"
- ⚠️  Debugging requires deep code diving
- ⚠️  Agents frequently conflict or block each other
- ⚠️  Locked into single model provider
- ⚠️  Deployment only works in specific environment

---

## Conclusion

CIRCE principles prioritize:
1. **Durability** (persistence-first)
2. **Reliability** (protocol-driven)
3. **Transparency** (human-legible)
4. **Scalability** (async autonomy)
5. **Flexibility** (model-agnostic)
6. **Practicality** (on-prem friendly)

These aren't just nice-to-haves—they're the foundation that makes multi-agent AI collaboration **trustworthy and production-ready**.

---

**Status**: Complete
**Next**: See CIRCULAR_LEARNING.md for the learning model
**Related**: PROTOCOLS.md for principle enforcement
