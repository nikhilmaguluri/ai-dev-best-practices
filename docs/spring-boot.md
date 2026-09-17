# AI-Assisted Development: Spring Boot

Last updated: 2026-09-17

Spring Boot is a strong fit for agent-assisted development: conventions are strict, the project layout is predictable, and the framework does heavy lifting the agent does not need to reinvent. The wins are real, but agents fail in specific, repeatable ways. This guide covers the setup that works and the pitfalls to guard.

## Project setup for agents

- Scaffold from start.spring.io coordinates and state them explicitly (Boot version, Java version, packaging). Agents guess versions badly; pin them in AGENTS.md.
- Prefer Java 21+ with virtual threads for new services. Tell the agent once, in the project instructions, not per prompt.
- Keep the standard layout sacred (`controller` / `service` / `repository` / `dto` / `entity`). Agents navigate conventional layouts far better than clever ones.
- Put build and test commands in AGENTS.md: `./mvnw -q test -Dtest=OrderServiceTest`, not "run the tests".

## Where agents shine

- Endpoint scaffolding: controller plus DTOs plus validation annotations from an OpenAPI spec or a written contract. Review the contract, let the agent write the boilerplate.
- Test generation: unit tests for services, `@DataJpaTest` slice tests for repositories, `@WebMvcTest` for controllers. Ask for the test in the same breath as the code; tests written later never get written.
- Boilerplate migrations: Lombok conversions, Jakarta namespace moves, config property renames across the codebase.
- Writing Spring AI or MCP server tools: agents do well here when given a skill with the current GA API, because their training data is full of outdated pre-GA examples.

## Pitfalls agents hit

- **Outdated API guesses.** Agents frequently emit pre-GA artifact names, old SDK versions, or deprecated annotations. For anything released in the last year (Spring AI, MCP Java SDK), give the agent a skill or doc pointer with current coordinates.
- **Lombok `@Data` on JPA entities.** Its generated `equals`/`hashCode`/`toString` break on lazy associations and mutable identifiers. Enforce `@Getter` plus `@Setter` plus `@NoArgsConstructor(access = PROTECTED)` instead, and say so in the project instructions.
- **Missing no-arg constructor.** JPA requires it; agents forget it when writing entities by hand. The Lombok pattern above covers it.
- **`@Enumerated` defaults.** Bare `@Enumerated` persists ordinals, which silently corrupt data when enum order changes. Require `EnumType.STRING` as a project rule.
- **N+1 queries.** Agents write the naive loop over lazy collections every time. Require `@EntityGraph` or fetch joins for collection reads, and make it a review checklist item.
- **String-concatenated queries.** Agents occasionally build JPQL with concatenation. Parameterized queries only, no exceptions.
- **`System.out.println` logging.** Beyond being wrong, in stdio-based tooling (like MCP servers) stdout logging corrupts the transport. SLF4J only.
- **DTOs vs entities at the boundary.** Agents return entities from controllers, leaking lazy-loading bombs and internal fields into JSON. Map to DTOs at the controller layer; consider MapStruct for the mechanical parts.

## Testing discipline

- One test per layer per feature: repository slice test, service unit test with mocked repository, controller web slice test. Tell the agent the pattern once.
- Integration tests with Testcontainers for anything touching real SQL semantics. H2 is fine for speed but lies about dialect behavior; agents will not flag the difference unless told.
- Transaction rollback scenarios deserve explicit tests. Agents rarely generate them unprompted.

## Sources

- https://github.com/cnrf/spring-boot-skills
- https://github.com/javiosyc/spring-boot-skills
- https://dev.to/kavitha_pazhanee_034b29ef/spring-boot-spring-data-jpa-code-review-checklist-595m
- https://dev.to/protsenko/spring-data-jpa-best-practices-entity-design-guide-ad
