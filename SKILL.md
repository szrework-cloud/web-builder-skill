---
name: web-builder
model: opus
description: "Generate professional, multi-page Next.js websites for real businesses. This skill should be used when the user provides a business context (Google page URL, business name, industry) and wants a complete website generated. Extracts business information, downloads real photos and logos, applies the ACTUAL brand colors from the existing site, and produces a full Next.js (App Router) website with TypeScript, Tailwind CSS, SSG, built-in SEO/GEO, and agency-level quality. Triggers on: 'create a website for', 'build a site for', 'generate a website', 'web-builder', or any request to create a business website from a Google listing or business description."
---

# Web Builder (Next.js)

Generate professional, multi-page business websites using **Next.js 15 (App Router)** with TypeScript and Tailwind CSS. Accept a business website URL, Google Business page, or business description as input, extract all relevant information, and produce a complete, polished, responsive, SEO-optimized website ready for Vercel deployment.

## Workflow

### Phase 1: Business Intelligence Gathering

**Step 1 - Fetch the business website** (CRITICAL):

If a website URL is provided, fetch it to extract:
- Business name, industry, services, address, phone, email, hours
- **EXACT color codes from the CSS** (hex values, CSS variables) - NEVER guess colors
- All image URLs (logo, project photos, team photos, banners)
- Reviews, certifications, partnerships

**Step 2 - Download all assets:**

Download to `public/images/` directory using curl:
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
1. **Accueil** (Homepage) `/` - Hero, services overview, testimonials, CTA
2. **A propos** (About) `/a-propos` - Story, team, values
3. **Services** (Services) `/services` - Detailed service offerings
4. **Contact** `/contact` - Form, map, phone, hours

**Optional pages (based on industry):**
- **Realisations / Portfolio** `/realisations` - For artisans, construction, agencies, photographers
- **Menu / Carte** `/menu` - For restaurants, bars, cafes
- **Tarifs** `/tarifs` - For services with pricing
- **Blog / Actualites** `/blog` - If the business has content to showcase
- **FAQ** `/faq` - For businesses with common questions

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

Colors are defined as CSS variables in `tailwind.config.ts` and `globals.css`. Ensure 4.5:1 contrast ratio minimum.

**Typography:**
- Use Google Fonts via `next/font/google` (automatic optimization, no CLS)
- One font family is fine (e.g., DM Sans for both display and body)
- Weight and size create hierarchy, not font pairing
- NEVER use Inter, Roboto, Arial, or system fonts

### Phase 4: Implementation

**Tech stack:**
- **Next.js 15** (App Router, `output: 'export'` for static export)
- **TypeScript** (strict mode)
- **Tailwind CSS 4** (utility-first)
- **next/image** (automatic optimization, WebP/AVIF, lazy loading)
- **next/font** (zero CLS Google Fonts)
- **Framer Motion** (scroll animations, page transitions)

**Initialize the project:**

```bash
npx create-next-app@latest {business-name}-website --typescript --tailwind --app --src-dir --no-eslint --no-import-alias
cd {business-name}-website
npm install framer-motion
```

Then replace generated files with the custom implementation.

**Homepage hero: ALWAYS full-screen photo background**

The hero section MUST use a real photo from the business as a full-width background with a dark gradient overlay for text readability. Use `next/image` with `fill` and `priority`:

```tsx
import Image from 'next/image'

function Hero() {
  return (
    <section className="relative min-h-screen flex items-center">
      <Image
        src="/images/hero.jpg"
        alt="Description"
        fill
        className="object-cover"
        priority
        quality={85}
      />
      <div className="absolute inset-0 bg-gradient-to-r from-black/90 to-black/40" />
      <div className="relative z-10 container mx-auto px-6">
        <h1 className="text-5xl font-bold text-white">...</h1>
      </div>
    </section>
  )
}
```

**Project directory structure:**

```
{business-name}-website/
  next.config.ts
  tailwind.config.ts
  tsconfig.json
  package.json
  public/
    images/
      hero.jpg            (downloaded from business site)
      logo.png            (downloaded from business site)
      project1.jpg        (downloaded from business site)
      ...
    favicon.svg           (generated: text-based SVG using brand accent color + first letter)
  src/
    app/
      layout.tsx          (root layout: fonts, metadata, navbar, footer)
      page.tsx            (homepage)
      a-propos/
        page.tsx
      services/
        page.tsx
      contact/
        page.tsx
      [additional-pages]/
        page.tsx
      robots.ts           (generated robots.txt)
      sitemap.ts          (generated sitemap.xml)
      not-found.tsx       (custom 404)
    components/
      navbar.tsx          (sticky, blur, mobile menu)
      footer.tsx
      hero.tsx
      section-heading.tsx
      contact-form.tsx
      scroll-reveal.tsx   (framer motion wrapper)
      [page-specific components]
    lib/
      constants.ts        (business info: name, address, phone, hours, etc.)
  GUIDE-DEPLOY.md
```

