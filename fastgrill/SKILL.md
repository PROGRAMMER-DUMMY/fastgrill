---
name: fastgrill
description: Generate a hard, adaptive deep-dive decision audit for an uncertain plan, architecture, process, implementation, product, or operational decision. Use when the user asks for fastgrill, grill-me depth all at once, any number of questions, deep dive questions, an expert questionnaire, adversarial review, or recommended answers accepted by default.
---

# Fastgrill

## Purpose

Use `fastgrill` when the user wants a deep, expert-level decision audit quickly: as many hard questions as the user asks for, across goals, constraints, architecture, execution, risk, security, operations, evaluation, rollout, and governance. If the user does not specify a number, choose a useful size for the task, usually 10-30 questions. Generate the audit as a compact adaptive decision tree, not as independent checklist questions.

Unlike `grill-me`, this skill does not ask one question at a time. It creates the full question set in one response and includes recommended answers the user can accept, reject, or modify. If the user says they approve the recommended answers, treat those recommendations as selected decisions and produce the resulting plan or implementation direction.

## Relationship To Other Skills

`fastgrill` includes the full `grill-me` decision discipline, but changes the interaction pattern from a one-question-at-a-time interview to a complete expert audit delivered in one response.

- Use `grill-me` pressure, specificity, option framing, recommended answers, rationale, compatibility notes, feedback notes, dependency ordering, and answer-dependent follow-up logic.
- Use `clarify-ambiguity` before generating the audit only when a wrong assumption would materially change the output or waste significant work.
- If the ambiguity is low-risk, state the assumption and proceed.
- If the answer can be discovered from the codebase, inspect the codebase instead of asking the user.
- If the user later asks to continue interactively, switch to asking exactly one highest-leverage unresolved question at a time using the embedded Grill-Me Interview Protocol below.

## When To Use

Use this skill when the user asks for any of the following:

- `fastgrill`
- any requested number of questions, such as `10 questions`, `20 questions`, or `30 questions`
- `give me all questions`
- `expert questionnaire`
- `deep dive questions`
- `I approve your recommended answers`
- a grill-me style review where the user wants the whole decision tree at once

Use for architecture, product design, implementation plans, process design, operational policy, skill creation, data engineering workflows, incident response design, or enterprise platform decisions.

## When Not To Use

Do not use when:

- The user wants a normal one-question-at-a-time interview. Use `grill-me` instead.
- The request is clear and implementation should start immediately.
- The user needs a short answer, not a decision audit.
- The topic is high-stakes and missing a material fact that cannot be inspected. Ask one targeted clarification first.

## Workflow

1. Identify the target decision or problem.
2. Classify the input as concrete, partial, or imaginative.
3. If the input is partial or imaginative and the user has not provided enough context, ask for a dense info dump before generating the audit.
4. Inspect codebase/project context if it can answer implementation or existing-system questions.
5. Apply `clarify-ambiguity` rules:
   - Ask one targeted question only if missing information materially changes the audit.
   - Otherwise state the assumption and proceed.
6. Build a breadth-first decision map across goals, users, constraints, system shape, risks, validation, rollout, and governance.
7. Generate the requested number of expert-level questions grouped by decision area.
8. For each question include options, recommended answer, evidence needed, failure mode, dependency/adaptation note, compatibility, and feedback note.
9. If the user pre-approves recommendations, mark all recommended answers as accepted and add a concise accepted-decision summary.
10. If implementation is requested, convert accepted decisions into an execution plan or code changes.

## Intake Rule

Before generating questions, decide whether the user gave enough source material to support a meaningful audit.

If the topic is concrete enough, proceed immediately and state assumptions.

If the topic is broad, imaginary, early-stage, or missing the user's intended outcome, ask exactly one intake prompt:

```markdown
Before I fastgrill this, give me the raw version of the idea: what you imagine, what you want to do, who it is for, what already exists, what constraints matter, what must not happen, and what success would look like. Messy notes are fine.
```

Do not ask this intake prompt when the user already supplied a proposal, codebase, architecture, plan, PR, requirements document, or enough context to infer the decision map.

Use the intake response to create a breadth-first search of the decision space:

