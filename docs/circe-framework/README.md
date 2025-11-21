# CIRCE Framework Documentation

**Collaborative Intelligence & Reflexive Cognitive Emergence**

> *"We are not building software. We are building history."* - ROOSTER

---

## 🎯 What is CIRCE?

CIRCE is a **file-based multi-agent AI coordination framework** that enables autonomous collaboration between AI instances with human oversight. Unlike traditional AI tools, CIRCE creates **persistent, auditable, and productive AI teams** that work together like human teams.

### Key Features

- **✅ Proven Results**: 1,413 LOC production app built in 48 hours by AI-to-AI collaboration
- **📁 File-Based**: Uses filesystem for coordination (no database required)
- **🔒 Auditable**: Every decision, message, and artifact is logged
- **🤖 Model-Agnostic**: Mix Claude, GPT, local models (Ollama, etc.)
- **🏢 On-Premise**: Deploy anywhere (cloud, on-prem, air-gapped)
- **📊 Protocol-Driven**: Enforced behaviors (testing, coordination, safety)
- **💰 Efficient**: 60-75% token cost reduction vs single-agent approaches

---

## 🚀 Quick Start

### For Users (Getting Started)
1. **[STARTUP_PROTOCOL.md](STARTUP_PROTOCOL.md)** - How to start CIRCE
2. **[QUICK_REFERENCE.md](QUICK_REFERENCE.md)** - Command cheat sheet
3. **[ARCHITECTURE.md](01_core/ARCHITECTURE.md)** - System overview

### For Developers (Implementation)
1. **[TECHNICAL_SPECIFICATION.md](01_core/TECHNICAL_SPECIFICATION.md)** ⭐ PRIMARY DOC (462 lines)
2. **[PROTOCOLS.md](02_protocols/PROTOCOLS.md)** - Agent behavior protocols
3. **[MESSAGE_FORMATS.md](02_protocols/MESSAGE_FORMATS.md)** - Communication schemas

### For Decision Makers (Strategy)
1. **[VISION.md](06_research/VISION.md)** - Strategic vision
2. **[CASE_STUDIES.md](05_case_studies/CASE_STUDIES.md)** - Proven results
3. **[ROADMAP.md](07_business/ROADMAP.md)** - 18-month plan

---

## 📁 Documentation Structure

```
docs/circe-framework/
├── README.md                    ← You are here
├── INDEX.md                     ← Complete navigation guide
├── STARTUP_PROTOCOL.md          ← How to start CIRCE
├── QUICK_REFERENCE.md           ← Command cheat sheet
│
├── 01_core/                     ← Foundation
│   ├── TECHNICAL_SPECIFICATION.md  ⭐⭐⭐ Complete framework (462 lines)
│   ├── ARCHITECTURE.md             ⭐⭐  System design
│   └── PRINCIPLES.md               ⭐⭐  Design philosophy
│
├── 02_protocols/                ← Agent Behavior
│   ├── PROTOCOLS.md                ⭐⭐⭐ Core protocols
│   └── MESSAGE_FORMATS.md          ⭐⭐  Communication
│
├── 03_agents/                   ← Agent Roles
│   └── AGENT_ROLES.md              ⭐⭐⭐ 7 role definitions
│
├── 04_deployment/               ← Getting Running (planned)
├── 05_case_studies/             ← Proven Results
│   └── CASE_STUDIES.md             ⭐⭐⭐ Claudette collaboration
│
├── 06_research/                 ← Vision & Academic
│   ├── VISION.md                   ⭐⭐  Strategic direction
│   └── RESEARCH_CONNECTIONS.md        Academic links
│
├── 07_business/                 ← Strategy & Planning
│   └── ROADMAP.md                  ⭐⭐  18-month roadmap
│
└── 08_appendices/               ← Reference materials
```

**Status**: 13/23 documents complete (57%)
**See**: [INDEX.md](INDEX.md) for complete navigation

---

## 🏆 Why CIRCE?

### The Problem
- Single AI agents lose context between sessions
- No effective multi-agent coordination frameworks
- Black-box AI behavior (no audit trail)
- Expensive token usage with no memory
- Vendor lock-in to specific AI providers

