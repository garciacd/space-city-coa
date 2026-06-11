# Space City CoA Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a trust-focused Astro marketing website for Space City CoA that makes collectors confident in the value of their Certificate of Authentication.

**Architecture:** Single Astro page (`index.astro`) composed of seven scoped components, styled with a shared global CSS design system. No JavaScript framework — plain HTML, CSS, and Astro's build tooling. Responsive via CSS Grid and media queries.

**Tech Stack:** Astro (static site generator), plain CSS with scoped component styles, deployed to Netlify or Vercel free tier.

---

## File Map

| File | Responsibility |
|---|---|
| `astro.config.mjs` | Astro build config |
| `src/styles/global.css` | CSS custom properties, reset, shared ornament utility classes |
| `src/pages/index.astro` | Page shell — imports all components, sets `<head>` meta |
| `src/components/Nav.astro` | Sticky navigation bar |
| `src/components/Hero.astro` | Full-viewport hero with certificate image, starfield, moon |
| `src/components/WhatIsCoA.astro` | "What is a CoA?" section with drop cap and photo placeholder |
| `src/components/WhyWitness.astro` | "Why a Third-Party Witness?" two-panel comparison |
| `src/components/TheProcess.astro` | Four-step process with gold numeral markers |
| `src/components/AboutSpaceCity.astro` | About section with photo placeholder and contact info |
| `src/components/Footer.astro` | Three-column footer with Lux Revelare seal |
| `public/coa-certificate.png` | Certificate image asset (copied from project root) |

---

## Task 1: Scaffold Astro Project

**Files:**
- Create: `astro.config.mjs`, `package.json`, `src/pages/index.astro`, project scaffold
- Create: `public/coa-certificate.png` (copy from project root)

- [ ] **Step 1: Initialize Astro in the project directory**

```bash
cd "/Users/cosmegarcia/Documents/All Coding Projects/SpaceCityCoAWebsite"
npm create astro@latest . -- --template minimal --install --git
```

When prompted:
- Template: **Minimal**
- Install dependencies: **Yes**
- Initialize git repo: **Yes**

- [ ] **Step 2: Verify scaffold succeeded**

```bash
ls src/pages/
```

Expected output: `index.astro`

- [ ] **Step 3: Copy the certificate image into the public directory**

```bash
cp "/Users/cosmegarcia/Documents/All Coding Projects/SpaceCityCoAWebsite/CoA Space City Books.png" \
   "/Users/cosmegarcia/Documents/All Coding Projects/SpaceCityCoAWebsite/public/coa-certificate.png"
```

- [ ] **Step 4: Add .superpowers to .gitignore**

Open `.gitignore` (created by Astro scaffold) and add at the bottom:
```
.superpowers/
```

- [ ] **Step 5: Start the dev server and verify it runs**

```bash
npm run dev
```

Expected output: `Local   http://localhost:4321/` (port may vary)

Open `http://localhost:4321` in a browser. Expected: Astro default minimal page loads without errors.

- [ ] **Step 6: Commit**

```bash
git add .
git commit -m "chore: scaffold Astro project with certificate image asset"
```

---

## Task 2: Global CSS Design System

**Files:**
- Create: `src/styles/global.css`
- Modify: `src/pages/index.astro` (import global CSS)

- [ ] **Step 1: Create global.css with design tokens, reset, and ornament utilities**

Create `src/styles/global.css` with this exact content:

