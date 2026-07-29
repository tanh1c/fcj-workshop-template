---
title: "Event 4"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

## Event 4: AWS Vietnam Community Meetup: AI Revolution, Open-Source Agents & Engineering Excellence

| Field | Information |
|---|---|
| Event name | AWS Vietnam Community Meetup: AI Revolution, Open-Source Agents & Engineering Excellence |
| Date | Saturday, 25/07/2026 |
| Time | 08:30–12:00; check-in from 08:30; program started at 09:00 |
| Location | AWS Hà Nội, 7th Floor, Grand Terra Tower, 36 Cát Linh, Đống Đa, Hà Nội |
| Role | In-person attendee |
| Main focus | AI-assisted software delivery, business value, open-source agent runtimes, and AI-native infrastructure |

## Overview and Objectives

The **AWS Vietnam Community Meetup: AI Revolution, Open-Source Agents & Engineering Excellence** connected software engineering, business adoption, open-source AI agents, and infrastructure modernization. The sessions examined how teams can use AI to improve delivery without transferring engineering responsibility to autonomous systems.

The event had four main objectives:

- Analyze bottlenecks across the software-delivery lifecycle, especially the difference between the individual Inner Loop and the shared Outer Loop.
- Connect the evolution from AI models and generative AI to agents and digital coworkers with measurable business value.
- Examine the architecture, memory design, and security risks of open-source agent runtimes such as OpenClaw.
- Explain how infrastructure engineering is moving from manual operations and conventional Infrastructure as Code toward AIOps, FinOps, AI security, and supervised agentic infrastructure.

## Speakers and Roles

| Speaker | Role / Topic |
|---|---|
| Henry (Đức) Bùi | Head of Engineering at CloudThinker; AI-assisted engineering excellence and the Outer Loop |
| Nguyễn Thu (Yuna) | AI Solutions and Sales Track consultant in the AWS Vietnam community; AI adoption and business value |
| Tuấn Vũ | AWS Community Builder and AWS Vietnam User Group representative; open-source AI agents and OpenClaw |
| Nam Lã | Cloud Engineer at Cloudino and AWS Vietnam User Group Admin; AI-native infrastructure and the infrastructure-engineering role |

## Key Sessions and Knowledge Gained

### Ship Fast with AI, Not by AI

AI can make writing code much faster without making the complete product-delivery process equally fast. When code generation accelerates, the bottleneck often moves to review, integration, testing, deployment, and operation.

The session separated software development into two loops:

- **Inner Loop:** write, run, and debug locally. AI can compress this individual loop significantly.
- **Outer Loop:** review, integrate, test, deploy, and operate. This shared team loop determines whether software reaches users safely and reliably.

A strong Outer Loop encodes engineering knowledge in version-controlled assets such as lint rules, automated checks, agent skills, repository instructions such as `CLAUDE.md`, and living documentation. Critical code should receive more verification, not merely more generated code. The central message was to build a **superpowered engineer**, not a superpowered agent: humans remain responsible for authorship and decisions while AI expands verification capacity.

### From AI Trends to Business Value

The business session described an evolution from conventional AI to generative AI, AI agents, and **digital coworkers**. Tools such as ChatGPT, Claude, Amazon QuickSight/Q, NotebookLM, and Gemini can support daily workflows, but their value must be measured through outcomes rather than novelty.

In Sales and Marketing, AI can assist with customer-data analysis, lead scoring, personalized communication, workflow automation, and revenue optimization. The lesson was that enterprise adoption succeeds when AI improves a defined process and produces measurable value such as higher productivity, conversion, or return on investment.

### OpenClaw and Open-Source Agent Runtimes

OpenClaw was presented as an **agent runtime**, not a single application. It represents a broader shift toward conversational software in which applications become composable capabilities and systems are increasingly assembled from models, tools, memory, and orchestration components.

Reliable agent memory requires more than a growing prompt. A practical memory architecture separates short-term and long-term information, can represent relationships through knowledge graphs, removes irrelevant information, and preserves privacy boundaries.

Capability growth also increases security exposure. Prompt injection, excessive tool permissions, memory leakage, and unintended system-code execution remain important risks. Open-source agent deployments therefore need scoped permissions, action audit logs, explicit trust boundaries, and rollback plans.

