# Homepage Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesign the inbarfishpilates.com homepage with hero, About Inbar bio, photo gallery, and prominent BookYourMat CTA — Direction 2 ("Warm & Inviting") per the design spec.

**Architecture:** Single static HTML file (`index.html`). All CSS inline in `<style>`. No build step, no framework. Photos dropped into `/photos/` as placeholder files; user replaces with real images later. Stitch generates a visual mockup first to lock direction, then we translate to HTML.

**Tech Stack:** HTML, CSS (custom properties), Google Fonts (Cormorant Garamond, Jost), Google Stitch (mockup), deployed via GitHub Pages with CNAME.

**Spec:** `docs/superpowers/specs/2026-04-17-homepage-redesign-design.md`

---

## Task 1: Generate Stitch Mockup for Visual Direction

**Files:**
- None created yet (Stitch output is visual, stored in Stitch account)

- [ ] **Step 1: Load Stitch tools**

Use ToolSearch with `select:mcp__stitch__generate_screen_from_text,mcp__stitch__create_project,mcp__stitch__list_projects` to load Stitch tool schemas.

- [ ] **Step 2: Create the Stitch project**

Call `mcp__stitch__create_project` with:
- name: "Inbar Fish Pilates Homepage"
- description: "Reformer Pilates studio landing page redesign"

Record the returned `project_id` for later calls.

- [ ] **Step 3: Generate the homepage screen**

Call `mcp__stitch__generate_screen_from_text` with the project ID and this prompt:

```
A minimal, warm wellness studio landing page for "Inbar Fish Pilates" — a reformer pilates studio in Sunnyvale, CA.

Palette: cream background #F7F4EF, warm white cards #FDFCFA, warm black text #1A1714, sage accent #8B9A7E, stone meta text #A89F91. Editorial and calm.

Typography: Cormorant Garamond serif for headings, Jost sans-serif light for body.

Page sections, top to bottom:
1. Thin header with "Inbar Fish Pilates" wordmark on left and "Book" text link on right.
2. Hero row: left side has H1 "Reformer Pilates in Sunnyvale", subhead "Small group classes and private sessions with Inbar Fish, BASI-certified instructor.", and a sage "Book a Session" button. Right side has a portrait photo (4:5 aspect, rounded corners).
3. About Inbar row: left side has a square portrait photo, right side has heading "About Inbar" and a paragraph about BASI certification and mind-body approach.
4. Sessions: three clean card tiles in a column — Group Class, Private Reformer 60min, Private Reformer 30min — each with name, duration, and "Contact for details" in sage.
5. Gallery: three 3:4 photo tiles in a horizontal row.
6. Full-width sage-tinted CTA band: "Ready to start? Schedule your session at BookYourMat.com." with a sage "Book Now" button.
7. Slim contact: email and Sunnyvale location.
8. Footer: copyright + Privacy Policy + Terms links.

Overall feel: quiet, refined, unhurried. Generous whitespace. Max content width around 960px.
```

- [ ] **Step 4: Review the generated mockup with user**

Report the Stitch screen URL back to the user, ask them to review, and wait for approval or revisions before continuing.

- [ ] **Step 5: Commit (nothing to commit — Stitch output lives in Stitch)**

Skip this step. Move to Task 2 only after user approves the mockup direction (or approves proceeding despite imperfections — Stitch is directional, not literal).

---

## Task 2: Set Up Photos Directory with Placeholders

**Files:**
- Create: `photos/hero.jpg`
- Create: `photos/portrait.jpg`
- Create: `photos/gallery-1.jpg`
- Create: `photos/gallery-2.jpg`
- Create: `photos/gallery-3.jpg`
- Create: `photos/README.md`

- [ ] **Step 1: Create the photos directory**

Run:
```bash
mkdir -p /Users/amirfish/inbar-fish-pilates/photos
```

- [ ] **Step 2: Add placeholder files**

Use Python to write a minimal valid 1x1 JPEG to each filename. Python 3 is reliably present on macOS. Run:

