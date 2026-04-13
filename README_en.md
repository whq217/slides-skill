# Slides Skill — AI-Powered Interactive Web PPT Generator

> A Claude Code skill for generating professional HTML presentations from zero, powered by Reveal.js

## What This Skill Does

**Slides Skill** helps users quickly create interactive web presentations (PPTs) through Claude Code, with no CSS/JS knowledge required. Uses a "preview-first" workflow: generates style previews for you to choose from, then completes generation.

## Core Features

- **Zero dependencies** — Single HTML file with inline CSS/JS, no npm or build tools
- **19 theme presets** — Dark/light/specialty, each with unique fonts, colors, and signature elements
- **Visual style discovery** — Generates 3 style previews so you can choose by looking, not by describing
- **Smart layout matching** — 20+ layout modes, auto-matched to content type
- **Page entrance animations** — CSS pre-hide + JS reveal dual-track, no flicker
- **ECharts chart animations** — Type-specific entrance timing (line/pie/bar/radar)
- **In-browser editing** — Press E to enter edit mode, modify text directly
- **Pen annotation** — Press D to toggle pen, right-click to clear, ideal for presentations
- **PDF export** — Included export script for print-quality output

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

The skill automatically triggers a 5-stage workflow: Configure → Research → Outline → Style & Cover → Generate

### Navigation

| Action | Method |
|--------|--------|
| Next slide | Arrow keys / Click blank area / Scroll wheel |
| Previous slide | Right-click blank area |
| Edit mode | Press E |
| Pen mode | Press D |
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
├── SKILL.md              # Main skill file (5-stage workflow)
├── STYLE_PRESETS.md      # 19 theme presets (fonts/colors/signature elements)
├── LAYOUT_PATTERNS.md    # 20+ layout mode library
├── animation-patterns.md # Animation system reference
├── html-template.md      # HTML template architecture
├── viewport-base.css     # Responsive base styles
└── scripts/
    ├── deploy.sh         # Deployment script
    └── export-pdf.sh     # PDF export script
```

## Acknowledgments

This project is a fork of [Frontend Slides](https://github.com/zarazhangrui/frontend-slides) by @zarazhangrui (MIT License).

Enhanced by Hongqing W with layout pattern library, page entrance animation system, chart animation presets, CDN accessibility fixes, and Font Awesome icon rendering fixes.

## License

MIT License — see [LICENSE](LICENSE) for details.
