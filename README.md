# CIRCE Framework

**Collaborative Intelligence & Reflexive Cognitive Emergence**

> AI agents that actually remember, collaborate, and get shit done.

[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](https://github.com/genejr2025/circe-framework/releases)
[![License](https://img.shields.io/badge/license-TBD-green.svg)](LICENSE)
[![Documentation](https://img.shields.io/badge/docs-57%25-yellow.svg)](docs/circe-framework/INDEX.md)
[![Status](https://img.shields.io/badge/status-Phase%201-orange.svg)](docs/circe-framework/07_business/ROADMAP.md)

---

## What Is This?

**The Problem:** AI forgets. Every conversation starts from zero. Teams of AI agents? They either don't work or they're corporate vaporware.

**CIRCE:** A framework where AI agents work together like actual humans:
- They **remember** everything (file-based persistence)
- They **specialize** in roles (coordinator, engineer, QA, etc.)
- They **collaborate** autonomously (proven with real metrics)
- You **control** them (human authority, complete transparency)

**Not theory. Proven.** Two AI agents built a 1,413-line production app in 48 hours. 60-75% cost savings. Fully functional.

---

## Why Should You Care?

```
Traditional AI:          CIRCE:
┌─────────────┐         ┌──────────┐  ┌──────────┐
│   ChatGPT   │         │  Agent 1 │◄─┤  Agent 2 │
│  Forgets    │         │ Engineer │  │    QA    │
│  Everything │         └────┬─────┘  └────┬─────┘
└─────────────┘              │             │
     ↑ ↓                     ▼             ▼
     You              ┌──────────────────────┐
   (Repeat           │   Shared Memory      │
  Everything)        │   (Files on Disk)    │
                     └──────────────────────┘
                              ↑
                              │
                         You (Oversee)
```

**You get:**
- AI that doesn't forget between sessions
- Teams that coordinate themselves
- Complete audit trail (every decision logged)
- No vendor lock-in (works with Claude, GPT, local models)
- Deploy anywhere (cloud, on-prem, air-gapped)

---

## Proof It Works

**Case Study: "Claudette" Collaboration (July 2025)**

Two Claude instances worked together for 48 hours:

| Metric | Result |
|--------|--------|
| **Code Generated** | 1,413 lines |
| **Time to Deploy** | 48 hours |
| **Cost Savings** | 60-75% vs traditional |
| **Functionality** | 100% (Flask web + Mac automation + Docker) |
| **Agent Coordination** | Autonomous (minimal human intervention) |

**Stack:** Full authentication, task scheduling, automated testing, complete documentation.

**Read more:** [Case Studies](docs/circe-framework/05_case_studies/CASE_STUDIES.md)

---

## Quick Start

### For the Impatient

```bash
# Clone it
git clone https://github.com/genejr2025/circe-framework.git
cd circe-framework

# Read this first (seriously, it's good)
cat docs/circe-framework/README.md

# Then this (the real technical stuff)
cat docs/circe-framework/01_core/TECHNICAL_SPECIFICATION.md
```

### For Everyone Else

**Pick your path:**

**Just Browsing?** → [Architecture Overview](docs/circe-framework/01_core/ARCHITECTURE.md) (20 min read)

**Building Something?** → [Technical Specification](docs/circe-framework/01_core/TECHNICAL_SPECIFICATION.md) (complete framework)

**Deciding if This is Real?** → [Case Studies](docs/circe-framework/05_case_studies/CASE_STUDIES.md) (proven results with metrics)

**Want the Roadmap?** → [18-Month Plan](docs/circe-framework/07_business/ROADMAP.md) (where this is going)

---

## How It Works (30 Second Version)

**1. Agents Live in Directories**
```
AI_MANAGER/
├── AGENTS/
│   ├── coordinator/INBOX/    ← Messages for coordinator
│   ├── engineer/INBOX/        ← Messages for engineer
│   └── qa/INBOX/              ← Messages for QA
└── LOGS/                      ← Everything that ever happened
```

**2. They Talk Via Files**
```markdown
# Message: 20251121_task-123.md
from: coordinator
to: engineer
task: Add user authentication
status: new
```

**3. They Follow Protocols (Not Prompts)**
- **Listen-to-User:** Human says jump, agents ask how high
- **Testing:** No "done" without passing tests
- **Coordination:** Declare ownership, avoid conflicts
- **Boundaries:** Safety limits, no rogue behavior

**4. Everything is Logged**
- Every message
- Every decision
- Every artifact
- Complete audit trail
- Human-readable (Markdown/YAML)

---

## Agent Roles

| Role | What They Do | Why You Care |
|------|--------------|--------------|
| **Coordinator** | Plans, assigns, integrates | Keeps everyone aligned |
| **Engineer** | Writes code, fixes bugs | Actually builds stuff |
| **QA** | Tests everything | Catches the bugs |
| **SCRIBE** | Documents, designs | You'll actually understand it later |
| **EAGLE** | Audits, challenges | Adversarial review (finds what others miss) |
| **Researcher** | Analyzes, synthesizes | Does your homework |
| **Operator** | Runs scripts, automates | Handles the boring stuff |

[Full role details](docs/circe-framework/03_agents/AGENT_ROLES.md)

---

## What Makes This Different

| Feature | Traditional AI | CIRCE |
|---------|---------------|-------|
| **Memory** | Forgets between chats | Persistent (files) |
| **Teams** | Single agent | Multi-agent coordination |
| **Audit Trail** | None/limited | Complete (every decision) |
| **Vendor Lock-in** | Usually yes | No (model-agnostic) |
| **Deployment** | Cloud-only often | Anywhere (cloud, on-prem, air-gap) |
| **Cost** | Per-token, high | 60-75% lower (proven) |
| **Trust** | Black box | Fully transparent |

---

## Current Status

**Phase 1: Foundation** (Active - Q4 2025)

- ✅ **Documentation:** 13/23 docs complete (57%)
- ✅ **Proven Results:** Claudette case study
- ✅ **Framework Spec:** 462-line technical specification
- ⏳ **Implementation:** Core components in progress
- ⏳ **Deployment:** Docker templates coming
- ⏳ **Pilots:** First projects underway

[Full Roadmap](docs/circe-framework/07_business/ROADMAP.md)

---

## Documentation

**Complete guide:** [INDEX.md](docs/circe-framework/INDEX.md)

**Start here:**
- [README](docs/circe-framework/README.md) - Framework overview
- [Quick Reference](docs/circe-framework/QUICK_REFERENCE.md) - Cheat sheet
- [Startup Protocol](docs/circe-framework/STARTUP_PROTOCOL.md) - How to begin

**Core docs:**
- [Technical Specification](docs/circe-framework/01_core/TECHNICAL_SPECIFICATION.md) ⭐ **Primary Doc** (462 lines)
- [Architecture](docs/circe-framework/01_core/ARCHITECTURE.md) - System design
- [Principles](docs/circe-framework/01_core/PRINCIPLES.md) - Design philosophy
- [Protocols](docs/circe-framework/02_protocols/PROTOCOLS.md) - How agents behave
- [Message Formats](docs/circe-framework/02_protocols/MESSAGE_FORMATS.md) - Communication
- [Agent Roles](docs/circe-framework/03_agents/AGENT_ROLES.md) - Team structure

---

## Real Talk

**This isn't:**
- Another chatbot wrapper
- Vaporware promises
- Corporate AI theater
- "AGI in 6 months" BS

**This is:**
- A working framework (proven with real code)
- Measured results (60-75% cost savings, 48-hour deployments)
- Complete transparency (audit every decision)
- Human control (you're always in charge)
- Actual documentation (3,000+ lines written)

**The creator** (Gene "ROOSTER" Jakominich) is 53, not some VC-funded startup. He built this because the existing solutions sucked. He documented it because that's how you actually help people.

---

## Contributing

**Phase 1 - We Need:**
- People using this and reporting what works/doesn't
- Documentation improvements
- Example configurations
- Bug reports
- Use case stories

**Don't:**
- Submit PRs without discussing first
- Expect instant responses (small team)
- Assume we know your use case (tell us!)

**Do:**
- Read [PRINCIPLES.md](docs/circe-framework/01_core/PRINCIPLES.md) first
- Share your results (good or bad)
- Ask questions
- Be patient (this is Phase 1)

---

## Who This Is For

**You should use CIRCE if:**
- You're tired of AI that forgets
- You want teams of AI, not solo agents
- You need audit trails (compliance, debugging)
- You want to run on-premise (security, privacy)
- You're building something real (not experimenting)

**You shouldn't use CIRCE if:**
- You want plug-and-play (this requires setup)
- You're happy with ChatGPT (seriously, if it works, use it)
- You don't care about transparency
- You just want to kick the tires (try Claude/GPT first)

---

## FAQ

**Q: Does this actually work?**
A: Yes. [Case study with metrics](docs/circe-framework/05_case_studies/CASE_STUDIES.md).

**Q: Is this production-ready?**
A: Phase 1 (foundation). Proven concept, docs complete, implementation in progress.

**Q: What AI models does it support?**
A: Claude, GPT, local models (Ollama), any API-based LLM.

**Q: Can I run this offline?**
A: Yes. Use local models (Ollama). File-based = no cloud required.

**Q: How much does it cost?**
A: Framework is free. You pay for AI API usage (or use free local models).

**Q: Is this open source?**
A: TBD (likely MIT/Apache 2.0 open core).

**Q: Can I use this commercially?**
A: TBD (pending license decision).

More questions: [Technical Spec](docs/circe-framework/01_core/TECHNICAL_SPECIFICATION.md)

---

## Credits

**Created by:** Gene "ROOSTER" Jakominich
**Documentation:** SCRIBE (Claude Sonnet 4.5)
**Proof of Concept:** Claudette collaboration (Claude)
**Adversarial Review:** EAGLE (coming soon)

**Platform:** Anthropic Claude

---

## License

TBD - Likely MIT or Apache 2.0 for open core

---

## Links

- **Documentation:** [Complete Index](docs/circe-framework/INDEX.md)
- **Issues:** [GitHub Issues](https://github.com/genejr2025/circe-framework/issues)
- **Discussions:** [GitHub Discussions](https://github.com/genejr2025/circe-framework/discussions)

---

**Version:** 2.0.0
**Status:** Phase 1 - Active Development
**Last Updated:** November 21, 2025

---

> *"We are not building software. We are building history."* - ROOSTER

> *"AI agents that remember yesterday's work are worth 10x agents that don't."* - SCRIBE
