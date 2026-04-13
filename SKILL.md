---
name: web-builder
model: opus
description: "Generate professional, multi-page websites for real businesses. This skill should be used when the user provides a business context (Google page URL, business name, industry) and wants a complete website generated. Extracts business information, downloads real photos and logos, applies the ACTUAL brand colors from the existing site, and produces a full multi-page HTML/CSS/JS website with agency-level quality. Triggers on: 'create a website for', 'build a site for', 'generate a website', 'web-builder', or any request to create a business website from a Google listing or business description."
---

# Web Builder

Generate professional, multi-page business websites that look like they were built by a web agency. Accept a business website URL, Google Business page, or business description as input, extract all relevant information, and produce a complete, polished, responsive website.

## Workflow

### Phase 1: Business Intelligence Gathering

**Step 1 - Fetch the business website** (CRITICAL):

If a website URL is provided, fetch it to extract:
- Business name, industry, services, address, phone, email, hours
- **EXACT color codes from the CSS** (hex values, CSS variables) - NEVER guess colors
- All image URLs (logo, project photos, team photos, banners)
- Reviews, certifications, partnerships

**Step 2 - Download all assets:**

Download to `images/` directory using curl:
- Logo file
- Project/portfolio photos
- Partner logos
- Any other relevant images from the site

These MUST be used in the generated site. No placeholder images when real ones exist.

**Step 3 - Additional context (if needed):**

If only a Google Business page or description is provided:
- Search for the company website and fetch it
- Extract colors and images from there
- If no website exists, ask the user for brand colors or derive from the logo

### Phase 2: Site Architecture

Determine pages based on the business type. Refer to `references/page-structures.md` for detailed page structures per industry.

**Standard pages (always include):**
1. **Accueil** (Homepage) - Hero, services overview, testimonials, CTA
2. **A propos** (About) - Story, team, values
3. **Services** (Services) - Detailed service offerings
4. **Contact** - Form, map, phone, hours

**Optional pages (based on industry):**
- **Realisations / Portfolio** - For artisans, construction, agencies, photographers
- **Menu / Carte** - For restaurants, bars, cafes
- **Tarifs** - For services with pricing
- **Blog / Actualites** - If the business has content to showcase
- **FAQ** - For businesses with common questions

Proceed directly to design and implementation — do NOT pause for user validation.

### Phase 3: Design Direction

**Use the `ui-ux-pro-max` skill** to inform all design decisions: color palettes, font pairings, UX guidelines, spacing systems, and interaction patterns. Invoke it with `/ui-ux-pro-max` for palette selection, typography, and component styling when needed.

**CRITICAL RULE: Match the design tone to the industry. Do NOT default to a generic "elegant" aesthetic.**

**Design tone by industry:**

| Industry | Typography | Style | Avoid |
|----------|-----------|-------|-------|
| BTP / Construction / Renovation | Bold sans-serif, uppercase, 700-800 weight | Industrial, solid, high contrast, strong | Elegant serifs, italic, soft/decorative, artistic |
| Luxury / High-end | Refined serif, light weight | Minimal, generous whitespace, muted palette | Bold, loud, uppercase everything |
| Tech / SaaS / Startup | Clean geometric sans-serif | Sharp, modern, vibrant accents | Old-fashioned, ornate |
| Restaurant / Food | Warm display font | Appetite-driven, warm colors, imagery-heavy | Cold, corporate |
| Medical / Health | Clean sans-serif | Trust-building, clean, accessible | Aggressive, dark themes |
| Creative / Agency | Experimental | Bold choices, break rules | Safe and generic |
| Legal / Finance | Conservative serif or sans-serif | Professional, restrained | Playful, casual |

**Color system (MANDATORY ORDER):**

1. **First choice**: Use the EXACT colors extracted from the business's existing website CSS
2. **Second choice**: Derive from the logo colors (download the logo, view it, pick the dominant colors)
3. **Fallback**: If extraction fails (external CSS not accessible, colors embedded in images), use the industry fallback palettes below

**Industry fallback color palettes (use ONLY if steps 1-2 fail):**

| Industry | Primary | Accent | Dark | Light BG |
|----------|---------|--------|------|----------|
| BTP / Construction | `#2B2B2B` | `#E8731A` (orange) | `#1A1A1A` | `#F7F6F3` |
| Restaurant / Food | `#2C1810` | `#C4513A` (warm red) | `#1A0F0A` | `#FBF8F4` |
| Tech / SaaS | `#0F172A` | `#3B82F6` (blue) | `#020617` | `#F8FAFC` |
| Medical / Health | `#1E293B` | `#0D9488` (teal) | `#0F172A` | `#F0FDFA` |
| Luxury / High-end | `#1C1917` | `#A16207` (gold) | `#0C0A09` | `#FAFAF9` |
| Creative / Agency | `#18181B` | `#8B5CF6` (violet) | `#09090B` | `#FAFAFA` |
| Legal / Finance | `#1E293B` | `#1D4ED8` (navy blue) | `#0F172A` | `#F8FAFC` |
| Beauty / Wellness | `#292524` | `#DB2777` (rose) | `#1C1917` | `#FDF2F8` |
| Real Estate | `#1E293B` | `#059669` (emerald) | `#0F172A` | `#F0FDF4` |
| Education | `#1E293B` | `#7C3AED` (purple) | `#0F172A` | `#FAF5FF` |

Define all colors as CSS custom properties. Ensure 4.5:1 contrast ratio minimum.

