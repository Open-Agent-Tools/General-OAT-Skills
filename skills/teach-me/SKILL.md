---
name: teach-me
description: >-
  Interactive lesson-based teaching skill. Researches a topic via the web,
  builds a structured curriculum of up to 10 lessons, and delivers them one
  at a time with comprehension questions. Supports difficulty flags
  (--beginner, --advanced, --expert) and lesson count (--lessons N).
  Use when the user wants to learn about any topic in a guided, incremental way.
allowed-tools:
  - Agent
  - Bash
  - Read
  - Grep
  - Glob
  - Write
  - WebSearch
  - WebFetch
  - AskUserQuestion
user-invocable: true
argument-hint: "<topic> [--beginner|--advanced|--expert] [--lessons N]"
---

# Teach Me — Interactive Lesson Skill

Deliver a structured, interactive learning experience on any topic. Research
the topic deeply, build a curriculum, then walk the user through lessons
one at a time with comprehension checks.

## Step 0 — Parse Arguments

Parse `$ARGUMENTS` to extract:

1. **Topic** — everything that is not a flag. This is required. If missing,
   ask the user what they want to learn about.
2. **Level flag** — one of `--beginner`, `--advanced`, `--expert`. If none
   is specified, default to `standard` (bachelor's-level professional with
   general domain background).
3. **Lesson count** — `--lessons N` where N is 1–10. Default is 10 if not
   specified.

**How levels affect content:**

| Level        | Vocabulary          | Depth                        | Scope                              |
|--------------|---------------------|------------------------------|------------------------------------|
| `--beginner` | Plain language, no jargon without definition | Foundational concepts, analogies, gentle intro | Core essentials only, skip advanced subtopics |
| `standard`   | Professional, assumes general education | Solid conceptual + practical coverage | Broad coverage of key subtopics |
| `--advanced` | Technical, domain-specific terms OK | Deeper mechanics, trade-offs, edge cases | Includes nuanced and less common subtopics |
| `--expert`   | Dense, assumes strong prior knowledge | Implementation details, internals, cutting edge | Comprehensive including niche and frontier topics |

## Step 1 — Research Phase

Tell the user:
> Researching **{topic}**... This will take a moment while I build your curriculum.

Launch **3 parallel research agents** using the Agent tool to gather
comprehensive material on the topic. Each agent should use WebSearch and
WebFetch to find current, authoritative information.

### Agent 1: Core Concepts & Foundations

subagent_type: general-purpose

Search the web for foundational material on the topic. Find:
- Authoritative tutorials, guides, and documentation
- Core terminology and definitions
- Key concepts a learner must understand
- The logical order concepts should be learned in
- Common misconceptions

Return a structured summary of findings with source URLs.

### Agent 2: Practical Applications & Examples

subagent_type: general-purpose

Search the web for practical, applied material on the topic. Find:
- Real-world use cases and examples
- Code snippets, patterns, or workflows (if technical)
- Best practices and common pitfalls
- Comparisons with alternative approaches
- Hands-on demonstrations or walkthroughs

Return a structured summary of findings with source URLs.

### Agent 3: Advanced Topics & Current State

subagent_type: general-purpose

Search the web for advanced and current material on the topic. Find:
- Recent developments, changes, or trends
- Advanced techniques and patterns
- Edge cases and limitations
- Expert-level insights and deep dives
- Community discussions and debates

Return a structured summary of findings with source URLs.

**Important:** Each research agent must use WebSearch to find relevant pages
and WebFetch to retrieve and read the most promising results. Aim for
breadth — the curriculum generation step will select and organize the
best material.

## Step 2 — Build Curriculum

Tell the user:
> Building your curriculum...

Using the combined research from all 3 agents, design a curriculum of
exactly **{lesson_count}** lessons. Consider:

1. **Logical progression** — each lesson builds on the previous one.
   Start with foundations, end with the most advanced material appropriate
   for the selected level.
2. **Scope filtering** — include/exclude subtopics based on the level flag
   (see the level table in Step 0).
3. **Balance** — mix conceptual explanation with practical examples. Include
   code snippets where they aid understanding.
4. **Completeness** — by the final lesson, the user should have a solid
   understanding of the topic at their selected level.

For each lesson, prepare:
- A **title**
- The **lesson body** (500–750 words). Write in clear prose. Use code
  snippets where they genuinely help illustrate a concept. Tailor
  vocabulary and depth to the selected level.
- A **multiple-choice comprehension question** with exactly **3 answer
  options** (A, B, C). One option must be clearly correct. The other
  two should be plausible distractors — not obviously wrong, but
  distinguishable from the correct answer by someone who understood
  the lesson. The question should test understanding, not just recall.
  (The 4th option slot is reserved for "Skip" in the UI.)

**Store the full set of lessons internally.** Do not output them all at once.

## Step 3 — Deliver Lessons Interactively

Tell the user:
> Ready! I've prepared **{lesson_count}** lessons on **{topic}** at the
> **{level}** level. Let's begin.

Then deliver lessons **one at a time** using this loop:

### For each lesson (1 through {lesson_count}):

1. **Print the lesson header:**
   ```
   ## Lesson {n}/{total}: {title}
   ```

2. **Print the lesson body** (500–750 words, written in Step 2).

3. **Present the multiple-choice question** using AskUserQuestion with
   the header set to `"Lesson {n}"`. Provide the 4 answer options plus
   a "Skip" option. Example structure:

   ```
   question: "{question_text}"
   header: "Lesson {n}"
   options:
     - label: "A) {option_a}"
       description: ""
     - label: "B) {option_b}"
       description: ""
     - label: "C) {option_c}"
       description: ""
     - label: "Skip"
       description: "Move to the next lesson"
   multiSelect: false
   ```

   **Important:** Do NOT put the answer explanation in the option
   descriptions — keep descriptions empty or minimal so the user
   isn't given the answer before choosing.

4. **Process the response:**
   - If the user selected **"Skip"**: acknowledge briefly and move to
     the next lesson.
   - If the user selected the **correct answer**: confirm they got it
     right with a 1–2 sentence explanation of why that answer is
     correct.
   - If the user selected an **incorrect answer**: provide a **condensed
     re-explanation** (3–5 sentences) that specifically addresses why
     the chosen answer is wrong and what the correct answer is, tying
     it back to the lesson content.

### After the final lesson:

Print a wrap-up message:
```
---
## Course Complete

That wraps up your {lesson_count} lessons on **{topic}**. Here's a quick
recap of what we covered:

{1-2 sentence summary of each lesson topic, as a numbered list}

Well done working through the material!
```

## Step 4 — Offer to Save

After the wrap-up, ask the user using AskUserQuestion:

> Would you like me to save these lessons to a markdown file for future
> reference?

- If **yes**: Write all lessons to a markdown file named
  `teach-me-{topic-slug}.md` in the current directory. The file should
  include:
  - A title and metadata header (topic, level, date)
  - All lessons with their full content
  - The comprehension questions (with answers noted)
  - A brief recap section at the end

  Confirm the file was saved and print the path.

- If **no** or the user declines: acknowledge and end.
