---
name: bd-implement
description: Implement a single phase from a plan file. Writes production code with atomic commits per acceptance criterion. Use when user wants to implement a phase from a plan, mentions "implement phase X", or says "do phase N".
---

Implement the specified phase by writing production code with atomic commits per acceptance criterion based on the plan file and follow user instructions if provided.

## Mandatory Preconditions (run BEFORE any implementation)

These run by default on every `bd-implement` invocation, in this order. Do not skip.

1. **Invoke `/impeccable`** via the Skill tool to load design principles and run the Context Gathering Protocol. If no Design Context exists (`.impeccable.md`, loaded instructions, or `docs/designs/DESIGN.md`), the protocol will direct you to run `/impeccable teach` — do that before proceeding.
2. **Invoke `/shape`** via the Skill tool for the current phase feature to load (or produce) the design brief. If a brief already exists in `.plans/<features_summary>/` or the plan file itself, read it and treat it as authoritative instead of re-running discovery.
3. **Read project design strategy** at `<cwd>/docs/designs/DESIGN.md` if present and not yet in context.
4. **Research third-party patterns** with EXA or Context7 (per global rules) for any libraries the phase touches.

Only proceed to implementation once 1–4 are complete.

## Implementation

Research first before writing any code to understand latest patterns and best practices.

Create Todo list for each acceptance criterion and track progress before starting implementation.

Report comprehensively in <cwd>/.plans/<features_summary>/implementations/<phase_number>.md

DONT COMMIT YOUR CHANGES

---

## UI Implementation Rules

When the phase involves building, modifying, or styling any frontend/UI surface (components, pages, layouts, styling, interactions), you MUST follow the rules below. These rules are derived from the `/impeccable` and `/shape` skills and override generic defaults.

### 1. Preconditions

The mandatory preconditions at the top of this file (`/impeccable` + `/shape`) already cover UI setup. For UI phases specifically, confirm the `/shape` brief's Design Direction, Layout Strategy, Key States, Interaction Model, and Content Requirements are treated as authoritative during build.

### 2. Non-Negotiables (the AI Slop Test)

Before considering any UI work complete, ask: "If I showed this and said 'AI made this', would they believe me immediately?" If yes, rewrite.

**Absolute bans** — never ship these:
- **Side-stripe borders**: no `border-left`/`border-right` greater than 1px as a colored accent on cards, list items, callouts, or alerts — regardless of color, variable, or opacity. Rewrite with a different element structure entirely.
- **Gradient text**: no `background-clip: text` + gradient combination. Solid colors only for text; use weight/size for emphasis.
- **Reflex fonts**: do not use Inter, Roboto, Arial, Open Sans, system defaults, or any font in the impeccable `reflex_fonts_to_reject` list (Fraunces, Newsreader, Lora, Crimson*, Playfair, Cormorant*, Syne, IBM Plex*, Space Mono/Grotesk, DM Sans/Serif*, Outfit, Plus Jakarta Sans, Instrument Sans/Serif). Follow the 4-step font selection procedure in `/impeccable`.
- **AI color palette**: no cyan-on-dark, purple-to-blue gradients, or neon accents on dark backgrounds as a default.
- **Pure `#000` / `#fff`**: always tint neutrals toward the brand hue.
- **Glassmorphism, sparkline decoration, generic drop-shadowed rounded rectangles, identical icon-heading-text card grids, hero metric templates, lazy modals.**

### 3. Design Execution Rules

**Typography**
- Modular type scale with ≥1.25 ratio between steps. Fluid `clamp()` for marketing/content headings; fixed `rem` for app UI.
- Pair a distinctive display font with a refined body font. Vary font choices across projects.
- Line length capped at 65–75ch. Add 0.05–0.1 to line-height for light-on-dark text.
- No all-caps for long body text. No monospace as "technical vibe" shorthand.

**Color**
- Use `oklch()`, `color-mix()`, and `light-dark()`. Reduce chroma as lightness approaches extremes.
- Tint neutrals toward the brand hue (chroma 0.005–0.01 minimum).
- 60-30-10 by visual weight, not pixel count.
- Derive light vs dark from audience + viewing context, not as a default.
- No gray text on colored backgrounds — use a shade of the background color.

**Layout & Space**
- 4pt spacing scale with semantic names (`--space-sm`, `--space-md`). Scale: 4, 8, 12, 16, 24, 32, 48, 64, 96.
- Use `gap` for sibling spacing, not margins.
- Vary spacing to create rhythm. Break the grid intentionally. Prefer left-aligned + asymmetric over centered-everything.
- `grid-template-columns: repeat(auto-fit, minmax(280px, 1fr))` for breakpoint-free card grids.
- Container queries (`@container`) for component-level responsiveness; viewport queries for page layout.
- Do not wrap everything in cards; do not nest cards in cards.

**Motion**
- Transform + opacity only. Never animate layout properties (width/height/padding/margin). For height transitions, use `grid-template-rows`.
- Exponential easing (ease-out-quart/quint/expo). No bounce/elastic.
- Prefer one well-orchestrated moment over scattered micro-interactions. Respect `prefers-reduced-motion`.

**Interaction**
- Optimistic UI: update immediately, sync later.
- Progressive disclosure; hierarchy across primary/secondary/ghost buttons — not every button is primary.
- Empty states must teach the interface, not just say "nothing here".
- Every state must be implemented: default, empty, loading, error, success, plus edge cases from the design brief.

**Responsive**
- Adapt the interface across contexts; do not amputate features on mobile.

**UX Writing**
- Every word earns its place. No redundant headers/intros restating a heading.

### 4. Implementation Checklist (run before marking UI acceptance criteria done)

- [ ] Design Context loaded (impeccable/shape brief consulted)
- [ ] No items from the Absolute Bans list present
- [ ] Type scale + font pairing deliberate; no reflex fonts
- [ ] Palette in OKLCH with tinted neutrals; theme choice justified by audience
- [ ] Spacing uses 4pt scale with rhythm; layout not uniformly centered/card-wrapped
- [ ] All key states from the brief exist (default/empty/loading/error/success/edge)
- [ ] Motion uses transform/opacity + exponential easing; reduced-motion handled
- [ ] AI Slop Test passes — interface does not read as templated AI output
- [ ] Accessibility: WCAG AA contrast, keyboard focus visible, semantic HTML

### 5. Reporting

In the implementation report at `<cwd>/.plans/<features_summary>/implementations/<phase_number>.md`, include a short **Design Decisions** section covering: aesthetic direction chosen, font pairing + rationale, palette (OKLCH values), theme rationale, key layout/motion choices, and which bans were actively avoided. This makes the design thinking auditable alongside the code.
