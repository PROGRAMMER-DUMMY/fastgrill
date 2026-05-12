# Fastgrill

Fastgrill is a model-agnostic AI workflow for turning vague ideas, architecture plans, product decisions, and implementation proposals into an adaptive decision audit.

It compresses a long `grill-me` style interview into one structured pass:

```text
intake -> breadth-first decision map -> deep questions -> recommended defaults -> adaptive follow-up map
```

## Why Use It

Normal AI brainstorming often produces generic questions. Interactive interviews can go deeper, but they take many turns and are hard to share with a team.

I built Fastgrill from a practical observation: in many `grill-me` sessions, the recommended answer was the answer I ultimately accepted. If those recommendations are usually correct, the model can simulate the interview path first, expose its assumptions, and let me correct only the branches that matter.

Fastgrill is designed for the middle path:

- ask for raw context when the idea is vague
- cover the whole decision space breadth-first
- generate hard questions with concrete tradeoffs
- choose recommended answers as provisional defaults
- adapt later questions from those defaults
- preserve coverage across goals, architecture, security, testing, rollout, operations, and governance
- output something you can actually review, share, or turn into a plan

It can be used as:

- a Codex skill
- a Codex CLI instruction or reusable agent workflow
- a Claude Code instruction/workflow
- a Gemini CLI instruction/workflow
- a Claude project instruction
- a ChatGPT custom instruction / reusable prompt
- a Gemini Gem instruction
- a system prompt for an internal AI agent
- a terminal-agent planning prompt for repo work, architecture review, and implementation decisions
- a plain Markdown workflow copied into any capable LLM

## Best For

- architecture review
- product planning
- AI agent or skill design
- implementation strategy
- operational policy
- process design
- pre-meeting decision audits
- turning messy ideas into execution plans

## Not Best For

Use a live one-question-at-a-time interview when the human's answers are likely to radically change the next question. Fastgrill simulates that path using recommended defaults, but it cannot replace real human feedback for highly uncertain decisions.

## Quick Setup For CLI Agents

No complex setup is required.

1. Open `fastgrill/SKILL.md`.
2. Copy the whole file.
3. Paste it into your CLI agent's instruction file, custom command, reusable prompt, or project rules.
4. Save it.
5. Ask the agent to use Fastgrill with however many questions you want.

```text
Use Fastgrill on this idea/proposal. Ask intake first if the context is too vague. Give me 15 questions.
```

For Claude Code, Gemini CLI, Codex CLI, Aider-style tools, or internal terminal agents, treat `SKILL.md` as a reusable planning instruction.

Fastgrill works best as a pre-implementation planning command:

```text
Use Fastgrill before editing files. Build the adaptive decision audit, choose recommended defaults, then produce the execution plan.
```

## Optional Codex Skill Install

If you use Codex skills, you can also copy the `fastgrill/` folder into your Codex skills directory:

```text
~/.codex/skills/fastgrill/
```

The required skill file is:

```text
fastgrill/SKILL.md
```

## Example Prompts

```text
$fastgrill this product idea. Ask intake first if needed.
```

```text
$fastgrill this architecture proposal. I approve your recommended answers unless you find a blocker.
```

```text
Use fastgrill on this AI agent workflow and produce the accepted decision summary.
```

## Output Shape

Fastgrill usually produces:

- assumptions
- accepted-default mode
- the number of questions you asked for
- evidence needed
- failure modes if wrong
- dependency and adaptation notes
- adaptive follow-up map
- accepted decision summary
- next execution plan

## License

MIT
