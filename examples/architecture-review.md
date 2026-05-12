# Example: Architecture Review

Prompt:

```text
$fastgrill this architecture proposal. I approve your recommended answers.

We are building an internal agent platform. Agents can read repo files, run tests, create patches, and ask for approval before risky actions. We need audit logs, team-level permissions, reusable skills, and a way to evaluate whether agent changes improved outcomes.
```

Expected behavior:

- Treat recommended answers as provisional decisions.
- Pressure-test permissions, sandboxing, audit logs, rollback, eval design, skill installation, data retention, and failure recovery.
- Produce an accepted decision summary and next execution plan.