**Typography:**
- One font family is fine (e.g., DM Sans for both display and body)
- Weight and size create hierarchy, not font pairing
- NEVER use Inter, Roboto, Arial, or system fonts

### Phase 4: Implementation

**Homepage hero: ALWAYS full-screen photo background**

The hero section MUST use a real photo from the business as a full-width background image with a dark gradient overlay for text readability. Never use split layouts with solid color panels or abstract geometric shapes.

```css
/* Correct hero pattern */
.hero {
  min-height: 100vh;
  background: url('images/hero.jpg') center/cover no-repeat;
  position: relative;
}
.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(to right, rgba(X,X,X,0.9) 0%, rgba(X,X,X,0.4) 100%);
}
```

**Project directory structure:**

```
{business-name}-website/
  index.html
  about.html
  services.html
  contact.html
  [additional-pages].html
  css/
    style.css
    animations.css
  js/
    main.js
    navigation.js
  images/
    hero.jpg          (downloaded from business site)
    logo.png          (downloaded from business site)
    project1.jpg      (downloaded from business site)
    ...
```

**Technical requirements:**

- Pure HTML5 / CSS3 / Vanilla JS (no frameworks, no build step)
- Fully responsive (mobile-first approach)
- Mobile hamburger menu with slide-in animation
- Sticky navbar with backdrop blur on scroll
- Scroll-triggered animations (intersection observer)
- Contact form with client-side validation
- Google Maps embed (if address available)
- SEO meta tags (title, description, Open Graph)
- GEO optimization (structured data, schema.org markup, clear content hierarchy) so the business is referenced by AI search engines (ChatGPT, Perplexity, Gemini) in addition to Google
- CSS variables for easy theme customization
- Semantic HTML with proper heading hierarchy
- ARIA labels on interactive elements

**Design quality standards:**

- Agency-level polish - every pixel intentional
- Micro-interactions on buttons, links, cards (hover, focus states)
- Scroll reveal animations
- Consistent spacing system (8px grid)
- Real photos from the business, not placeholders
- Custom selection colors matching the brand
- Styled scrollbar (webkit)

**Copywriting rules:**

- Match the tone to the industry:
  - BTP/Construction: Direct, concrete, factual. "28 ans. 500 chantiers. Zero malfacon." NOT "Votre facade merite l'excellence"
  - Luxury: Refined, measured, understated
  - Tech: Clear, benefit-focused, modern
  - Restaurant: Warm, appetite-driven, inviting
- Write in the business's language (French for French businesses)
- Use real business data (hours, address, phone, services, certifications)
- Mark only truly missing content with [PLACEHOLDER]

### Phase 5: Quality Check

After generating all files:

1. Verify all internal links work between pages
2. Open the site in the browser with `open index.html`
3. Ensure consistent design language across all pages
4. Verify real photos are properly displayed
5. Check that brand colors match the original site
6. Test navigation (mobile menu, active states)

### Phase 6: Contact Form — Email Setup Guide

After generating the site, create a `GUIDE-EMAIL.md` file at the root of the project directory with clear, beginner-friendly instructions to receive contact form submissions by email.

**The guide MUST include these 3 options, ranked from easiest to most advanced:**

**Option 1 — Formspree (recommandé, 0 code)**
1. Aller sur https://formspree.io et créer un compte gratuit
2. Créer un nouveau formulaire → copier l'endpoint (ex: `https://formspree.io/f/xABcdEfG`)
3. Dans `contact.html`, remplacer `action="#"` par `action="https://formspree.io/f/VOTRE_ID"`
4. Remplacer `method="POST"` si ce n'est pas déjà le cas
5. C'est tout — les messages arrivent par email
- Gratuit : 50 soumissions/mois
- Payant : illimité à partir de 10$/mois

**Option 2 — Netlify Forms (si hébergé sur Netlify)**
1. Ajouter `netlify` dans la balise `<form>` : `<form name="contact" netlify>`
2. Déployer sur Netlify (drag & drop du dossier)
3. Les soumissions apparaissent dans le dashboard Netlify + notification email configurable
- Gratuit : 100 soumissions/mois

**Option 3 — EmailJS (envoi direct depuis le navigateur)**
1. Créer un compte sur https://www.emailjs.com
2. Connecter son email (Gmail, Outlook, etc.)
3. Créer un template d'email
4. Copier les 3 IDs (service, template, public key)
5. Ajouter le script dans `contact.html` (fournir le code exact à copier-coller)
- Gratuit : 200 emails/mois

**The guide must be written in French, with screenshots-style step descriptions, and assume the reader has zero technical knowledge.**

## Important Notes

- **USE REAL BRAND COLORS** - Fetch them from the existing site CSS. Never invent a color palette.
- **DOWNLOAD REAL PHOTOS** - Use curl to save images locally. No placeholder SVGs when photos exist.
- **MATCH THE INDUSTRY TONE** - A construction company is not a boutique. A restaurant is not a law firm. The design must feel right for the sector.
- **HERO = FULL PHOTO** - Always full-screen background image with gradient overlay. No split layouts, no solid color backgrounds, no abstract shapes.
- **COPY = INDUSTRY-APPROPRIATE** - Direct and concrete for BTP. Refined for luxury. Warm for food. Never generic.
- NEVER produce a generic template. Every site must feel custom-designed.
- The site should look like a 3000-5000 EUR agency deliverable.
- All text must match the business's language and locale.
- If the user provides a logo, use it. If not, use a text-based logo.
