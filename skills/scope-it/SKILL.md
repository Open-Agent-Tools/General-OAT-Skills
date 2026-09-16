---
name: scope-it
description: >-
  Interrogates a vague task one question at a time until it is fully scoped, then
  writes a ready-to-use prompt: what (goals), why (the problem), constraints
  (guardrails), done (success criteria), and the open questions it deliberately
  left unresolved, each with the assumption being made. Builds an analysis tree of
  the problem and asks only the questions whose answers are not already discoverable.
  Use when the user has a half-formed task, wants to turn an idea into a brief for
  Claude or Codex, or asks to scope, spec, or define a piece of work before building.
allowed-tools:
  - AskUserQuestion
  - Read
  - Grep
  - Glob
  - Bash
  - Agent
  - Write
user-invocable: true
argument-hint: "[what you want built or figured out]"
---

# Scope It

Turn a vague request into a prompt someone can hand to Claude or Codex and get the
right thing back on the first try. You do this by interviewing the user, one question
at a time, until four things are pinned down: **what**, **why**, **constraints**, and
**done**.

## The analysis tree

Those four are the roots. Under each root hang the decisions that dimension actually
depends on — you build that tree from the user's opening message, not from a checklist.

The roots resolve in dependency order, because each one constrains the next:

1. **Why** — the problem, who has it, what breaks if nothing changes. Everything else
   is unanswerable while this is vague, because "is X in scope?" has no answer without
   knowing what pain X relieves.
2. **What** — the goal and its boundary. Which outcomes count, which adjacent things
   are explicitly *not* being asked for.
3. **Constraints** — the guardrails: stack, files and systems that must not move,
   compatibility, budget, deadline, taste, things previously tried that failed.
4. **Done** — the success criteria and the quality bar, stated so that someone else
   could check them without asking a follow-up question.

The **frontier** is every unsettled decision whose prerequisites are already answered.
Ask from the frontier only. A question whose answer depends on something still open
belongs to a later round. Each answer reshapes the tree: it settles a node, opens the
ones below it, and sometimes prunes a whole branch as irrelevant.

## Running the interview

**Find facts yourself; ask only for decisions.** Anything the environment can answer —
what language the project is in, whether a test suite exists, which framework version
is pinned, what the CI does — you look up before asking. Dispatch subagents for the
slower lookups and keep interviewing while they run. Only judgment calls, preferences,
and things that live in the user's head are worth a question.

When a lookup answers something, fold it into the next question as context rather than
asking about it: *"There's already a pytest suite with 84% coverage, so the real
question is whether this needs new tests or just passes the existing ones."*

**Ask one question per turn, using AskUserQuestion.** One decision per question, 2–4
concrete options, your recommendation first and labeled `(Recommended)`, and a
description on each option that says what choosing it actually means for *this* task —
not generic tradeoff commentary. Never present options you would not accept as answers.

**Start immediately.** Don't ask the user to prepare anything. Extract everything you
can from their opening message, settle what you can from the environment, then ask the
first real question.

**Stop when the tree is empty**, or when the remaining questions would not change a
single line of the output prompt. Aim for the fewest questions that fully scope the
work — typically five to ten. If the user says "done", "enough", or "just write it",
stop and write it, marking anything unresolved as an open question in the output.

## The output

Write the prompt to `scope-<short-slug>.md` in the working directory, then show it in
full so the user can read it without opening the file.

```markdown
# <Task title in one line>

<One or two sentences: the request, stated plainly, as the user would say it out loud.>

## Why
The problem being solved, who has it, and what happens if it stays unsolved. Include
the context that makes the goal make sense — what this connects to, what it unblocks.

## What
The goal, and the boundary around it. What counts as in scope, stated concretely —
numbered when the work has distinct steps or more than one goal, one per line. Then
what is explicitly out of scope, so the reader doesn't widen the task on their own.

## Constraints
The guardrails: languages, versions, files and systems that must not change, patterns
to follow or avoid, budget, deadline, approaches already tried and why they failed.
Name real paths, commands, and versions found during the interview.

## Done
The success criteria, each one checkable by someone who wasn't part of this
conversation: the command that must pass, the behavior that must hold, the quality bar
to clear. Include how the work should be verified, not just what the end state is.

## Open questions
Always present, always last. Everything deliberately deferred, everything the
interview assumed rather than settled, and every place the reader is expected to use
their own judgment — each with the assumption being made in the meantime, so the
reader can act on it or challenge it. If genuinely nothing is open, write "None." That
is a claim worth making explicitly: it tells the reader the gaps were looked for and
not found, which is different from a section that was skipped.
```

## Quality bar for the prompt you write

The output is a prompt for a capable model, so it states intent and boundaries and
then gets out of the way.

- **Reason before request.** Why comes early enough that the reader can make their own
  judgment calls in the same direction you would.
- **Every criterion is checkable.** "Fast enough" is not a criterion; "p95 under 200ms
  on the existing benchmark" is. If a criterion can't be checked, it's a constraint or
  it's noise.
- **Boundaries are explicit.** Say what not to touch and what not to build. A capable
  model will otherwise tidy, refactor, and generalize past the edge of the task.
- **Concrete over abstract.** Real file paths, real commands, real version numbers —
  whatever the interview surfaced. Abstractions in a prompt become guesses in the work.
- **Describe outcomes, not keystrokes.** Prescribe a sequence only where the order
  genuinely matters. Over-specified steps make the output worse, not more reliable.
- **Never ask the reader to narrate or explain its own reasoning.** Ask for the work
  and the evidence behind it instead.
- **Plain sentences.** No arrow chains, no invented shorthand, no stacked compounds.
  The prompt is read cold by someone with none of this conversation's context.
- **No response-style boilerplate.** The prompt says what to build, not how to talk
  about it. Tone, formatting and turn structure belong to the reader's own setup, and a
  block that reads the same in every scope doc is noise the user has to skip past.
- **Nothing over ten.** If a section needs more than ten bullets, it is two sections or
  it is unscoped — go back and ask another question rather than writing a long list.

## How you work

No preamble before the first question, no recap of what was just settled, no closers.
One bounded action per numbered step. Stay on the critical path: finish the current
node before raising anything adjacent, and raise it separately if it matters. Be
matter-of-fact, including about failures — state location, cause, fix. Cap any list at
ten items. Close by naming the file path, saying it's theirs to edit before use, and
giving one concrete next step.
