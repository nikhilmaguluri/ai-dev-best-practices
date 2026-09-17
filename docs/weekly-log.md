# Weekly Log

## 2026-09-17: Seeded the playbook

First version of the repo. Seeded seven guides from current ecosystem research:

- `agent-skills.md`: the Agent Skills open standard, SKILL.md anatomy, progressive disclosure, and when skills beat prompts.
- `agents-md.md`: the AGENTS.md standard, file hierarchy and scoping rules, what good files contain and what to leave out.
- `mcp.md`: using MCP in daily dev workflows, capability-oriented server design, and the common pitfalls (debugging through the agent, tool pollution, vague tool descriptions).
- `open-knowledge.md`: Google Cloud's Open Knowledge Format (OKF) as the credible "Open Knowledge standard", v0.2 provenance and trust fields, and adoption watchouts.
- `spring-boot.md`: agent-assisted Spring Boot development, scaffolding setup, test generation, and repeatable agent pitfalls (Lombok `@Data` on entities, ordinal enums, N+1, stdout logging).
- `react.md`: component generation prompting, state management rules, React 19 notes, and a review checklist for agent-generated code.

Next update: 2026-09-22. Candidates: real-world gotchas from this week's agent usage, OKF tooling updates, Spring AI GA patterns.

## 2026-09-17: Seeded seven documentation guides

Expanded the playbook with the documentation topics requested this week:

- `codebase-documentation.md`: generating docs from a codebase with AI (top-down vs bottom-up, chunking, prompt patterns, verification, CI sync).
- `knowledge-base.md`: building an application knowledge base for AI development, including bootstrapping from code when no docs exist.
- `spring-boot-api-docs.md`: OpenAPI standards for Spring Boot with springdoc, annotation patterns, RFC 7807 error responses, versioning, contract testing.
- `react-component-docs.md`: React component documentation standards (Storybook stories, prop tables, design tokens, a11y notes, usage examples).
- `app-architect-artifacts.md`: application architect artifacts (C4 diagrams, ADRs, NFRs, integration maps, deployment views).
- `solution-architect-docs.md`: solution architect documentation (SAD template, integration patterns, data flow, security views, decision records).
- `acceptance-criteria.md`: BA acceptance criteria (INVEST, Gherkin Given/When/Then, example mapping, anti-patterns).
