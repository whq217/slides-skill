# Slides Skill — AI-Powered Interactive Web Presentation Generator

> A Claude Code skill for generating professional HTML presentations from zero, powered by Reveal.js

## What This Skill Does

**Slides Skill** helps you quickly create interactive web presentations through Claude Code — no CSS/JS knowledge required. Uses a "preview-first" workflow: generates style previews for you to choose from, then completes generation.

## Core Features

- **Zero dependencies** — Single HTML file with inline CSS/JS, no npm or build tools
- **19 theme presets** — Dark/light/specialty, including 6 brand styles, each with unique fonts, colors, and signature elements
- **33 layout modes** — Cover/data/chart/list/comparison/image-text-cards/special-function/summary, auto-matched to content focus
- **Visual style discovery** — Generates 3 style previews so you pick by looking, not describing
- **Page entrance animations** — CSS pre-hide + JS reveal dual-track, smooth and flicker-free
- **Fragment narrative animations** — Reveal.js native fragments for step-by-step reveal of key numbers, conclusions, and critical content
- **ECharts chart animations** — Chart-type-specific entrance timing (line/pie/bar/radar), auto-rotates chart types across adjacent pages
- **Hover highlighting** — Cards and data blocks lift up with themed glow on hover, auto-adapts to dark/light backgrounds
- **In-browser editing** — Press E to enter edit mode, modify text directly, saves persist
- **Pen annotation** — Press D to toggle pen, right-click to clear, ideal for live presentations
- **Speaker mode** — Press S to open a separate speaker window with current slide notes, next slide preview, and a timer
- **Fixed top navigation** — Chapter jump links with auto-highlighting for the current section
- **Accessibility-aware animations** — Auto-detects system `prefers-reduced-motion` setting, respects accessibility needs

## Output & Export

- **PDF export** — Included export script for print-quality output, optional `--compact` flag (reduces file size by 50-70%)
- **Vercel deployment** — One-click deploy script, generates a live URL
- **Offline version export** — CDN resources inlined for fully offline HTML, ideal for no-network environments

## Content Processing

- **Multi-format input** — Directly reads Word (.docx), PDF, and Markdown files, extracts content to build PPTs
- **PPT conversion** — Extract text and images from existing .pptx files, convert to web presentations
- **Local image embedding** — Supports JPG/PNG/WebP/SVG, auto base64 inlined into HTML
- **Automatic content optimization** — Focused pruning, one-rule-per-page, conclusion-first structure, data-meets-story

## Quick Start

### Installation

Copy the skill files to your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/slides

cp SKILL.md STYLE_PRESETS.md LAYOUT_PATTERNS.md animation-patterns.md html-template.md viewport-base.css ~/.claude/skills/slides/
cp -r scripts ~/.claude/skills/slides/
```

### Usage

In Claude Code, say:

```
Help me create a presentation about XXX
```

The skill automatically triggers a 7-stage workflow: Read Rules → Configure → Content Prep → Content Optimization → Outline → Style & Cover → Generate

### Navigation

| Action | Method |
|--------|--------|
| Next slide | Arrow keys / Click blank area / Scroll wheel |
| Previous slide | Right-click blank area |
| Edit mode | Press E |
| Pen mode | Press D |
| Speaker mode | Press S |
| Clear pen | Right-click |

## Tech Stack

| Technology | Purpose |
|-----------|---------|
| Reveal.js 4.x | Presentation framework |
| ECharts 5.x | Chart rendering |
| Font Awesome 6 | Icon library |
| Google Fonts | Typography |

## Project Structure

```
slides/
├── SKILL.md              # Main skill file (7-stage workflow)
├── STYLE_PRESETS.md      # 19 theme presets (including 6 brand styles)
├── LAYOUT_PATTERNS.md    # 33 layout mode library
├── animation-patterns.md # Animation system reference
├── html-template.md      # HTML template architecture
├── viewport-base.css     # Responsive base styles
├── boilerplate.html      # Ready-to-use scaffold file
└── scripts/
    ├── deploy.sh         # Vercel deployment script
    ├── export-pdf.sh     # PDF export script (with --compact option)
    └── extract-pptx.py   # PPT extraction script
```

## Acknowledgments

This project is based on [Frontend Slides](https://github.com/zarazhangrui/frontend-slides) by @zarazhangrui (MIT License).

Enhanced by Hongqing W with: 33 layout modes, page entrance animation system, chart animation presets, speaker mode, fragment narrative animations, hover highlighting, CDN accessibility fixes, Font Awesome icon rendering fixes, and more.

The "Content Optimization" module (Stage 3) draws inspiration from [女娲·Skill造人术](https://github.com/alchaincyf/nuwa-skill) — Steve Jobs Perspective Skill (MIT License).

## License

MIT License — see [LICENSE](LICENSE) for details.
