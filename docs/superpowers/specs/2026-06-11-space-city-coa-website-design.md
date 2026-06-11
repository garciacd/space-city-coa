# Space City CoA Website — Design Spec
**Date:** 2026-06-11
**Phase:** 1 (Marketing/Brochure)

---

## Overview

A trust-focused marketing website for Space City CoA, the certificate of authentication service operated by Space City Books. Space City Books acts as an independent third-party witness to autograph signings at conventions, issuing Certificates of Authentication (CoA) for collectibles including comic books, books, and other items signed by celebrities, artists, and authors.

**Primary audience:** Collectors who have received or are considering a Space City CoA and need to understand its value.

**Primary goal:** Education and credibility — make collectors certain their CoA means something. No hard call-to-action.

**Phases:**
- Phase 1 (this spec): Marketing/brochure site
- Phase 2: Convention booking page
- Phase 3 (possible): CoA verification portal

---

## Tech Stack

- **Framework:** Astro (static site generator)
- **Deployment:** Netlify or Vercel (free tier)
- **Styling:** Plain CSS (scoped component styles in Astro)
- **No CMS** for Phase 1 — content is static and edited directly in source

Phase 2 booking drops in as a new Astro page (`/book`) with no structural changes to Phase 1.

---

## Visual Design System

### Color Palette
| Token | Value | Usage |
|---|---|---|
| `--bg-deep` | `#060a14` | Page base, hero background |
| `--bg-mid` | `#0a0e1a` | Alternate section backgrounds |
| `--bg-raised` | `#1a1f35` | Cards, raised elements |
| `--gold` | `#c9a84c` | Accents, borders, headings, icons |
| `--gold-dim` | `#c9a84c66` | Subtle borders, muted accents |
| `--text-primary` | `#e8d5a3` | Parchment cream — headings, display |
| `--text-body` | `#e8d5a388` | Body copy |

### Typography
- **Display/headings:** Georgia, serif — echoes the certificate's document authority
- **Body copy:** System sans-serif stack (`-apple-system, BlinkMacSystemFont, Arial, sans-serif`) — legible, clean
- **Labels/eyebrows:** Uppercase, wide letter-spacing (2–4px), small size (9–11px)

### Decorative Language
**Primary — Illuminated Manuscript:**
- Ornate corner bracket borders on section containers (`border: 1px solid var(--gold-dim)` with corner accent marks)
- `✦ · ✦ · ✦` flourish dividers between sections and within content
- Illuminated drop caps on section-opening paragraphs (large gold initial letter)
- Gold rule lines (`linear-gradient` from gold to transparent)

**Secondary — Space/Cosmic:**
- Subtle starfield on section backgrounds (CSS radial-gradient points, not an image)
- Moon motif appears once in the hero background (CSS circle with radial gradient)
- These are atmospheric — never compete with text or ornamental borders

### Certificate as Brand Asset
The actual `CoA Space City Books.png` certificate image is the primary brand asset. It is featured large in the hero, slightly rotated (~1.5deg) to feel like a real physical document, with a subtle gold box-shadow glow. Hover straightens it to 0deg.

---

## Site Structure

Single Astro page (`src/pages/index.astro`) with anchor-linked navigation. All sections flow vertically on one page.

```
/
├── src/
│   ├── pages/
│   │   └── index.astro          ← Phase 1: all sections
│   ├── components/
│   │   ├── Nav.astro
│   │   ├── Hero.astro
│   │   ├── WhatIsCoA.astro
│   │   ├── WhyWitness.astro
│   │   ├── TheProcess.astro
│   │   ├── AboutSpaceCity.astro
│   │   └── Footer.astro
│   └── styles/
│       └── global.css
├── public/
│   └── coa-certificate.png      ← certificate image asset
└── docs/
    └── superpowers/specs/       ← this file
```

---

## Sections