**SEO & Metadata (MANDATORY — Next.js native):**

Every page exports metadata using Next.js `generateMetadata` or static `metadata`:

```tsx
// src/app/layout.tsx
import type { Metadata } from 'next'

export const metadata: Metadata = {
  metadataBase: new URL('https://www.DOMAIN.com'),
  title: {
    default: 'Business Name — Tagline + Location',
    template: '%s | Business Name',
  },
  description: 'Main site description (150-160 chars)',
  openGraph: {
    type: 'website',
    locale: 'fr_FR',
    siteName: 'Business Name',
    images: [{ url: '/images/og-image.jpg', width: 1200, height: 630 }],
  },
  alternates: {
    canonical: 'https://www.DOMAIN.com',
  },
}
```

```tsx
// src/app/services/page.tsx
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'Nos Services',
  description: 'Page-specific description (150-160 chars)',
  alternates: {
    canonical: 'https://www.DOMAIN.com/services',
  },
}
```

**robots.ts (MANDATORY):**

```tsx
// src/app/robots.ts
import type { MetadataRoute } from 'next'

export default function robots(): MetadataRoute.Robots {
  return {
    rules: { userAgent: '*', allow: '/' },
    sitemap: 'https://www.DOMAIN.com/sitemap.xml',
  }
}
```

**sitemap.ts (MANDATORY):**

```tsx
// src/app/sitemap.ts
import type { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
  const baseUrl = 'https://www.DOMAIN.com'
  return [
    { url: baseUrl, lastModified: new Date(), priority: 1.0 },
    { url: `${baseUrl}/services`, lastModified: new Date(), priority: 0.8 },
    { url: `${baseUrl}/realisations`, lastModified: new Date(), priority: 0.7 },
    { url: `${baseUrl}/a-propos`, lastModified: new Date(), priority: 0.6 },
    { url: `${baseUrl}/contact`, lastModified: new Date(), priority: 0.8 },
  ]
}
```

**Schema.org JSON-LD (MANDATORY on homepage):**

```tsx
// In src/app/page.tsx
export default function Home() {
  const jsonLd = {
    '@context': 'https://schema.org',
    '@type': 'LocalBusiness',
    name: 'Business Name',
    telephone: '+33XXXXXXXXX',
    address: {
      '@type': 'PostalAddress',
      streetAddress: '...',
      addressLocality: '...',
      postalCode: '...',
      addressCountry: 'FR',
    },
    geo: {
      '@type': 'GeoCoordinates',
      latitude: XX.XXXX,
      longitude: XX.XXXX,
    },
    url: 'https://www.DOMAIN.com',
    areaServed: '...',
  }

  return (
    <>
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLd) }}
      />
      {/* Page content */}
    </>
  )
}
```

**Heading hierarchy:** One `<h1>` per page, `<h2>` for sections, `<h3>` for subsections. Descriptive `alt` on ALL images.

**Favicon (MANDATORY):**

Generate an SVG favicon using the brand accent color and the first letter of the business name:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <rect width="100" height="100" rx="12" fill="ACCENT_COLOR"/>
  <text x="50" y="68" font-family="sans-serif" font-size="60" font-weight="800"
        fill="white" text-anchor="middle">X</text>
</svg>
```

Link in layout.tsx: `icons: { icon: '/favicon.svg' }` inside the metadata export.

**Image optimization (next/image handles it):**

- Use `<Image>` from `next/image` everywhere (automatic WebP/AVIF, lazy loading, responsive sizes)
- Set `priority` on above-the-fold images (hero, logo in navbar)
- Always provide `width`/`height` or use `fill` with a sized container
- Descriptive `alt` text on every image

**Static export configuration:**

```ts
// next.config.ts
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  output: 'export',
  images: {
    unoptimized: true, // Required for static export
  },
}

export default nextConfig
```

Note: With `output: 'export'`, `next/image` still provides proper `<img>` tags with `srcset` and `loading="lazy"`, but server-side optimization is disabled. Images are served as-is, so download quality photos and compress them with `cwebp` or `sips` before placing in `public/images/`.

**Scroll animations (Framer Motion):**

Create a reusable `ScrollReveal` component:

```tsx
'use client'
import { motion } from 'framer-motion'
import { ReactNode } from 'react'

