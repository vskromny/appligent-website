# Appligent AI Design System Handoff  
*Swiss-modern | PDF-ready | March 2026*

---

## 1. Overview & Principles

Appligent AI’s design system reflects Swiss-modern precision—clear grids, purposeful color, minimal noise, and measurable outcomes. Inspired by Linear.app and Ramotion, it builds trust through restrained use of color, distinctive typography (Space Grotesk & Inter), and micro-interactions targeted for performance (all <150ms, snappy cubic-bezier transitions).

> **Guiding Principles**  
> - Every UI decision is metric-backed (conversion, engagement, compliance).  
> - Color-use is disciplined: teal for function, amber for emphasis only (<10% of surface).  
> - Typography is never substituted: Space Grotesk (headings, 500/600), Inter (body, 400/500), strict 4px baseline grid.  
> - Responsive at all breakpoints (360, 600, 960, 1280, 1600px) with optical alignment adjustments.  
> - Accessibility (WCAG AA) and GDPR/FADP compliance are non-negotiable.

---

## 2. Color System

| Name        | Hex      | Usage                         | Light Sample      | Dark Sample       |
|-------------|----------|-------------------------------|-------------------|-------------------|
| Primary     | `#00BFA6`| CTAs, icons, hero backgrounds | `▇` Teal BG       | `▇` Teal BG       |
| Highlight   | `#FF8A00`| KPIs, badges, buttons (<10%)  | `▇` Amber         | `▇` Amber         |
| Ink         | `#0B1A1F`| Main text, nav, outlines      | `▇` Charcoal      | `▇` Charcoal      |
| Fog         | `#F5F7F8`| BG, cards, field fills        | `▇` Light Grey    | `▇` Light Grey    |
| Cloud       | `#D7E2E6`| Dividers, subtle fills        | `▇` Cloud         | `▇` Cloud         |
| Focus       | `#1F6FEB`| Input focus, accessibility    | `▇` Blue          | `▇` Blue          |

### Gradients

- **Hero Gradient**:  
  `linear-gradient(135deg, #021013, #0B1A1F)`
- **CTA Band Offset**:  
  Teal base `#00BFA6` with 8px amber stripe.

> Color usage is audited for sufficient contrast and motion safety. Teal is always paired with neutral or white space for functional clarity.

---

## 3. Typography

### Font Pairings

| Font           | Usage               | Weight(s) | CSS/Token           |
|----------------|---------------------|-----------|---------------------|
| Space Grotesk  | Headlines, metrics  | 500/600   | `--font-display`    |
| Inter          | Body, form, nav     | 400/500   | `--font-body`       |

### Scale & Line Heights

| Token       | Size (px/rem)    | Line Height | Typical Usage           |
|-------------|------------------|-------------|-------------------------|
| `text-xs`   | 13 / 0.8125rem   | 1.5         | Annotations, captions   |
| `text-sm`   | 15 / 0.9375rem   | 1.5         | Button, input, nav      |
| `text-base` | 17 / 1.0625rem   | 1.6         | Body, forms, lists      |
| `text-lg`   | 20 / 1.25rem     | 1.4         | Card titles, section hdg|
| `text-xl`   | 24–28 / clamp    | 1.2–1.3     | Major headings, hero    |
| `text-hero` | 40–60 / clamp    | 1.1         | Main hero, key metrics  |

> All typography aligns to a 4px baseline. No substitutions—widen letter-spacing for Space Grotesk at larger sizes.

**CSS Tokens:**
```css
:root {
  --font-display: "Space Grotesk", system-ui;
  --font-body: "Inter", system-ui;
  --text-xs: 0.8125rem;
  --text-sm: 0.9375rem;
  --text-base: 1.0625rem;
  --text-lg: 1.25rem;
  --text-xl: clamp(1.5rem, 1.2vw, 1.75rem);
  --text-hero: clamp(2.5rem, 4vw, 3.75rem);
}
```

---

## 4. Core UI Elements & Components

### Navigation

- **Pattern**: Sticky top bar, logo left, nav links center/right, CTA button right.
- **Spacing**: `space-4` (16px) between links, `space-8` (32px) outer.
- **Accessibility**: Focus ring (`#1F6FEB`), keyboard trap, skip-to-content.
- **Metric:** Navigation click analytics; “Book a call” CTA tracks `nav_cta_click`.