### ① Nav
- Sticky, `position: sticky; top: 0`
- Dark background with bottom gold border (`1px solid var(--gold-dim)`)
- Logo left: `Space City CoA` — "Space City" in gold, "CoA" in parchment
- Links right: *About · Process · Contact* — uppercase, small, letter-spaced
- Links are anchor hrefs to section IDs

### ② Hero
- Full viewport height (`min-height: 100vh`)
- Two-column layout: text left, certificate image right
- **Left:** eyebrow label ("Space City Books"), headline *"A signature is only as good as the witness behind it."* (italic gold on second line), 1–2 sentence description, horizontal gold rule, `— Lux Revelare — Light Reveals —` motto
- **Right:** `CoA Space City Books.png` displayed at full column width, rotated 1.5deg, gold glow box-shadow, hover to straighten
- Background: deep navy with subtle starfield (CSS) and moon motif top-right

### ③ What is a CoA?
- Section ID: `#what-is-coa` (no direct nav link — reached by scrolling after the hero)
- Alternating background: `--bg-mid`
- Illuminated drop cap on opening paragraph
- Ornate corner bracket border
- Gold `✦` flourish divider above section heading
- Content: plain-language explanation of what a Certificate of Authentication is and why it protects a collector's investment
- Photo placeholder: labeled `[Photo: signing moment at a convention]`, sized `400×300px`

### ④ Why a Third-Party Witness?
- Background: `--bg-deep`
- The core trust argument: contrasts seller-issued certificates (conflict of interest) with Space City Books as an independent witness (no stake in the transaction)
- Visual treatment: two-panel comparison — *Seller-Issued* (with warning cues) vs *Space City CoA Witness* (with trust cues)
- Ornate section border, gold flourish divider

### ⑤ The Process
- Section ID: `#process`
- Background: `--bg-mid`
- Four steps with ornately styled gold numeral markers:
  1. *At the convention* — collector brings item to signing table
  2. *Space City Books witnesses* — independent representative present at the moment of signing
  3. *Certificate issued* — CoA generated with item description, event name, date
  4. *Collector receives CoA* — physical certificate handed to collector on the spot
- Steps laid out horizontally on desktop, stacked on mobile
- Gold connecting line between steps

### ⑥ About Space City Books
- Section ID: `#about` (nav "About" link points here)
- Background: `--bg-deep`
- Who Space City Books is, Houston roots, the meaning of *Lux Revelare* ("Light Reveals") — commitment to transparency and truth in authentication
- Photo placeholder: labeled `[Photo: Space City Books team]`, sized `480×360px`
- Contact info: email and/or social links (placeholders to be filled in)
- Ornate border treatment

### ⑦ Footer
- Dark background (`--bg-deep`), top gold border
- Section ID: `#contact` (nav "Contact" link points here)
- Three columns: Brand (logo + tagline) · Links (nav anchors) · Contact (email, socials)
- Lux Revelare seal centered at bottom
- Copyright line: `© [year] Space City Books. All rights reserved.`

---

## Content Notes

All copy is written fresh during implementation, aimed at collectors. Tone: authoritative but approachable — a knowledgeable friend who takes authentication seriously.

Every photo is a placeholder (`<div class="photo-placeholder">`) with explicit pixel dimensions and a text label describing the intended photo. Swapping in real photos requires only replacing the placeholder with an `<img>` tag — no layout changes.

---

## Responsive Behavior

- **Hero:** Two-column on desktop → stacked (certificate above text) on mobile
- **Process steps:** Horizontal row on desktop → vertical stack on mobile
- **Why Witness comparison:** Side-by-side on desktop → stacked on mobile
- Nav collapses to hamburger menu on mobile (or hides links below logo)

---

## Phase 2 Hook

Convention booking is added as `src/pages/book.astro` — a separate page. The nav gains a *Book* link pointing to `/book`. No changes required to any Phase 1 section.

---

## Out of Scope (Phase 1)

- Convention schedule / upcoming events listing
- Online booking or scheduling
- CoA verification portal
- User accounts or authentication
- CMS or admin interface
- E-commerce