export function ScrollReveal({ children, className }: { children: ReactNode; className?: string }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 30 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, margin: '-100px' }}
      transition={{ duration: 0.6, ease: 'easeOut' }}
      className={className}
    >
      {children}
    </motion.div>
  )
}
```

**Contact form:**

Use a client component with form validation. The form action points to Formspree (configured post-deploy):

```tsx
'use client'
import { useState, FormEvent } from 'react'

export function ContactForm() {
  const [status, setStatus] = useState<'idle' | 'sending' | 'sent' | 'error'>('idle')

  async function handleSubmit(e: FormEvent<HTMLFormElement>) {
    e.preventDefault()
    setStatus('sending')
    const form = e.currentTarget
    const data = new FormData(form)

    try {
      const res = await fetch(form.action, {
        method: 'POST',
        body: data,
        headers: { Accept: 'application/json' },
      })
      if (res.ok) {
        setStatus('sent')
        form.reset()
      } else {
        setStatus('error')
      }
    } catch {
      setStatus('error')
    }
  }

  return (
    <form action="https://formspree.io/f/VOTRE_ID" method="POST" onSubmit={handleSubmit}>
      {/* form fields */}
    </form>
  )
}
```

**Design quality standards:**

- Agency-level polish - every pixel intentional
- Micro-interactions on buttons, links, cards (Tailwind hover/focus utilities + Framer Motion)
- Scroll reveal animations via `ScrollReveal` component
- Consistent spacing system (Tailwind default 4px/8px grid)
- Real photos from the business, not placeholders
- Custom selection colors matching the brand (`::selection` in globals.css)

**Copywriting rules:**

- Match the tone to the industry:
  - BTP/Construction: Direct, concrete, factual. "28 ans. 500 chantiers. Zero malfacon." NOT "Votre facade merite l'excellence"
  - Luxury: Refined, measured, understated
  - Tech: Clear, benefit-focused, modern
  - Restaurant: Warm, appetite-driven, inviting
- Write in the business's language (French for French businesses)
- Use real business data (hours, address, phone, services, certifications)
- Mark only truly missing content with [PLACEHOLDER]

### Phase 5: Build & Quality Check

After generating all files:

1. Run `npm run build` to verify the project compiles without errors
2. Run `npx serve out` (or `npm run dev`) to preview
3. Open the site in the browser
4. Verify all internal links work between pages
5. Ensure consistent design language across all pages
6. Verify real photos are properly displayed
7. Check that brand colors match the original site
8. Test navigation (mobile menu, active states)
9. Test responsive design (mobile, tablet, desktop)

### Phase 6: Deployment Guide

After generating the site, create a `GUIDE-DEPLOY.md` file at the root of the project directory with clear, beginner-friendly instructions.

**The guide MUST include these sections:**

**1. Deployer sur Vercel (recommande, gratuit)**
1. Aller sur https://vercel.com et connecter son compte GitHub
2. Importer le repo
3. Vercel detecte Next.js automatiquement — cliquer Deploy
4. Le site est live en 30 secondes avec HTTPS et CDN mondial
5. Chaque push sur `main` redeploy automatiquement

**2. Configurer le nom de domaine**
1. Dans Vercel > Settings > Domains
2. Ajouter son domaine (ex: `www.monentreprise.com`)
3. Modifier les DNS chez son registrar (pointer vers Vercel)
4. SSL automatique

**3. Recevoir les emails du formulaire de contact**
- **Option A — Formspree (0 code)**: Creer un compte sur formspree.io, copier l'endpoint, remplacer `VOTRE_ID` dans `contact-form.tsx`
- **Option B — Netlify Forms**: Si deploye sur Netlify au lieu de Vercel
- **Option C — EmailJS**: Envoi direct depuis le navigateur

**4. Google Search Console**
1. Aller sur https://search.google.com/search-console
2. Ajouter la propriete (methode DNS ou balise HTML)
3. Soumettre le sitemap : `https://www.DOMAIN.com/sitemap.xml`
4. Attendre l'indexation (quelques jours)

**5. Google Business Profile**
1. Sur https://business.google.com, ajouter l'URL du site
2. Verifier que les infos (adresse, horaires, telephone) correspondent au site

**The guide must be written in French and assume the reader has zero technical knowledge.**

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
- **ALWAYS use `output: 'export'`** for static generation — no server needed, deploy anywhere.
