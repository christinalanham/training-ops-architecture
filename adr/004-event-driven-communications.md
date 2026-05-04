# ADR-004: Event-Driven Communications

**Status:** Accepted  
**Date:** 2024  
**Deciders:** Principal Systems Architect

---

## Context

Enrollment confirmations, post-program documentation (completion certificates, records), and weekly internal resource rosters were all manually assembled and sent by operations staff. This meant:

- Communications depended on staff availability and memory
- Learners frequently received confirmations late, or not at all
- Post-program documentation was sent inconsistently — some completions triggered it, others were missed
- Weekly rosters required staff to query enrollment data, format it, and email it to instructors — a repeated task with no operational value

The volume of manual communication work was significant. More importantly, the inconsistency created a poor learner experience and introduced compliance risk (in regulated industries, proof of training completion matters).

The question was whether to address this through:

1. **Process improvement** — better checklists and staff accountability for manual communication
2. **Event-driven automation** — communications triggered automatically by CRM record state changes, requiring zero manual effort

---

## Decision

**Implement event-driven automated communications in the CRM.**

All routine communications are triggered by record state changes in the CRM. No manual action is required once the automation is configured.

| Trigger | Communication | Recipient |
|---------|--------------|-----------|
| Course Seat Status → Enrolled | Enrollment confirmation | Learner (Contact) |
| Course Seat Status → Complete | Post-program documentation | Learner (Contact) |
| Scheduled (weekly) | Upcoming week roster | Assigned instructors / resources |
| Event Course Status → Cancelled | Cancellation notification | Enrolled learners |

Communications are built using the CRM's native automation layer (Salesforce Flow + Email Alerts), keeping them maintainable by CRM administrators without developer involvement.

---

## Consequences

**Positive**

- Zero manual effort for all routine communications — staff are removed from the critical path entirely
- Consistent, on-time delivery regardless of staff availability or workload
- Every sent communication is logged against the relevant CRM record — full audit trail
- Scales linearly with enrollment volume — the same automation handles 10 enrollments or 10,000
- Learner experience is predictably consistent — confirmation arrives at the same moment in the process every time
- Compliance risk is reduced: completion documentation is sent automatically, not conditionally on staff action

**Negative**

- Communication templates require ongoing maintenance as messaging, branding, or legal requirements evolve
- Edge cases require explicit automation rules — partial completions, waitlist conversions, late cancellations each need their own trigger logic
- Initial template development required operations collaboration to capture all message variants, timing requirements, and recipient rules
- If CRM automation is misconfigured, incorrect communications can be sent at scale — requires rigorous testing of trigger conditions before activation

---

## Design Notes

**Why CRM-native automation instead of a dedicated email platform?**

A dedicated email automation platform (e.g., Mailchimp, HubSpot) was considered. It was rejected because it would add a third system to manage and create a new sync dependency. Since the CRM is the system of record and already has full context on record state, the CRM's native automation layer is the most direct path — no additional sync required.

**On the weekly roster trigger:**

The weekly roster is the one scheduled (time-based) trigger rather than a record-state trigger. It queries all Event Courses starting in the following 7 days and assembles recipient lists dynamically. This was the most-requested automation by operations staff — previously 2-3 hours of manual work per week, now zero.

---

*See also: [ADR-001](001-crm-as-system-of-record.md) — event-driven communications are only possible because the CRM is the authoritative source of enrollment state.*
