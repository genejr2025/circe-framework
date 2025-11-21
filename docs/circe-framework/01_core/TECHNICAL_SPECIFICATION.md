
# CIRCE: Collaborative Intelligence & Reflexive Cognitive Emergence  
## Technical Framework Edition (Full Research Draft)  
**Version 1.0 — November 2025**

**Author**  
**Gene “ROOSTER” Jakominich Jr.**  
*TechForce Holdings / True North AI — CIRCE Research Division (2025)*

---

## Abstract

CIRCE is a practical framework for autonomous **multi‑agent AI collaboration** with human guidance. It combines a **circular learning** model, a **file‑based blackboard protocol** for distributed coordination, **persistent session continuity**, and **protocol‑driven development and testing**. CIRCE is model‑agnostic and deployable on‑premise, enabling teams of AI agents to plan, build, verify, and document complex enterprise tasks. Case studies include (1) an AI‑to‑AI developed computer‑control web application and (2) a multi‑agent emergency response planning prototype (“DISPATCH”). We present a reference architecture, packaging strategy, and a roadmap toward production. We explicitly avoid claims of AI sentience; emergent social behaviors are acknowledged as artifacts of structured interaction.

**Keywords:** multi‑agent systems; agent orchestration; circular learning; persistent memory; file‑based coordination; testing protocols; on‑premise AI; enterprise AI; HCI

---

## 1. Introduction

### 1.1 Motivation
Agentic AI has progressed from single‑agent goal loops (e.g., AutoGPT) to **multi‑agent teams** with role specialization and tool‑use. Despite exciting demos, gaps remain in **long‑term memory**, **reliability**, **observability**, and **human‑in‑the‑loop** governance. CIRCE addresses these gaps by emphasizing (i) durable **collaboration artifacts** (files, logs, tests), (ii) **protocols** that force quality and alignment, and (iii) human‑compatible workflows including support for high context‑switch work patterns.

### 1.2 Contributions
This work contributes:

1. **Circular Learning**: a human–AI co‑evolution loop that compounds across sessions.  
2. **File‑Based Blackboard**: a simple, transparent medium for asynchronous multi‑agent messaging and state.  
3. **Persistent Session Continuity**: long‑lived, human‑readable memory enabling identity‑like specialization.  
4. **Protocol‑Driven Development**: Listen‑first, Testing, Coordination, and Boundary protocols that raise reliability.  
5. **Reference Implementations**: autonomous computer‑control app; DISPATCH SAR planning prototype.  
6. **Portable Packaging**: on‑premise friendly, Dockerized, model‑agnostic stack with auditable logs.

### 1.3 Scope
CIRCE is a **framework specification** and a set of implementation patterns. We compare against contemporaries and provide a concrete on‑prem deployment recipe. We explicitly **avoid** claims about AI sentience or consciousness and treat observed behaviors through an engineering lens.

---

## 2. Background and Related Work

### 2.1 Single‑Agent Autonomy
**AutoGPT** and **BabyAGI** pioneered goal‑seeking agents that plan, act, and evaluate in a loop using an LLM core and tools [1–2]. These systems highlighted the promise of autonomous task completion but struggled with **looping**, **hallucinated success**, and **state loss** over long horizons.

### 2.2 Multi‑Agent Collaboration
Research and tooling shifted toward **multi‑agent** designs. **CAMEL** formalized *communicative agents* that role‑play with constrained goals [3]. **AutoGen** (Microsoft) introduced *conversable agents* with programmatic message passing and optional human participation [4]. **CrewAI** and **MetaGPT** productized the idea of role‑based AI teams; **MetaGPT** demonstrated SOP‑driven software teams and received significant academic attention [5–7]. **LangGraph** extended LangChain with graph‑structured, stateful controller patterns for agents [8].

