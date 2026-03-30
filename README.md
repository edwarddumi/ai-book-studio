# 📚 AI Book Studio

A complete skill suite for writing, editing, packaging, and designing full-length books using Claude AI.

Write a 50,000+ word book — any genre — from outline to publication-ready file, with a professional cover.

---

## What's Included

| Skill | What It Does |
|-------|-------------|
| `book-writer` | Generates outlines and writes full chapters — multi-genre, publication-quality prose |
| `book-reviewer` | Developmental edits (big picture) and line edits (sentence level) |
| `book-packager` | Assembles chapters into a KDP/Gumroad/Leanpub-ready `.docx` |
| `book-cover` | Generates 3 professional cover art variations via AI image generation |

---

## Supported Genres

- Business & Entrepreneurship
- Real Estate
- Self-Help & Personal Development
- How-To & Instructional
- Fiction (literary, thriller, romance, fantasy)
- Memoir

---

## Requirements

- [Claude.ai](https://claude.ai) account (Pro recommended for long writing sessions)
- `OPENROUTER_API_KEY` — for the `book-cover` skill (image generation via [OpenRouter](https://openrouter.ai))
- Skills installed in your Claude environment

---

## Quick Start

### 1. Install the Skills

Copy each skill folder from `skills/` into your Claude skills directory:

```
skills/
  book-writer/
  book-reviewer/
  book-packager/
  book-cover/
```

### 2. Create Your Book Config

Copy `templates/book-config-template.md` and fill it out for your book:

```bash
cp templates/book-config-template.md books/my-book-title/book-config.md
```

Fill in the fields — the more detail you provide, the better the output.
See `examples/sample-book-config.md` for a complete example.

### 3. Start Writing

Open Claude and say:

> "Write my book. The config is at books/my-book-title/book-config.md"

Claude will:
1. Confirm your config
2. Generate a full outline
3. Ask for your approval
4. Start writing chapter by chapter

---

## Example Workflow

```
You: "Write my book. Config is at books/the-first-deal/book-config.md"

Claude: [Reads config, generates 22-chapter outline]
        "Here's your full outline. Ready to start writing Chapter 1?"

You: "Looks great. Let's go."

Claude: [Writes Chapter 1 — ~2,500 words]
        "✅ Chapter 1 written: 'The Day I Decided'
         Words: 2,487 / 2,500
         Progress: 1 / 22 chapters
         Next: Chapter 2 — 'Getting Licensed Without Quitting Your Job'"

You: "Review that chapter before we continue."

Claude: [Runs developmental + line edit]
        "Here's your editorial feedback..."

You: "Looks good. Keep going."

Claude: [Continues through all 22 chapters]

You: "Package it for KDP."

Claude: [Assembles full manuscript, exports .docx]
        "✅ Book packaged: the-first-deal-final.docx (~220 pages)"

You: "Generate 3 cover options."

Claude: [Generates 3 AI covers via OpenRouter/Flux]
        "Here are your cover variations..."
```

---

## Book Project Structure

Each book lives in its own folder:

```
books/
  your-book-title/
    book-config.md          ← your book's settings and progress tracker
    outline.md              ← generated outline (approved before writing)
    chapters/
      ch01-chapter-title.md
      ch02-chapter-title.md
      ...
    reviews/
      ch01-review.md
    exports/
      your-book-title-final.docx
      your-book-title-final.md  (if markdown output selected)
      cover.jpg
      cover-prompt.md
```

---

## Configuration Reference

See `templates/book-config-template.md` for all available fields.

Key fields:

| Field | Description |
|-------|-------------|
| `genre` | Selects genre-specific writing guidance |
| `tone` | How Claude writes — be specific ("Gary Vee meets Malcolm Gladwell") |
| `chapter_structure` | The repeating format for each chapter |
| `core_premise` | One sentence: what is this book and why does it matter? |
| `platform` | Affects formatting in the packager |

---

## Cover Generation Setup

The `book-cover` skill uses OpenRouter to access image generation models.

1. Sign up at [openrouter.ai](https://openrouter.ai)
2. Get your API key
3. Set it as `OPENROUTER_API_KEY` in your environment
4. Recommended model: `black-forest-labs/flux-1.1-pro`

---

## Contributing

This is an open-source project. PRs welcome for:
- New genre reference files (`skills/book-writer/references/genre-*.md`)
- Improved prompts in the cover skill
- Additional platform support in the packager
- New skill ideas (translations? audio scripts? workbooks?)

---

## License

MIT — free to use, modify, and distribute.

---

## Built With

- [Claude AI](https://claude.ai) by Anthropic
- [OpenRouter](https://openrouter.ai) for image generation routing
- [Flux 1.1 Pro](https://blackforestlabs.ai) for cover art
- Love for the written word ❤️
