# Diego Ataide Rodrigues

Applied AI & Infrastructure Automation Engineer

[diegoarod@gmail.com](mailto:diegoarod@gmail.com)

> Note: Due to internal confidentiality constraints at Google, proprietary source code, internal URLs, and internal screenshots are omitted. The case studies below describe the architecture, system design patterns, safety guardrails, and evaluation pipelines I designed and shipped personally in production environments.

## About Me

I am an applied AI and infrastructure automation engineer with 10+ years of experience building production systems at Google. My work sits at the intersection of machine learning operations, distributed systems, and human-in-the-loop automation.

I design and ship systems that combine:
- LLM orchestration and multi-agent workflows
- deterministic programmatic control planes
- safety-first execution patterns
- observability, evaluation pipelines, and continuous learning loops
- backend automation for high-scale operational environments

My focus is on building reliable AI systems that do real work in production: routing complex tasks, reducing operational toil, improving decision quality, and keeping execution safe and transparent.

---

## Selected Case Studies

### 1) Autonomous Multi-Agent Triage & Diagnostic Orchestrator

#### Overview
I designed, implemented, tested, and shipped an internal multi-agent AI triage platform that orchestrated 18 specialized domain agents and 60+ tool skills across operational workflows. The system integrated with MCP-based tools, CLI interfaces, a web command center, chat-based diagnostics, and an evaluation pipeline for quality monitoring.

#### Problem
Production issue queues spanned multiple domains, including:
- optical transport
- capacity planning
- hardware logistics
- software and service workflows

Manual triage required engineers to cross-reference several telemetry and ticket systems per incident. Traditional single-prompt LLM assistants were not reliable enough for real operations because they often hallucinated root causes or posted noisy, unvetted updates to live production workflows.

#### Architecture
```mermaid
flowchart LR
    In["Inbound Production Issues\n& Telemetry Context"] --> Dedup["Context Compression,\nBacklog Watermarking\n& Fast-Path Router"]
    Dedup --> Hub["Dynamic Routing Engine\n(18 Domain-Specialist Agents\n+ 60 MCP / CLI Tool Skills)"]
    Hub --> P1["Phase 1: Silent Pre-Evaluation\nZero Public Ticket Mutations\nPrivate Chat Diagnostic Brief"]
    P1 --> Gate{"Human-in-the-Loop\n1-Click Approval\nor Override"}
    Gate -- "Approved" --> P2["Phase 2: Validated Execution\nStakeholder-Safe 1-Line Summary\n& Automated Routing / Closure"]
    Gate -- "Edited / Overridden" --> Learn["Continuous-Learning Ledger\nCaptures User Preference\nfor Future Runs"]
    P2 --> Eval["Automated Post-Closure\nLLM Grading & SLA Dashboard"]