```bash
cd /Users/amirfish/inbar-fish-pilates/photos
python3 - <<'PY'
import base64, pathlib
JPG = base64.b64decode(
    b"/9j/4AAQSkZJRgABAQEASABIAAD/2wBDAAEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEB"
    b"AQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQH/2wBDAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEB"
    b"AQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQH/wAARCAABAAEDAREAAhEBAxEB/8QAFAABAAAA"
    b"AAAAAAAAAAAAAAAACP/EABQBAQAAAAAAAAAAAAAAAAAAAAX/2gAMAwEAAhADEAAAAQ7/AP/EABQQAQAA"
    b"AAAAAAAAAAAAAAAAAAD/2gAIAQEAAT8Afw//xAAUEQEAAAAAAAAAAAAAAAAAAAAA/9oACAECAQE/AH8P"
    b"/8QAFBEBAAAAAAAAAAAAAAAAAAAAAP/aAAgBAwEBPwB/D//Z"
)
for name in ["hero.jpg", "portrait.jpg", "gallery-1.jpg", "gallery-2.jpg", "gallery-3.jpg"]:
    pathlib.Path(name).write_bytes(JPG)
print("wrote 5 placeholders")
PY
```

Expected output: `wrote 5 placeholders`. Five 1x1 JPEG files now exist, each ~160 bytes. Layout holds via CSS aspect-ratio; user replaces with real photos later.

- [ ] **Step 3: Add a README so the user knows what to replace**

Create `photos/README.md` with:

```markdown
# Photos

Replace these placeholder files with real photos:

- `hero.jpg` — hero image (recommended 4:5 portrait aspect, ~1200x1500px)
- `portrait.jpg` — Inbar portrait for About section (square, ~800x800px)
- `gallery-1.jpg`, `gallery-2.jpg`, `gallery-3.jpg` — gallery tiles (3:4 aspect, ~900x1200px)

Keep filenames the same so `index.html` picks them up automatically.
```

- [ ] **Step 4: Verify files exist**

Run:
```bash
ls -la /Users/amirfish/inbar-fish-pilates/photos/
```

Expected: 5 .jpg files + README.md.

- [ ] **Step 5: Commit**

```bash
cd /Users/amirfish/inbar-fish-pilates
git add photos/
git commit -m "Add photos directory with placeholders and replacement guide"
```

---

## Task 3: Expand Page Scaffold (max-width, header, CSS variables)

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Bump max-width and add new CSS variables**

In `index.html`, find the `.page` rule (currently `max-width: 720px`) and update it. Also extend `:root` with new variables.

Replace the existing `:root` block with:

```css
:root {
    --cream: #F7F4EF;
    --warm-black: #1A1714;
    --stone: #A89F91;
    --sage: #8B9A7E;
    --sage-dark: #6F7D64;
    --sage-light: #C2CEBC;
    --sage-band: #E8EDE3;
    --warm-white: #FDFCFA;
    --border: #E8E3DB;
}
```

Replace the existing `.page` rule with:

```css
.page {
    max-width: 960px;
    margin: 0 auto;
    padding: 40px 24px 60px;
}
```

- [ ] **Step 2: Add the header element**

Inside `<body><div class="page">`, immediately after the opening `<div class="page">`, insert:

```html
<header class="site-header">
    <span class="wordmark">Inbar Fish Pilates</span>
    <a href="#book" class="header-book">Book</a>
</header>
```

Then remove the existing top `<h1>Inbar Fish Pilates</h1>` and its tagline `<p class="tagline">` — those move into the new hero section in Task 4.

- [ ] **Step 3: Add header CSS**

Inside the `<style>` block, after the existing `.page` rule, add:

```css
.site-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-bottom: 40px;
    border-bottom: 1px solid var(--border);
    margin-bottom: 48px;
}

.wordmark {
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.15rem;
    font-weight: 600;
    letter-spacing: 0.02em;
}

.header-book {
    font-family: 'Jost', sans-serif;
    font-size: 0.9rem;
    text-transform: uppercase;
    letter-spacing: 0.15em;
    color: var(--sage);
    text-decoration: none;
    border-bottom: 1px solid var(--sage-light);
    padding-bottom: 2px;
    transition: border-color 0.2s;
}

.header-book:hover {
    border-color: var(--sage);
}
```

