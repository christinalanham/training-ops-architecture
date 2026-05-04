# ADR-001: CRM as System of Record

**Status:** Accepted  
**Date:** 2024  
**Deciders:** Principal Systems Architect

---

## Context

The organization operated three systems — a CRM, an eCommerce platform, and an LMS — with no shared source of truth. Training operational data was fragmented:

- The CRM held customer and sales data but had no training-specific data model
- The LMS held learning delivery data but was opaque and non-integrable
- The eCommerce platform held purchasing data disconnected from operational reality
- Critical data (course schedules, instructor assignments, seat availability, qualification frameworks, exam records) lived in offline spreadsheets and tribal knowledge

Any change — a new course offering, a cancellation, a seat count update — required manual action in multiple systems. This created data inconsistency, duplication of effort, and staff dependency on individuals who held institutional knowledge.

The organization needed a single authoritative source for all training operational data.

---

## Decision

**Designate the CRM as the authoritative system of record for all training operations.**

All training domain objects are owned and originated in the CRM. Downstream systems (eCommerce, LMS, communication tools) are consumers of CRM data — they receive it, they do not generate it.

This decision was informed by two factors:

1. The CRM already held the customer and account relationships that training data needed to connect to (enrollments are linked to Contacts; bookings are linked to Accounts)
2. The CRM's automation, reporting, and integration capabilities exceeded what was available in either the LMS or eCommerce platform

---

## Consequences

**Positive**

- Single point of truth eliminates data inconsistency across systems
- Staff learn one system for all operational data entry — reduces training burden and error surface
- Enables automation: downstream systems react to CRM record state changes rather than requiring manual triggers
- Full auditability of all training records in one place
- Cross-dimensional reporting becomes possible without cross-system joins (enrollments, bookings, completions, qualifications — all in one query layer)
- Custom objects in the CRM participate in native automation, approval processes, and reporting

**Negative**

- CRM becomes a critical dependency; its availability directly affects downstream systems
- CRM administrators are now responsible for maintaining a training domain data model in addition to the traditional sales/service model
- Initial migration required extracting years of data from spreadsheets and the LMS into structured CRM records — significant one-time effort
- Integration failures between CRM and downstream systems are now operational incidents, not inconveniences

---

## Alternatives Considered

**LMS as System of Record:** Rejected. The LMS was designed for content delivery, not operational management. It lacked the integration capabilities, reporting flexibility, and relationship model needed to connect training data to customer and account records.

**Purpose-built training management system:** Rejected. Would have added a fourth system to an already fragmented ecosystem and required additional budget, vendor management, and integration work. The CRM already had the infrastructure needed.

---

*See also: [ADR-002](002-custom-objects-over-lms-extension.md) for the decision on how the training domain was modeled within the CRM.*
