# Open Knowledge Format (OKF)

Last updated: 2026-09-17

## What "Open Knowledge standard" refers to

In the AI-agent context, this is Google Cloud's **Open Knowledge Format (OKF)**: an open specification published June 2026 (v0.1, now v0.2) for structuring organizational knowledge as agent-readable files. Spec: https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md. There is no other established "Open Knowledge standard" in the agent space, so OKF is the credible match. Brief ambiguity note: the name is generic and the spec is young, so verify you mean OKF before citing it in any document.

## What it is

A directory of markdown files with YAML frontmatter. No schema registry, no central authority, no required tooling or SDK. If you can `cat` a file you can read OKF; if you can `git clone` a repo you can ship it. It formalizes the "LLM wiki" pattern: the metadata, context, and curated insight surrounding your data and systems, packaged so both humans and agents can consume it.

```
sales/
  index.md
  datasets/
    index.md
    orders_db.md
  metrics/
    index.md
    weekly_active_users.md
```

## Why v0.2 matters

v0.1 nailed portability. v0.2 adds what agent-maintained corpora actually need, since knowledge bases are increasingly written by agents, not people:

- Provenance: what was this created from, and how was it verified.
- Trust: how much should a consumer believe it.
- Freshness and lifecycle: is it still true, is it the current version.
- Attestation: was this number produced the way we said it must be.

These are first-class fields now, while the format stays minimally opinionated.

## How to use it in dev workflows

- Replace bespoke context pipelines (your hand-rolled CLAUDE.md, Obsidian vault exports, wiki scrapers) with OKF bundles. Any tool that adds OKF support can then read every other tool's bundles.
- Treat bundles as versioned artifacts in git. Diffable knowledge means reviewable knowledge: PRs against the knowledge base, not silent wiki edits.
- Start with one bounded domain (runbooks for one service, data definitions for one warehouse) before trying to boil the org's ocean.
- Keep the human-verified layer explicit. Mark which entries a person checked versus what an agent generated. Agents should know when to act and when to escalate.

## Watchouts

- Adoption is early. The spec is Apache 2.0 and vendor-neutral by design, but ecosystem support is still forming. Do not bet a critical workflow on tooling that does not exist yet.
- OKF is a format, not a platform. It will not fix knowledge nobody wrote down. The hard part is still capturing the unwritten stuff.

## Sources

- https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md
- https://www.techtimes.com/articles/318416/20260615/google-cloud-open-knowledge-format-turns-scattered-org-knowledge-agent-ready-bundles.htm
