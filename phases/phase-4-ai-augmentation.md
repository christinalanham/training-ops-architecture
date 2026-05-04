# Phase 4 — AI-Augmented Operations

**Status:** 📋 Planned  
**Depends on:** Phases 1–3 (full data model, quality baseline, self-service layer)

---

## Objective

Apply AI-assisted analysis and decision support on top of the operational platform built in Phases 1–3. The goal is not to replace human judgment — it is to give humans better information faster, and to handle pattern recognition at a scale and consistency that manual review cannot match.

Phase 4 only works because Phases 1–3 exist. AI augmentation requires clean, structured, connected data. Without the foundation, there is nothing to augment.

---

## Problem Statement

Even with self-service and automation in place, some problems require pattern recognition across large datasets:

- Which courses should be retired or redesigned based on completion and satisfaction signals?
- Which accounts show early warning signs of disengagement before renewals are at risk?
- What is the optimal scheduling pattern for courses given historical enrollment, cancellation, and completion data?
- Are there qualification gaps across an account's workforce that the account hasn't noticed yet?
- Can we predict which learners are likely to miss completion deadlines with enough lead time to intervene?

These questions are answerable from the data — but only if someone is consistently looking for the patterns. AI augmentation makes this continuous.

---

## Planned Capabilities

### Predictive Enrollment & Scheduling

Use historical enrollment patterns to:
- Forecast demand for upcoming course instances before they're filled or cancelled
- Recommend optimal scheduling (day of week, time of year, location) for new offerings
- Flag courses likely to fall below minimum enrollment threshold before the cancellation window

### Account Qualification Gap Analysis

Automated analysis of Account-level qualification coverage:
- Identify credentials that are lapsing or will lapse within a rolling window
- Surface accounts with high compliance risk before renewals become urgent
- Generate proactive outreach recommendations for the sales / account management team

### Learner Completion Risk Scoring

Score Course Seats by completion risk based on behavioral signals:
- Patterns in historical data (role type, booking lead time, prior completion rate)
- Early indicators within current enrollment (attendance confirmation lag, communication response)
- Automated intervention triggers when a learner crosses a risk threshold

### AI-Assisted Content & Curriculum Recommendations

Based on completion rates, qualification data, and account profiles:
- Surface curriculum gaps — courses that learners need but aren't in the catalog
- Flag courses with consistently low completion rates for instructional design review
- Recommend learning paths for individual learners based on their qualification history

---

## Architecture Considerations

Phase 4 will require an AI/ML layer connected to the CRM data model. Options under consideration:

- **CRM-native AI** (Salesforce Einstein) — lowest integration overhead; limited model customization
- **External ML pipeline** (GCP Vertex AI or equivalent) — higher capability, requires data export and model management
- **AI-assisted analysis** (Claude / Gemini API integration) — well-suited for pattern narration and recommendation generation on structured data exports; does not require a trained model

The appropriate approach will depend on data volume, latency requirements, and the organization's AI governance framework. Decision to be formalized as an ADR when Phase 4 planning begins.

---

## Success Criteria

Operations and account management teams receive proactive, actionable intelligence — not just dashboards to look at. The system surfaces problems before stakeholders have to go looking for them.