- [ ] **Step 4: Open the file in a browser to sanity-check**

Run:
```bash
open /Users/amirfish/inbar-fish-pilates/index.html
```

Expected: page still loads without JS errors; wordmark on top-left, "Book" link on top-right; services and contact still render below the existing divider. The page is currently missing H1/tagline (Task 4 adds them back inside the hero).

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "Scaffold homepage header and extend CSS variables"
```

---

## Task 4: Build Hero Section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add hero HTML**

Immediately after the closing `</header>`, insert:

```html
<section class="hero">
    <div class="hero-text">
        <h1>Reformer Pilates in Sunnyvale</h1>
        <p class="hero-sub">Small group classes and private sessions with Inbar Fish, BASI-certified instructor.</p>
        <a href="https://bookyourmat.com/book/inbar-fish-pilates" class="btn btn-primary">Book a Session →</a>
    </div>
    <div class="hero-photo">
        <img src="photos/hero.jpg" alt="Inbar Fish teaching reformer pilates">
    </div>
</section>
```

- [ ] **Step 2: Add hero CSS**

Inside `<style>`, add after the `.header-book:hover` rule:

```css
.hero {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 64px;
    align-items: center;
    margin-bottom: 100px;
}

.hero-text h1 {
    font-family: 'Cormorant Garamond', serif;
    font-weight: 400;
    font-size: clamp(2.6rem, 6vw, 4.2rem);
    letter-spacing: -0.02em;
    line-height: 1.1;
    margin-bottom: 20px;
}

.hero-sub {
    font-size: 1.1rem;
    color: var(--warm-black);
    opacity: 0.75;
    margin-bottom: 32px;
    max-width: 420px;
}

.hero-photo img {
    width: 100%;
    aspect-ratio: 4 / 5;
    object-fit: cover;
    border-radius: 12px;
    display: block;
}

.btn {
    display: inline-block;
    font-family: 'Jost', sans-serif;
    font-size: 0.95rem;
    font-weight: 400;
    letter-spacing: 0.08em;
    padding: 14px 32px;
    border-radius: 6px;
    text-decoration: none;
    transition: background 0.2s, transform 0.1s;
}

.btn-primary {
    background: var(--sage);
    color: var(--warm-white);
}

.btn-primary:hover {
    background: var(--sage-dark);
}

.btn-primary:active {
    transform: translateY(1px);
}

@media (max-width: 700px) {
    .hero {
        grid-template-columns: 1fr;
        gap: 40px;
    }
    .hero-photo { order: -1; }
}
```

- [ ] **Step 3: Remove the old top H1/tagline if still present**

The old `<h1>Inbar Fish Pilates</h1>` and `<p class="tagline">...</p>` were marked for removal in Task 3. Confirm they are no longer in the file. If still there, delete them.

- [ ] **Step 4: Reload in browser to verify**

Run:
```bash
open /Users/amirfish/inbar-fish-pilates/index.html
```

Expected: Hero renders with H1 on left, placeholder (tiny dot) photo on right with rounded corners. "Book a Session →" button is sage-colored. On narrow viewport (<700px) photo stacks above text. Clicking the button opens BookYourMat.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "Add hero section with primary booking CTA"
```

---

## Task 5: Build About Inbar Section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add the About HTML**

After `</section>` closing the hero, add:

```html
<section class="about">
    <div class="about-photo">
        <img src="photos/portrait.jpg" alt="Portrait of Inbar Fish" loading="lazy">
    </div>
    <div class="about-text">
        <h2>About Inbar</h2>
        <p>Inbar Fish is a certified instructor who completed her training at the BASI Academy in El Dorado Hills, CA. Her approach emphasizes the mind-body connection and the therapeutic, restorative nature of "soft" exercises — refreshing the practitioner while gradually building physical strength through more demanding movements.</p>
    </div>
</section>
```

- [ ] **Step 2: Add About CSS**

Inside `<style>`, append after the hero media query:

```css
.about {
    display: grid;
    grid-template-columns: 1fr 1.2fr;
    gap: 64px;
    align-items: center;
    margin-bottom: 100px;
}

.about-photo img {
    width: 100%;
    aspect-ratio: 1 / 1;
    object-fit: cover;
    border-radius: 12px;
    display: block;
}

.about-text h2 {
    font-family: 'Cormorant Garamond', serif;
    font-weight: 600;
    font-size: 2rem;
    margin-bottom: 20px;
    letter-spacing: -0.01em;
}

.about-text p {
    font-size: 1.05rem;
    line-height: 1.8;
}

@media (max-width: 700px) {
    .about {
        grid-template-columns: 1fr;
        gap: 32px;
    }
}
```

- [ ] **Step 3: Also update the existing global `h2` rule**

Find the existing `h2 { ... }` rule (around line 63-69 in the original file) and delete it — it's now superseded by the section-specific rules. If other sections still need an h2 style, they'll get their own rules in later tasks.

- [ ] **Step 4: Reload and verify**

Run:
```bash
open /Users/amirfish/inbar-fish-pilates/index.html
```

Expected: About section renders below hero, portrait placeholder on left, heading + bio on right. Collapses to single column under 700px.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "Add About Inbar section with BASI bio"
```

---

## Task 6: Refine Sessions Section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Replace the existing Sessions block**

Find the existing `<h2>Sessions</h2>` and its `<p>` intro plus the `<div class="services">...</div>` (around lines 167-192 in the original file). Also delete the `<div class="divider"></div>` that sits above it.

Replace all of that with:

```html
<section class="sessions">
    <div class="section-heading">
        <h2>Sessions</h2>
        <p class="section-intro">Small group classes and private reformer sessions designed to build strength, improve flexibility, and restore balance — with personalized attention in every session.</p>
    </div>
    <div class="services">
        <div class="service-card">
            <div>
                <div class="service-name">Group Class</div>
                <div class="service-detail">Up to 3 participants · 60 minutes</div>
            </div>
            <div class="service-price">Contact for details</div>
        </div>
        <div class="service-card">
            <div>
                <div class="service-name">Private Reformer Session</div>
                <div class="service-detail">One-on-one · 60 minutes</div>
            </div>
            <div class="service-price">Contact for details</div>
        </div>
        <div class="service-card">
            <div>
                <div class="service-name">Private Reformer Session</div>
                <div class="service-detail">One-on-one · 30 minutes</div>
            </div>
            <div class="service-price">Contact for details</div>
        </div>
    </div>
</section>
```

- [ ] **Step 2: Add sessions section CSS**

Inside `<style>`, append:

```css
.sessions {
    margin-bottom: 100px;
}

.section-heading {
    margin-bottom: 32px;
    max-width: 620px;
}

.section-heading h2 {
    font-family: 'Cormorant Garamond', serif;
    font-weight: 600;
    font-size: 2rem;
    margin-bottom: 12px;
    letter-spacing: -0.01em;
}

.section-intro {
    color: var(--warm-black);
    opacity: 0.75;
    font-size: 1.02rem;
}
```

The existing `.services`, `.service-card`, `.service-name`, `.service-detail`, `.service-price` rules stay as they are — they already match the desired look.

- [ ] **Step 3: Reload and verify**

Run:
```bash
open /Users/amirfish/inbar-fish-pilates/index.html
```

Expected: Sessions heading, short intro, then three cards that look the same as before (same styling).

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Refactor sessions section with semantic heading block"
```

---

## Task 7: Build Gallery Section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add gallery HTML**

After `</section>` closing `.sessions`, add:

```html
<section class="gallery">
    <div class="gallery-grid">
        <figure class="gallery-tile">
            <img src="photos/gallery-1.jpg" alt="Studio interior" loading="lazy">
        </figure>
        <figure class="gallery-tile">
            <img src="photos/gallery-2.jpg" alt="Reformer equipment" loading="lazy">
        </figure>
        <figure class="gallery-tile">
            <img src="photos/gallery-3.jpg" alt="Client mid-session" loading="lazy">
        </figure>
    </div>
</section>
```