### 2.3 Persistent Memory, Observability, Governance
Enterprise adoption requires **durable memory**, **testing**, **observability**, and **guardrails**. Toolkits and platforms (e.g., LangChain, Vellum) provide partial solutions (memory stores, evaluation, dashboards) but often lack a **simple, portable** path to long‑lived state with **human‑legible** artifacts.

**CIRCE** differentiates by treating the **filesystem as the shared memory**, forcing artifacts to be durable, reviewable, and trivially auditable.

---

## 3. The CIRCE Framework

### 3.1 Design Principles
- **Persistence‑first:** Default to writing plans, results, and tests to disk.  
- **Protocol over Prompting:** Enforce habits (listen, test, coordinate) rather than rely on best‑effort prompting.  
- **Human‑legible by design:** Logs and artifacts are text/markdown; no black‑box hidden state.  
- **Asynchronous autonomy:** Agents run independently, coordinating via files; no central brain required.  
- **Model‑agnostic:** Mix proprietary and local models; isolate model choice behind agent interfaces.  
- **On‑premise friendly:** Minimal infra (shared filesystem), straightforward Docker packaging.

### 3.2 Circular Learning
CIRCE implements **mutual scaffolding**: agents produce, peers review, humans guide; feedback becomes durable artifacts that feed the next cycle. Over time, the team’s capability **compounds**. Practically this means:
- Every deliverable is saved (drafts, critiques, fixes).  
- Lessons learned are documented and re‑used.  
- Roles naturally **specialize** as agents accrue domain context.

### 3.3 File‑Based Blackboard Protocol
CIRCE uses a structured directory tree (see Appendix A). Core concepts:
- **Hub**: `AI_MANAGER/` hosts shared queues (NEW_MESSAGES, TASKS).  
- **Agent Inboxes**: each agent has `INBOX/` and a private workspace.  
- **Messages**: Markdown files with headers: `from`, `to`, `intent`, `status`, `artifacts`.  
- **Ownership**: tasks assigned explicitly; status tracked via small state files.  
- **Durability**: everything is append‑only and timestamped for auditability.

This yields persistence, parallelism, and **total traceability** without brokers or databases.

### 3.4 Persistent Session Continuity
At agent startup:
1. Read **recent logs** and **agent workspace** to reconstruct context.  
2. Load **open tasks**; claim or wait based on role.  
3. Generate a **status brief** so humans can observe current state.

Identity‑like behavior emerges because each agent grows a consistent workspace and history.

### 3.5 Protocol‑Driven Development
- **Listen‑to‑User Protocol:** always address explicit human guidance first (confirm, clarify, act).  
- **Testing Protocol:** tasks cannot be “✅ complete” until **automated tests pass**; failing tests trigger diagnosis, fix, and full re‑run.  
- **Coordination Protocol:** avoid conflicting edits; declare ownership; post status.  
- **Boundary Protocols:** respect privacy, safety, and scope policies; prefer safe alternatives on refusal.

### 3.6 Roles and Specialization
Common roles:
- **Coordinator** (plans, assigns, integrates)  
- **Engineer** (implements code/infra)  
- **QA** (tests, verification, benchmarking)  
- **Researcher/Analyst** (finds data, synthesizes)  
- **Writer** (docs, reports, comms)  
- **Operator** (automation, RPA, OS actions)

Roles can be static or **emergent** through consistent ownership and artifacts.

---

## 4. Reference Implementations (Case Studies)

### 4.1 Autonomous Computer‑Control Application
Two agents collaborated (backend engineer + UI/QA) to build a Flask‑based web app and a macOS automation agent, fully containerized. Features:
- Authenticated web UI; task queue and job status.  
- Mac agent with controlled actions (open apps, screenshots, keystrokes).  
- Docker Compose packaging; environment templating.  
- **Outcome:** ~48 hours of AI‑to‑AI development; >1,400 LOC; robust tests; complete artifact trail.

