# Building Agent Skills

Last updated: 2026-09-17

Agent Skills are the open standard (agentskills.io) for packaging procedural knowledge so agents load it exactly when needed. A skill is a folder with a `SKILL.md` file, optional scripts, and reference docs. Build once, use across Claude Code, Cursor, Codex, and other skills-compatible agents.

## When a skill beats a prompt

- You explain the same workflow for the third time. Package it.
- The task needs domain procedures the model was never trained on (your deploy runbook, your API's quirks).
- The task needs deterministic steps plus code (scripts that run outside the model's context).

Prompts are for one-offs. MCP gives the agent live tools and data. Skills give it know-how. Use all three together for serious workflows.

## SKILL.md anatomy

```
my-skill/
  SKILL.md            # required: frontmatter + core instructions
  scripts/            # optional: executables the agent runs
  references/         # optional: deep docs, loaded only when needed
  assets/             # optional: templates, examples
```

Frontmatter rules:

```yaml
---
name: generating-changelogs
description: Generates a changelog entry from git history. Use when asked to write release notes or summarize recent commits.
---
```

- `name`: max 64 chars, lowercase letters, numbers, hyphens only. Prefer gerund form (`testing-code`, `processing-pdfs`). Never vague names like `helper` or `utils`.
- `description`: max 1024 chars, third person, and this is the trigger. State WHAT the skill does and WHEN to use it. This one field decides whether the agent ever loads your skill, so spend most of your writing effort here.

## Progressive disclosure

Three levels, and this is the whole point of the format:

1. Metadata (name + description) lives in the system prompt, always. Cheap.
2. SKILL.md body loads only when the agent decides the skill is relevant.
3. `scripts/` and `references/` load only when the instructions point at them.

Design for this. Put the workflow in the body, push long tables and deep docs into `references/`, and put anything deterministic into scripts. Scripts solve problems instead of asking the model to reason through them.

## Best practices

- Keep the body under ~500 lines. If it is longer, you are writing a manual, not a skill. Split it.
- Write concrete examples, not abstract advice. A `good.java` / `bad.java` pair teaches more than three paragraphs of guidance.
- Give goals and constraints, not prescriptive step-by-step instructions. Let the agent decide how.
- Every skill gets a Troubleshooting or Gotchas section. Append to it every time the agent fails in that domain. This section compounds in value.
- Test with at least a few real scenarios before calling it done, and re-test after model upgrades. Skills rot as models change.
- One concern per skill. A giant SKILL.md covering everything gets loaded for nothing and ignored for everything.

## Sources

- https://agentskills.io
- https://docs.claude.com/en/docs/agents-and-tools/agent-skills/best-practices
- https://github.com/jdutton/vibe-agent-toolkit/blob/HEAD/docs/external/anthropic-skill-authoring-best-practices.md
