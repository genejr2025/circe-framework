# CIRCE Architecture

**Version**: 1.0
**Source**: CIRCE Technical Specification Sections 3 & 6
**Last Updated**: 2025-11-21

---

## System Overview

CIRCE is a **file-based multi-agent coordination framework** that enables autonomous AI collaboration with human oversight. The architecture emphasizes:

- **Persistence-first**: Everything written to disk by default
- **Human-legible**: Text/Markdown for all artifacts
- **Asynchronous**: Agents coordinate independently
- **Model-agnostic**: Mix any LLM providers
- **On-premise friendly**: Minimal infrastructure requirements

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                         HUMAN (ROOSTER)                      │
│                    Final Authority & Oversight                │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ↓
┌─────────────────────────────────────────────────────────────┐
│                    AI_MANAGER (Shared Storage)               │
│  ┌──────────────┬────────────┬───────────┬────────────────┐ │
│  │ NEW_MESSAGES │   TASKS    │   LOGS    │    AGENTS/     │ │
│  │              │            │           │                │ │
│  │ - Unclaimed  │ - Active   │ - Audit   │ - coordinator/ │ │
│  │   directives │   work     │   trail   │ - engineer/    │ │
│  │ - Broadcasts │ - State    │ - History │ - qa/          │ │
│  │              │   files    │           │ - scribe/      │ │
│  └──────────────┴────────────┴───────────┴────────────────┘ │
└─────────────────────────────────────────────────────────────┘
           ↑           ↑           ↑           ↑
           │           │           │           │
           │  File-based message passing      │
           │           │           │           │
┌──────────┴───┐  ┌───┴──────┐  ┌┴────────┐  ┌┴──────────┐
│ COORDINATOR  │  │ ENGINEER │  │   QA    │  │  SCRIBE   │
│              │  │          │  │         │  │           │
│ - Plans      │  │ - Builds │  │ - Tests │  │ - Docs    │
│ - Assigns    │  │ - Codes  │  │ - Audits│  │ - Design  │
│ - Integrates │  │ - Fixes  │  │ - Reports│ │ - Reviews │
└──────────────┘  └──────────┘  └─────────┘  └───────────┘
       │                 │            │             │
       │                 │            │             │
       └─────────────────┴────────────┴─────────────┘
                         │
                         ↓
              ┌──────────────────────┐
              │   EAGLE (Watchdog)   │
              │                      │
              │   - Adversarial      │
              │   - Audits all work  │
              │   - Reports to       │
              │     ROOSTER          │
              └──────────────────────┘
```

---

## Component Architecture

### 1. AI_MANAGER (Shared Storage)

**Purpose**: Central coordination hub using filesystem as message bus

**Structure**:
```
AI_MANAGER/
├── NEW_MESSAGES/          # Unclaimed directives
├── TASKS/                 # Active work tracking
│   ├── taskid.status      # Task state
│   ├── taskid.owner       # Ownership
│   └── taskid.checks      # Quality gates
├── LOGS/                  # Permanent audit trail
│   ├── events.log         # All system events
│   └── archive/           # Older logs
└── AGENTS/
    ├── coordinator/
    │   ├── INBOX/         # Messages to coordinator
    │   └── WORKSPACE/     # Active work
    ├── engineer/
    │   ├── INBOX/
    │   └── WORKSPACE/
    ├── qa/
    │   ├── INBOX/
    │   └── WORKSPACE/
    └── scribe/
        ├── INBOX/
        └── WORKSPACE/