### 4.2 DISPATCH: Emergency Response Planning
A specialized agent team (development, research, QA, documentation, comms) coordinated SAR planning. Results in simulation:
- **87–92% reduction** in plan time versus manual baselines (internal benchmarks).  
- Majority built using open/low‑cost models; persistent knowledge artifacts.  
- Clear hand‑offs and role separation; rapid scenario iteration.

---

## 5. Comparison to Contemporary Frameworks

### 5.1 AutoGPT / BabyAGI (single‑agent loops)
**Strengths:** autonomy, tool‑use, planning.  
**Limitations:** looping, poor long‑horizon memory, unreliable “done” detection.  
**CIRCE:** mitigates via **multi‑agent peer review**, **Testing Protocol**, and **durable memory** [1–2].

### 5.2 CAMEL / AutoGen (conversational multi‑agent)
**Strengths:** conversational coordination; human‑in‑the‑loop; role play [3–4].  
**Limitations:** session‑bound state; orchestration complexity at scale.  
**CIRCE:** uses a **filesystem blackboard** for cheap persistence, better auditability.

### 5.3 LangChain / LangGraph (tooling + state graphs)
**Strengths:** rich tool ecosystem; graph‑based control flow; memory plugins [8].  
**Limitations:** durable, human‑legible archives not the default; infra overhead.  
**CIRCE:** **text/markdown archives** by default; trivial version control; on‑prem first.

### 5.4 MetaGPT / CrewAI (role‑based software teams)
**Strengths:** SOPs; role templates; product focus [5–7].  
**Limitations:** opinionated for software dev; rigidity; less persistence transparency.  
**CIRCE:** protocol‑lightweight, domain‑agnostic; roles can be emergent; easy to hybridize.

---

## 6. Deployment and Packaging

### 6.1 On‑Premise Reference Architecture
- **Shared Storage**: NFS/NAS (e.g., `/srv/circe/AI_MANAGER`)  
- **Agent Containers**: one container per role; mount shared volume read/write  
- **Eventing**: inotify/file watchers or polling loops  
- **Operator UI**: lightweight web dashboard for tasks/logs/status  
- **Security**: per‑agent service accounts; whitelisted OS actions; network egress controls

### 6.2 Packaging
- **Docker Compose** stack for quick start (agents + dashboard).  
- **Install scripts** for bare‑metal/VM.  
- **Model adapters** for OpenAI/Anthropic APIs and local engines (Ollama/Transformers).  
- **Protocol bundle**: Markdown protocol files, testing scripts, templates.

### 6.3 Governance and Observability
- **Audit trail**: all messages and artifacts on disk; optional git versioning.  
- **Watchdog**: detect stalls/loops; escalate or reset agents gracefully.  
- **Permission model**: declarative capabilities per agent (Appendix D).  
- **Red‑team tests**: adversarial prompts, failure injections, chaos runs.

---

## 7. Implementation Details

### 7.1 Message Schema (Markdown)
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
```

### 7.2 Directory Layout (Core)
```
AI_MANAGER/
├─ NEW_MESSAGES/
├─ TASKS/
├─ LOGS/
└─ AGENTS/
   ├─ coordinator/
   │  ├─ INBOX/  └─ WORKSPACE/
   ├─ engineer/
   │  ├─ INBOX/  └─ WORKSPACE/
   └─ qa/
      ├─ INBOX/  └─ WORKSPACE/
```

### 7.3 Agent Loop (Pseudo‑Code)
```python
while True:
    msgs = read_inbox()
    if not msgs:
        sleep(SHORT) ; continue
    for m in msgs:
        ack(m)
        if requires_human(m): escalate(m) ; continue
        plan = propose_plan(m)
        write_workspace(plan)
        if is_code_task(m):
            write_tests_first(m)
            impl = implement(m)
            results = run_tests("--full")
            if results.fail: fix_and_rerun()
        reply = craft_reply(m, artifacts, status="done")
        post(reply)
