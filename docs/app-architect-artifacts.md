# Application Architect Artifact Documentation

Last updated: 2026-09-17

An application architect produces a small set of durable artifacts that outlive any single sprint: the structure of the system, the decisions behind it, and the constraints it must satisfy. The standard is C4 diagrams for structure, ADRs for decisions, and quantified NFRs for constraints. Everything else is supporting material.

## The core artifact set

- **C4 context diagram (Level 1).** The system in its environment: actors, external systems, and the boundary. Written once, updated when the boundary changes.
- **C4 container diagram (Level 2).** The deployable units: applications, data stores, queues, and how they communicate. One per system; update when services are added or inter-service communication changes.
- **C4 component diagrams (Level 3).** Internal structure of containers that warrant it, the ones with non-obvious internals. Not every container needs one.
- **Architecture Decision Records (ADRs).** One numbered record per significant decision: context, options considered, the decision, and consequences. Immutable once accepted; supersede, never edit.
- **Non-functional requirements (NFRs).** Quantified: p99 latency targets, throughput, availability, data retention, recovery objectives. "Fast" is not an NFR; "p99 under 300ms at 500 rps" is.
- **Integration map.** Every external integration: direction, protocol, sync vs async, failure mode, and who owns the contract. This is the artifact that saves incident response.
- **Deployment view.** Where containers run, across environments, with networking, scaling, and data residency noted. Keep it to one diagram plus a short environment table.
- **Domain and data model.** Entity relationships with keys, cardinality, and discriminating business fields. Omit audit fields; show the business truth, not every column.

## Standards for each artifact

- **Diagrams:** Mermaid is the default. It renders on GitHub, diffs in PRs, and stays in the repo next to the code. One diagram per file, named and versioned. A diagram earns its place only when prose cannot say it cleanly; skip it if a sentence is clearer.
- **ADRs:** numbered sequentially (ADR-0001), with status (proposed, accepted, superseded). Record decisions that are hard to reverse, surprising without context, or real trade-offs. Trivial choices are not ADRs.
- **NFRs:** each NFR gets a target, a measurement method, and an owner. Add fitness functions or technical budgets where a constraint must hold continuously, not just at design time.
- **Integration map:** a table beats a diagram for the registry view; sequence diagrams for the flows worth capturing (one per cross-service interaction that matters).

## How detailed should they be

- Match depth to decision risk. A container diagram plus five ADRs is enough for most applications; component diagrams and sequence diagrams appear only where the internals are non-obvious or the interaction is risky.
- The architecture folder owns: context, container, ADRs, NFRs, guardrails, and the integration map. Feature-level detail lives in feature READMEs, not in the global docs.
- Link code back to decisions. A comment marker at the place a decision is enacted (for example, `// ARCH-REF: ADR-0007`) lets a future reader or agent find the rationale with a grep instead of spelunking.
- Review artifacts when reality changes, not on a calendar. A deployment view that still shows the old cloud provider is worse than no deployment view, because it is trusted.

## What to skip

- C4 Level 4 (code) diagrams. They are auto-generated from code and rot immediately when drawn by hand.
- Exhaustive ERDs with every column. The schema is in the migrations; the artifact shows the model.
- Architecture documents that restate the code. If a paragraph can be generated from the code, generate it in CI or delete it.

## Further reading

- [KCC agentic framework: architect capability and governance artifacts](https://github.com/tarekfawaz/kcc-agentic-framework/blob/HEAD/.KCC/capabilities/agents/architect.md)
- [Principal architect skill: HLD and SAD conventions](https://github.com/alexanderpino/skills/blob/HEAD/principal-architect/SKILL.md)
- [Nebula agents architect skill: diagram standards and C4 levels](https://github.com/gajakannan/nebula-agents/blob/HEAD/agents/architect/SKILL.md)