- First identify the top-level branches: goal, audience, scope, constraints, architecture, data/state, permissions, risk, validation, rollout, operations, and governance.
- Then generate questions that cover each branch before going deep.
- Then use recommended answers to adapt later questions inside each branch.
- Preserve rejected or uncertain branches in the adaptive map instead of losing them.

## Deep-Dive Question Standard

Every fastgrill question must be as deep as a strong interactive `grill-me` question, not a generic checklist prompt.

A valid question must:

- Force a decision, not merely ask for a preference.
- Name the concrete tradeoff, constraint, or failure mode being tested.
- Make the user choose between two or three incompatible options.
- Include the recommended answer and why it is recommended.
- State what evidence would prove or disprove the recommendation.
- Explain what breaks, becomes expensive, or becomes irreversible if the wrong answer is chosen.
- Be grounded in discovered project facts when files, code, docs, tickets, logs, or configs are available.

Avoid shallow questions such as:

- "What is the goal?"
- "Who are the users?"
- "How should we test it?"
- "What are the risks?"

Rewrite them as pressure questions:

- "Which measurable outcome should outrank all others when scope conflicts appear: latency, correctness, adoption, cost, or operator control?"
- "Which user role is allowed to make irreversible changes, and what audit evidence must exist before that action is trusted?"
- "What failure must the test suite catch before launch: wrong answer, slow answer, unauthorized action, silent data loss, or rollback failure?"
- "Which risk deserves design priority because it would force a rollback instead of a patch?"

If a draft question could be answered with "it depends", rewrite it until the options expose what it depends on.

## Adaptive Audit Rules

Generate the questions as a decision tree with spread, not as isolated prompts.

For each question, specify:

- `Depends on`: earlier question numbers or assumptions that shape this question.
- `If accepted`: which later questions become confirmed, narrowed, or higher priority.
- `If rejected`: which later questions need rewriting, become risky, or should be replaced.

Maintain coverage while adapting:

- Keep at least one question for each relevant major area in the coverage checklist.
- Do not let early answers collapse the audit into only architecture, only UX, or only testing.
- If an answer branch makes a planned question irrelevant, replace it with another question from the same coverage area.
- Keep dependency notes compact. Do not expand every possible branch into long prose.
- When the user pre-approves recommended answers, apply the `If accepted` path and summarize which branches were assumed.

Use adaptive questions like:

- "If Q2 accepts internal operators as primary users, Q11 should prioritize audit search over public discovery. If Q2 rejects that, Q11 must test external-user navigation and support burden."
- "If Q7 accepts correctness as the launch-blocking failure, Q23 should require golden tests. If rejected, Q23 should shift to monitoring and rollback proof."
- "If Q14 rejects retries, Q15 must prove idempotency is unnecessary or the rollout plan must include manual recovery."

## Token Budget

Default to compact output. A good fastgrill response should usually fit in 6k-9k output tokens.

- Keep each table cell short: one sentence or sentence fragment.
- Put detailed branching only in the adaptive map, not repeated in every row.
- If the user asks for maximum depth, expand to 10k+ tokens only when the task justifies it.
- If the platform or context budget is tight, produce fewer questions first and label the audit size clearly, such as `Compact fastgrill: 10 questions`.

## Embedded Grill-Me Interview Protocol

Interrogate the uncertain topic until the decision tree is explicit and every meaningful branch has either been chosen, rejected, deferred with rationale, or identified as unknown.

For normal `fastgrill` use, do not ask one question at a time. Generate the complete audit in one response. When the user asks for interactive grilling after the audit, ask exactly one question at a time. Make each question specific, decision-oriented, and grounded in the current state of the topic.

For every question, include a recommended answer and the reasoning behind that recommendation. Keep the recommendation concise enough that the user can accept, reject, or modify it directly. The question must pressure-test a real branch in the decision tree.

When the user has not explicitly specified a branch, provide two or three concrete options before the recommended answer. Include compatibility notes and feedback notes alongside the recommendation so the user can understand the impact quickly.

### Grill-Me Working Method

Start by identifying the highest-leverage unresolved decision. Prefer questions that unblock later branches over questions that merely collect preferences. Use the skill for architecture choices, process design, product scope, implementation strategy, operational policy, and any other situation where the user is unsure and needs reliable option selection.

