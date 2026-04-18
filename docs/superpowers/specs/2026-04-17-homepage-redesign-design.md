# Homepage Redesign — Design Spec

**Date:** 2026-04-17
**Project:** inbarfishpilates.com homepage
**Direction:** Warm & Inviting landing page with photos, bio, and prominent booking CTA

## Goals

1. Make the homepage feel warmer and more inviting than the current minimal text-only layout.
2. Introduce Inbar as a person (bio + portrait) to build trust.
3. Add photography that conveys the studio and practice.
4. Make booking a primary, obvious action via BookYourMat.com integration.
5. Preserve the existing brand palette and typography.

## Out of Scope

- Multi-page redesign (privacy/terms pages unchanged).
- E-commerce, scheduling widgets, or account features (BookYourMat handles this).
- CMS integration — content stays hardcoded in `index.html`.
- Price display (current "Contact for details" stays).

## Page Structure

```
1. HEADER       Minimal — name mark (top-left), "Book" anchor link (top-right)
2. HERO         Photo (right) + H1 + subhead + primary CTA button
3. ABOUT INBAR  Portrait (left) + BASI bio + philosophy
4. SESSIONS     Three refined service cards (Group, Private 60, Private 30)
5. GALLERY      Horizontal row of 3 photos
6. BOOK CTA     Full-width sage-tinted band with booking button
7. CONTACT      Email + Sunnyvale location (slimmed from current)
8. FOOTER       © + Privacy/Terms (unchanged)
```

## Content

### Hero
- **H1:** "Reformer Pilates in Sunnyvale"
- **Subhead:** "Small group classes and private sessions with Inbar Fish, BASI-certified instructor."
- **Primary CTA:** "Book a Session →" (links to BookYourMat URL)
- **Photo slot:** hero image, 4:5 portrait aspect

### About Inbar
- **Section title:** "About Inbar"
- **Body:**
  > Inbar Fish is a certified instructor who completed her training at the BASI Academy in El Dorado Hills, CA. Her approach emphasizes the mind-body connection and the therapeutic, restorative nature of "soft" exercises — refreshing the practitioner while gradually building physical strength through more demanding movements.
- **Photo slot:** portrait, square aspect, left-aligned

### Sessions
Keep current three services and "Contact for details" pricing:
- Group Class — up to 3 participants, 60 min
- Private Reformer Session — one-on-one, 60 min
- Private Reformer Session — one-on-one, 30 min

### Gallery
- 3 photo tiles, 3:4 aspect, horizontal row
- Optional captions: Studio · Reformer · In motion
- Subtle hover lift

### Book CTA band
- Full-width sage-tinted section
- Copy: "Ready to start? Schedule your session at **BookYourMat.com**."
- Button: "Book Now →"
- Link target: `https://bookyourmat.com/book/inbar-fish-pilates`

### Contact
- Email: `info@inbarfishpilates.com`
- Location: Sunnyvale, CA 94087
- Slim single block, no heavy framing now that booking is its own section

## Visual Treatment

### Palette (existing, unchanged)
- Cream `#F7F4EF` — page background
- Warm white `#FDFCFA` — card surfaces
- Warm black `#1A1714` — text
- Sage `#8B9A7E` — accents and CTAs
- Sage light `#C2CEBC` — links/underlines
- Stone `#A89F91` — meta/tagline text

### Typography (existing, unchanged)
- Cormorant Garamond — headings; hero H1 scaled up to ~clamp(3rem, 7vw, 5rem)
- Jost 300 — body, tagline, buttons

### Layout
- Max-width: 960px (bumped from 720px to let photos breathe)
- Two-column rows (hero, about) collapse to single column under 700px
- ~100px vertical rhythm between sections

### Photos
- Hero: 4:5 portrait, 12px rounded corners
- About portrait: square, 12px rounded
- Gallery: 3:4 tiles, equal-height row, 12px rounded, subtle hover lift
- All: `loading="lazy"` except hero; `alt` text populated

### Buttons
- Primary: solid sage background, warm-white text, 14px 32px padding, 6px radius
- Hover: darkens ~10%

### Motion
- Fade-up on scroll for sections
- Subtle hover lifts on photos/cards
- No complex animation

## Workflow

1. Use Google Stitch (via `mcp__stitch__*`) to generate a mockup capturing hero + about visual direction.
2. Review Stitch output with user, iterate if needed.
3. Translate approved mockup into `index.html`, extending existing CSS.
4. Add placeholder `<img>` tags pointing to `/photos/*.jpg` — user drops real photos in later.
5. Verify locally, commit.

## Implementation Notes

- Single file edit: `index.html`.
- Keep existing CSS custom properties; add new ones only where needed (e.g., `--sage-band` for CTA band tint).
- No build step, no framework. Site stays fully static.
- Privacy/terms pages untouched.

## Risks

- Stitch output is a visual mockup, not production HTML — translation step required.
- Placeholder photo proportions may not match final images; final tuning expected after real photos are provided.
- BookYourMat link is external; if the slug changes we update the href in one place.

## Success Criteria

- Homepage renders correctly on mobile (≤500px), tablet, and desktop.
- Hero section visible above the fold with visible CTA on common laptop sizes.
- BookYourMat CTA appears at least twice (hero + dedicated band).
- Bio content integrated verbatim from user-provided source (lightly edited for flow).
- Existing palette, fonts, and overall calm aesthetic preserved.