### AI-Native Infrastructure

The infrastructure session explained the progression from conventional Infrastructure as Code to AI-assisted and agentic operations. AI can generate Terraform or AWS CDK from natural-language requirements, review infrastructure pull requests, detect configuration drift, troubleshoot systems, and derive architecture diagrams from code.

Relevant AWS capabilities include Amazon Q Developer for coding and operations, Amazon Bedrock for building AI applications and agents, Amazon CloudWatch and GuardDuty for observability and intelligent risk detection, and AWS Cost Optimization Hub for cost recommendations.

The infrastructure engineer's role is consequently moving from repetitive manual operation toward designing constraints and supervising self-running systems. This transition still requires human review because automated infrastructure actions affect security, reliability, and cost.

## Key Takeaways

### Design Mindset

- Ship **with AI, not by AI**: AI should strengthen testing and verification rather than replace engineering accountability.
- Treat software as assembled capabilities when integration is safer and more efficient than rebuilding every component.
- Keep humans responsible for architecture, critical decisions, and release approval.

### Technical Architecture

- Invest in the Outer Loop through repository instructions, lint rules, automated tests, test harnesses, and versioned documentation.
- Give agents scoped permissions and retain audit logs for every consequential action.
- Use rollback plans and explicit safety boundaries before agents can modify code or infrastructure.
- Build structured, layered memory rather than relying only on unbounded conversational prompts.

### Modernization Strategy

- Adopt AIOps and FinOps incrementally for anomaly detection, configuration review, incident support, and cost optimization.
- Introduce agents into business workflows as governed digital coworkers with measurable objectives.
- Treat agentic infrastructure as a supervised operating model, not permission for uncontrolled automation.

## Connection to My Internship Project

The event directly reinforced the design of my **AI Coding Agent Risk Scoring on AWS SageMaker** project. The project evaluates the full trajectory of an agent run rather than only the generated code, reflecting the session's emphasis on the Outer Loop. Features such as tests passed, lint status, tool-sequence validity, evidence-supported summaries, and destructive-command detection measure whether a coding task was verified and delivered responsibly.

The security discussion also matches the project's hybrid decision policy. Model scores do not replace deterministic rules for destructive commands or sensitive files, and risky outcomes remain subject to human review. This is consistent with scoped tool access, auditability, and explicit release governance.

Finally, the event's focus on human responsibility supports the project's Registry boundary: passing `risky_recall >= 0.85` permits registration only. Model approval and deployment remain separate manual decisions, just as AI-assisted infrastructure should remain under engineering supervision.

## Event Experience

The meetup combined engineering depth with practical business and infrastructure perspectives. The Inner Loop and Outer Loop comparison made a common AI-adoption problem concrete: generating code faster does not automatically improve delivery speed or reliability.

The OpenClaw analysis clarified the difference between a chatbot and an agent runtime, especially the importance of tools, memory, permissions, and auditability. The infrastructure discussion provided a practical view of how cloud engineers can adapt to AI by becoming stronger system designers and supervisors rather than treating automation as a replacement for engineering judgment.

## Lessons Learned

- Accelerating verification and testing can create more value than accelerating code generation alone.
- Agent access to tools and infrastructure must be protected by scoped permissions, audit logs, and rollback controls.
- Structured memory improves long-running agent behavior but creates additional privacy and security responsibilities.
- AIOps and AI-driven Infrastructure as Code should be introduced incrementally with human review.
- Systems thinking, architecture, and critical reasoning remain core engineering capabilities regardless of AI progress.

## Participation Evidence

I attended this meetup in person at AWS Hà Nội on 25/07/2026. The two photos below document my participation at the event venue.

![In-person participation evidence at the AWS Vietnam Community Meetup](/images/events/IMG_4749.jpeg)

*In-person participation at AWS Hà Nội during the meetup.*

![AWS Vietnam Community Meetup venue evidence](/images/events/IMG_4750.jpeg)

*The event venue and activities during the meetup.*

---

[Previous](/4-eventparticipated/4.3-event3/) | [Back to Events Participated](/4-eventparticipated/)
