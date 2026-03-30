---
name: book-cover
description: >
  Use this skill to generate professional book cover art for any book.
  Triggers when the user says "generate a cover", "create a book cover", "make a cover for
  my book", "design the cover", "I need a cover", "generate cover art", or any request to
  produce visual cover imagery for a book. Also triggers automatically after packaging is
  complete if the user asks what's left to do. Uses OpenRouter to access image generation
  models — requires OPENROUTER_API_KEY to be set. Generates 2-3 cover variations and
  returns image URLs for review. Works with any genre.
---

# Book Cover Skill

This skill generates professional book cover art using image generation models via OpenRouter.
It reads the `book-config.md` to understand genre, tone, and audience, then crafts targeted
prompts designed to produce covers that convert — because a great cover sells the book.

---

## Requirements

- `OPENROUTER_API_KEY` must be set in environment variables
- `books/<title-slug>/book-config.md` must exist (or user must provide book details)
- Recommended model: `black-forest-labs/flux-1.1-pro` (best quality/cost ratio)
- Alternative: `google/imagen-4` (excellent text rendering)
- Fallback: `openai/dall-e-3` (widely available)

---

## Step 1: Load Book Context

Load from `book-config.md`:
- `title`
- `subtitle`
- `author_name`
- `genre`
- `target_audience`
- `tone`
- `core_premise`

If no config exists, ask the user for these fields before proceeding.

---

## Step 2: Build the Cover Prompt

A book cover prompt has five components. Build each one based on the book config:

### 1. Visual Concept
The central image or composition. Must be genre-appropriate and emotionally resonant.

**Genre → Visual Concept Guide:**

| Genre | Visual Direction |
|-------|-----------------|
| Business/Entrepreneurship | Clean, bold, abstract — geometric shapes, upward trajectories, cityscapes, hands at work |
| Self-Help | Human figure, light/dawn imagery, open spaces, transformation symbolism |
| Real Estate | Architecture, keys, skylines, interior spaces, hands shaking or holding keys |
| Fiction — Thriller | Dark atmosphere, shadows, motion blur, dramatic lighting |
| Fiction — Romance | Warm light, two figures, soft focus, nature or intimate settings |
| Fiction — Fantasy | Epic landscapes, magical elements, creatures, atmospheric lighting |
| Fiction — Literary | Minimalist, typographic, symbolic object, painterly textures |
| How-To | Clean and professional, relevant object or tool, clear and uncluttered |
| Memoir | Portrait-style, personal, warm, specific location or era |

### 2. Style & Medium
How it should look (photorealistic, illustrated, painterly, etc.)

### 3. Color Palette
2-4 colors that match the tone. Examples:
- Dark thriller: deep navy, charcoal, blood red accent
- Business: navy + gold, or white + electric blue
- Self-help: warm sunrise tones, coral, soft gold
- Fantasy: jewel tones, deep purple, midnight blue

### 4. Composition Notes
Where the title text will go (top third, bottom third, overlay), empty space requirements,
focal point placement.

### 5. Quality Directives
Always append: `professional book cover design, award-winning cover art, high resolution,
print-ready, no watermarks, sharp focus`

### Full Prompt Template

```
[Visual concept description]. [Style and medium]. [Color palette]. 
Book cover composition with [text placement] reserved for title text.
[Mood/atmosphere]. [Any specific elements]. 
Professional book cover design, award-winning cover art, high resolution, 
print-ready, sharp focus, no watermarks.
```

### Negative Prompt (always include)
```
amateur, clipart, low resolution, blurry, distorted, text already on image,
watermarks, ugly, generic stock photo, boring, overly literal
```

---

## Step 3: Generate Variations

Generate **3 variations** with different visual approaches. For each variation, write a
distinct prompt that explores a different creative direction (different color palette,
different composition, different visual metaphor).

Label them:
- **Variation A**: [brief description of the approach, e.g., "Minimalist typographic"]
- **Variation B**: [e.g., "Atmospheric photo-real scene"]
- **Variation C**: [e.g., "Bold graphic illustration"]

---

## Step 4: API Call via OpenRouter

```javascript
const response = await fetch("https://openrouter.ai/api/v1/images/generations", {
  method: "POST",
  headers: {
    "Authorization": `Bearer ${process.env.OPENROUTER_API_KEY}`,
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    model: "black-forest-labs/flux-1.1-pro",
    prompt: coverPrompt,
    negative_prompt: negativePrompt,
    width: 1600,   // KDP standard: 1600 x 2560 (6x9 at 266 DPI)
    height: 2560,
    num_images: 1
  })
});

const data = await response.json();
const imageUrl = data.data[0].url;
```

Make 3 separate API calls — one per variation.

### KDP Cover Specifications
- **Trim size 6x9**: 2700 x 4050 pixels at 300 DPI (with bleed)
- **Minimum**: 1000 pixels on shortest side
- **Aspect ratio**: 1:1.6 (width:height) for 6x9 trim
- **Format**: JPEG or TIFF for final upload

---

## Step 5: Present Results

After generating all three variations, show the user:

```
🎨 Cover Variations Generated for "[Book Title]"

**Variation A — [Approach Name]**
[Image URL]
Prompt used: [brief summary of the concept]

**Variation B — [Approach Name]**
[Image URL]
Prompt used: [brief summary]

**Variation C — [Approach Name]**
[Image URL]
Prompt used: [brief summary]

---
Which variation do you prefer? I can:
• Refine a variation (adjust colors, composition, mood)
• Generate 3 more with a different direction
• Combine elements from multiple variations
• Add your title and author name as text overlay (via a script)
```

---

## Step 6: Refinement Loop

If the user wants changes, adjust the prompt based on their feedback and regenerate.

**Common refinement requests and how to handle them:**

| Request | Adjustment |
|---------|-----------|
| "Make it darker / moodier" | Adjust color palette to deeper tones, add "dramatic lighting, dark atmosphere" |
| "More minimalist" | Remove visual complexity from prompt, add "minimalist, clean, negative space" |
| "More professional" | Add "corporate, polished, premium design, Fortune 500 aesthetic" |
| "Different color scheme" | Replace color palette component entirely |
| "More [genre]-feeling" | Load genre visual guide, emphasize genre-specific keywords |
| "I don't like the character" | Remove character elements, go abstract or object-focused |

---

## Step 7: Save Final Cover

When the user approves a cover:
1. Download the image and save to: `books/<title-slug>/exports/cover.jpg`
2. Update `book-config.md`: add `cover_complete: true` and `cover_image: exports/cover.jpg`
3. Note the final prompt used for future reference: save to `books/<title-slug>/exports/cover-prompt.md`

---

## Title Text Overlay (Optional)

If the user wants title text added to the cover image programmatically:

```bash
# Using ImageMagick (if available)
convert cover.jpg \
  -gravity North -pointsize 80 -fill white \
  -font "Georgia-Bold" -annotate +0+100 "BOOK TITLE" \
  -gravity South -pointsize 40 -fill white \
  -font "Georgia" -annotate +0+100 "Author Name" \
  cover-with-text.jpg
```

Note: For best results, have the image generation leave deliberate empty space
where text will go, then add text in design software (Canva, Adobe Express, etc.)
or via a tool like Canva's book cover builder.