```css
/* ── Design Tokens ─────────────────────────────── */
:root {
  --bg-deep:       #060a14;
  --bg-mid:        #0a0e1a;
  --bg-raised:     #1a1f35;
  --gold:          #c9a84c;
  --gold-dim:      rgba(201, 168, 76, 0.4);
  --text-primary:  #e8d5a3;
  --text-body:     rgba(232, 213, 163, 0.53);
  --font-serif:    Georgia, 'Times New Roman', serif;
  --font-sans:     -apple-system, BlinkMacSystemFont, Arial, sans-serif;
}

/* ── Reset ──────────────────────────────────────── */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  background: var(--bg-deep);
  color: var(--text-primary);
  font-family: var(--font-sans);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}
img { display: block; max-width: 100%; height: auto; }
a { color: inherit; }

/* ── Ornament Utilities ─────────────────────────── */

/* ✦ · ✦ · ✦ flourish divider */
.flourish {
  text-align: center;
  color: var(--gold-dim);
  font-size: 0.875rem;
  letter-spacing: 0.5rem;
  margin: 1.5rem 0;
}

/* UPPERCASE eyebrow label with flanking rules */
.section-eyebrow {
  font-size: 0.5625rem;
  letter-spacing: 0.25rem;
  text-transform: uppercase;
  color: var(--gold);
  font-family: var(--font-sans);
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-bottom: 1rem;
}
.section-eyebrow::before,
.section-eyebrow::after {
  content: '';
  flex: 1;
  max-width: 3rem;
  height: 1px;
  background: var(--gold-dim);
}

/* Gradient gold rule */
.gold-rule {
  width: 5rem;
  height: 1px;
  background: linear-gradient(to right, var(--gold), transparent);
  margin: 1.75rem 0;
}

/* Section with ornate corner bracket border */
.ornate-border {
  position: relative;
}
.ornate-border::before {
  content: '';
  position: absolute;
  inset: 1rem;
  border: 1px solid var(--gold-dim);
  pointer-events: none;
  z-index: 0;
}
.ornate-border .corner {
  position: absolute;
  width: 1rem;
  height: 1rem;
  border-color: var(--gold);
  border-style: solid;
  z-index: 1;
}
.ornate-border .corner.tl { top: 0.5rem; left: 0.5rem; border-width: 1px 0 0 1px; }
.ornate-border .corner.tr { top: 0.5rem; right: 0.5rem; border-width: 1px 1px 0 0; }
.ornate-border .corner.bl { bottom: 0.5rem; left: 0.5rem; border-width: 0 0 1px 1px; }
.ornate-border .corner.br { bottom: 0.5rem; right: 0.5rem; border-width: 0 1px 1px 0; }

/* Illuminated drop cap */
.drop-cap::first-letter {
  float: left;
  font-size: 4.5rem;
  line-height: 0.75;
  color: var(--gold);
  margin: 0.1em 0.1em 0 0;
  font-family: var(--font-serif);
  text-shadow: 0 0 20px rgba(201, 168, 76, 0.4);
}

/* Photo placeholder */
.photo-placeholder {
  background: var(--bg-raised);
  border: 1px dashed var(--gold-dim);
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--gold-dim);
  font-size: 0.6875rem;
  letter-spacing: 0.0625rem;
  font-family: var(--font-sans);
  text-align: center;
  padding: 1rem;
}

/* ── Section Base ───────────────────────────────── */
.ornate-section {
  padding: 5rem 3rem;
}
.section-inner {
  max-width: 1100px;
  margin: 0 auto;
  position: relative;
  z-index: 1;
}
.section-heading {
  font-family: var(--font-serif);
  font-size: clamp(1.25rem, 2.5vw, 2rem);
  font-weight: normal;
  color: var(--text-primary);
  margin-bottom: 2rem;
  text-align: center;
}
.body-text {
  color: var(--text-body);
  font-size: 0.9375rem;
  line-height: 1.8;
  margin-bottom: 1.25rem;
}

@media (max-width: 768px) {
  .ornate-section { padding: 4rem 1.5rem; }
}
```

- [ ] **Step 2: Replace the contents of src/pages/index.astro with the page shell**

```astro
---
import '../styles/global.css';
---
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Space City Books provides independent third-party Certificates of Authentication for signatures at conventions. Authenticated in person. Witnessed by Space City Books." />
    <title>Space City CoA — Certificate of Authentication</title>
  </head>
  <body>
    <p style="color: white; padding: 2rem;">Design system loaded — components coming soon.</p>
  </body>
</html>
```

- [ ] **Step 3: Verify the dev server shows a dark page without errors**

```bash
npm run dev
```

Open `http://localhost:4321`. Expected: black/dark background, white placeholder text, no console errors.

- [ ] **Step 4: Commit**

```bash
git add src/styles/global.css src/pages/index.astro
git commit -m "feat: add global CSS design system with tokens and ornament utilities"
```

---

## Task 3: Nav Component

**Files:**
- Create: `src/components/Nav.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create src/components/Nav.astro**

```astro
---
---
<nav>
  <a href="/" class="nav-logo">
    <span class="logo-space">Space City</span>
    <span class="logo-coa">CoA</span>
  </a>
  <ul class="nav-links">
    <li><a href="#about">About</a></li>
    <li><a href="#process">Process</a></li>
    <li><a href="#contact">Contact</a></li>
  </ul>
</nav>

<style>
  nav {
    position: sticky;
    top: 0;
    z-index: 100;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1.125rem 3rem;
    background: rgba(6, 10, 20, 0.95);
    border-bottom: 1px solid var(--gold-dim);
    backdrop-filter: blur(8px);
  }

  .nav-logo {
    text-decoration: none;
    font-family: var(--font-serif);
    font-size: 0.875rem;
    letter-spacing: 0.1875rem;
    text-transform: uppercase;
  }
  .logo-space { color: var(--gold); }
  .logo-coa   { color: var(--text-primary); }

  .nav-links {
    list-style: none;
    display: flex;
    gap: 2rem;
  }
  .nav-links a {
    color: var(--text-body);
    text-decoration: none;
    font-size: 0.625rem;
    letter-spacing: 0.125rem;
    text-transform: uppercase;
    font-family: var(--font-sans);
    transition: color 0.2s;
  }
  .nav-links a:hover { color: var(--gold); }

  @media (max-width: 640px) {
    nav { padding: 1rem 1.5rem; }
    .nav-links { gap: 1.25rem; }
  }
</style>
```

- [ ] **Step 2: Import Nav in index.astro**

Replace the `<body>` contents in `src/pages/index.astro`:

```astro
---
import '../styles/global.css';
import Nav from '../components/Nav.astro';
---
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Space City Books provides independent third-party Certificates of Authentication for signatures at conventions. Authenticated in person. Witnessed by Space City Books." />
    <title>Space City CoA — Certificate of Authentication</title>
  </head>
  <body>
    <Nav />
    <main>
      <p style="color: white; padding: 2rem; min-height: 100vh;">Hero coming in next task.</p>
    </main>
  </body>
