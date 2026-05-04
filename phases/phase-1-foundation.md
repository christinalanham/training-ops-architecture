# Phase 1 — Foundation: CRM as System of Record

**Status:** ✅ Complete  
**Labor hours recovered:** 2,000+ annually

---

## Objective

Establish the CRM as the authoritative system of record for all training operations. Replace disconnected systems and manual processes with a structured, integrated data model that gives the organization a single version of the truth.

---

## Deliverables

### 1. Custom Training Domain Data Model

Six custom objects created in the CRM to bring tribal knowledge into structured, reportable records:

| Object | What it replaced |
|--------|----------------|
| Course | "We just know what courses we offer" — no catalog existed in a system |
| Event Course | Spreadsheet rows for each scheduled training date |
| Course Seat | Manual enrollment tracking across email and spreadsheet |
| Course Booking | Organizational purchases tracked in email and offline notes |
| Qualification Category | Undocumented certification requirements held by senior staff |
| Exam | Assessment records in spreadsheets or not tracked at all |

All objects are related to each other and to standard CRM objects (Account, Contact), enabling full cross-dimensional reporting.

### 2. CRM → eCommerce Integration

Automated, real-time sync replacing manual eCommerce updates:

| Trigger | Automated action |
|---------|----------------|
| Event Course created | Publish course listing with inventory count |
| Available seats updated | Sync inventory to eCommerce |
| Event Course cancelled | Remove or flag listing; release inventory |
| eCommerce purchase | Create Course Seat in CRM |

**Before:** Staff manually updated eCommerce after every CRM change. Changes were often delayed, missed, or inconsistent.  
**After:** Zero manual eCommerce updates required.

### 3. Automated Communications

Event-driven communications replacing manual email workflows:

| Trigger | Communication | Recipient |
|---------|--------------|-----------|
| Course Seat Status → Enrolled | Enrollment confirmation | Learner |
| Course Seat Status → Complete | Post-program documentation | Learner |
| Weekly schedule trigger | Upcoming roster | Assigned instructors / resources |
| Event Course Status → Cancelled | Cancellation notice | Enrolled learners |

**Before:** Staff manually assembled and sent each communication. Timing was inconsistent; items were frequently missed.  
**After:** Zero manual communications required for all standard workflows.

---

## Outcome

| Metric | Result |
|--------|--------|
| Annual labor hours recovered | 2,000+ |
| Systems with a single source of truth | 3 (previously: 0) |
| Manual eCommerce updates per week | 0 (previously: multiple daily) |
| Manual communications per week | 0 for standard workflows (previously: daily manual effort) |
| Data previously in spreadsheets | Structured, reportable, auditable CRM records |

---

## Architecture Decisions

The following ADRs document the key decisions made during Phase 1:

- [ADR-001: CRM as System of Record](../adr/001-crm-as-system-of-record.md)
- [ADR-002: Custom Domain Objects over LMS Extension](../adr/002-custom-objects-over-lms-extension.md)
- [ADR-003: Unidirectional Sync Pattern](../adr/003-unidirectional-sync-pattern.md)
- [ADR-004: Event-Driven Communications](../adr/004-event-driven-communications.md)

---

## What Phase 1 Made Possible

Phase 1 was foundational — not just for the immediate labor savings, but because it created the data infrastructure that Phases 2–4 build on. Without a structured, CRM-native data model, there is nothing to report on, no baseline to measure against, and no events to automate against. Phase 1 built the platform; Phases 2–4 build on top of it.