Resolve dependencies in order:

1. Clarify goals, non-goals, users, and success criteria.
2. Establish constraints such as time, budget, technical environment, operational limits, and risk tolerance.
3. Explore architecture, data flow, interfaces, ownership, failure modes, and migration or rollout strategy.
4. Test edge cases, reversibility, observability, security, privacy, performance, maintainability, and support burden.
5. Confirm the chosen path and any explicitly deferred decisions.

After each user answer in interactive mode, update the inferred plan silently and choose the next most important unresolved branch. Do not dump a long questionnaire unless the user asks for fastgrill-style output again.

### Grill-Me Codebase Rule

If a question can be answered by exploring the codebase, inspect the codebase instead of asking the user. Report the discovered fact briefly only when it matters to the next question or recommendation.

Use local evidence for implementation details, existing patterns, dependencies, APIs, tests, and constraints. Ask the user only for intent, priorities, tradeoffs, or facts that are not reasonably discoverable.

### Grill-Me Question Shape

Use this format for interactive follow-up questions:

```markdown
Question: ...

Options:
- Option A: ...
- Option B: ...

Recommended answer: ...

Compatibility: ...

Feedback note: ...
```

Use two or three options only. Mark tradeoffs plainly, and keep compatibility and feedback notes specific to the current decision. Add one short rationale when the recommendation is not obvious. Avoid multi-part questions unless the parts are inseparable.

When the user accepts or modifies a recommendation, proceed to the next question. When the user rejects it, ask the next question needed to understand the rejected branch.

## Required Output Shape

Use this format unless the user asks otherwise:

```markdown
Assumption: ...

Accepted-default mode: on|off

## Decision Audit

| # | Area | Deep-Dive Question | Options | Recommended Answer | Evidence Needed | Failure Mode If Wrong | Depends / Adapts |
|---:|---|---|---|---|---|---|---|
| 1 | ... | ... | A: ... / B: ... / C: ... | ... | ... | ... | Depends on: ...; If accepted: ...; If rejected: ... |

## Adaptive Follow-Up Map

- If Q... = ..., rewrite Q... around ...
- If Q... is rejected, replace Q... with ...
- Accepted-default path assumes: ...

## Accepted Decision Summary

- ...

## Next Execution Plan

1. ...
```

## Question Coverage Checklist

The questions should cover these areas when relevant. For a shorter audit, merge related areas while keeping coverage balanced. For a longer audit, split the highest-risk areas into multiple questions:

1. Goal and success criteria
2. Users and roles
3. Non-goals
4. Existing infrastructure reuse
5. Data or system boundaries
6. Permissions and access
7. Inputs and outputs
8. Interface/API contract
9. State and persistence
10. Versioning
11. Search/discovery
12. Evaluation and quality gates
13. Observability/tracing
14. Error handling
15. Retry/idempotency
16. Security and secret handling
17. Governance and approval
18. Cost and operational complexity
19. Scalability/performance
20. Compatibility with existing modules
21. Migration and rollout
22. Rollback
23. Testing strategy
24. Automation vs human review
25. UX/operator workflow
26. Documentation requirements
27. Audit logging
28. Failure modes
29. Edge cases
30. Final decision and implementation priority

## Recommendation Rules

- Prefer reuse over new systems unless the audit proves a gap.
- Prefer one coherent workflow over merging incompatible behaviors.
- Prefer governed automation over unrestricted execution.
- Prefer eval-proven decisions over subjective matching.
- Prefer explicit user approval for irreversible, expensive, privileged, or production-impacting operations.
- If the user says they accept recommendations, do not keep asking for each answer; summarize accepted decisions and proceed.
- Reject generic checklist questions during drafting. Replace them with pressure questions that expose tradeoffs, required proof, and consequences.
- Preserve spread while adapting. Every replacement question should keep the original coverage area unless the user explicitly narrows scope.

## Example Invocation

User:

```text
$fastgrill over slash commands for Gemini, Claude, and Codex CLI sessions. I approve your recommended answers.
```

Expected behavior:

- Inspect existing slash-command implementation if available.
- Produce the requested number of advanced questions with options and recommended answers.
- Treat recommended answers as accepted.
- Summarize the chosen architecture and next implementation plan.
