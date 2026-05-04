# ADR-003: Unidirectional Sync Pattern (CRM → eCommerce)

**Status:** Accepted  
**Date:** 2024  
**Deciders:** Principal Systems Architect

---

## Context

The eCommerce platform needed accurate, real-time data about course availability: which courses were open for purchase, how many seats remained, and which had been cancelled. Previously, staff manually updated eCommerce listings after making changes in the CRM or spreadsheets — a process that was slow, error-prone, and frequently out of sync.

The result: course listings showed incorrect inventory, cancelled courses remained purchasable, and new offerings appeared late. Customer-facing errors and staff cleanup time were both significant.

Two integration approaches were considered:

1. **Bidirectional sync** — CRM and eCommerce can each write to each other; changes in either system propagate to the other
2. **Unidirectional sync** — CRM is the write source; eCommerce is a read consumer; changes flow one direction only

---

## Decision

**Implement a unidirectional sync from CRM to eCommerce.**

The CRM is the authoritative write source for all course availability data. The eCommerce platform is a consumer — it reflects the CRM's state but does not originate changes to it.

Sync events are triggered by CRM record state changes:

| CRM Event | eCommerce Action |
|-----------|-----------------|
| Event Course created | New listing created with initial inventory |
| Available seats updated | Inventory count synced |
| Event Course cancelled | Listing removed or flagged as unavailable |
| Event Course details updated (date, location) | Listing updated |

Purchase events flow in the opposite direction via a separate webhook path: eCommerce purchase → integration layer → Course Seat created in CRM. This is treated as an *inbound event*, not a sync, to preserve the clear directional boundary.

---

## Consequences

**Positive**

- Eliminates manual eCommerce updates entirely — operations staff make one change in the CRM; eCommerce reflects it automatically
- Prevents inventory discrepancies and overselling — eCommerce inventory is always derived from the CRM record
- Clear data lineage: any eCommerce listing state can be traced back to a specific CRM record
- Conflict-free writes — there is never a situation where CRM and eCommerce have simultaneously written conflicting values
- Simplifies debugging — if eCommerce shows incorrect data, the investigation starts and ends in the CRM

**Negative**

- eCommerce cannot initiate data changes back to CRM directly — purchase events require a separate integration path (webhook → integration layer → CRM record creation)
- Sync latency: eCommerce may lag CRM state by seconds to minutes depending on integration layer performance — acceptable for this use case but worth monitoring
- The integration layer (Celigo/Zapier) is now a critical dependency in the availability chain — if it fails, CRM changes do not reach eCommerce
- Any future eCommerce platform replacement must implement the same event consumption contract

---

## Alternatives Considered

**Bidirectional sync:** Rejected. Bidirectional sync introduces write conflicts and circular update risks that are difficult to debug and can result in data loss or corruption. Given that the CRM is the system of record (ADR-001), allowing eCommerce to write back to the CRM would undermine that design principle.

**Polling (eCommerce polls CRM for changes):** Rejected. Polling introduces unnecessary latency and CRM API load. Event-driven sync is more responsive and more efficient.

---

## Integration Layer Note

The sync is implemented via a low-code integration platform (Celigo, with Zapier as a fallback for simpler event types). This was chosen deliberately to keep the integration layer maintainable by operations-adjacent staff rather than requiring dedicated developer intervention for routine changes.

This is a deliberate trade-off: lower code sophistication in exchange for lower operational dependency on engineering resources.
