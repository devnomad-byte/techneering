---
name: craft
description: Create distinctive, production-grade frontend interfaces with high design quality. Auto-stacked onto an implementation path when a task involves frontend/UI work — pages, components, layouts, styles, interactions. Also available on demand via the Skill tool as tn:craft.
---

# Frontend Design

Create distinctive, production-grade frontend interfaces that avoid generic "AI slop" aesthetics. Implement real working code with exceptional attention to aesthetic details and creative choices.

## When This Skill Runs

- **Auto-stacked (normal):** when `tn:compass` or `tn:assemble` detects frontend work via the Frontend Detection Rules (page/component/UI/layout/style, React/Vue/CSS/Tailwind, form/button/modal, etc. — full keyword list lives in `tn:compass`). In this mode it runs during the implementation phase of an existing path, then returns to the caller.
- **On demand:** invoke directly via the Skill tool as `tn:craft` (the model calls it through the Skill tool with the `tn:` prefix; users may also trigger it from the CLI). Use this for "just design me a page" requests that skip the spec flow.

craft is a **domain** skill — it never stands alone as a path; it stacks onto implementation and returns to its caller.

## Core Principle

```
BOLD AESTHETIC CHOICES + METICULOUS EXECUTION = MEMORABLE INTERFACES.
No generic AI aesthetics. Every design must be intentional and distinctive.
```

## Steps

### Step 1: Design Thinking

Before coding, understand the context and commit to a BOLD aesthetic direction:

- **Purpose**: What problem does this interface solve? Who uses it?
- **Tone**: Pick an extreme: brutally minimal, maximalist chaos, retro-futuristic, organic/natural, luxury/refined, playful/toy-like, editorial/magazine, brutalist/raw, art deco/geometric, soft/pastel, industrial/utilitarian, etc.
- **Constraints**: Technical requirements (framework, performance, accessibility)
- **Differentiation**: What makes this UNFORGETTABLE? What's the one thing someone will remember?

**CRITICAL**: Choose a clear conceptual direction and execute it with precision. Bold maximalism and refined minimalism both work — the key is intentionality, not intensity.

### Step 2: Implement

Produce working code (HTML/CSS/JS, React, Vue, etc.) that is:
- Production-grade and functional
- Visually striking and memorable
- Cohesive with a clear aesthetic point-of-view
- Meticulously refined in every detail

### Step 3: Frontend Aesthetics

Focus on:
- **Typography**: Distinctive, characterful font choices. Avoid Inter, Roboto, Arial.
- **Color & Theme**: CSS variables for consistency. Dominant colors with sharp accents.
- **Motion**: CSS-only for HTML. Motion library for React. High-impact page loads, scroll-triggering, hover states.
- **Spatial Composition**: Unexpected layouts. Asymmetry. Grid-breaking. Generous negative space OR controlled density.
- **Backgrounds & Details**: Atmosphere and depth. Gradient meshes, noise textures, geometric patterns, dramatic shadows.

**NEVER use**: Inter/Roboto/Arial, purple gradients on white, predictable layouts, cookie-cutter design.

### Step 4: Match Complexity to Vision

- Maximalist designs → elaborate code with extensive animations
- Minimalist designs → restraint, precision, careful spacing and typography
- Elegance comes from executing the vision well

## Red Flags

| Thought | Reality |
|---------|---------|
| "Just use Inter/Roboto" | These are AI's default fonts — zero distinctiveness |
| "Purple gradient on white background" | Classic AI-slop aesthetic — must avoid |
| "Use template/component library defaults" | Default style = no design |
| "Design doesn't need thinking, just write code" | Frontend code without Design Thinking is just layout, not design |

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Role | Domain skill — auto-stacked on frontend tasks, also callable on demand via Skill tool as tn:craft |
| Aesthetic direction | Bold choices, precise execution |
| Forbidden fonts | Inter, Roboto, Arial, system fonts |
| Forbidden palette | Purple gradient + white background |
| Frameworks | HTML/CSS/JS, React (with Motion), Vue |

## Next Step

When done:
- Normal case → return to caller (usually a skill in the implementation flow)
- User requests adjustments → keep refining the design
- User requests stop → end the current flow

## Guardrails

- Never use generic AI aesthetics (Inter, Roboto, purple gradients)
- Never produce the same design twice — vary themes, fonts, aesthetics
- Never add emojis unless the user explicitly requests them
- Match implementation complexity to the aesthetic vision
- Auto-stacked when implementation involves frontend/UI code — no need to invoke it explicitly via the Skill tool unless going off-path