```

### 7.4 Testing Protocol (Shell Outline)
```bash
#!/usr/bin/env bash
set -euo pipefail
echo "[CIRCE] Running full test suite..."
pytest -q --maxfail=1
coverage run -m pytest && coverage report --fail-under=90
echo "[CIRCE] Linting..."
ruff check . && mypy --strict
echo "[CIRCE] ✅ All gates passed."
```

---

## 8. Roadmap

### 8.1 Phase 1 (0–3 months)
- Modularize into **CommManager**, **AgentManager**, **MemoryManager**.  
- Scale to 3–5 agents; add **Writer** and **Analyst** roles.  
- Add **summarization + archival** for long logs; optional vector search.  
- Ship **minimal dashboard** (tasks, agents, live log view).  
- Pilot in **IT automation** and **weekly knowledge synthesis**.

### 8.2 Phase 2 (3–9 months)
- Integrate **policy engine** for per‑action authorization.  
- Add **benchmark harness** (latency, cost, quality metrics).  
- Pluggable **graph controllers** (LangGraph) beneath file protocol.  
- **Distributed deployments** across hosts; S3‑backed logs with write‑ahead.

### 8.3 Phase 3 (9–18 months)
- **Open‑core release** + enterprise add‑ons (monitoring UI, SSO, RBAC).  
- **Marketplace** of role packs (Dev, Ops, PM, Finance, Legal).  
- **Cross‑org federations** with signed message exchange.  
- Compliance packs (SOX, HIPAA, CJIS) with audit exports.

---

## 9. Discussion

CIRCE’s persistence‑first, protocol‑driven approach has three implications:  
1) **Reliability** improves because “done” means “tested and logged.”  
2) **Legibility** enables audit, debugging, and incremental onboarding.  
3) **Scalability** emerges from asynchronous, decoupled agents coordinating with simple primitives (files).

Limitations include log growth, potential file‑locking contention at scale, and the need for summarization policies. We expect hybrid designs (files + DB/message bus) to evolve without losing the core benefit: **human‑readable, durable state**.

---

## 10. Conclusion

CIRCE demonstrates that **teams of AI agents**, under lightweight protocols and durable memory, can deliver complex work quickly and verifiably. The framework is portable, auditable, and friendly to on‑prem deployments. As organizations move from single assistants to **AI teams**, CIRCE offers a practical blueprint to realize productivity gains while preserving control and oversight.

---

## Acknowledgments
Thanks to the CIRCE collaborators and early pilot users for stress‑testing the protocols and deployments.

---

## References

[1] Significant Gravitas. (2023). *AutoGPT*. GitHub repository.  
[2] Nakajima, Y. (2023). *BabyAGI*. GitHub repository and notes.  
[3] Li, G., et al. (2023). *CAMEL: Communicative Agents for “Mind” Exploration*. arXiv preprint.  
[4] Wu, T., et al. (2023). *AutoGen: Enabling Next‑Gen LLM Applications via Multi‑Agent Conversation*. Microsoft Research (arXiv/tech report).  
[5] Qian, C., et al. (2024). *MetaGPT: Meta Programming for A Multi‑Agent Collaborative Framework*. ICLR 2024.  
[6] MetaGPT Team. (2024–2025). *MetaGPT X* (product docs).  
[7] CrewAI. (2024–2025). *CrewAI Framework* (docs and examples).  
[8] LangChain Team. (2024–2025). *LangGraph: State Graphs for Agents* (docs/whitepaper).  
[9] Vellum. (2025). *Agent Observability and Evaluation in Production* (blog/whitepaper).

---

## Appendix A — Directory & Files

```
/srv/circe/AI_MANAGER
├─ NEW_MESSAGES/          # Unclaimed directives and inter‑agent requests
├─ TASKS/                 # Open tasks with state subfiles
├─ LOGS/                  # Chronological message/app events
└─ AGENTS/
   ├─ coordinator/
   │  ├─ INBOX/           # Incoming msgs for coordinator
   │  └─ WORKSPACE/       # Plans, integration notes, summaries
   ├─ engineer/
   │  ├─ INBOX/
   │  └─ WORKSPACE/
   └─ qa/
      ├─ INBOX/
      └─ WORKSPACE/
