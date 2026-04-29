---
name: imgforge
description: Generate and edit images via OpenAI gpt-image-2 from the terminal. Zero dependencies — calls the API directly with urllib. Use when an agent or user needs to create images from text, edit existing images, composite multiple images, or inpaint with masks. Reads OPENAI_API_KEY from env. Writes PNG/JPEG/WebP to disk.
---

# imgforge

Zero-dependency CLI for OpenAI image generation. Single Python file, no SDK, no pip install. Calls the API directly via urllib.

## Invocation

```bash
# Generate
imgforge gen "PROMPT" [-o OUTPUT] [-s SIZE] [-q QUALITY] [-n COUNT] [-f FORMAT]

# Edit (when OpenAI enables gpt-image-2 on edits endpoint)
imgforge edit IMAGE... "INSTRUCTION" [-o OUTPUT] [-m MASK] [-s SIZE]

# Cost estimate
imgforge cost [-s SIZE] [-q QUALITY] [-n COUNT]
```

Requires `OPENAI_API_KEY` in env. Prints output path(s) to stdout. Exit 0 success, 1 API error, 2 bad args.

## Before generating: read the templates

Do not write prompts from scratch. Before generating any image, check `templates/` for a matching category and adapt a proven prompt.

1. Scan the template list below for a relevant category
2. Read that template file
3. Adapt the closest example to the user's request — keep the structural patterns (layout-first, exact text in quotes, specific style anchors)

| Template | File | Use when |
|----------|------|----------|
| Product hero | `templates/product-hero.md` | App icons, device mockups, SaaS dashboards, CLI screenshots |
| Social media | `templates/social-media.md` | OG images, GitHub previews, YouTube thumbnails, tweet images |
| Diagrams | `templates/diagrams.md` | Architecture diagrams, flowcharts, ER diagrams, API flows |
| Blog images | `templates/blog-images.md` | Hero images, comparison splits, step illustrations, abstract patterns |
| Creative | `templates/creative.md` | Isometric art, pixel art, posters, neon signs, flat illustrations |

If nothing matches, write the prompt following these rules:
- Put canvas/layout/aspect ratio BEFORE the subject
- Wrap any displayed text in quotes
- Name specific styles ("flat vector illustration" not "nice looking")
- Describe scene density over adjectives
- Include lighting, material, and palette as separate clauses

## Size selection

Pick based on intent, not preference:

| Intent | Flag | Resolution |
|--------|------|-----------|
| Social square, icon | `-s square` | 1024x1024 |
| Mobile screenshot, poster | `-s portrait` | 1024x1536 |
| Blog header, OG image | `-s landscape` | 1536x1024 |
| Print, high-res asset | `-s hd` | 2048x2048 |
| Widescreen, cinematic | `-s 4k` | 3840x2160 |

Or pass any `WxH` value directly.

## Quality as budget dial

| Quality | ~Cost | When to use |
|---------|-------|-------------|
| `-q low` | $0.011 | Drafts, exploration, bulk generation, testing prompt ideas |
| `-q medium` | $0.042 | General use, social media, non-final assets |
| `-q high` | $0.167 | Final assets, typography, dense text, print, shipping-facing |

Default is `auto`. Rules:
- User says "draft", "explore", "try", "variations", "cheap" → `low`
- User says "final", "hero", "poster", "typography", "print" → `high`
- Multiple images (`-n 4+`) → default to `low` unless user specifies
- Unsure → `medium`

Always mention the estimated cost in your response. The CLI prints it automatically.

## Output handling

- Images save to cwd with auto-generated names: `YYYYMMDD-HHMMSS-slug.png`
- Use `-o path` for specific output paths
- `-n 4` produces `output-1.png`, `output-2.png`, etc.
- `-f webp` or `-f jpeg` for non-PNG formats

## Common patterns

```bash
# App store screenshot
imgforge gen "An iPhone 15 Pro showing a clean task list app..." -s portrait -q high -o screenshot.png

# GitHub social preview
imgforge gen "Dark gradient background, terminal window with colorful output..." -s landscape -q medium

# Architecture diagram for docs
imgforge gen "Clean system architecture diagram, three tiers..." -s landscape -q high -o arch.png

# Bulk logo exploration
imgforge gen "Minimal geometric logo mark, blue and white" -n 4 -s square -q low

# Blog post header
imgforge gen "Abstract code particles dissolving into light..." -s landscape -q medium -o header.png
```

## Error handling

| Exit | Meaning | What to do |
|------|---------|-----------|
| 0 | Success | Report the file path and cost |
| 1 | API error | Surface the error message — usually rate limit, billing, or moderation |
| 2 | Bad args / no key | Fix the command or set OPENAI_API_KEY |

On moderation blocks: surface the API message verbatim. Do not retry with a "cleaned" prompt unless the user asks.
