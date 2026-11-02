---
name: arch-diagram
description: Create professional, light-themed architecture diagrams as standalone HTML files with SVG graphics. Use when the user asks for system architecture diagrams, infrastructure diagrams, cloud architecture visualizations, security diagrams, network topology diagrams, or any technical diagram showing system components and their relationships.
license: MIT
metadata:
  version: "1.1"
  author: Cocoon AI (hello@cocoon-ai.com)
---

# arch-diagram (Light Mode)

Create professional, high-contrast light-themed technical architecture diagrams as self-contained HTML files with inline SVG graphics and CSS styling.

## Design System (Light Mode)

### Color Palette

Use these semantic colors for component types (soft pastel fills paired with high-contrast 600-level strokes for crisp readability on white/light backgrounds):

| Component Type | Fill (rgba) | Stroke | Accent / Label Text |
|---------------|-------------|--------|---------------------|
| Frontend | `rgba(207, 250, 254, 0.7)` (cyan-100) | `#0891b2` (cyan-600) | `#0e7490` (cyan-700) |
| Backend | `rgba(209, 250, 229, 0.7)` (emerald-100) | `#059669` (emerald-600) | `#047857` (emerald-700) |
| Database | `rgba(237, 233, 254, 0.7)` (violet-100) | `#7c3aed` (violet-600) | `#6d28d9` (violet-700) |
| AWS/Cloud | `rgba(254, 243, 199, 0.7)` (amber-100) | `#d97706` (amber-600) | `#b45309` (amber-700) |
| Security | `rgba(255, 228, 230, 0.7)` (rose-100) | `#e11d48` (rose-600) | `#be123c` (rose-700) |
| Message Bus | `rgba(255, 237, 213, 0.8)` (orange-100) | `#ea580c` (orange-600) | `#c2410c` (orange-700) |
| External/Generic | `rgba(241, 245, 249, 0.85)` (slate-100) | `#64748b` (slate-500) | `#334155` (slate-700) |

### Typography

Use JetBrains Mono for technical labels and Noto Sans JP / system sans-serif for body content:
```html
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600;700&display=swap" rel="stylesheet">
```

Font sizes: 12px for component names, 9px for sublabels, 8px for annotations, 7px for tiny labels.
Primary text color: `#0f172a` (slate-900). Secondary/sublabel text color: `#475569` (slate-600).

### Visual Elements

**Page Background:** `#f8fafc` (slate-50) with `#ffffff` (white) cards and `#cbd5e1` (slate-300) borders.

**SVG Canvas Background:** `#ffffff` (white) with a subtle light grid pattern:
```svg
<pattern id="grid" width="40" height="40" patternUnits="userSpaceOnUse">
  <path d="M 40 0 L 0 0 0 40" fill="none" stroke="#e2e8f0" stroke-width="0.75"/>
</pattern>
```

**Component boxes:** Rounded rectangles (`rx="6"`) with 1.5px stroke and soft semi-transparent pastel fills.

**Security groups:** Dashed stroke (`stroke-dasharray="4,4"`), transparent fill, rose color (`#e11d48`).

**Region boundaries:** Larger dashed stroke (`stroke-dasharray="8,4"`), amber (`#d97706`) or emerald (`#059669`) color, `rx="12"`, very subtle tint fill (e.g., `rgba(254, 243, 199, 0.25)`).

**Arrows:** Use SVG marker for arrowheads:
```svg
<marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
  <polygon points="0 0, 10 3.5, 0 7" fill="#475569" />
</marker>
```

**Arrow z-order:** Draw connection arrows early in the SVG (after the background grid) so they render behind component boxes. SVG elements are painted in document order, so arrows drawn first will appear behind shapes drawn later.

**Masking arrows behind semi-transparent fills:** Since component boxes use semi-transparent fills (`rgba(..., 0.7)`), arrows behind them will show through. To fully mask arrows, draw an opaque white background rect (`fill="#ffffff"`) at the same position before drawing the semi-transparent styled rect on top:
```svg
<!-- Opaque white background to mask arrows -->
<rect x="X" y="Y" width="W" height="H" rx="6" fill="#ffffff"/>
<!-- Styled component on top -->
<rect x="X" y="Y" width="W" height="H" rx="6" fill="rgba(237, 233, 254, 0.7)" stroke="#7c3aed" stroke-width="1.5"/>
```

