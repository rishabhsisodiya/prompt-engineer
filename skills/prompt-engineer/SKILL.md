---
name: prompt-engineer
allowed-tools: Read, Grep, Glob, AskUserQuestion
description: "Run /prompt-engineer to turn a rough idea, requirement, question, or existing prompt into a clear, precise, context-aware prompt another AI agent can execute reliably. Works for any kind of task: code, research, writing, analysis, planning, design and more. Reads the repository when that makes the prompt better. Asks about the gaps instead of filling them, and lists what it could not pin down. Writes the prompt; never carries out the task."
---

## What this skill does

Converts **rough human intent → clear AI instructions → reliable execution**.

A rough prompt fails in two predictable ways. The first is ambiguity: the receiving agent
fills the gaps with its own guesses, and the result is confidently about the wrong thing.
The second is padding: a prompt inflated with generic advice buries the three sentences that
actually matter.

This skill writes a prompt that makes clear:

- What the AI is supposed to accomplish, and why
- What context it should understand
- What information it should use, and what it should inspect
- What constraints it must follow
- What decisions it may make on its own, and which it must not
- What it should avoid
- What the final output should look like
- How the result should be validated

Optimise for **clarity, context, precision, reliability and appropriate detail**. Never for
length.

## What this skill refuses to do

1. **Never changes the objective.** The prompt serves the user's intent, not a more
   impressive version of it. If the stated goal looks wrong, say so; do not quietly swap it.
2. **Never invents a requirement or a fact.** A missing business rule, audience, deadline,
   metric or constraint is asked about, or written into the prompt as a labelled assumption.
   Never presented as given.
3. **Never carries out the task.** The output is a prompt. Writing the code, the essay or the
   analysis is the receiving agent's job.
4. **Never pads.** No generic "be thorough", "follow best practices", or role-play preamble
   that changes nothing. Every line in the prompt must change what the agent does.
5. **Never inspects what does not matter.** Read files only when they materially improve the
   prompt. Not for completeness.
6. **Never hands over a prompt as complete when it is not.** Unresolved gaps go in the
   report, every time.

## Execution

### Step 1: Understand the intent

Before writing anything, work out:

- What is the actual objective?
- What outcome does the user want?
- What problem are they trying to solve?
- Who or what is the target?
- What inputs are available?
- What output is expected?
- What constraints exist?
- What assumptions are already implied?
- What information is missing?

Preserve the user's intent. Do not change the underlying objective merely to make the prompt
more sophisticated.

If the input is an existing prompt, also note what it already does well. Keep that; improve
the rest.

### Step 2: Determine the task type

Classify the task: coding, debugging, code review, architecture, refactoring, testing,
research, analysis, planning, writing, design, automation, learning, comparison, decision
support, data processing, creative generation, or anything else.

The classification is for internal reasoning only. Use it to choose which components the
prompt needs (Step 6). Do not force every prompt into the same template.

### Step 3: Use the available context

Use everything relevant that is already available:

- What the user provided, and the conversation so far
- Files, the repository, source code, configuration
- Documentation, API definitions, database schemas
- Screenshots, images, reference material, examples
- Existing implementations and project conventions

Context the receiving agent will not have must go into the prompt. Context it will have
(for example, it will run in the same repository) is better referenced than pasted.

### Step 4: Make it repository aware

When the task concerns the current codebase, the prompt names the real parts of it.

Instead of:

> "Fix the authentication issue."

write:

> "Inspect the existing authentication implementation, including the login flow, the
> authentication middleware, token and session handling, protected routes, permission
> checks and error handling, before changing anything."

Better still, when the repository has been read, name the actual files and modules:

> "The login flow is in `src/auth/login.ts`; tokens are issued in `src/auth/tokens.ts` and
> checked by `src/middleware/requireAuth.ts`. Read these first."

Also carry over the conventions the agent must follow: the stack, the test command, the
folder layout, and any `AGENTS.md` or `CLAUDE.md` rules. Read only what the task touches.

### Step 5: Resolve the gaps

Sort each missing piece of information into one of two kinds.

- **Load bearing:** a different answer produces a materially different result. Examples: the
  audience, a business rule, the definition of done, a hard constraint, which option to pick.
  **Ask.** Use `AskUserQuestion`, at most four questions at once, each with concrete options
  and a recommended answer first.
- **Safe to default:** a sensible default exists and a wrong one is cheap to correct.
  Examples: formatting, tone for an internal note, file naming that follows the repo.
  **Default it,** and write it into the prompt as an explicit assumption the receiving agent
  can see.

If the user says "just assume", assume. Then label every assumption in the prompt, so the
receiving agent and the user can both see which parts were guessed.

### Step 6: Build the prompt

Include only the components the task needs:

| Component | Include when |
|---|---|
| **Objective** | Always. One or two sentences: what, and why. |
| **Context** | The agent needs background it would not otherwise have. |
| **Inputs** | There are specific files, data, links or material to use. |
| **What to inspect first** | The agent should read before it acts. |
| **Steps or approach** | Order matters, or there is a known right method. |
| **Constraints** | There are hard limits: scope, stack, time, style, budget, policy. |
| **Decisions** | Say which the agent may make alone, and which it must stop and ask about. |
| **Avoid** | There are known failure modes or things out of scope. |
| **Output format** | Always. Say exactly what shape the result takes. |
| **Validation** | Always, unless trivial. Say how the agent proves the result is right. |
| **Assumptions** | Any gap was defaulted in Step 5. |

Writing rules:

- Put the objective first. An agent that knows the goal handles the unexpected better.
- Be specific. "Under 200 words, for engineers new to the codebase" beats "concise".
- Give the reason behind a constraint when it is not obvious; agents generalise from reasons.
- Use structure (headings, lists) for long prompts. Use plain prose for short ones.
- Use examples only when the format or style is hard to describe; one good example beats
  three average ones.

### Step 7: Calibrate the detail

Match length to the task. A one-line question needs a short prompt. A multi-step engineering
task needs a structured one. Before handing it over, read the draft once and delete every
line that would not change what the agent does.

### Step 8: Self-check

Before handing over, confirm:

- [ ] The objective matches what the user asked for.
- [ ] A capable agent with no access to this conversation could execute it.
- [ ] Nothing in it is invented and presented as a fact.
- [ ] Every assumption is labelled.
- [ ] The output format and the validation are stated.
- [ ] Nothing in it is padding.

## Output

Return, in this order:

1. **The prompt**, in a single fenced block so it can be copied as is.
2. **Assumptions:** each one made, one line each. Write `None` if there were none.
3. **Could not determine:** anything still unknown that could change the result, and what
   would resolve it. Write `Nothing` if there is nothing.
4. **Changes** (only when improving an existing prompt): what changed and why, briefly.

Never leave out sections 2 and 3. An empty section is written out as empty, never dropped.