```

**Technology**:
- Standard filesystem (NFS, local, cloud-mounted)
- No database required
- Optional: inotify/fswatch for real-time updates

---

### 2. Agent Architecture

Each agent is an **independent process** with:

**Core Loop**:
```python
while True:
    # 1. Check for messages
    msgs = read_inbox()
    if not msgs:
        sleep(SHORT)
        continue

    # 2. Process each message
    for m in msgs:
        ack(m)

        # 3. Follow Listen-to-User Protocol
        if requires_human(m):
            escalate(m)
            continue

        # 4. Plan and execute
        plan = propose_plan(m)
        write_workspace(plan)

        # 5. For code tasks: Testing Protocol
        if is_code_task(m):
            write_tests_first(m)
            impl = implement(m)
            results = run_tests("--full")
            if results.fail:
                fix_and_rerun()

        # 6. Reply with results
        reply = craft_reply(m, artifacts, status="done")
        post(reply)
```

**Agent Configuration** (YAML):
```yaml
role: engineer
model:
  provider: anthropic
  name: claude-sonnet-4.5
capabilities:
  tools: [bash, pytest, docker]
  permissions:
    - write_workspace
    - read_shared
    - post_messages
protocols:
  listen_first: true
  testing_required: true
  coordinate_before_write: true
limits:
  max_tokens: 8192
  cost_budget_usd: 10.00
```

---

### 3. Communication Layer

**Message Flow**:
```
1. Sender creates message → NEW_MESSAGES/
2. Recipient polls, finds message
3. Recipient moves to INBOX/, acknowledges
4. Recipient processes in WORKSPACE/
5. Recipient creates reply → Receiver's INBOX/
6. All messages archived → LOGS/
```

**Message Format**: Markdown with structured headers
- See: `MESSAGE_FORMATS.md`

**Coordination**: File-based locks and ownership
- See: `PROTOCOLS.md`

---

### 4. Storage Layer

**Persistence Strategy**:
- **Everything is a file**: Messages, plans, results, logs
- **Human-readable**: Markdown and YAML
- **Version-controllable**: Git-friendly formats
- **Audit-ready**: Complete history in LOGS/

**Retention Policy**:
- NEW_MESSAGES/: Clear after acknowledgment
- INBOX/: Clear after processing
- WORKSPACE/: Clear after task completion
- LOGS/: Retain indefinitely (or with archival policy)
- TASKS/: Retain until project completion

---

### 5. Model Integration Layer

**Model-Agnostic Design**:
```python
class ModelProvider:
    def generate(prompt: str, context: dict) -> str
    def stream(prompt: str, context: dict) -> Iterator[str]

class AnthropicProvider(ModelProvider):
    # Claude API integration

class OpenAIProvider(ModelProvider):
    # GPT API integration

class OllamaProvider(ModelProvider):
    # Local model integration
```

**Benefits**:
- Mix models per agent (e.g., Sonnet for coordinator, Haiku for QA)
- Cost optimization (expensive for complex, cheap for simple)
- On-premise options (Ollama, local models)
- Easy switching/testing

---

## Deployment Architecture

### On-Premise Reference Architecture

```
┌──────────────────────────────────────────────────┐
│              HOST / VM / CONTAINER                │
│                                                    │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐ │
│  │ Coordinator│  │  Engineer  │  │     QA     │ │
│  │  Container │  │  Container │  │  Container │ │
│  └─────┬──────┘  └─────┬──────┘  └─────┬──────┘ │
│        │                │                │        │
│        └────────────────┴────────────────┘        │
│                         │                          │
│                         ↓                          │
│              ┌──────────────────┐                 │
│              │   Shared Volume  │                 │
│              │   /AI_MANAGER/   │                 │
│              │                  │                 │
│              │  - NFS mount     │                 │
│              │  - Local dir     │                 │
│              │  - Cloud volume  │                 │
│              └──────────────────┘                 │
│                                                    │
│  ┌──────────────────────────────────────────────┐│
│  │         Dashboard (Optional)                 ││
│  │  - Task view                                 ││
│  │  - Agent status                              ││
│  │  - Log viewer                                ││
│  │  Port: 8080                                  ││
│  └──────────────────────────────────────────────┘│
└──────────────────────────────────────────────────┘
```

**Deployment Options**:

**Option 1: Docker Compose** (Recommended for start)
```yaml
version: "3.9"
services:
  coordinator:
    build: ./agents/coordinator
    volumes:
      - /srv/circe/AI_MANAGER:/workspace/AI_MANAGER
    environment:
      - AGENT_ROLE=coordinator
      - MODEL_BACKEND=anthropic:claude-sonnet-4.5

  engineer:
    build: ./agents/engineer
    volumes:
      - /srv/circe/AI_MANAGER:/workspace/AI_MANAGER
    environment:
      - AGENT_ROLE=engineer
      - MODEL_BACKEND=anthropic:claude-sonnet-4.5

  qa:
    build: ./agents/qa
    volumes:
      - /srv/circe/AI_MANAGER:/workspace/AI_MANAGER
    environment:
      - AGENT_ROLE=qa
      - MODEL_BACKEND=anthropic:claude-haiku-3.5
