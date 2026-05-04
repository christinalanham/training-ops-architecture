# Phase 2 — Reporting & Analytics Layer

**Status:** 🔄 In Progress  
**Depends on:** Phase 1 (custom data model, CRM as system of record)

---

## Objective

Surface the operational data captured in Phase 1 as actionable intelligence. Build dashboards, KPI frameworks, and data quality tooling so that leaders can make decisions based on what the data says — not what they think they remember.

---

## Problem Statement

Phase 1 gave the organization a structured data model for the first time. But data in a database without a way to see it is still invisible. Key questions remained unanswered in real-time:

- Which courses are trending toward cancellation due to low enrollment?
- Which accounts are most and least engaged with training?
- What is the completion rate by course type, region, or qualification category?
- Where are the gaps between booked seats and completed seats?
- Are qualification renewals happening on time, or are credentials lapsing?

---

## Planned Deliverables

### Operational Dashboards

Real-time CRM dashboards for:

- **Enrollment pipeline** — seats booked vs. available across all upcoming Event Courses
- **Completion tracking** — Course Seat status distributions by time period, course type, account
- **Qualification health** — active certifications vs. upcoming expirations by Contact and Account
- **Revenue operations** — bookings, utilization rates, cancellation rates by period

### KPI Framework

Define and instrument the metrics that matter. Establish baselines from the first full cycle of Phase 1 data, then track trends. Target metrics include:

- Enrollment-to-completion rate
- Seat utilization rate (seats purchased vs. seats used)
- Time-to-enrollment from booking
- Cancellation rate and lead time
- Qualification renewal compliance rate

### Data Quality Framework

A structured data model is only as good as the data in it. Phase 2 includes:

- Validation rules on critical fields to prevent incomplete records
- Duplicate management rules for Contact and Account records
- Reporting on data completeness to identify gaps
- A documented data dictionary for the six custom objects

---

## Dependencies

- Phase 1 custom objects must be fully populated with at least one enrollment cycle of data
- Operations team alignment on which KPIs matter and what definitions to use
- Agreement on dashboard audience (operations staff vs. leadership vs. accounts)

---

## Success Criteria

Leaders can answer the questions listed in the Problem Statement above by looking at a dashboard — not by asking an operations staff member to pull a spreadsheet.
