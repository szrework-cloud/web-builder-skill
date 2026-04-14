# Web Builder (Next.js) — Claude Code Skill

Skill pour [Claude Code](https://docs.anthropic.com/en/docs/claude-code) qui genere des sites web professionnels multi-pages avec **Next.js 15**, TypeScript et Tailwind CSS pour des entreprises reelles.

Donnez-lui un lien Google, une URL de site ou un nom d'entreprise — il fait le reste : extraction des infos, telechargement des photos, design adapte au secteur, SEO natif, deploy-ready sur Vercel.

## Ce que ca genere

- **4+ pages** : Accueil, Services/Carte, A propos, Contact (+ pages specifiques par industrie)
- **Next.js 15** App Router avec TypeScript strict
- **Tailwind CSS 4** pour le styling
- **Photos reelles** telechargees depuis le site de l'entreprise
- **Couleurs de marque** extraites du CSS ou du logo (+ fallback par industrie)
- **Responsive** mobile-first avec menu hamburger anime
- **SEO natif** : `generateMetadata`, `robots.ts`, `sitemap.ts`, Schema.org JSON-LD, Open Graph
- **next/image** pour l'optimisation automatique des images
- **next/font** pour les Google Fonts sans CLS
- **Framer Motion** pour les animations au scroll
- **Formulaire de contact** avec Formspree
- **Static export** (`output: 'export'`) — deploy anywhere, zero serveur
- **GUIDE-DEPLOY.md** : guide debutant Vercel + domaine + Search Console

## Installation

```bash
# Cloner dans le dossier skills de Claude Code
git clone https://github.com/szrework-cloud/web-builder-skill.git ~/.claude/skills/web-builder
```

C'est tout. Le skill est disponible immediatement dans Claude Code.

## Utilisation

Dans Claude Code, tapez :

```
/web-builder https://www.example.com
```

Ou avec un lien Google Business / Google Maps :

```
/web-builder https://www.google.fr/maps/place/Mon+Entreprise
```

Ou simplement :

```
/web-builder Boulangerie Dupont, 12 rue des Lilas, Lyon
```

## Secteurs supportes

Le skill adapte automatiquement le design (typo, ton, couleurs) au secteur :

| Secteur | Style | Palette fallback |
|---------|-------|-----------------|
| BTP / Construction | Bold, industriel, uppercase | Charbon + Orange |
| Restaurant / Food | Chaleureux, appetissant | Brun + Rouge chaud |
| Tech / SaaS | Clean, geometrique | Navy + Bleu |
| Medical / Sante | Confiance, accessible | Slate + Teal |
| Luxe / Haut de gamme | Raffine, epure | Noir + Or |
| Creative / Agence | Experimental, bold | Noir + Violet |
| Juridique / Finance | Conservateur, pro | Navy + Bleu fonce |
| Beaute / Bien-etre | Doux, elegant | Brun + Rose |
| Immobilier | Pro, moderne | Navy + Emeraude |
| Education | Accessible, clair | Navy + Violet |

## Stack technique

| Tech | Role |
|------|------|
| Next.js 15 (App Router) | Framework, SSG, routing |
| TypeScript | Typage strict |
| Tailwind CSS 4 | Styling utility-first |
| Framer Motion | Animations scroll |
| next/image | Optimisation images |
| next/font | Google Fonts sans CLS |

## Structure du site genere

```
mon-entreprise-website/
  next.config.ts
  tailwind.config.ts
  package.json
  public/
    images/
      hero.jpg
      logo.png
      ...
    favicon.svg
  src/
    app/
      layout.tsx            <- Root layout (fonts, metadata, nav, footer)
      page.tsx              <- Homepage + JSON-LD
      a-propos/page.tsx
      services/page.tsx
      contact/page.tsx
      robots.ts             <- robots.txt natif
      sitemap.ts            <- sitemap.xml natif
      not-found.tsx         <- 404 custom
    components/
      navbar.tsx
      footer.tsx
      hero.tsx
      scroll-reveal.tsx
      contact-form.tsx
      section-heading.tsx
    lib/
      constants.ts          <- Donnees business centralisees
  GUIDE-DEPLOY.md           <- Guide deploy Vercel + domaine + email
```

## Fichiers du skill

```
~/.claude/skills/web-builder/
  SKILL.md                          <- Instructions du skill
  references/
    page-structures.md              <- Structures de pages par industrie
```

## Prerequis

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installe
- Node.js 18+ (pour `npx create-next-app`)
- Modele : Claude Opus (configure automatiquement dans le skill)
- Skill recommande : `ui-ux-pro-max` (pour palettes, typos et guidelines UX avancees)

## License

MIT
