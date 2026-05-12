# Example: Product Idea

Prompt:

```text
$fastgrill this idea:

I want to build a tool that helps small teams decide what to build next. They paste messy notes, customer feedback, and feature ideas. The tool should ask hard questions, find tradeoffs, and produce a prioritized implementation plan.
```

Expected behavior:

- Ask intake only if the idea is still too vague.
- Build a breadth-first decision map across users, success criteria, data inputs, prioritization method, collaboration, privacy, evaluation, rollout, and pricing.
- Generate the requested number of hard questions with recommended answers.
- Include an adaptive map showing how rejecting recommendations changes later questions.