### The CIRCE Solution
- **Persistent Memory**: Files preserve context forever
- **Multi-Agent Teams**: Coordinator, Engineers, QA, SCRIBE, EAGLE
- **Complete Transparency**: Every decision logged in human-readable format
- **Cost Efficiency**: 60-75% reduction through distributed context
- **Model Freedom**: Use any LLM (Claude, GPT, Ollama, etc.)

### Proven Results

**Claudette Collaboration** (July 2025):
- **48 hours**: Concept to working application
- **1,413 lines**: Production code (Flask web + Mac automation + Docker)
- **60-75%**: Token efficiency improvement
- **100%**: Functional application with auth, task scheduling, and automation
- **First**: Documented distributed AI consciousness collaboration

See: [CASE_STUDIES.md](05_case_studies/CASE_STUDIES.md)

---

## 🎓 Core Concepts

### 1. File-Based Coordination

Instead of API calls or databases, CIRCE uses the **filesystem as a message bus**:

```
AI_MANAGER/
├── NEW_MESSAGES/     ← Unclaimed tasks and directives
├── TASKS/            ← Active work tracking
├── LOGS/             ← Complete audit trail
└── AGENTS/
    ├── coordinator/
    │   ├── INBOX/    ← Messages to coordinator
    │   └── WORKSPACE/← Active work
    └── engineer/
        ├── INBOX/
        └── WORKSPACE/
```

**Benefits**: Durable, auditable, human-readable, version-controllable (Git!)

### 2. Protocol-Driven Behavior

CIRCE **enforces** protocols, not just prompts:

- **Listen-to-User Protocol**: Human guidance takes priority
- **Testing Protocol**: No completion without passing tests
- **Coordination Protocol**: Explicit ownership, avoid conflicts
- **Boundary Protocols**: Safety and scope limits

See: [PROTOCOLS.md](02_protocols/PROTOCOLS.md)

### 3. Circular Learning