- [ ] **Step 2: Add gallery CSS**

Inside `<style>`, append:

```css
.gallery {
    margin-bottom: 100px;
}

.gallery-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
}

.gallery-tile {
    margin: 0;
    overflow: hidden;
    border-radius: 12px;
    transition: transform 0.3s ease;
}

.gallery-tile img {
    width: 100%;
    aspect-ratio: 3 / 4;
    object-fit: cover;
    display: block;
}

.gallery-tile:hover {
    transform: translateY(-4px);
}

@media (max-width: 700px) {
    .gallery-grid {
        grid-template-columns: 1fr;
        gap: 16px;
    }
}
```

- [ ] **Step 3: Reload and verify**

Run:
```bash
open /Users/amirfish/inbar-fish-pilates/index.html
```

Expected: Three placeholder tiles in a row below sessions, stacking to a single column under 700px. Small hover lift on each tile.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add gallery section with three photo tiles"
```

---

## Task 8: Build BookYourMat CTA Band

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add CTA band HTML**

After the gallery `</section>`, add:

```html
<section id="book" class="book-band">
    <h2 class="book-heading">Ready to start?</h2>
    <p class="book-sub">Schedule your session at <strong>BookYourMat.com</strong>.</p>
    <a href="https://bookyourmat.com/book/inbar-fish-pilates" class="btn btn-primary">Book Now →</a>
</section>
```

Note the `id="book"` — this matches the header's `#book` anchor link.

- [ ] **Step 2: Add CTA band CSS**

Inside `<style>`, append:

```css
.book-band {
    background: var(--sage-band);
    border-radius: 12px;
    padding: 64px 32px;
    text-align: center;
    margin-bottom: 100px;
}

.book-heading {
    font-family: 'Cormorant Garamond', serif;
    font-weight: 600;
    font-size: 2.2rem;
    margin-bottom: 12px;
    letter-spacing: -0.01em;
}

.book-sub {
    font-size: 1.05rem;
    color: var(--warm-black);
    opacity: 0.8;
    margin-bottom: 28px;
}

.book-sub strong {
    color: var(--sage-dark);
    font-weight: 500;
}
```

- [ ] **Step 3: Reload and verify**

Run:
```bash
open /Users/amirfish/inbar-fish-pilates/index.html
```

Expected: Sage-tinted band with centered heading, subtext mentioning BookYourMat.com, and a sage "Book Now →" button. Clicking it opens BookYourMat booking page. Clicking the top-right "Book" header link scrolls to this band.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add BookYourMat CTA band with anchor target"
```

---

## Task 9: Slim the Contact Section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Replace the existing Contact block**

Find the current `<h2>Get in Touch</h2>` and the `<div class="contact-block">...</div>` and the preceding `<div class="divider"></div>`.

Replace with:

```html
<section class="contact">
    <p>Questions? Email <a href="mailto:info@inbarfishpilates.com">info@inbarfishpilates.com</a>.</p>
    <p class="contact-loc">Sunnyvale, CA 94087</p>
</section>
```

- [ ] **Step 2: Replace the existing `.contact-block` CSS with new `.contact` CSS**

Find the existing `.contact-block` rules and delete them. Add:

```css
.contact {
    text-align: center;
    padding: 48px 0;
    border-top: 1px solid var(--border);
    margin-bottom: 40px;
}

.contact p {
    font-size: 1rem;
    margin-bottom: 8px;
}

.contact a {
    color: var(--sage);
    text-decoration: none;
    border-bottom: 1px solid var(--sage-light);
    transition: border-color 0.2s;
}

.contact a:hover {
    border-color: var(--sage);
}

.contact-loc {
    color: var(--stone);
    font-size: 0.92rem;
    margin-bottom: 0;
}
```

Also remove the now-unused `.divider` rule and any remaining `<div class="divider"></div>` elements from the HTML.

- [ ] **Step 3: Reload and verify**

Run:
```bash
open /Users/amirfish/inbar-fish-pilates/index.html
```

Expected: Slim centered contact block below the CTA band, with a top border. Email link is sage-colored. Location line in stone below.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Slim contact section now that booking is its own band"
```

