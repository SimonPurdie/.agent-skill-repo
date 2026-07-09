---
name: b2b-saas-site
description: >
  Build professional B2B SaaS marketing sites and landing pages. Contains design system
  tokens, section-level layout patterns, copy conventions, and conversion best practices
  tuned for enterprise-facing software products. Use when the user wants to create a
  landing page, marketing site, or any public-facing B2B SaaS website — including full
  sites, individual sections (hero, pricing, features, testimonials, CTA), or iterating
  on an existing B2B site.
---

# B2B SaaS Site Builder

## Workflow

1. **Identify scope** — full site, single page, or specific section(s)?
2. **Extract brand** — pull any colors, fonts, or logo info from the user. If none given, derive a palette from the product domain (see Design Tokens below for defaults).
3. **Pick sections** — use the Section Playbook below to select and order sections.
4. **Build** — single-file React (JSX) artifact. Wire the design tokens as CSS custom properties at the top. Build each section as its own component.
5. **Review against the Anti-Patterns checklist** before delivering.

---

## Design Tokens (defaults — override with user's brand)

```css
:root {
  /* Neutrals */
  --bg-base:       #ffffff;
  --bg-surface:    #f4f6f8;       /* card backgrounds, alternating sections */
  --bg-dark:       #0f1117;       /* dark sections, footer */
  --text-primary:  #1a1d23;
  --text-secondary:#5a5f6e;
  --text-muted:    #8a8f9e;
  --border:        #e2e4e9;

  /* Brand accent — replace these */
  --accent:        #2d6be4;       /* primary CTA, links, active states */
  --accent-hover:  #1f56c3;
  --accent-light:  #eef3fd;       /* accent tinted backgrounds */

  /* Success / trust */
  --green:         #16a34a;

  /* Typography */
  --font-display: 'Inter', sans-serif;   /* headings — swap for something with character */
  --font-body:    'Inter', sans-serif;

  /* Spacing scale (8px base) */
  --space-xs:  4px;  --space-s:   8px;  --space-m:  16px;
  --space-l:   24px; --space-xl:  40px; --space-2xl:64px; --space-3xl:96px;

  /* Radii */
  --radius-s:  6px;
  --radius-m:  10px;
  --radius-l:  16px;

  /* Shadows */
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08);
  --shadow-md: 0 4px 12px rgba(0,0,0,.10);
  --shadow-lg: 0 8px 30px rgba(0,0,0,.12);
}
```

**Typography scale** — use these consistently. H1 should feel commanding but not loud.

| Role      | Size      | Weight | Line-height |
|-----------|-----------|--------|-------------|
| H1        | 48–56px   | 700    | 1.1         |
| H2        | 36–40px   | 700    | 1.2         |
| H3        | 24px      | 600    | 1.3         |
| Body      | 16px      | 400    | 1.6         |
| Caption   | 13–14px   | 400    | 1.5         |
| Eyebrow   | 12–13px   | 600    | 1.4         | (uppercase, letter-spacing: 0.08em)

---

## Section Playbook

Each entry: **what it does**, **layout**, **key details**. Sections are listed in the most common page order.

### Nav / Header
- Fixed or sticky. White or transparent-to-solid on scroll.
- Logo left. Links center (or right on smaller navs). CTA button far right (outline or filled accent).
- Mobile: hamburger menu. Keep it functional — don't skip this.
- Max content width: 1200px. Pad sides generously.

### Hero
- **The most important section.** This is where you win or lose the visitor.
- Layout: centered or left-aligned. Left-aligned converts better for B2B — leads the eye into the page.
- Structure (top to bottom): Eyebrow label → H1 (one clear value prop, max 8 words) → subheading (one sentence expanding on it) → 2 CTAs (primary filled, secondary ghost/outline) → Social proof bar (logos of customers) below the CTAs.
- Visual: a product screenshot, dashboard mockup, or abstract graphic — NOT a stock photo of handshaking business people.
- Background: subtle gradient, mesh, or very light pattern. Not a solid color — it's too flat.

