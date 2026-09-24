# prompt-engineer

An [Agent Skill](https://agentskills.io) that turns a rough idea, requirement, question or
existing prompt into a clear, context-aware prompt another AI agent can execute reliably.

A vague prompt gets a confident answer to the wrong question. The agent fills every gap with
a guess, and nothing in the result tells you which parts were guessed. This skill asks about
the gaps that matter, labels the ones it defaulted, and writes the rest down precisely.

It works for any kind of task: code, debugging, research, writing, analysis, planning,
design, data work and more. It never reads your files: for a coding task, the prompt tells
the receiving agent what to inspect first and which conventions to follow, and that agent
does the reading. That keeps the skill light and the same everywhere.

Share a reference document and the prompt follows its format. Share none, and the prompt
applies the recognised standard structure for that kind of deliverable (a user manual, PRD,
test plan, API guide, report and so on) and writes the section outline in.

## What you get

Every run returns:

1. **The prompt**, ready to copy, and as short as it can be while still working.
2. **Open points:** every assumption it made and everything still unknown. Always present,
   even when empty. A result comes with its limits attached.

It asks at most one round of questions. It never loads unless you run it, so it costs
nothing in sessions where you do not.

## What it will not do

- Change your objective into a more impressive one.
- Invent a requirement, fact or business rule and present it as given.
- Carry out the task itself. It writes the prompt; the receiving agent does the work.
- Pad the prompt with generic advice that changes nothing.

## Install

Install it as a Claude Code plugin, available in every project:

```
claude plugin marketplace add rishabhsisodiya/prompt-engineer
claude plugin install prompt-engineer@prompt-engineer --scope user
```

Start a new session, then run:

```
/prompt-engineer:prompt-engineer <your rough idea, requirement, or existing prompt>
```

Plugin skills are namespaced by the plugin, hence the double name.

To pick up a new version:

```
claude plugin marketplace update prompt-engineer
claude plugin update prompt-engineer@prompt-engineer
```

To remove it:

```
claude plugin uninstall prompt-engineer@prompt-engineer
claude plugin marketplace remove prompt-engineer
```

Installed and checked with `claude plugin validate` on macOS. The skill itself is plain
Markdown in `skills/prompt-engineer/SKILL.md`, so another agent that reads Agent Skills can
use it by copying that folder, but only Claude Code has been tried.

## Related

[stop-guessing](https://github.com/rishabhsisodiya/stop-guessing) applies the same idea to a
coding workflow: a set of skills that stop a coding agent from inventing the decisions you
never made.

## License

MIT. See [LICENSE](LICENSE).