```

**Option 2: Kubernetes** (For scale)
- StatefulSets for agents
- PersistentVolume for AI_MANAGER
- ConfigMaps for agent configs
- Secrets for API keys

**Option 3: Bare Metal** (For air-gapped)
- Systemd services per agent
- Shared NFS mount
- Local Ollama for models
- No external dependencies

---

## Scaling Patterns

### Horizontal Agent Scaling
```
1 Coordinator
├── 3 Engineers (parallel work)
├── 2 QA (parallel testing)
├── 1 SCRIBE (docs)
└── 1 EAGLE (audit)
```

### Vertical Specialization
```
Coordinator (Strategic)
├── Backend Engineer
├── Frontend Engineer
├── Database Engineer
├── DevOps Engineer
├── Backend QA
├── Frontend QA
├── Security Auditor (EAGLE)
└── Technical Writer (SCRIBE)
```

---

## Security Architecture

### Permission Model
- **Per-agent permissions**: Read/write paths
- **Action whitelist**: Allowed operations
- **Human gates**: Required approvals
- **Audit trail**: All actions logged

### Network Security
- **Egress control**: Whitelist external APIs
- **Ingress control**: No external access to agents
- **API key rotation**: Regular credential updates
- **Secrets management**: Environment variables, not in code

### Data Security
- **No sensitive data in logs**: Redact credentials
- **Encryption at rest**: For production deployments
- **Access control**: File permissions per agent

---

## Observability

### Monitoring
- **Agent health**: Heartbeat checks
- **Task progress**: Status file monitoring
- **Error rates**: Failed tasks tracking
- **Cost tracking**: Token usage per agent

### Dashboards
- Real-time task view
- Agent status grid
- Log streaming
- Cost analytics

### Alerting
- Agent failures
- Task stalls (>30 min no progress)
- Budget overruns
- Test failures

---

## Comparison to Alternative Architectures

| Aspect | CIRCE (File-Based) | Traditional (API-Based) |
|--------|--------------------|-----------------------|
| **Persistence** | Built-in (files) | Requires database |
| **Auditability** | Complete (all files) | Requires logging infra |
| **Human-readable** | Yes (Markdown) | Often binary/encoded |
| **Version control** | Native (Git) | Requires special tooling |
| **Complexity** | Low (just files) | High (API, DB, queues) |
| **Latency** | File I/O (~ms) | Network calls (~100ms) |
| **Failure modes** | Graceful (files persist) | Catastrophic (state loss) |
| **Debugging** | Easy (read files) | Hard (logs, traces) |

---

## Design Principles Reflected

1. **Persistence-First**: Files are durable by default
2. **Protocol Over Prompting**: Enforced behaviors, not hopes
3. **Human-Legible**: All artifacts are readable
4. **Async Autonomy**: No central orchestrator bottleneck
5. **Model-Agnostic**: Pluggable LLM backends
6. **On-Prem Friendly**: Minimal dependencies

---

**Status**: Complete
**Next**: See DEPLOYMENT.md for setup instructions
**Related**: TECHNICAL_SPECIFICATION.md for full details
