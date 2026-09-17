# AI-Assisted Development: React

Last updated: 2026-09-17

React is where agents produce the most visible output and the most slop. The difference is almost entirely in the spec you give them. Vague prompts produce plausible-looking components with wrong props, missing types, and state in the wrong place. Precise prompts produce production code.

## Prompting for components

Be explicit about the contract, every time:

```
Create a reusable Button component that accepts label, onClick, and a
variant prop (primary | secondary | danger). TypeScript interfaces
required, React.memo optimized, no external dependencies.
```

Bad: "make a button component." The agent will guess everything and guess wrong.

Build complex UIs component by component, not all at once: primitives first, composition second, validation and state third, performance last. Each phase is reviewable; a single giant generation is not.

## State management rules to give the agent

State bugs are the number one defect class in agent-generated React. Settle the architecture in AGENTS.md so the agent never improvises:

- Server state: TanStack Query. The agent must not hand-roll fetch plus useEffect caching.
- Form state: React Hook Form plus Zod. Validation schemas live next to the form.
- Local UI state: plain hooks. Global client state: Context or a light store. Pick one per project and name it.
- Never put derived data in state. If it can be computed during render, compute it during render.

## React 19 notes

- The `use` hook reads promises and context directly in render (suspends until resolved). Agents trained on older React will not reach for it; mention it when you want Suspense-based data flow.
- Server Components are the default in Next.js App Router. Tell the agent which components must stay client-side (`"use client"` boundaries) or it will sprinkle the directive everywhere.
- Ref as prop is now standard; agents still write `forwardRef` out of habit. Either works, but consistency matters for review.

## Testing with agents

- Ask for tests in the same prompt as the component. Vitest plus React Testing Library for units, Playwright for flows. Tests requested later get skipped.
- Agent-generated tests love tautologies (asserting the mock was called with what the mock returns). Require at least one assertion on rendered output or user-visible behavior per test.
- For hooks, require tests through rendered components or `renderHook`, not by calling hook internals directly.

## Review checklist for agent-generated React

- Props have explicit TypeScript interfaces, exported for reuse.
- `useEffect` dependency arrays are complete; no missing-dep suppressions without a comment explaining why.
- No state duplicated from props. No `useState(props.value)` without a reset key or effect syncing it.
- Accessibility basics present: labels on inputs, keyboard handling on custom controls, semantic HTML.
- No inline object/array literals as props to memoized children (defeats the memo).
- Bundle awareness: the agent imported a whole library for one function. Flag it.

## Sources

- https://github.com/sstteeward/monitoring-system/blob/HEAD/.opencode/skills/REACT-SKILL-BEST-PRACTICES.md
- https://github.com/pf-jared/claude-review-test/blob/HEAD/.agents/skills/vercel-react-best-practices/AGENTS.md
- https://github.com/samuelcolvin/forgettable/blob/HEAD/services/python-agent/docs/react.md
- https://github.com/okeyba/xagi-frontend-templates/blob/HEAD/packages/react-next/AGENTS.md