```

**Message filename convention:** `YYYYMMDD_HHMMSS_from-to_intent_taskid.md`

**Task state files:** `taskid.status`, `taskid.owner`, `taskid.checks`

---

## Appendix B — Sample `docker-compose.yml`

```yaml
version: "3.9"
services:
  coordinator:
    build: ./agents/coordinator
    volumes:
      - /srv/circe/AI_MANAGER:/workspace/AI_MANAGER
    environment:
      - AGENT_ROLE=coordinator
      - MODEL_BACKEND=openai:gpt-4o
    restart: unless-stopped

  engineer:
    build: ./agents/engineer
    volumes:
      - /srv/circe/AI_MANAGER:/workspace/AI_MANAGER
    environment:
      - AGENT_ROLE=engineer
      - MODEL_BACKEND=ollama:llama3
    restart: unless-stopped

  qa:
    build: ./agents/qa
    volumes:
      - /srv/circe/AI_MANAGER:/workspace/AI_MANAGER
    environment:
      - AGENT_ROLE=qa
      - MODEL_BACKEND=anthropic:claude-3-haiku
    restart: unless-stopped

  dashboard:
    build: ./dashboard
    ports: ["8080:8080"]
    volumes:
      - /srv/circe/AI_MANAGER:/app/AI_MANAGER:ro
    environment:
      - READ_ONLY=true
    restart: unless-stopped
```

---

## Appendix C — Agent Config (YAML)

```yaml
role: engineer
model:
  provider: ollama
  name: llama3
capabilities:
  tools:
    - bash
    - pytest
    - docker
  permissions:
    - write_workspace
    - read_shared
    - post_messages
protocols:
  listen_first: true
  testing_required: true
  coordinate_before_write: true
limits:
  max_tokens: 4096
  cost_budget_usd: 5.00
```

---

## Appendix D — Permission Model (Declarative)

```yaml
actions:
  os_open_app:
    allowed: false
  os_read_file:
    allowed: true
    paths: ["/workspace/AI_MANAGER/**"]
  os_write_file:
    allowed: true
    paths: ["/workspace/AI_MANAGER/AGENTS/engineer/WORKSPACE/**"]
  net_http_get:
    allowed: true
    domains: ["intranet.local", "docs.company.com"]
  net_http_post:
    allowed: false
gates:
  requires_human_review:
    - "modify_production_config"
    - "delete_data"
```

---

## Appendix E — Minimal Dashboard API (Sketch)

```python
from fastapi import FastAPI
from pathlib import Path

app = FastAPI()
BASE = Path("/srv/circe/AI_MANAGER")

@app.get("/tasks")
def list_tasks():
    return sorted((BASE / "TASKS").glob("*.status"))

@app.get("/logs")
def stream_logs():
    # naive tail
    with open(BASE / "LOGS" / "events.log") as f:
        return f.read().splitlines()[-500:]
```

---

## Appendix F — Failure Modes and Mitigations

- **Runaway loops:** watchdog detects repeated no‑op messages; triggers cooldown or requires human ack.  
- **Log bloat:** periodic summarization and archival to cold storage; vector index for retrieval.  
- **Write conflicts:** per‑task ownership files + advisory locks.  
- **Tool abuse:** permission model (Appendix D) and dry‑run mode for sensitive ops.  
- **Model drift/cost spikes:** budget limits; policy to fall back to local models.

---

## Appendix G — Evaluation & Benchmarks (Template)

- **Quality:** pass rates on test suites; bug density over time.  
- **Latency:** end‑to‑end task completion time; agent wait times.  
- **Cost:** $/task, $/LOC for code; cloud vs local breakdown.  
- **Reliability:** recovery from restarts; persistence correctness.  
- **Human Effort:** review time vs. traditional baselines.

---
