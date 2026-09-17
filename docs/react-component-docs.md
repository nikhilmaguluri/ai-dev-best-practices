# React UI Component Documentation Standards

Last updated: 2026-09-17

A well-documented component is one a developer can use without reading its source. Storybook is the standard surface: stories for every state, generated prop tables, and written guidance for the decisions automation cannot make (when to use it, when not to, and what accessible means here).

## Stories as documentation

- Write a story for every state, not just the happy one: default, variants, hover, focus, active, disabled, loading, error, empty, long-text overflow, right-to-left. Undocumented states are the states that break in production.
- Use `tags: ["autodocs"]` for automatic docs pages, and MDX docs pages when you need full control over structure, ordering, and guidance. Autodocs generates the reference; MDX writes the narrative.
- Stories do triple duty: documentation, a manual playground for designers and QA, and automated tests via play functions. A story without a play function for interaction is a story that can silently break.
- Enable the a11y addon with violations set to error, not warning. A11y regressions should fail the check, not scroll by.

## Prop documentation

- Document every prop with JSDoc or TSDoc comments on the TypeScript interface. These feed the generated prop table automatically: Name, Type, Default, Description.
- Descriptions answer "what happens if I pass this", not "this is the X prop". Include defaults and mark required props explicitly.
- For multi-part components, one prop table per part. Compound components (Menu plus MenuTrigger plus MenuItem) need each part documented separately.
- Complex prop unions and discriminated props need examples in the docs, not just type signatures. Show the common combinations, not all theoretical ones.

## Design tokens

- Every visual value in a component must trace to a design token: color, spacing, radius, typography, elevation. No hardcoded values; agents will happily copy hardcoded styles everywhere.
- Document the tokens each component consumes, ideally in the docs page next to the stories. When a token changes, the docs show which components are affected.
- Sync tokens from Figma through tokens.json and Style Dictionary so the pipeline is Figma to tokens to CSS to docs, not four manual copies.

## Accessibility notes

- Document the keyboard interactions: what Tab, Enter, Escape, and arrow keys do for interactive components. Build on headless primitives (Radix, Base UI, React Aria) that get this right, then document what you kept.
- Note the ARIA roles and properties the component renders, and what the screen reader announces on state changes. If you composed primitives, say so and point at the primitive's docs for the rest.
- Set a maturity level: production-ready, limited use cases, or do not use. An accessible-by-construction component library is worth more than per-component a11y heroics.

## Usage examples and do/don't patterns

- Every component gets a basic usage example that can be copied and run. Pair it with one realistic example (a form with validation, a table with filters), not five trivial ones.
- Document related components and when to choose between them (Dialog vs Popover vs Tooltip). Agents and developers both reach for the wrong one when the boundary is unstated.
- Do/don't pairs beat paragraphs. "Do: use for destructive confirmations. Don't: use for multi-step flows; use the wizard pattern instead." Short, opinionated, checkable.
- Link each component doc back to the design guidance for its visual options. The states in Storybook should mirror the design page's options list one to one.

## Further reading

- [Storybook design system skill](https://github.com/dobzha/dobzha-storybook-ds-skill)
- [React Spectrum documentation standard reference notes](https://github.com/storybook-tmp/base-ui/blob/HEAD/research/a-documentation-standard/reference-notes/react-spectrum.md)
- [Build and Deploy Your Own Design System: Atomic Design, React, Storybook, Tailwind](https://www.designsystemscollective.com/build-and-deploy-your-own-design-system-atomic-design-react-storybook-tailwind-046b6face53c)
- [MDX component docs pattern](https://github.com/alexjv89/engineering-standards/blob/HEAD/storybook/documentation/mdx-component-docs.md)
