# Page Structures Reference (Next.js App Router)

## Standard Multi-Page Website Structure

### Homepage (`src/app/page.tsx`)
- Hero section with `next/image` fill + gradient overlay + headline + CTA
- Services/offerings overview (3-6 cards)
- Social proof (testimonials, client logos, stats)
- About teaser with `Link` to full page
- Recent work/portfolio preview
- Contact CTA section
- Schema.org `LocalBusiness` JSON-LD

### About Page (`src/app/a-propos/page.tsx`)
- Company story / mission
- Team section with photos and roles
- Values / approach
- Timeline / milestones (optional)
- Certifications / awards (if applicable)

### Services Page (`src/app/services/page.tsx`)
- Service categories with descriptions
- Pricing tiers (if applicable)
- Process / how it works section
- FAQ accordion (client component)
- CTA to contact

### Portfolio / Realisations (`src/app/realisations/page.tsx`)
- Filterable project grid (client component for filters)
- Project cards with `next/image`, title, category
- Individual project detail views (optional: `src/app/realisations/[slug]/page.tsx`)

### Contact Page (`src/app/contact/page.tsx`)
- `ContactForm` client component (name, email, phone, message, subject)
- Google Maps embed or address
- Business hours
- Phone, email, social links
- Optional: booking/appointment CTA

### Blog (`src/app/blog/page.tsx`) - Optional
- Article cards with `next/image` thumbnail, title, excerpt, date
- Categories / tags
- Individual posts: `src/app/blog/[slug]/page.tsx`

## Component Patterns

### Shared Components (`src/components/`)

**Navbar (`navbar.tsx`)**
- `'use client'` — needs state for mobile menu
- Logo left, links center or right
- Mobile hamburger menu with Framer Motion slide-in
- Sticky on scroll with `backdrop-blur` (Tailwind: `sticky top-0 backdrop-blur-md`)
- Active page indicator via `usePathname()`

**Footer (`footer.tsx`)**
- Multi-column: about, links, services, contact
- Social media icons
- Legal links (mentions legales, CGV, RGPD)
- Copyright with dynamic year

**ScrollReveal (`scroll-reveal.tsx`)**
- `'use client'` — Framer Motion `motion.div`
- `whileInView` with `viewport={{ once: true }}`
- Reusable wrapper for any section

**SectionHeading (`section-heading.tsx`)**
- Consistent heading style across pages
- Subtitle + accent line

**ContactForm (`contact-form.tsx`)**
- `'use client'` — form state management
- Client-side validation
- Formspree integration
- Status feedback (idle, sending, sent, error)

### Hero Variants
- **Full photo** (DEFAULT): `next/image` fill + gradient overlay + text
- **Centered**: Large headline centered, CTA below
- **Video background**: `<video>` with overlay text (optional)

### Testimonials
- Carousel with photo, quote, name, company (client component)
- Grid of testimonial cards
- Single featured quote with large typography

## Layout Structure

```
src/app/layout.tsx          → Root layout (fonts, metadata, Navbar, Footer)
  src/app/page.tsx          → Homepage
  src/app/a-propos/page.tsx → About
  src/app/services/page.tsx → Services
  src/app/contact/page.tsx  → Contact
  src/app/realisations/page.tsx → Portfolio (optional)
  src/app/menu/page.tsx     → Menu (restaurant only)
  src/app/tarifs/page.tsx   → Pricing (optional)
  src/app/blog/page.tsx     → Blog (optional)
  src/app/not-found.tsx     → Custom 404
  src/app/robots.ts         → robots.txt
  src/app/sitemap.ts        → sitemap.xml
```

## Industry-Specific Additions

### Restaurant / Food
- Menu section with categories and prices (`src/app/menu/page.tsx`)
- Reservation widget/link (external booking service)
- Photo gallery of dishes with `next/image`
- Opening hours prominently displayed

### Medical / Health
- Services with detailed descriptions
- Doctor/practitioner profiles
- Online appointment booking link
- Emergency contact info

### Real Estate
- Property listing grid with filters (client component)
- Property detail with gallery, specs, map (`src/app/biens/[slug]/page.tsx`)
- Agent contact cards

### Artisan / Trade
- Before/after gallery (client component with slider)
- Service area map
- Certifications and insurance info
- Quote request form

### E-commerce / Retail
- Product showcase
- Store locations
- Brand story
- Delivery/return info

### Tech / Startup / SaaS
- Feature comparison grid
- Integration logos
- Pricing table (client component with toggle monthly/yearly)
- Demo/trial CTA