</html>
```

- [ ] **Step 3: Visual verification**

Open `http://localhost:4321`. Verify:
- [ ] Sticky dark nav bar visible at top
- [ ] "Space City" in gold, "CoA" in parchment cream
- [ ] Three links: About · Process · Contact, muted color
- [ ] Nav links turn gold on hover
- [ ] Nav stays fixed when scrolling the placeholder content

- [ ] **Step 4: Commit**

```bash
git add src/components/Nav.astro src/pages/index.astro
git commit -m "feat: add sticky Nav component with anchor links"
```

---

## Task 4: Hero Component

**Files:**
- Create: `src/components/Hero.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create src/components/Hero.astro**

```astro
---
---
<section class="hero">
  <div class="starfield" aria-hidden="true"></div>
  <div class="moon"      aria-hidden="true"></div>

  <div class="hero-inner">
    <div class="hero-text">
      <p class="section-eyebrow">Space City Books</p>
      <h1 class="hero-headline">
        A signature is only as good as
        <em>the witness behind it.</em>
      </h1>
      <p class="hero-description">
        Space City Books serves as an independent third-party witness to signings at conventions —
        issuing Certificates of Authentication that collectors can trust.
      </p>
      <div class="gold-rule"></div>
      <p class="hero-motto">— <span>Lux Revelare</span> — Light Reveals —</p>
    </div>

    <div class="hero-cert">
      <div class="cert-wrapper">
        <img
          src="/coa-certificate.png"
          alt="Space City Books Certificate of Authentication — official document certifying an independent third-party witness was present at a signing"
          width="600"
          height="400"
        />
      </div>
    </div>
  </div>
</section>