### Social Proof Bar (Logo Bar)
- A horizontal strip of 5–8 grayscale customer logos.
- Label: "Trusted by teams at…" in muted text above.
- If you don't have real logos, use company-name text in a muted, medium-weight font. Do NOT make up fake company names.
- This goes right after the hero and/or after the pricing section. Repetition is intentional.

### Features
- H2 + short paragraph intro, then the grid.
- Layout: 3-column grid on desktop, 1 on mobile. Each card: icon (simple, 24px, accent-colored) → feature name (H3) → 1–2 sentence description. No more.
- Cards sit on --bg-surface with --shadow-sm. Subtle hover lift (transform: translateY(-2px), shadow upgrade).
- Alternative layout for deeper features: 2-column, alternating image-left / image-right with text beside each. Good for "how it works" style sections.

### How It Works
- Numbered steps (1, 2, 3). Max 3–4 steps.
- Each step: number (large, accent-colored) → short title → one sentence.
- Connect steps with a subtle dashed or dotted line/connector. Makes the flow scannable.
- Dark background (--bg-dark, white text) works well here to break page rhythm.

### Pricing
- **Second most conversion-critical section.**
- Layout: 3 cards (Starter / Pro / Enterprise or similar). Middle card is "most popular" — make it visually distinct: accent border or accent background tint, a "Most Popular" badge.
- Each card: plan name → price (large) → billing toggle (monthly / annual with "Save 20%" nudge) → short description → feature list with checkmarks → CTA button.
- Enterprise card often has "Contact Sales" instead of a price. That's fine and expected.
- Feature lists: use ✓ (green) and — (muted) for comparison clarity.
- Pricing numbers should feel confident. Don't use tiny font. $XX/mo per seat, billed annually — be explicit about billing.

### Testimonials
- 3-column card grid or a 2-column layout with a featured (larger) quote.
- Each card: quote text (in italics or with a large opening quotation mark as a decorative element) → avatar (or initials circle) → name → title, Company.
- Do NOT use generic praise. Quotes should sound specific and real. If the user has no real quotes, write ones that sound plausible and specific ("Reduced our onboarding time by 40%") rather than generic ("Great product!").
- If fabricating quotes, make it clear to the user that these are placeholder testimonials.

### CTA / Call to Action (bottom)
- A full-width banner near the bottom of the page.
- Dark or accent background. Big H2. One sentence of urgency or value. One prominent CTA button (white or light on dark).
- This is the "last chance" section. Keep it simple — headline + button.

### Footer
- Dark background (--bg-dark). 3–4 columns: company info + logo, product links, resources/docs links, legal/contact.
- Copyright line at very bottom. Social icons if relevant.
- Keep links muted white (opacity 0.6), hover to full white.

---

## Conversion & Trust Hierarchy

Order of trust signals matters. Visitors decide in this sequence:

1. **Credibility** — Logo bar, clean professional design, no typos.
2. **Clarity** — Can I understand what this does in 5 seconds? (Hero does this job.)
3. **Proof** — Testimonials, case study numbers, customer logos repeated.
4. **Value** — Features and pricing justify the cost.
5. **Low-friction action** — CTA is obvious and the commitment feels small ("Start free trial", "See a demo" — not "Buy now").

CTAs should always reduce perceived risk. "Start free" beats "Sign up". "See how it works" beats "Learn more".

---

## Anti-Patterns Checklist

Before delivering, verify none of these are present:

- [ ] Purple/blue gradients as the default "tech" vibe (pick something intentional)
- [ ] Stock photography of business people shaking hands or pointing at screens
- [ ] "Revolutionary" / "Game-changing" / "Next-generation" in copy (earned, not stated)
- [ ] More than 2 CTAs competing for attention on one screen
- [ ] Walls of text — every block of body copy should be ≤2 sentences
- [ ] Features section that's really just a list of buzzwords without concrete benefit
- [ ] Mobile layout that's just a squished desktop layout (stack, resize, hide if needed)
- [ ] Fake "as seen in" logos or fabricated social proof (flag placeholders clearly)
- [ ] Uniform border-radius everywhere (6px for small elements, 10–16px for cards — vary it)
- [ ] Animation that feels gratuitous — motion should be subtle: fade-up on scroll, soft hover lifts. Nothing spinning or bouncing.
