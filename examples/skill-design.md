# Example: Skill Design

Prompt:

```text
$fastgrill a Codex skill that turns vague user ideas into implementation briefs. Keep output compact but deep.
```

Expected behavior:

- If the user's idea lacks enough detail, ask for one raw info dump first.
- Preserve spread across trigger design, skill boundaries, output format, dependency handling, examples, validation, and portability.
- Avoid generic questions like "What is the goal?" and rewrite them as decision-pressure questions.
