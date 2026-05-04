# Phase 3 — Self-Service & Automation Expansion

**Status:** 📋 Planned  
**Depends on:** Phase 1 (data model, integration), Phase 2 (reporting baseline)

---

## Objective

Reduce the operations team's involvement in routine transactions by enabling learners, accounts, and instructors to self-serve. Expand automation coverage to handle edge cases and higher-complexity workflows that Phase 1 left as manual.

---

## Problem Statement

Phase 1 automated the core lifecycle. Phase 2 made it visible. Phase 3 addresses the remaining manual touchpoints:

- Learners cannot see their own enrollment history, completion records, or qualification status — they must ask operations
- Account administrators cannot see bookings, seat utilization, or upcoming training for their organization — they must ask operations
- Instructors have limited visibility into their own schedule outside of the weekly automated roster
- Edge cases (waitlist movements, rebookings, partial completions) still require manual intervention
- The organization cannot scale enrollment volume without scaling the operations headcount — unless self-service absorbs the routine work

---

## Planned Deliverables

### Learner Self-Service Portal

An Experience Cloud (or equivalent) portal where learners can:

- View their enrollment history and upcoming courses
- Download completion certificates and post-program documentation
- Check qualification status and upcoming renewal requirements
- Register for open courses directly (connecting to eCommerce or CRM booking flows)

### Account Administrator Portal

A portal for organizational account contacts to:

- View all bookings and seat utilization for their account
- Manage seat assignments (assign a seat to a colleague, reassign, release)
- Track training compliance across their organization's population
- Request new bookings without requiring an operations staff intermediary

### Expanded Automation Coverage

Phase 1 handled the standard lifecycle. Phase 3 handles the exceptions:

| Scenario | Automated Action |
|----------|----------------|
| Course reaches minimum enrollment threshold | Trigger confirmation to instructor |
| Course falls below cancellation threshold within N days | Alert operations + draft communication |
| Learner on waitlist; seat becomes available | Automatic waitlist notification |
| Qualification expiring within 90 days | Proactive renewal reminder to learner and account |
| Seat assigned but attendance not confirmed within N days | Follow-up trigger |

---

## Success Criteria

Operations staff are no longer the bottleneck for routine transactions. Learners and accounts can self-serve for enrollment history, documentation, and status checks without operations involvement.
