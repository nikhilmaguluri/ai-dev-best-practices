# Writing Good Acceptance Criteria

Last updated: 2026-09-17

Acceptance criteria define what "done" means for a user story: the conditions that must hold for the story to deliver its value. They are written for the developer, the tester, and the stakeholder at once, and increasingly for the AI agent implementing the ticket. The rule of thumb: a tester should be able to mark each criterion pass or fail without interpretation.

## Acceptance criteria vs requirements

- Requirements say what the system must do. Acceptance criteria say how we will know it does it.
- A requirement: "Users can reset their password." Acceptance criteria: the specific conditions, edge cases, and error behaviors that prove the reset flow works.
- Criteria belong on the story, written before development starts, ideally during refinement. Criteria written after the code exists are a description, not a specification.
- If a criterion cannot be tested, it is not acceptance criteria. "The page loads fast" is a requirement fragment; "the page renders within 2 seconds on a standard connection" is testable.

## INVEST for stories

Good criteria start with a good story. INVEST is the checklist:

- **Independent.** The story can be built and tested on its own, without waiting on another story.
- **Negotiable.** The story is a conversation placeholder, not a contract carved in stone. Details emerge in refinement.
- **Valuable.** It delivers something a user or stakeholder cares about. Technical chores get framed in terms of the value they unlock.
- **Estimable.** The team can size it because the scope is understood. Vague stories are not estimable, which is a signal to split or clarify.
- **Small.** Fits in a sprint. If writing criteria takes longer than building, the story is too big.
- **Testable.** The team can say unambiguously whether it is done. This is where acceptance criteria do their work.

## Gherkin: Given, When, Then

Scenario-based criteria use Given (starting state), When (the action), Then (the expected outcome), with And and But for continuation:

```gherkin
Scenario: Valid file upload
  Given the user is logged in
  And the file is a PDF under 10 MB
  When the user selects the file and clicks "Upload"
  Then the file is stored in the user's account
  And a confirmation message is displayed

Scenario: Oversized file
  Given the user is logged in
  And the file is a PDF over 10 MB
  When the user tries to upload the file
  Then the upload is rejected
  And an error message states the 10 MB limit
```

- **Given** is the preconditions and test data, not the trigger. **When** is the single action being tested. **Then** is the observable outcome.
- One scenario per distinct behavior: happy path, each error case, each edge case. Aim for one to three scenarios per story; four or more usually means the story should split.
- Gherkin scenarios translate directly into automated tests (Cucumber, SpecFlow, Behave). Write them so the automation author does not have to re-derive the cases.

## Example mapping

Before writing criteria, run a quick example mapping session: the story on a card, rules (acceptance criteria in plain language) beneath it, examples under each rule, and open questions off to the side. It takes 20 minutes and surfaces the edge cases that otherwise appear mid-sprint. Rules become Gherkin scenarios; questions get answered before development, not during.

## Formats and when to use each

- **Rule-based (bullets).** Best for simple display rules, permissions, and validations: "Only admins can delete a published report." Direct and cheap to write.
- **Scenario-based (Gherkin).** Best for flows, triggers, state changes, and conditional logic. Shows behavior over time.
- Mix them freely. A story can have three bullet rules and two scenarios; the format serves the content, not the other way around.
- For AI-assisted implementation, rule-style criteria tend to be consumed faster by agents (directly implementable if-then statements), while scenarios are better for test generation. Content matters more than format; domain decisions must be pinned down either way, because conventions are in the model's training data and your business rules are not.

## Anti-patterns

- **Vague adjectives.** "Fast", "easy", "intuitive", "secure". Replace with numbers, named behaviors, or named standards.
- **Implementation details.** "Uses a stored procedure" is not a criterion; "the report reflects data committed in the last hour" is. Criteria describe observable behavior, never the how.
- **Compound criteria.** One criterion, one check. "The user is redirected and an email is sent and the audit log records it" is three criteria hiding in one sentence.
- **Missing negative cases.** Every happy path needs its error siblings: invalid input, unauthorized access, empty states, timeouts. The errors are where production bugs live.
- **Criteria that restate the story.** "User can upload files" adds nothing to a story titled "upload files". Criteria add precision, not repetition.
- **Unbounded scope.** "All browsers supported" means nothing. Name the browsers and versions, or name the standard (WCAG 2.2 AA) that defines done.

## Further reading

- [Acceptance Criteria: How to Define and Deliver User Stories](https://dev.to/taskford/acceptance-criteria-how-to-define-and-deliver-user-stories-1hf)
- [How to Write Effective Gherkin Acceptance Criteria](https://testquality.com/gherkin-language-user-stories-and-scenarios/)
- [Acceptance criteria and AI agents: why domain decisions must be pinned down](https://www.linkedin.com/pulse/your-ai-agent-doesnt-care-how-you-write-acceptance-criteria-sayer-gglpe/)
- [Acceptance Criteria in Gherkin Syntax](https://www.scrum.org/node/88639)
