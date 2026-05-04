# Training Operations Architecture
### Enterprise ILT Platform Redesign — Architecture Case Study

---

## Overview

This repository documents the architecture for redesigning a fragmented, manual Instructor-Led Training (ILT) operations platform into an integrated, automated ecosystem.

**Phase 1 outcome: 2,000+ annual labor hours recovered.**

The work is organized as Architecture Decision Records (ADRs), system diagrams, and phased roadmap documentation. It is intended as a professional reference — the thinking behind the architecture, not the implementation.

---

## The Problem

The organization operated three disconnected systems with no shared source of truth:

| System | Role | Pain Point |
|--------|------|------------|
| CRM (Salesforce) | Customer & sales data | No training-specific data model |
| eCommerce Platform | Course purchasing | Manual inventory sync, no CRM awareness |
| LMS | Learning delivery | Opaque data, no reportability or integration |

Critical operational data lived in tribal knowledge and offline spreadsheets: course schedules, instructor assignments, seat availability, qualification requirements, exam records.

Every enrollment required manual data entry in multiple systems. Every communication was manually triggered. Every week, staff hand-assembled rosters and sent them to instructors.

See: [Current State Diagram](diagrams/current-state.mmd)

---

## The Architecture

The redesign established the **CRM as the authoritative system of record** for all training operations. Downstream systems receive synchronized data; they do not originate it.

See: [Target State Diagram](diagrams/target-state.mmd)

### Custom Data Model

Six custom objects were created to bring tribal knowledge into structured, reportable records:

| Object | Purpose |
|--------|---------|
| **Course** | Master catalog of training offerings |
| **Event Course** | A scheduled instance of a Course (date, location, capacity) |
| **Course Seat** | Individual enrollment — links a Contact to an Event Course |
| **Course Booking** | Exam-specific enrollment with time allowance per exam |
| **Qualification Category** | Credential and certification framework |
| **Exam** | Assessment records tied to Qualification Categories |

See: [Data Model Diagram](diagrams/data-model.mmd)

### Integrations

**CRM → eCommerce (unidirectional sync)**
- New Event Course created → listing automatically published to eCommerce
- Seat count updates → inventory synced automatically
- Cancellation → listing removed or flagged
- Purchase on eCommerce → Course Seat created in CRM

**CRM → Automated Communications (event-driven)**
- Enrollment confirmed → learner receives confirmation
- Program marked complete → learner receives post-program documentation
- Weekly schedule trigger → assigned resources receive upcoming roster

See: [Integration Flow Diagram](diagrams/integration-flow.mmd)

---

## Architecture Decision Records

| ADR | Decision | Status |
|-----|----------|--------|
| [ADR-001](adr/001-crm-as-system-of-record.md) | CRM as System of Record | ✅ Accepted |
| [ADR-002](adr/002-custom-objects-over-lms-extension.md) | Custom Domain Objects over LMS Extension | ✅ Accepted |
| [ADR-003](adr/003-unidirectional-sync-pattern.md) | Unidirectional Sync Pattern (CRM → eCommerce) | ✅ Accepted |
| [ADR-004](adr/004-event-driven-communications.md) | Event-Driven Communications | ✅ Accepted |

---

## Phase 1 Outcomes

| Before | After |
|--------|-------|
| 3 siloed systems, no shared truth | CRM as single source of truth |
| Critical data in spreadsheets and tribal knowledge | Structured, reportable custom objects |
| Multi-system manual data entry for every enrollment | Single entry point, automated downstream sync |
| Manual enrollment communications | Zero-touch automated comms on record state changes |
| Weekly rosters assembled and sent by hand | Automated weekly distribution |
| **2,000+ labor hours consumed annually** | **2,000+ labor hours recovered annually** |

---

## Roadmap

| Phase | Focus | Status |
|-------|-------|--------|
| [Phase 1 — Foundation](phases/phase-1-foundation.md) | CRM as System of Record, custom data model, integration, automated comms | ✅ Complete |
| [Phase 2 — Reporting & Analytics Layer](phases/phase-2-reporting-analytics.md) | Operational dashboards, KPI tracking, data quality framework | 🔄 In Progress |
| [Phase 3 — Self-Service & Automation Expansion](phases/phase-3-self-service.md) | Learner self-service, expanded automation, reduced ops touchpoints | 📋 Planned |
| [Phase 4 — AI-Augmented Operations](phases/phase-4-ai-augmentation.md) | Predictive scheduling, AI-assisted compliance, intelligent routing | 📋 Planned |

---

## Tech Stack

- **CRM:** Salesforce (Sales Cloud, custom objects, Apex, Flow automation, Lightning Web Components)
- **Integration layer:** Celigo, Zapier
- **Design approach:** AI-assisted architecture using Claude and Google Gemini for design validation, pattern analysis, and documentation

---

## How to Use This Repository

- **ADRs** capture the reasoning behind key decisions — start here to understand why the architecture looks the way it does
- **Diagrams** are written in [Mermaid](https://mermaid.js.org/) — render them in any Mermaid-compatible viewer or GitHub's built-in renderer
- **Phase documentation** describes what was built and what's planned — useful for understanding scope and sequencing

---

*This is an architecture case study. Implementation details are proprietary. Patterns, data models, and design decisions are documented as professional reference material.*
