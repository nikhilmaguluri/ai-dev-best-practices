# Solution Architect Documentation

Last updated: 2026-09-17

Where the application architect documents a system, the solution architect documents a solution: business problem, the landscape it fits into, the integrations that make it real, and the security and compliance posture that makes it shippable. The standard deliverable is the Solution Architecture Document (SAD), supported by ADRs, integration views, and a risk register.

## The Solution Architecture Document

A SAD is the single document a new stakeholder reads to understand the whole solution. The widely used template sections:

1. **Executive summary.** What the solution is and why it exists, in terms a non-technical stakeholder can follow. One page maximum.
2. **Architecture vision.** Principles the solution follows, constraints it accepts, and assumptions it makes. Constraints are first-class content, not footnotes.
3. **Business requirements.** The business problems and functional scope, linked back to the business case. Not a feature list; the outcomes the solution must deliver.
4. **Technology baseline.** Current state: existing systems, tech stack, and what is being reused vs replaced vs built new.
5. **System context (C4 Level 1).** The solution's boundary, actors, and external systems. Draw it once, label every relationship with its protocol.
6. **Logical view (C4 Level 2 container).** The solution's building blocks and their interactions. This is the diagram that gets printed and pinned.
7. **Component view (C4 Level 3).** For the parts of the solution with non-obvious internals. Skip it where the container view already says enough.
8. **Data architecture and ERD.** Canonical entities, ownership, data residency, and the data lifecycle. Include where data crosses trust boundaries.
9. **Integration and data flow.** Sequence diagrams for the flows that matter: the money movement, the sync that cannot fail silently, the event chain. Name the integration pattern per flow (request-response, pub-sub, saga, CDC).
10. **Security architecture.** Threat model (STRIDE mapped to concrete controls), authentication and authorization design, secrets management, and audit logging. Mandatory, not optional.
11. **Deployment view.** Environments, regions, networking, scaling, and disaster recovery posture. RTO and RPO stated, not implied.
12. **Non-functional requirements.** Performance, scalability, availability, observability, each quantified with a measurement method.
13. **Architectural decisions.** ADR log with links to the full records. The SAD summarizes; the ADRs carry the reasoning.
14. **Risks and mitigation.** Each risk with likelihood, impact, owner, and mitigation or acceptance. A risk without an owner is a prediction, not a plan.

## Integration patterns

- Document the pattern per integration, not just the endpoint: synchronous request-response for queries, events for state changes, sagas for distributed transactions, CDC for data replication.
- For each integration, record: direction, protocol, sync vs async, retry and idempotency behavior, timeout and circuit-breaker policy, and the failure mode (what the user sees when it is down).
- Prefer sequence diagrams over prose for flows with more than two hops. Prose hides ordering bugs; diagrams surface them.

## Data flow and security views

- Trace data from entry to rest: what enters, what is transformed, where it is stored, who can read it. Data flow diagrams per non-trivial flow.
- Security views answer: where are the trust boundaries, what crosses them, what controls guard the crossing. Include a data classification (public, internal, confidential, regulated) and show it on the data flow.
- Where regulated or personal data is processed, the SAD carries a privacy impact assessment or references it. Set an explicit reviewed flag; do not leave it ambiguous.

## Decision records and templates

- Every SAD links an ADR log. Decisions that are hard to reverse, surprising without context, or real trade-offs get full records; routine choices stay in the SAD body.
- Keep the SAD as a template in the repo (Markdown with Mermaid diagrams that render on GitHub) so each solution starts from the same skeleton and reviewers know where to look.
- Version the SAD with the solution. When the solution changes shape, the SAD changes with it; a SAD that lags the implementation misleads everyone who reads it.
- Approval gates matter: no SAD, and no ADR, is final until the reconciled plan has explicit sign-off. Write the approval status into the document.

## Good practices

- One solution, one SAD. When the solution spans multiple systems, the SAD sits above the individual application docs and links them; it does not duplicate them.
- Keep the executive summary honest. If the business case has a weak spot, the SAD names it. Architects who hide trade-offs lose trust exactly once.
- Review the risk register per release, not per year. Risks change shape as the solution ships; a stale register is theater.

## Further reading

- [Solution Architecture Document and ADR best practices](https://github.com/liemqv/SAD-Solution-Architecture-Document-Best-Practices)
- [Solution Architecture Document best practices](https://github.com/cozgg/sad-solution-architecture-document-best-practices)
- [SAD template with Mermaid examples](https://github.com/liemqv/sad-adr-best-practices/blob/HEAD/ar-SA/SAD-Template.md)
- [Principal architect skill: HLD and SAD conventions](https://github.com/alexanderpino/skills/blob/HEAD/principal-architect/SKILL.md)