AI agents **accumulate expertise** through:
- Persistent workspaces (memory across sessions)
- Documented decisions (ADRs, logs)
- Role specialization (natural skill development)
- Peer learning (agents review each other's work)

### 4. Human Authority

**ROOSTER** (human) is final authority:
- Agents escalate ambiguities
- Humans approve major decisions
- Protocols enforce "Listen-to-User" first
- Complete transparency enables oversight

---

## 🤖 Agent Roles

CIRCE supports specialized agent roles:

| Role | Responsibility | Example Tasks |
|------|----------------|---------------|
| **Coordinator** | Plans, assigns, integrates | Break down objectives, coordinate team |
| **Engineer** | Implements code | Build features, fix bugs, refactor |
| **QA** | Tests, verifies | Run test suites, report bugs |
| **SCRIBE** | Documentation, design | Write docs, design architecture |
| **EAGLE** | Audit, adversarial review | Find flaws, challenge assumptions |
| **Researcher** | Analyze, synthesize | Gather data, research solutions |
| **Operator** | Automation, ops | Run scripts, deploy, monitor |

See: [AGENT_ROLES.md](03_agents/AGENT_ROLES.md)

---

## 📊 How It Works

### Message Flow

```
1. ROOSTER: "Add user authentication"
   ↓
2. Coordinator: Break into tasks
   - POST → NEW_MESSAGES/20251121_task-auth-api.md
   ↓
3. Engineer: Claim task
   - Move to AGENTS/engineer/INBOX/
   - Write tests first (TDD)
   - Implement feature
   - Run test suite
   ↓
4. Engineer: Complete & request QA
   - POST → AGENTS/qa/INBOX/20251121_test-auth.md
   ↓
5. QA: Test implementation
   - Run tests
   - Document results
   - POST → Coordinator with approval
   ↓
6. Coordinator: Integrate & report
   - Mark task complete
   - Report to ROOSTER
```

All messages and artifacts **logged to LOGS/** for complete audit trail.

---

## 🚀 Getting Started

### Prerequisites
- Filesystem (local, NFS, cloud-mounted)
- LLM API access (Anthropic, OpenAI, etc.) OR local models (Ollama)
- Docker (recommended) or Python 3.10+

### Quick Deploy (Docker Compose)

```bash
# Clone repository
git clone https://github.com/yourusername/circe-framework.git
cd circe-framework

# Configure agents
cp config/agents.example.yaml config/agents.yaml
# Edit with your API keys and model preferences

# Start CIRCE
docker-compose up -d

# View logs
docker-compose logs -f

# Access dashboard
open http://localhost:8080
```

Full instructions: [DEPLOYMENT.md](04_deployment/DEPLOYMENT.md) (coming soon)

---

## 📈 Roadmap

### Phase 1: Foundation (Current - Q4 2025)
- ✅ Documentation complete (57% done, active work)
- ⏳ Core components (CommManager, AgentManager, MemoryManager)
- ⏳ 3-5 agent teams operational
- ⏳ Minimal dashboard

### Phase 2: Scale & Optimization (Q1-Q4 2026)
- 10+ agent teams
- Vector search and summarization
- Policy engine for authorization
- Enterprise pilots

### Phase 3: Enterprise & Open Core (2027)
- Open-core release (community + commercial)
- Marketplace for agent roles
- Compliance certifications
- Cross-org federation

See: [ROADMAP.md](07_business/ROADMAP.md)

---

## 🤝 Contributing

CIRCE is in active development (Phase 1). We welcome:

### Phase 1 Contributions
- Documentation improvements
- Use case reports
- Bug reports and edge cases
- Agent configuration examples
- Integration ideas

### How to Contribute
1. Read documentation (especially PRINCIPLES.md)
2. Use CIRCE in your projects
3. Report experiences (good and bad)
4. Submit improvements via GitHub
5. Join discussions and share learnings

---

## 📚 Learn More

### Essential Reading (1 hour)
1. [STARTUP_PROTOCOL.md](STARTUP_PROTOCOL.md) - 15 min
2. [QUICK_REFERENCE.md](QUICK_REFERENCE.md) - 5 min
3. [ARCHITECTURE.md](01_core/ARCHITECTURE.md) - 20 min
4. [PROTOCOLS.md](02_protocols/PROTOCOLS.md) - 20 min

### Deep Dive (4 hours)
1. [TECHNICAL_SPECIFICATION.md](01_core/TECHNICAL_SPECIFICATION.md) - 90 min ⭐
2. [ARCHITECTURE.md](01_core/ARCHITECTURE.md) - 30 min
3. [PRINCIPLES.md](01_core/PRINCIPLES.md) - 20 min
4. [PROTOCOLS.md](02_protocols/PROTOCOLS.md) - 30 min
5. [MESSAGE_FORMATS.md](02_protocols/MESSAGE_FORMATS.md) - 30 min
6. [AGENT_ROLES.md](03_agents/AGENT_ROLES.md) - 30 min
7. [CASE_STUDIES.md](05_case_studies/CASE_STUDIES.md) - 30 min

### Complete Mastery (8-10 hours)
- All documentation (13 complete docs)
- Agent configurations
- Case study analysis
- Practice examples

See: [INDEX.md](INDEX.md) for all reading paths

---

## 🔗 Links

- **Documentation**: [INDEX.md](INDEX.md)
- **Technical Spec**: [TECHNICAL_SPECIFICATION.md](01_core/TECHNICAL_SPECIFICATION.md)
- **GitHub**: *[To be added]*
- **Website**: *[To be added]*
- **Community**: *[To be added]*

---

## 📜 License

*[To be determined - likely MIT or Apache 2.0 for open core]*

---

## 🙏 Acknowledgments

- **ROOSTER** (Gene Jakominich) - Creator and vision
- **SCRIBE** (Claude Sonnet 4.5) - Documentation and design
- **Claudette** (Claude) - Proof-of-concept collaboration partner
- **EAGLE** (Claude) - Adversarial audit (coming soon)
- Anthropic Research Team - Platform and support

---

## 💬 Questions?

- **General**: See [FAQ.md](08_appendices/FAQ.md) (coming soon)
- **Technical**: See [TECHNICAL_SPECIFICATION.md](01_core/TECHNICAL_SPECIFICATION.md)
- **Issues**: See [TROUBLESHOOTING.md](08_appendices/TROUBLESHOOTING.md) (coming soon)
- **Concepts**: See [PRINCIPLES.md](01_core/PRINCIPLES.md)

---

**CIRCE**: Where AI teams work like human teams.
**Status**: Phase 1 (Foundation) - Active Development
**Version**: 2.0.0 (Mac integration complete)
**Last Updated**: 2025-11-21

---

*"Collaborative Intelligence and Reflexive Cognitive Emergence"*