### Hero Module

- **Composition**: 12-col grid; 7col text/CTA, 4col proof (logos or KPI chips).
- **Accent**: Gradient BG, main CTA pill style, automation metric badge.
- **Annotations**: Quantified outcome visible, minimum LCP <1.5s.
- **Spacing**: `space-6`/`space-8` stack; CTA above fold.

### Cards (Expertise Grid)

- **Responsive**: 3x2 desktop, 2x3 tablet, stacked mobile.
- **Features**: Icon “notch”, bold title, 120 char max copy, stat reveal on hover.
- **Spacing**: `space-3` (12px) grid, align to 4px baseline, radius `soft`.
- **Analytics:** Card click = `expertise_tile_click`.

### Timeline (Process Steps)

- **Desktop**: Horizontal sticky, 5 steps (Discover→Govern), icon for each.
- **Mobile**: Accordion steps.
- **Motion**: Step-in animation <150ms.
- **Spacing**: `space-6` horizontal gap, step width flexible.
- **Analytics:** Step viewed = `timeline_step_view`.

### CTA Band

- **Treatment**: Full-bleed teal, thin amber offset (8px), centered CTA + secondary link.
- **Spacing**: Padding `space-12` (48px), minimum button height 48px.
- **Copy Constraint**: ≤80 characters.
- **Analytics:** CTA = `cta_band_cta`

### Register Form (GDPR Lead Form)

- **Form Fields**: Name, Work Email, Company, Scope, Consent (checkbox), Optional Notes.
- **Compliance**: Explicit consent & data residency, privacy link ≤32px.
- **Spacing**: Fields vertically spaced `space-4` (16px), group boxes `space-8` (32px).
- **Error Handling**: Inline, color `highlight` for error, clear focus.
- **Anti-spam**: Hidden field `workstyle`.
- **Analytics:** Submit = `lead_submit`
- **Sample Snippet:**
```html
<label class="consent">
  <input type="checkbox" required>
  <span>I agree to Appligent AI storing my data in Switzerland...</span>
</label>
<p class="privacy-text">
  We operate under GDPR & Swiss FADP. <a href="/privacy">Privacy policy</a>.
</p>
```

---

## 5. Interactive States & Motion

- **States**:  
  - *Hover*: Subtle shadow (`shadow-card`), background tint (2–4%).  
  - *Focus*: Blue outline (`#1F6FEB`), always visible, 2px thickness.  
  - *Pressed*: 96% scale, softened shadow, fast transition (<120ms).
- **Motion**:  
  - All UI feedback <150ms using   
    `cubic-bezier(0.32, 0.72, 0, 1)`   
  - No excessive bounce or opacity fade.  
  - Disable animations if reduced-motion is set.
- **Consistency**:  
  - Animation speeds and easing must not diverge by section; tokens enforced.

---

## 6. Accessibility Checklist & Asset Requirements

### WCAG & Compliance

| Requirement                  | How to Achieve / Test             |
|------------------------------|-----------------------------------|
| Text contrast AA             | Check all text vs bg/accents      |
| Keyboard navigation          | Nav, cards, forms, CTA accessible |
| Focus style                  | Always visible, tokenized color   |
| ARIA labels, landmarks       | All nav and forms marked up       |
| Responsive at all breakpoints| Test at 360, 600, 960, 1280, 1600 |
| Error feedback               | Inline; role="alert" where needed |
| Consent & Privacy            | Checkbox + privacy link ≤32px     |

### Asset Checklist

- Fonts (Space Grotesk, Inter) self-hosted, subsetted.
- SVG icons: monochrome, scalable, tested on dark/fog backgrounds.
- Favicon and OpenGraph image present (+ alt text).
- All section metrics/KPI text pre-approved and logged.
- Form endpoints must log and store explicit consent.

---

> **Implementation Notes:**  
> - Copy this document into your md→PDF generator; no further styling necessary.  
> - Each table and code block is mapped to system tokens for handoff.   
> - All annotations explain intent, compliance, and measurement.   
> - Before dev start: QA each component using this checklist and provide feedback via Linear.

---

**End of Appligent AI Design System PDF Handoff**