---

## Task 10: Add Subtle Scroll Motion

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add the fade-up CSS**

Inside `<style>`, append:

```css
.fade-in {
    opacity: 0;
    transform: translateY(16px);
    transition: opacity 0.7s ease, transform 0.7s ease;
}

.fade-in.visible {
    opacity: 1;
    transform: translateY(0);
}

@media (prefers-reduced-motion: reduce) {
    .fade-in {
        opacity: 1;
        transform: none;
        transition: none;
    }
}
```

- [ ] **Step 2: Add the `fade-in` class to each section**

Add `class="fade-in"` (merge with existing class attributes) to:
- `<section class="hero">` → `<section class="hero fade-in">`
- `<section class="about">` → `<section class="about fade-in">`
- `<section class="sessions">` → `<section class="sessions fade-in">`
- `<section class="gallery">` → `<section class="gallery fade-in">`
- `<section id="book" class="book-band">` → `<section id="book" class="book-band fade-in">`

- [ ] **Step 3: Add the IntersectionObserver script**

Just before `</body>`, add:

```html
<script>
    (function () {
        if (!('IntersectionObserver' in window)) {
            document.querySelectorAll('.fade-in').forEach(el => el.classList.add('visible'));
            return;
        }
        const obs = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('visible');
                    obs.unobserve(entry.target);
                }
            });
        }, { threshold: 0.12 });
        document.querySelectorAll('.fade-in').forEach(el => obs.observe(el));
    })();
</script>
```

- [ ] **Step 4: Reload and verify**

Run:
```bash
open /Users/amirfish/inbar-fish-pilates/index.html
```

Expected: Each section fades up gently on scroll. Hero is visible on load (already in viewport). No motion if OS prefers-reduced-motion is set.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "Add subtle fade-up motion on scroll with reduced-motion fallback"
```

---

## Task 11: Final Verification

**Files:**
- None modified

- [ ] **Step 1: Full-page visual check in browser**

Run:
```bash
open /Users/amirfish/inbar-fish-pilates/index.html
```

Walk through the page from top to bottom and confirm:
- Header: wordmark left, "Book" link right; "Book" link scrolls to the CTA band.
- Hero: H1, subhead, sage "Book a Session →" button linking to BookYourMat; placeholder hero photo on right.
- About: portrait placeholder on left, heading + BASI bio on right.
- Sessions: heading, intro, three cards all reading "Contact for details".
- Gallery: three placeholder tiles in a row.
- Book band: sage-tinted, centered, "Book Now →" button linking to BookYourMat.
- Contact: slim, centered, email + location.
- Footer: © + Privacy + Terms.

- [ ] **Step 2: Responsive check**

In the browser, resize the window to ~375px wide (iPhone). Confirm:
- Hero photo stacks above text.
- About portrait stacks above text.
- Gallery stacks to one column.
- All tap targets remain reachable.
- No horizontal scroll.

- [ ] **Step 3: Link check**

Click every link:
- Header "Book" → scrolls to `#book` band.
- Hero "Book a Session →" → opens https://bookyourmat.com/book/inbar-fish-pilates in a new tab (or same tab; both ok).
- Book band "Book Now →" → same URL.
- Email link → opens mail client.
- Privacy and Terms → load existing pages.

- [ ] **Step 4: Run HTML through a quick sanity check**

Run:
```bash
grep -c '<section' /Users/amirfish/inbar-fish-pilates/index.html
```

Expected: 5 (hero, about, sessions, gallery, book) + 1 (contact) = 6.

Run:
```bash
grep -c 'bookyourmat.com/book/inbar-fish-pilates' /Users/amirfish/inbar-fish-pilates/index.html
```

Expected: at least 2 (hero button + band button).

- [ ] **Step 5: No-op commit / done**

If any issues surfaced, fix them and commit. Otherwise no final commit needed — previous tasks each committed their piece.

Report back to the user with:
- Summary of what shipped
- Reminder to replace `photos/*.jpg` placeholders with real images
- Note that the BookYourMat URL is wired into both CTAs
