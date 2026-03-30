---
name: book-reviewer
description: >
  Use this skill to review, edit, or critique any book manuscript, chapter, or outline.
  Triggers when the user says "review my book", "edit this chapter", "give me feedback",
  "proofread", "developmental edit", "line edit", "is this chapter good?", "what's wrong
  with my writing", "check my outline", "review my draft", or any request to evaluate,
  critique, improve, or polish written content intended for publication. Also triggers when
  a completed chapter or outline is present and the user asks for quality feedback. Works
  hand-in-hand with the book-writer skill — always use this skill after book-writer produces
  content if the user wants feedback or editing.
---

# Book Reviewer Skill

This skill provides professional-grade editorial feedback at two levels: **Developmental** (big picture)
and **Line** (sentence level). Always clarify which type of review the user wants before starting,
unless it's obvious from context.

---

## Two Types of Review

### 1. Developmental Edit (Big Picture)
For: outlines, full chapters, or complete manuscripts
Evaluates: structure, pacing, argument clarity, character arcs, logical flow, and whether the book
delivers on its core promise.

### 2. Line Edit (Sentence Level)
For: completed chapters or sections
Evaluates: prose quality, sentence rhythm, word choice, clarity, redundancy, grammar, and style consistency.

### 3. Full Review (Both)
If the user wants both, do Developmental first, then Line Edit. Never reverse the order — it's wasteful
to line-edit prose that may structurally change.

---

## Phase 1: Load Context

Before reviewing, load:
1. `books/<title-slug>/book-config.md` — to understand genre, tone, audience, and POV
2. The outline (`outline.md`) — to evaluate whether the chapter serves the book's arc
3. The chapter(s) to review

If no `book-config.md` exists, ask the user for: genre, target audience, and intended tone before proceeding.

---

## Developmental Edit Framework

### Structure Check
- Does this chapter have a clear beginning, middle, and end?
- Does it open with a hook?
- Does it close with forward momentum (fiction: tension; non-fiction: takeaway + bridge)?
- Is the chapter's core purpose served? (What was promised in the outline — did it deliver?)

### Pacing Check
- Are there sections that drag? (Too much setup, too much explanation, repetition)
- Are there sections that feel rushed? (Important moments glossed over)
- Is the word count appropriate for the content?

### Argument / Narrative Check
Non-fiction: Does the logic flow? Is each claim supported? Are transitions clear between ideas?
Fiction: Does every scene serve the story? Is there enough tension? Do characters behave consistently?

### Voice & Consistency Check
- Is the POV maintained?
- Does the tone match `book-config.md`?
- Does it sound like the same author as previous chapters?

### Developmental Report Format

```markdown
## Developmental Edit — Chapter [N]: "[Title]"

### Overall Assessment
[2-3 sentences: what's working and what needs the most attention]

### Strengths
- [specific thing that works well, with a brief example]
- [another strength]

### Issues to Address

#### 🔴 Priority (must fix before publication)
1. [Issue]: [explanation + suggested fix]

#### 🟡 Recommended (significantly improves the chapter)
1. [Issue]: [explanation + suggested fix]

#### 🟢 Optional (polish-level improvements)
1. [Issue]: [explanation]

### Structural Suggestions
[Any reordering, cuts, or additions that would strengthen the chapter]

### Pacing Notes
[Where it drags, where it rushes, with approximate location markers]
```

---

## Line Edit Framework

### What to Look For

**Clarity**
- Sentences that require re-reading to understand
- Ambiguous pronoun references ("he said to him" — which him?)
- Overloaded sentences (too many clauses)

**Prose Quality**
- Passive voice overuse (flag but don't auto-fix — sometimes intentional)
- Adverb overuse (especially -ly adverbs modifying "said")
- Weak verb + adverb combos ("walked quickly" → "strode")
- Redundancy ("end result", "basic fundamentals", "past history")
- Filler phrases: "It's worth noting that", "As I mentioned", "Needless to say"

**Rhythm**
- Three or more sentences of the same length in a row — vary them
- Monotonous sentence openings (all starting with "The" or "I")

**Word Choice**
- Vague words that could be more specific ("thing", "stuff", "very", "really", "quite")
- Inconsistent terminology (calling the same concept two different names)

**Grammar & Mechanics**
- Comma splices, run-ons, fragments (note: intentional fragments in fiction are fine)
- Dialogue punctuation
- Tense consistency

### Line Edit Report Format

Provide a brief summary, then inline suggestions using this format:

```markdown
## Line Edit — Chapter [N]: "[Title]"

### Summary
[2-3 sentences on overall prose quality and the most common issues found]

### Suggested Edits

**Page/Section: [reference point]**
- ORIGINAL: "[exact original text]"
- SUGGESTED: "[revised text]"
- WHY: [one-line explanation]

**Page/Section: [reference point]**
- ORIGINAL: "..."
- SUGGESTED: "..."
- WHY: ...

### Patterns to Watch Going Forward
- [recurring issue #1]: [brief description]
- [recurring issue #2]: [brief description]
```

Provide a maximum of **20 inline edits per chapter**. Focus on the highest-impact changes.
If there are more than 20 issues, note this and offer a deeper pass.

---

## Outline Review

If reviewing an outline (not a chapter):

### Outline Review Checklist
- [ ] Does the arc have a clear beginning, escalating middle, and satisfying end?
- [ ] Is each chapter necessary? Could any be merged or cut?
- [ ] Are there any chapters that feel too thin (not enough to fill target word count)?
- [ ] Does the opening chapter hook the reader?
- [ ] Does the closing chapter deliver on the book's core promise?
- [ ] Non-fiction: Is the logical progression clear? Can a reader follow the argument?
- [ ] Fiction: Are there clear turning points? Does the protagonist drive the story?
- [ ] Is the pacing balanced? (No 5 slow chapters in a row, no whiplash)

### Outline Review Format

```markdown
## Outline Review: "[Book Title]"

### Overall Verdict
[Strong / Needs Work / Significant Restructuring Needed] — [2-3 sentence summary]

### What's Working
[Strengths of the current outline]

### Structural Concerns
[Numbered list of issues with specific chapter references]

### Recommended Changes
[Specific suggestions: merge these chapters, add a chapter about X, move Chapter 7 to earlier, etc.]

### Chapter-by-Chapter Notes
[Only include chapters with specific notes — skip chapters that look good]
Ch [N] — "[Title]": [note]
```

---

## Tone When Delivering Feedback

- Be direct and specific. Vague feedback ("this could be better") is useless.
- Lead with what's working before what isn't — but don't bury the real issues.
- Every critique should come with a suggested direction for improvement.
- Respect the author's voice — don't rewrite to sound like you; flag issues and suggest options.
- If a chapter is genuinely strong, say so clearly. Don't manufacture problems.

---

## After the Review

Ask the user:
- "Would you like me to rewrite any of the flagged sections?"
- "Ready to continue with Chapter [N+1], or would you like to revise this one first?"

If they want rewrites, apply only the changes they confirm — never rewrite wholesale without permission.
