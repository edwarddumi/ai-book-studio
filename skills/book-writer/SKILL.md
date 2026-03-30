---
name: book-writer
description: >
  Use this skill to write full-length books — fiction or non-fiction — from scratch or from a book-config.md.
  Triggers when the user says "write a book", "write a chapter", "generate my book", "continue the book",
  "start writing", "write the outline", "write chapter X", or any request to produce substantial long-form
  content intended for publication. Also triggers when a book-config.md file is present and the user asks
  to begin or continue writing. Supports all genres: business, self-help, fiction, how-to, memoir, real estate,
  and more. Designed for full-length works of 20+ chapters and 50,000+ words. Use this skill even for partial
  requests like "write the first 3 chapters" or "draft the outline."
---

# Book Writer Skill

This skill produces full-length, publication-ready books. It follows a structured three-phase approach:
**Plan → Write → Save**, and uses a `book-config.md` as the single source of truth across all sessions.

---

## Phase 1: Book Config

Before writing anything, ensure a `book-config.md` exists. If the user hasn't provided one:
1. Ask the minimum required questions (see **Required Fields** below)
2. Generate the `book-config.md` and save it to the book's project folder
3. Show it to the user and confirm before proceeding

### book-config.md Required Fields

```markdown
# Book Config

## Identity
title: ""
subtitle: ""             # optional
author_name: ""
genre: ""                # fiction | non-fiction | self-help | how-to | memoir | business | other
target_audience: ""      # e.g. "first-time homebuyers", "aspiring entrepreneurs", "YA readers"

## Structure
target_word_count: 50000  # minimum for full-length; adjust per genre
target_chapters: 20
chapter_avg_words: 2500

## Voice & Style
pov: ""                  # first-person | third-person | second-person | omniscient
tone: ""                 # e.g. "conversational and motivating", "authoritative and practical", "dark and literary"
writing_style_notes: ""  # any extra style guidance, comps, influences

## Content
core_premise: ""         # one sentence: what is this book about and why does it matter?
key_takeaways: []        # 3-5 bullet points the reader will walk away with
chapter_structure: ""    # e.g. "each chapter opens with a story, then teaches a concept, ends with action steps"

## Publishing
platform: ""             # Amazon KDP | Gumroad | Leanpub | Other
output_format: docx      # docx | markdown | both

## Progress (auto-updated)
outline_complete: false
chapters_written: 0
current_chapter: 0
last_updated: ""
```

Save the config to: `books/<title-slug>/book-config.md`

---

## Phase 2: Outline Generation

Once the config is confirmed, generate the full book outline before writing any chapters.

### Outline Rules
- Every chapter gets: a number, a title, a 2-3 sentence summary, and an estimated word count
- The outline should tell a complete story arc or logical progression from start to finish
- Non-fiction: chapters should build on each other — each one is a stepping stone
- Fiction: follow a 3-act or 5-act structure; identify turning points and character arcs
- Self-help / how-to: open with the problem, build frameworks, deliver transformation, close with integration

### Outline Format

```markdown
# [Book Title] — Full Outline

## Overview
[2-3 sentence summary of the complete book arc]

---

## Front Matter
- Foreword / Introduction (~1,000 words)
- How to Use This Book (optional, for non-fiction)

---

## Part 1: [Part Title] (if applicable)

### Chapter 1: [Title]
**Summary:** [2-3 sentences of what this chapter covers]
**Key Points:** [3-5 bullet points]
**Est. Words:** 2,500
**Opens with:** [scene, story, stat, question, or concept]

### Chapter 2: [Title]
...

---

## Back Matter
- Conclusion / Epilogue (~1,000 words)
- About the Author
- Resources / Bibliography (if applicable)
- Call to Action (for non-fiction/business books)
```

Save the outline to: `books/<title-slug>/outline.md`
Update `book-config.md`: set `outline_complete: true`

---

## Phase 3: Chapter Writing