**Auth/security flows:** Dashed lines in rose color (`#e11d48`).

**Message buses / Event buses:** Small connector elements between services. Use orange color (`#ea580c` stroke, `rgba(255, 237, 213, 0.8)` fill):
```svg
<rect x="X" y="Y" width="120" height="20" rx="4" fill="#ffffff"/>
<rect x="X" y="Y" width="120" height="20" rx="4" fill="rgba(255, 237, 213, 0.8)" stroke="#ea580c" stroke-width="1"/>
<text x="CENTER_X" y="Y+14" fill="#c2410c" font-size="7" font-weight="600" text-anchor="middle">Kafka / RabbitMQ</text>
```

### Spacing Rules

**CRITICAL:** When stacking components vertically, ensure proper spacing to avoid overlaps:

- **Standard component height:** 60px for services, 80-120px for larger components
- **Minimum vertical gap between components:** 40px
- **Inline connectors (message buses):** Place IN the gap between components, not overlapping

**Example vertical layout:**
```
Component A: y=70,  height=60  → ends at y=130
Gap:         y=130 to y=170   → 40px gap, place bus at y=140 (20px tall)
Component B: y=170, height=60  → ends at y=230
```

**Wrong:** Placing a message bus at y=160 when Component B starts at y=170 (causes overlap)
**Right:** Placing a message bus at y=140, centered in the 40px gap (y=130 to y=170)

### Legend Placement

**CRITICAL:** Place legends OUTSIDE all boundary boxes (region boundaries, cluster boundaries, security groups).

- Calculate where all boundaries end (y position + height)
- Place legend at least 20px below the lowest boundary
- Expand SVG viewBox height if needed to accommodate

**Example:**
```
Kubernetes Cluster: y=30, height=460 → ends at y=490
Legend should start at: y=510 or below
SVG viewBox height: at least 560 to fit legend
```

**Wrong:** Legend at y=470 inside a cluster boundary that ends at y=490
**Right:** Legend at y=510, below the cluster boundary, with viewBox height extended

### Layout Structure

1. **Header** - Title with pulsing dot indicator, subtitle
2. **Main SVG diagram** - Contained in a clean white rounded border card with subtle shadow
3. **Summary cards** - Grid of 3 cards below diagram with key details
4. **Footer** - Minimal metadata line

### Component Box Pattern (Light Mode)

```svg
<rect x="X" y="Y" width="W" height="H" rx="6" fill="#ffffff"/>
<rect x="X" y="Y" width="W" height="H" rx="6" fill="FILL_COLOR" stroke="STROKE_COLOR" stroke-width="1.5"/>
<text x="CENTER_X" y="Y+20" fill="#0f172a" font-size="11" font-weight="600" text-anchor="middle">LABEL</text>
<text x="CENTER_X" y="Y+36" fill="#475569" font-size="9" text-anchor="middle">sublabel</text>
```

### Info Card Pattern

```html
<div class="card">
  <div class="card-header">
    <div class="card-dot COLOR"></div>
    <h3>Title</h3>
  </div>
  <ul>
    <li>• Item one</li>
    <li>• Item two</li>
  </ul>
</div>
```

## Template

Copy and customize the template at `assets/template.html`. Key customization points:

1. Update the `<title>` and header text
2. Modify SVG viewBox dimensions if needed (default: `1000 x 680`)
3. Add/remove/reposition component boxes
4. Draw connection arrows between components
5. Update the three summary cards
6. Update footer metadata

## Output

Always produce a single self-contained `.html` file in **light mode** with:
- Embedded CSS (no external stylesheets except Google Fonts)
- Inline SVG (no external images)
- No JavaScript required (pure CSS animations)

The file should render cleanly in light mode when opened directly in any modern browser.
