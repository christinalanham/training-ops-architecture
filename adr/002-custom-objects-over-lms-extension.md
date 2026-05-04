# ADR-002: Custom Domain Objects over LMS Extension

**Status:** Accepted  
**Date:** 2024  
**Deciders:** Principal Systems Architect

---

## Context

Once the decision was made to use the CRM as the system of record (see [ADR-001](001-crm-as-system-of-record.md)), a second question arose: how do we model the training domain within the CRM?

Two options were on the table:

1. **Extend the LMS** — build deeper integration between the LMS and CRM, using the LMS as the source of training-domain truth and syncing data into the CRM
2. **Model the domain in the CRM** — create custom objects in the CRM to represent training concepts natively

The training domain included concepts that had no home in any existing system: scheduled course instances, individual seat assignments, organizational bookings, qualification categories, and exam records. These concepts either didn't exist in the LMS, existed only in opaque LMS-internal tables, or existed only in spreadsheets.

---

## Decision

**Model the training domain directly in the CRM using custom objects.**

Six custom objects were designed and created:

| Object | Represents |
|--------|-----------|
| **Course** | A training offering in the master catalog |
| **Event Course** | A scheduled instance of a Course (specific date, location, capacity) |
| **Course Seat** | An individual enrollment — one Contact in one Event Course |
| **Course Booking** | An organizational purchase — an Account buying multiple Seats |
| **Qualification Category** | A credential or certification framework |
| **Exam** | An assessment attempt by a Contact against a Qualification Category |

This model was designed to mirror how training operations staff actually thought about their work — not how the LMS categorized it.

---

## Consequences

**Positive**

- Full reportability on all training operations within the CRM — no external queries required
- Training data is co-located with customer and account data, enabling relationship-level analytics (e.g., which accounts have the most active learners; which courses have the highest completion rates by segment)
- Custom objects participate natively in Salesforce automation: Flows, Apex triggers, approval processes, email alerts
- No LMS vendor lock-in for operational data — if the LMS changes, the operational record stays intact
- The data model can evolve independently of LMS upgrade cycles
- Gave the operations team a structured, searchable system to replace their spreadsheets — significant adoption driver

**Negative**

- Ongoing maintenance responsibility for the custom data model — schema changes require admin/developer involvement
- The LMS is still required for content delivery; introduces a conceptual separation between the "operational record" (CRM) and the "learning experience" (LMS)
- Initial data modeling required deep operational interviews to surface all the implicit rules and relationships that existed only in tribal knowledge
- Risk of model drift if future teams add CRM fields/objects without understanding the original design intent — mitigated by this documentation

---

## Design Notes

The object model was informed by how staff actually described their work, not by existing system schemas. Key insight: "a booking" and "a seat" are different things. An Account buys seats in bulk (a Booking); individual Contacts are assigned to specific seats (Course Seats). This distinction wasn't represented anywhere before — it only existed in people's heads.

Qualification Categories were added as a separate object (rather than a picklist on Course) specifically to enable future exam and recertification tracking. This proved prescient — Exam records were added to the model within the same phase.

---

*See also: [ADR-001](001-crm-as-system-of-record.md) — the decision to use the CRM as system of record that this ADR builds on.*
