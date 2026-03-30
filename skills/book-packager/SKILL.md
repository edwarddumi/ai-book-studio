---
name: book-packager
description: >
  Use this skill to export, format, or package a completed book manuscript for publication.
  Triggers when the user says "export my book", "format for KDP", "package the book",
  "create the final document", "make it ready to publish", "export to Word", "create the
  docx", "format for Gumroad", "make an epub", or any request to turn written chapters
  into a publication-ready file. Also triggers when all chapters are written and the user
  asks what to do next. Uses the docx skill for Word output and produces KDP-ready formatting
  by default.
---

# Book Packager Skill

This skill assembles all written chapters into a single, publication-ready document.
It reads from the `book-config.md` and chapter files, then produces a formatted `.docx`
(and optionally a clean `.md`) ready for upload to Amazon KDP, Gumroad, or Leanpub.

---

## Step 1: Pre-Flight Check

Before packaging, verify:
1. Load `books/<title-slug>/book-config.md`
2. Confirm `chapters_written` matches `target_chapters` (or ask user if packaging a partial draft)
3. List all chapter files in `books/<title-slug>/chapters/` — confirm none are missing
4. Check `output_format` in the config (`docx` | `markdown` | `both`)

Report to user:
```
📚 Ready to package: "[Book Title]"
   Chapters found: [N] / [target]
   Output format: [format]
   Platform: [platform]
   Estimated pages: ~[word_count / 250] pages
```

---

## Step 2: Assemble Front Matter

Create the following front matter sections (in order):

### Title Page
- Book title (large, centered)
- Subtitle if present (smaller, centered)
- Author name
- Publisher / imprint (if provided in config, otherwise leave blank)
- Year

### Copyright Page
```
Copyright © [year] [author_name]

All rights reserved. No part of this publication may be reproduced, distributed,
or transmitted in any form or by any means without the prior written permission
of the author.

[If non-fiction/business] The information in this book is provided for educational
purposes only. The author makes no guarantees about outcomes.

[For KDP] Published by [author_name] / Independently published
ISBN: [leave blank — to be filled in by author]

First edition, [year]
```

### Dedication (if provided in book-config.md)

### Table of Contents
Auto-generated from chapter titles and part headings.

### Foreword / Introduction
Pull from the written content if it exists, or generate a placeholder with `[INSERT INTRODUCTION HERE]`.

---

## Step 3: Assemble Body

Combine chapters in order:
1. Read each `ch{N:02d}-*.md` file in sequence
2. Insert a page break before each new chapter
3. Format chapter headings as Heading 1
4. Format subheadings as Heading 2 / Heading 3
5. Preserve all paragraph breaks
6. Ensure consistent spacing throughout

---

## Step 4: Assemble Back Matter

### Conclusion / Epilogue
Pull from written content or create placeholder.

### About the Author
Pull from `book-config.md` if `author_bio` field exists, otherwise insert placeholder:
`[INSERT AUTHOR BIO HERE — 150-200 words recommended]`

### Resources / Bibliography
Only include if referenced in chapters. Insert placeholder if non-fiction:
`[INSERT REFERENCES / RESOURCES HERE]`

### Call to Action (for non-fiction/business)
```
[INSERT CALL TO ACTION HERE]
Suggested: direct readers to your website, email list, social media, or next book.
```

---

## Step 5: Export as .docx

Use the `docx` skill to generate the Word document. Apply these settings:

### KDP-Standard Formatting
```javascript
// Page size: 6 x 9 inches (standard trade paperback)
// Width: 8640 DXA, Height: 12960 DXA
// Margins: top/bottom 1080 DXA (0.75"), inside 1008 DXA (0.7"), outside 864 DXA (0.6")

// Typography
// Body font: Georgia or Times New Roman, 11pt
// Chapter titles (H1): 24pt, bold, centered, Georgia
// Subheadings (H2): 14pt, bold, left-aligned
// Line spacing: 1.15 (body), 1.0 (block quotes)
// First paragraph after heading: no indent
// All other paragraphs: 0.3" first-line indent, no extra spacing between
```

### Title Page Styling
- Title: 36pt, bold, centered, 3 inches from top
- Subtitle: 18pt, centered
- Author: 14pt, centered, positioned near bottom third of page

### Chapter Opener Styling
- Chapter number: small caps or italic, 12pt, centered
- Chapter title: 24pt, bold, centered
- Drop 2-3 lines of white space before body text begins

### Headers / Footers
- Header: book title (left) | author name (right) — alternating pages for print
- Footer: page number, centered
- No header/footer on title page, copyright page, or chapter opener pages

---

## Step 6: Output & Deliver

Save the packaged file to: `books/<title-slug>/exports/<title-slug>-final.docx`

Also save a clean markdown version if `output_format` includes markdown:
`books/<title-slug>/exports/<title-slug>-final.md`

Report to user:
```
✅ Book packaged successfully!

   File: books/<title-slug>/exports/<title-slug>-final.docx
   Pages: ~[estimated pages]
   Words: [total word count]

   Platform checklist:
   □ Amazon KDP: Upload docx directly — set trim size to 6x9 in KDP settings
   □ Gumroad: Convert to PDF before upload (File → Export as PDF in Word)
   □ Leanpub: Use the markdown export and upload to Leanpub's manuscript folder
```

---

## Platform-Specific Notes

### Amazon KDP
- KDP accepts .docx directly — no need to convert to PDF for the interior
- The cover is uploaded separately (use the `book-cover` skill)
- Set "bleed" to none for most non-fiction/business books
- Review the KDP previewer carefully before publishing

### Gumroad
- Convert to PDF for best results (preserves formatting)
- Consider also offering the .docx as a bonus for "editable" version
- Gumroad works best with a compelling cover image — use the `book-cover` skill

### Leanpub
- Leanpub uses Markdown natively — upload the `.md` export
- Their system generates PDF, EPUB, and MOBI from your markdown
- Follow Leanpub's specific markdown conventions (they have a guide)