<style>
  .hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 5rem 3rem;
    position: relative;
    overflow: hidden;
    background:
      radial-gradient(ellipse at 20% 50%, rgba(27, 42, 74, 0.13) 0%, transparent 60%),
      radial-gradient(ellipse at 80% 30%, rgba(201, 168, 76, 0.03) 0%, transparent 50%),
      var(--bg-deep);
  }

  /* Starfield — CSS-only radial gradient dots */
  .starfield {
    position: absolute;
    inset: 0;
    pointer-events: none;
    background-image:
      radial-gradient(1px 1px at  8% 12%, rgba(255,255,255,0.8)  0%, transparent 100%),
      radial-gradient(1px 1px at 22%  6%, rgba(201,168,76,0.67)  0%, transparent 100%),
      radial-gradient(1px 1px at 38% 18%, rgba(255,255,255,0.67) 0%, transparent 100%),
      radial-gradient(1.5px 1.5px at 52% 9%, rgba(201,168,76,0.87) 0%, transparent 100%),
      radial-gradient(1px 1px at 67% 15%, rgba(255,255,255,0.53) 0%, transparent 100%),
      radial-gradient(1px 1px at 79%  5%, rgba(255,255,255,0.73) 0%, transparent 100%),
      radial-gradient(1px 1px at 91% 22%, rgba(201,168,76,0.53)  0%, transparent 100%),
      radial-gradient(1px 1px at 15% 45%, rgba(255,255,255,0.27) 0%, transparent 100%),
      radial-gradient(1px 1px at 33% 72%, rgba(255,255,255,0.33) 0%, transparent 100%),
      radial-gradient(1.5px 1.5px at 85% 60%, rgba(201,168,76,0.4) 0%, transparent 100%),
      radial-gradient(1px 1px at 95% 80%, rgba(255,255,255,0.27) 0%, transparent 100%),
      radial-gradient(1px 1px at 47% 88%, rgba(255,255,255,0.27) 0%, transparent 100%),
      radial-gradient(1px 1px at  6% 77%, rgba(201,168,76,0.27)  0%, transparent 100%),
      radial-gradient(1px 1px at 72% 40%, rgba(255,255,255,0.4)  0%, transparent 100%);
  }

  /* Moon motif — appears once, top-right */
  .moon {
    position: absolute;
    top: 3rem;
    right: 4rem;
    width: 3.25rem;
    height: 3.25rem;
    border-radius: 50%;
    background: radial-gradient(circle at 35% 35%, #f5e9c8, #c9a070);
    box-shadow: 0 0 1.25rem rgba(201,168,76,0.27), inset -6px -3px 12px rgba(0,0,0,0.3);
    opacity: 0.6;
    pointer-events: none;
  }

  .hero-inner {
    display: grid;
    grid-template-columns: 1fr 1fr;
    align-items: center;
    gap: 4.5rem;
    max-width: 1200px;
    width: 100%;
    position: relative;
    z-index: 1;
  }

  /* Text column */
  .hero-headline {
    font-family: var(--font-serif);
    font-size: clamp(1.75rem, 3.5vw, 3rem);
    font-weight: normal;
    line-height: 1.2;
    color: var(--text-primary);
  }
  .hero-headline em {
    display: block;
    color: var(--gold);
    font-style: italic;
  }
  .hero-description {
    font-size: 0.875rem;
    color: var(--text-body);
    line-height: 1.75;
    max-width: 27.5rem;
    margin-top: 1.25rem;
  }
  .hero-motto {
    font-size: 0.625rem;
    letter-spacing: 0.1875rem;
    text-transform: uppercase;
    color: rgba(201,168,76,0.53);
    font-family: var(--font-sans);
  }
  .hero-motto span { color: var(--gold); }

  /* Certificate image column */
  .cert-wrapper {
    transform: rotate(1.5deg);
    transition: transform 0.4s ease;
  }
  .cert-wrapper:hover { transform: rotate(0deg) scale(1.02); }
  .cert-wrapper img {
    width: 100%;
    border-radius: 2px;
    box-shadow:
      0 0 0 1px rgba(201,168,76,0.2),
      0 0 2.5rem rgba(201,168,76,0.1),
      0 1.5rem 5rem rgba(0,0,0,0.7);
  }

  /* Mobile: stack certificate above text */
  @media (max-width: 768px) {
    .hero { padding: 5rem 1.5rem 3rem; }
    .hero-inner {
      grid-template-columns: 1fr;
      gap: 2.5rem;
    }
    .hero-cert { max-width: 30rem; margin: 0 auto; width: 100%; }
    .cert-wrapper        { transform: none; }
    .cert-wrapper:hover  { transform: none; }
  }
</style>
```

- [ ] **Step 2: Add Hero to index.astro**

```astro
---
import '../styles/global.css';
import Nav  from '../components/Nav.astro';
import Hero from '../components/Hero.astro';
---
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Space City Books provides independent third-party Certificates of Authentication for signatures at conventions. Authenticated in person. Witnessed by Space City Books." />
    <title>Space City CoA — Certificate of Authentication</title>
  </head>
  <body>
    <Nav />
    <main>
      <Hero />
    </main>
  </body>
</html>
```

- [ ] **Step 3: Visual verification**

Open `http://localhost:4321`. Verify:
- [ ] Full-viewport dark hero section
- [ ] Subtle star dots visible in the background
- [ ] Small moon visible top-right
- [ ] Certificate image displayed on the right, slightly rotated
- [ ] Certificate hover: rotates to 0deg and very slightly grows
- [ ] Headline present with "the witness behind it." in italic gold
- [ ] "Lux Revelare" motto visible in muted gold at the bottom of the text column
- [ ] On a narrow window (< 768px): certificate stacks above the text

- [ ] **Step 4: Commit**

```bash
git add src/components/Hero.astro src/pages/index.astro
git commit -m "feat: add Hero section with certificate image, starfield, and moon motif"
```

---

## Task 5: WhatIsCoA Component

**Files:**
- Create: `src/components/WhatIsCoA.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create src/components/WhatIsCoA.astro**

```astro
---
---
<section id="what-is-coa" class="ornate-section ornate-border" style="background: var(--bg-mid);">
  <span class="corner tl" aria-hidden="true"></span>
  <span class="corner tr" aria-hidden="true"></span>
  <span class="corner bl" aria-hidden="true"></span>
  <span class="corner br" aria-hidden="true"></span>

  <div class="section-inner">
    <div class="flourish">✦ · ✦ · ✦</div>
    <h2 class="section-heading">What is a Certificate of Authentication?</h2>

    <div class="content-grid">
      <div class="text-col">
        <p class="body-text drop-cap">
          A Certificate of Authentication is an official document that confirms a signature was witnessed
          in person by an independent party. It records the item that was signed, the event at which the
          signing occurred, and the date — creating a verifiable record that protects your investment.
        </p>
        <p class="body-text">
          In the world of collectibles, signatures hold tremendous value — but only when their authenticity
          can be proven. A CoA from Space City Books tells any future buyer, appraiser, or fellow collector
          exactly what happened, where, and when. It transforms a signed item from a personal memento into
          a documented piece of history.
        </p>
        <p class="body-text">
          Without an independent witness, a certificate is only as reliable as the person who issued it.
          With Space City Books present at the signing, the CoA carries the weight of an impartial,
          named third party — someone with nothing to gain and everything to lose by being dishonest.
        </p>
      </div>
      <div class="photo-col">
        <div class="photo-placeholder" style="width: 400px; height: 300px;">
          [Photo: signing moment at a convention]
        </div>
      </div>
    </div>
  </div>
</section>

<style>
  .content-grid {
    display: grid;
    grid-template-columns: 1fr auto;
    gap: 3rem;
    align-items: start;
    margin-top: 1.5rem;
  }

  @media (max-width: 900px) {
    .content-grid { grid-template-columns: 1fr; }
    .photo-placeholder { width: 100% !important; max-width: 400px; }
  }
</style>
```

- [ ] **Step 2: Add WhatIsCoA to index.astro**

```astro
---
import '../styles/global.css';
import Nav        from '../components/Nav.astro';
import Hero       from '../components/Hero.astro';
import WhatIsCoA  from '../components/WhatIsCoA.astro';
---
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Space City Books provides independent third-party Certificates of Authentication for signatures at conventions. Authenticated in person. Witnessed by Space City Books." />
    <title>Space City CoA — Certificate of Authentication</title>
  </head>
  <body>
    <Nav />
    <main>
      <Hero />
      <WhatIsCoA />
    </main>
  </body>
</html>
```

- [ ] **Step 3: Visual verification**

Scroll past the hero. Verify:
- [ ] `--bg-mid` background (slightly lighter than hero)
- [ ] `✦ · ✦ · ✦` flourish above heading
- [ ] Section heading in serif font
- [ ] First paragraph has large gold drop cap
- [ ] Photo placeholder visible, bordered in dashed gold
- [ ] Ornate corner brackets visible at all four corners of the section
- [ ] Inner border line visible inside the corners

- [ ] **Step 4: Commit**

```bash
git add src/components/WhatIsCoA.astro src/pages/index.astro
git commit -m "feat: add WhatIsCoA section with drop cap and ornate border"
```

---

## Task 6: WhyWitness Component

**Files:**
- Create: `src/components/WhyWitness.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create src/components/WhyWitness.astro**

```astro
---
---
<section class="ornate-section" style="background: var(--bg-deep);">
  <div class="section-inner">
    <div class="flourish">✦ · ✦ · ✦</div>
    <h2 class="section-heading">Why Does a Third-Party Witness Matter?</h2>
    <p class="section-intro body-text">
      Not all Certificates of Authentication are equal. The difference lies in
      who issues them — and what stake they have in the transaction.
    </p>

    <div class="comparison">
      <div class="panel panel-warning">
        <div class="panel-header">
          <span class="panel-icon warning-icon">⚠</span>
          <h3>Seller-Issued Certificate</h3>
        </div>
        <ul class="panel-list">
          <li>Issued by the person selling the item</li>
          <li>No independent verification of the signing</li>
          <li>Direct financial interest in the sale</li>
          <li>Value depends entirely on the seller's word</li>
          <li>Cannot be confirmed by a neutral third party</li>
        </ul>
      </div>

      <div class="comparison-divider" aria-hidden="true">✦</div>

      <div class="panel panel-trust">
        <div class="panel-header">
          <span class="panel-icon trust-icon">✓</span>
          <h3>Space City Books CoA</h3>
        </div>
        <ul class="panel-list">
          <li>Issued by Space City Books — independent of the sale</li>
          <li>Representative physically present at the moment of signing</li>
          <li>No financial stake in the transaction</li>
          <li>Documents item, event name, and date of signing</li>
          <li>Value backed by an impartial, named witness</li>
        </ul>
      </div>
    </div>
  </div>
</section>

<style>
  .section-intro {
    text-align: center;
    max-width: 37.5rem;
    margin: 0 auto 3rem;
  }

  .comparison {
    display: grid;
    grid-template-columns: 1fr auto 1fr;
    gap: 1.5rem;
    align-items: center;
  }

  .panel {
    padding: 2rem;
    border: 1px solid var(--gold-dim);
  }
  .panel-warning { border-color: rgba(201,168,76,0.15); }
  .panel-trust   { border-color: var(--gold-dim); }

  .panel-header {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    margin-bottom: 1.5rem;
  }
  .panel-icon {
    font-size: 1.25rem;
    width: 2rem;
    text-align: center;
  }
  .warning-icon { color: rgba(201,168,76,0.33); }
  .trust-icon   { color: var(--gold); }

  .panel-header h3 {
    font-family: var(--font-serif);
    font-size: 1rem;
    font-weight: normal;
    color: var(--text-primary);
  }
  .panel-warning .panel-header h3 { color: var(--text-body); }

  .panel-list {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
  }
  .panel-list li {
    font-size: 0.875rem;
    color: var(--text-body);
    padding-left: 1.25rem;
    position: relative;
    line-height: 1.5;
  }
  .panel-list li::before {
    content: '·';
    position: absolute;
    left: 0;
    color: var(--gold-dim);
  }
  .panel-trust .panel-list li         { color: var(--text-primary); }
  .panel-trust .panel-list li::before { color: var(--gold); }

  .comparison-divider {
    font-size: 1.5rem;
    color: var(--gold-dim);
    padding: 0 0.5rem;
  }

  @media (max-width: 768px) {
    .comparison { grid-template-columns: 1fr; }
    .comparison-divider { display: none; }
  }
</style>
```

- [ ] **Step 2: Add WhyWitness to index.astro**

```astro
---
import '../styles/global.css';
import Nav        from '../components/Nav.astro';
import Hero       from '../components/Hero.astro';
import WhatIsCoA  from '../components/WhatIsCoA.astro';
import WhyWitness from '../components/WhyWitness.astro';
---
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Space City Books provides independent third-party Certificates of Authentication for signatures at conventions. Authenticated in person. Witnessed by Space City Books." />
    <title>Space City CoA — Certificate of Authentication</title>
  </head>
  <body>
    <Nav />
    <main>
      <Hero />
      <WhatIsCoA />
      <WhyWitness />
    </main>
  </body>
</html>
```

- [ ] **Step 3: Visual verification**

Scroll to the WhyWitness section. Verify:
- [ ] Two panels side by side (three-column grid with divider)
- [ ] Left panel (Seller-Issued): muted border, dimmed heading, ⚠ icon in muted gold
- [ ] Right panel (Space City CoA): gold border, bright heading, ✓ icon in gold, bullet items in parchment
- [ ] `✦` divider between panels
- [ ] On narrow screen: panels stack vertically, divider hidden

- [ ] **Step 4: Commit**

```bash
git add src/components/WhyWitness.astro src/pages/index.astro
git commit -m "feat: add WhyWitness two-panel comparison section"
```

---

## Task 7: TheProcess Component

**Files:**
- Create: `src/components/TheProcess.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create src/components/TheProcess.astro**

```astro
---
const steps = [
  {
    numeral: 'I',
    title: 'At the Convention',
    description:
      'The collector brings their item to the signing table at a convention where Space City Books is present as an independent witness.',
  },
  {
    numeral: 'II',
    title: 'Space City Books Witnesses',
    description:
      'A Space City Books representative watches the celebrity, artist, or author sign the item in person — with no financial stake in the transaction.',
  },
  {
    numeral: 'III',
    title: 'Certificate Issued',
    description:
      'The Certificate of Authentication is completed on-site, recording the item description, name of event, and date of signing.',
  },
  {
    numeral: 'IV',
    title: 'Collector Receives CoA',
    description:
      'The collector walks away with both their signed item and the CoA — a permanent, independent record of exactly what happened.',
  },
];
---
<section id="process" class="ornate-section" style="background: var(--bg-mid);">
  <div class="section-inner">
    <div class="flourish">✦ · ✦ · ✦</div>
    <h2 class="section-heading">The Process</h2>

    <div class="steps">
      {steps.map((step) => (
        <div class="step">
          <div class="step-number">{step.numeral}</div>
          <h3 class="step-title">{step.title}</h3>
          <p class="step-description">{step.description}</p>
        </div>
      ))}
    </div>
  </div>
</section>

<style>
  .steps {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 2rem;
    margin-top: 3rem;
    position: relative;
  }

  /* Gold connector line running through all step circles */
  .steps::before {
    content: '';
    position: absolute;
    top: 1.25rem; /* vertically centers on the circle */
    left: calc(12.5% + 1.25rem);
    right: calc(12.5% + 1.25rem);
    height: 1px;
    background: linear-gradient(
      to right,
      transparent,
      var(--gold-dim) 10%,
      var(--gold-dim) 90%,
      transparent
    );
  }

  .step {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    position: relative;
  }

  .step-number {
    width: 2.5rem;
    height: 2.5rem;
    border: 1px solid var(--gold);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: var(--font-serif);
    font-size: 0.875rem;
    color: var(--gold);
    background: var(--bg-mid); /* masks the connector line behind it */
    position: relative;
    z-index: 1;
    flex-shrink: 0;
    margin-bottom: 1.5rem;
  }

  .step-title {
    font-family: var(--font-serif);
    font-size: 1rem;
    font-weight: normal;
    color: var(--text-primary);
    margin-bottom: 0.75rem;
  }

  .step-description {
    font-size: 0.8125rem;
    color: var(--text-body);
    line-height: 1.7;
  }

  @media (max-width: 768px) {
    .steps {
      grid-template-columns: 1fr;
      gap: 2.5rem;
    }
    .steps::before { display: none; }
    .step { align-items: flex-start; text-align: left; }
  }
</style>
```

- [ ] **Step 2: Add TheProcess to index.astro**

```astro
---
import '../styles/global.css';
import Nav        from '../components/Nav.astro';
import Hero       from '../components/Hero.astro';
import WhatIsCoA  from '../components/WhatIsCoA.astro';
import WhyWitness from '../components/WhyWitness.astro';
import TheProcess from '../components/TheProcess.astro';
---
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Space City Books provides independent third-party Certificates of Authentication for signatures at conventions. Authenticated in person. Witnessed by Space City Books." />
    <title>Space City CoA — Certificate of Authentication</title>
  </head>
  <body>
    <Nav />
    <main>
      <Hero />
      <WhatIsCoA />
      <WhyWitness />
      <TheProcess />
    </main>
  </body>
</html>
```

- [ ] **Step 3: Visual verification**

Scroll to The Process section. Verify:
- [ ] Four steps laid out in a horizontal row
- [ ] Each step has a gold-bordered circle with Roman numeral (I, II, III, IV)
- [ ] A thin gold horizontal line connects the circles
- [ ] Each step has a serif title and body description beneath the circle
- [ ] On narrow screen: steps stack vertically, connector line hidden, text left-aligned

- [ ] **Step 4: Commit**

```bash
git add src/components/TheProcess.astro src/pages/index.astro
git commit -m "feat: add TheProcess four-step section with gold connector"
```

---

## Task 8: AboutSpaceCity Component

**Files:**
- Create: `src/components/AboutSpaceCity.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create src/components/AboutSpaceCity.astro**

```astro
---
---
<section id="about" class="ornate-section ornate-border" style="background: var(--bg-deep);">
  <span class="corner tl" aria-hidden="true"></span>
  <span class="corner tr" aria-hidden="true"></span>
  <span class="corner bl" aria-hidden="true"></span>
  <span class="corner br" aria-hidden="true"></span>

  <div class="section-inner">
    <div class="flourish">✦ · ✦ · ✦</div>
    <h2 class="section-heading">About Space City Books</h2>

    <div class="about-grid">
      <div class="photo-col">
        <div class="photo-placeholder" style="width: 480px; height: 360px;">
          [Photo: Space City Books team]
        </div>
      </div>

      <div class="text-col">
        <p class="body-text drop-cap">
          Space City Books is a Houston-based independent bookseller with deep roots in the comics,
          sci-fi, and pop culture community. We believe every signed item tells a story — and that
          story deserves to be told honestly.
        </p>
        <p class="body-text">
          Our Certificate of Authentication service was born from a simple conviction: collectors
          deserve more than a seller's promise. By placing an independent witness at the moment of
          signing, we put our name — and our credibility — on the line for every CoA we issue.
        </p>
        <p class="body-text">
          We have no stake in the transaction. We're not selling the item. We're not the buyer.
          We're simply there — watching, documenting, and certifying — so you don't have to take
          anyone's word for it.
        </p>

        <div class="lux-motto">
          <div class="gold-rule"></div>
          <p class="motto-latin">Lux Revelare</p>
          <p class="motto-translation">Light Reveals</p>
          <p class="body-text">
            Our guiding principle: transparency is the foundation of trust. We don't hide behind
            paperwork — we show up, we watch, we certify.
          </p>
        </div>

        <div class="contact-block">
          <p class="section-eyebrow">Get in touch</p>
          <a href="mailto:hello@spacecitybooks.com" class="contact-link">
            hello@spacecitybooks.com
          </a>
        </div>
      </div>
    </div>
  </div>
</section>

<style>
  .about-grid {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 3rem;
    align-items: start;
    margin-top: 1.5rem;
  }

  .lux-motto { margin-top: 0.5rem; }

  .motto-latin {
    font-family: var(--font-serif);
    font-size: 1.375rem;
    color: var(--gold);
    font-style: italic;
    margin-bottom: 0.25rem;
  }
  .motto-translation {
    font-size: 0.625rem;
    letter-spacing: 0.25rem;
    text-transform: uppercase;
    color: var(--text-body);
    font-family: var(--font-sans);
    margin-bottom: 1rem;
  }

  .contact-block { margin-top: 2rem; }
  .contact-link {
    color: var(--text-primary);
    text-decoration: none;
    font-size: 0.9375rem;
    border-bottom: 1px solid var(--gold-dim);
    padding-bottom: 2px;
    transition: color 0.2s, border-color 0.2s;
  }
  .contact-link:hover { color: var(--gold); border-color: var(--gold); }

  @media (max-width: 900px) {
    .about-grid { grid-template-columns: 1fr; }
    .photo-placeholder { width: 100% !important; max-width: 480px; }
  }
</style>
```

- [ ] **Step 2: Add AboutSpaceCity to index.astro**

```astro
---
import '../styles/global.css';
import Nav             from '../components/Nav.astro';
import Hero            from '../components/Hero.astro';
import WhatIsCoA       from '../components/WhatIsCoA.astro';
import WhyWitness      from '../components/WhyWitness.astro';
import TheProcess      from '../components/TheProcess.astro';
import AboutSpaceCity  from '../components/AboutSpaceCity.astro';
---
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Space City Books provides independent third-party Certificates of Authentication for signatures at conventions. Authenticated in person. Witnessed by Space City Books." />
    <title>Space City CoA — Certificate of Authentication</title>
  </head>
  <body>
    <Nav />
    <main>
      <Hero />
      <WhatIsCoA />
      <WhyWitness />
      <TheProcess />
      <AboutSpaceCity />
    </main>
  </body>
</html>
```

- [ ] **Step 3: Visual verification**

Scroll to About section. Verify:
- [ ] Ornate corner brackets visible
- [ ] Photo placeholder (480×360) on the left
- [ ] Text column on the right with drop cap on first paragraph
- [ ] "Lux Revelare" in italic serif gold
- [ ] "Light Reveals" in small uppercase below it
- [ ] Gold rule above the Lux Revelare block
- [ ] Contact email link present, underlined in muted gold
- [ ] Email link turns gold on hover

- [ ] **Step 4: Commit**

```bash
git add src/components/AboutSpaceCity.astro src/pages/index.astro
git commit -m "feat: add AboutSpaceCity section with Lux Revelare motto and contact"
```

---

## Task 9: Footer Component

**Files:**
- Create: `src/components/Footer.astro`
- Modify: `src/pages/index.astro` (final wiring)

- [ ] **Step 1: Create src/components/Footer.astro**

```astro
---
const year = new Date().getFullYear();
---
<footer id="contact">
  <div class="footer-inner">
    <div class="footer-brand">
      <p class="footer-logo">Space City <span>CoA</span></p>
      <p class="footer-tagline">Independent authentication you can trust.</p>
    </div>

    <div class="footer-nav">
      <p class="col-label">Navigate</p>
      <nav>
        <a href="#what-is-coa">What is a CoA?</a>
        <a href="#process">The Process</a>
        <a href="#about">About</a>
      </nav>
    </div>

    <div class="footer-contact">
      <p class="col-label">Contact</p>
      <a href="mailto:hello@spacecitybooks.com">hello@spacecitybooks.com</a>
      <div class="social-links">
        <a href="#" aria-label="Instagram">Instagram</a>
        <a href="#" aria-label="Facebook">Facebook</a>
      </div>
    </div>
  </div>

  <div class="footer-seal">
    <div class="seal-circle">
      <p>Lux</p>
      <p>Revelare</p>
    </div>
  </div>

  <p class="footer-copyright">© {year} Space City Books. All rights reserved.</p>
</footer>

<style>
  footer {
    background: var(--bg-deep);
    border-top: 1px solid var(--gold-dim);
    padding: 4rem 3rem 2rem;
  }

  .footer-inner {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 3rem;
    max-width: 1100px;
    margin: 0 auto 3rem;
  }

  .footer-logo {
    font-family: var(--font-serif);
    font-size: 1rem;
    letter-spacing: 0.1875rem;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 0.5rem;
  }
  .footer-logo span { color: var(--text-primary); }

  .footer-tagline {
    font-size: 0.8125rem;
    color: var(--text-body);
    font-style: italic;
  }

  .col-label {
    font-size: 0.5625rem;
    letter-spacing: 0.1875rem;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 1rem;
    font-family: var(--font-sans);
  }

  .footer-nav nav {
    display: flex;
    flex-direction: column;
    gap: 0.625rem;
  }
  .footer-nav a,
  .footer-contact a {
    color: var(--text-body);
    text-decoration: none;
    font-size: 0.8125rem;
    transition: color 0.2s;
  }
  .footer-nav a:hover,
  .footer-contact a:hover { color: var(--gold); }

  .footer-contact {
    display: flex;
    flex-direction: column;
    gap: 0.625rem;
  }
  .social-links {
    display: flex;
    gap: 1rem;
    margin-top: 0.375rem;
  }

  .footer-seal {
    display: flex;
    justify-content: center;
    margin-bottom: 2rem;
  }
  .seal-circle {
    width: 3.75rem;
    height: 3.75rem;
    border-radius: 50%;
    border: 1px solid var(--gold-dim);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: 0.5rem;
    letter-spacing: 0.0625rem;
    text-transform: uppercase;
    color: var(--gold-dim);
    line-height: 1.4;
  }

  .footer-copyright {
    text-align: center;
    font-size: 0.6875rem;
    color: rgba(201,168,76,0.27);
    letter-spacing: 0.0625rem;
    font-family: var(--font-sans);
    border-top: 1px solid rgba(201,168,76,0.07);
    padding-top: 1.5rem;
  }

  @media (max-width: 768px) {
    footer { padding: 3rem 1.5rem 2rem; }
    .footer-inner { grid-template-columns: 1fr; gap: 2rem; }
  }
</style>
```

- [ ] **Step 2: Wire all components in final index.astro**

Replace `src/pages/index.astro` entirely:

```astro
---
import '../styles/global.css';
import Nav            from '../components/Nav.astro';
import Hero           from '../components/Hero.astro';
import WhatIsCoA      from '../components/WhatIsCoA.astro';
import WhyWitness     from '../components/WhyWitness.astro';
import TheProcess     from '../components/TheProcess.astro';
import AboutSpaceCity from '../components/AboutSpaceCity.astro';
import Footer         from '../components/Footer.astro';
---
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Space City Books provides independent third-party Certificates of Authentication for signatures at conventions. Authenticated in person. Witnessed by Space City Books." />
    <title>Space City CoA — Certificate of Authentication</title>
    <link rel="icon" href="/coa-certificate.png" type="image/png" />
  </head>
  <body>
    <Nav />
    <main>
      <Hero />
      <WhatIsCoA />
      <WhyWitness />
      <TheProcess />
      <AboutSpaceCity />
    </main>
    <Footer />
  </body>
</html>
```

- [ ] **Step 3: Visual verification — full page scroll**

Do a full scroll from top to bottom. Verify:
- [ ] Nav stays sticky the entire scroll
- [ ] All five content sections visible with alternating dark backgrounds
- [ ] Footer appears with three-column layout
- [ ] Lux Revelare seal circle visible above copyright line
- [ ] Copyright year is correct (2026)
- [ ] Footer nav links: "What is a CoA? · The Process · About"
- [ ] Clicking "About" in the main nav scrolls to the AboutSpaceCity section
- [ ] Clicking "Process" in the main nav scrolls to TheProcess section
- [ ] Clicking "Contact" in the main nav scrolls to the footer

- [ ] **Step 4: Commit**

```bash
git add src/components/Footer.astro src/pages/index.astro
git commit -m "feat: add Footer and wire all sections into final index.astro"
```

---

## Task 10: Production Build & Deploy Configuration

**Files:**
- Create: `netlify.toml`
- Verify: `astro.config.mjs`

- [ ] **Step 1: Run a production build and check for errors**

```bash
npm run build
```

Expected: output like:
```
 building client...
 ✓ Completed in X.XXs.

 src/pages/index.astro
  └─ /index.html (+X KB)
```

No errors in output.

- [ ] **Step 2: Preview the production build locally**

```bash
npm run preview
```

Open the URL shown (typically `http://localhost:4321`). Do a full visual check of the production build — same checks as Task 9 Step 3.

- [ ] **Step 3: Create netlify.toml for zero-config deployment**

Create `netlify.toml` at the project root:

```toml
[build]
  command = "npm run build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "18"
```

- [ ] **Step 4: Final commit**

```bash
git add netlify.toml
git commit -m "chore: add Netlify deploy config"
```

- [ ] **Step 5: Verify deploy readiness**

```bash
git log --oneline
```

Expected: eight commits from Tasks 1–10, clean history.

```bash
git status
```

Expected: `nothing to commit, working tree clean`

---

## Deployment (Post-Build)

1. Push this repository to GitHub
2. Log in to [netlify.com](https://netlify.com) → "Add new site" → "Import an existing project"
3. Connect the GitHub repo
4. Netlify auto-detects `netlify.toml` — click **Deploy**
5. Site goes live at a generated `*.netlify.app` URL
6. Optionally add a custom domain in Netlify's domain settings