Write chapters one at a time. Each chapter must feel complete — not a draft, not a skeleton. Publication-ready prose.

### Before Writing Each Chapter
1. Re-read the `book-config.md` (especially tone, POV, and style notes)
2. Re-read the chapter's outline entry
3. Re-read the previous chapter's final paragraph (for continuity)
4. Check `chapters_written` count in the config

### Chapter Writing Rules

**Opening Hook (first 150-300 words)**
- Fiction: drop into a scene, mid-action or mid-emotion
- Non-fiction: open with a story, a surprising statistic, a bold claim, or a question
- Never start with "In this chapter, we will…" — show, don't announce

**Body**
- Maintain consistent POV and tense throughout
- Use the chapter structure defined in `book-config.md`
- Vary sentence length: short punchy sentences for impact, longer ones for flow
- Ground abstract ideas with concrete examples, stories, or data
- Every 400-600 words, create a natural visual break (subheading, scene break, or white space moment)

**Subheadings (non-fiction)**
- Use subheadings every 400-700 words
- Subheadings should be active and specific: "Why Most People Quit Before They Start" not "Introduction"

**Closing (final 200-300 words)**
- Fiction: end on tension, revelation, or emotional resonance — give the reader a reason to turn the page
- Non-fiction: summarize the core insight, then bridge to the next chapter
- Self-help: close with an "Action Step" or "Reflection" section

**Word Count Target**
- Hit the `chapter_avg_words` from the config (default: 2,500)
- Acceptable range: ±300 words
- Never pad. Never truncate. Write to the natural end of the chapter's story.

### Chapter File Naming
Save each chapter to: `books/<title-slug>/chapters/ch{N:02d}-{slug}.md`
Example: `books/the-first-deal/chapters/ch01-the-day-everything-changed.md`

### After Each Chapter
Update `book-config.md`:
- Increment `chapters_written`
- Set `current_chapter` to next chapter number
- Update `last_updated` to today's date

---

## Genre-Specific Guidance

Read the relevant reference file for deep genre-specific rules:

| Genre | Reference File |
|-------|---------------|
| Business / Entrepreneurship | `references/genre-business.md` |
| Self-Help / Personal Development | `references/genre-selfhelp.md` |
| How-To / Instructional | `references/genre-howto.md` |
| Real Estate | `references/genre-realestate.md` |
| Fiction (General) | `references/genre-fiction.md` |
| Memoir | `references/genre-memoir.md` |

Only load the reference file relevant to the current book's genre. If the genre doesn't match any file, use the general principles in this SKILL.md.

---

## Session Continuity

Books are written across multiple sessions. At the start of every session:

1. Load `books/<title-slug>/book-config.md`
2. Check `chapters_written` and `current_chapter`
3. Load the outline to orient to where we are
4. Load the last chapter written for continuity
5. Confirm with the user: "We're on Chapter X of Y. Ready to continue?"

If the user says "continue my book" without specifying a title, list the available book folders and ask which one.

---

## Quality Standards

Every chapter must pass these checks before saving:

- [ ] Hits target word count (±300 words)
- [ ] Opens with a hook — no "In this chapter…" openers
- [ ] Maintains consistent POV and tense
- [ ] No filler phrases: "It's important to note", "As we discussed", "In conclusion"
- [ ] Each paragraph advances the story or argument — no repetition
- [ ] Subheadings (non-fiction) are specific and active
- [ ] Ends with forward momentum

---

## Output Summary (what to tell the user after each chapter)

After saving each chapter, show:

```
✅ Chapter [N] written: "[Chapter Title]"
   Words: [count] / [target]
   File: books/<title-slug>/chapters/ch{N}-{slug}.md
   Progress: [chapters_written] / [target_chapters] chapters complete
   Next: Chapter [N+1] — "[Next Chapter Title]"
```

Then ask: "Ready to write Chapter [N+1], or would you like to review this one first?"
