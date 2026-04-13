# Web Builder — Claude Code Skill

Skill pour [Claude Code](https://docs.anthropic.com/en/docs/claude-code) qui genere des sites web professionnels multi-pages pour des entreprises reelles.

Donnez-lui un lien Google, une URL de site ou un nom d'entreprise — il fait le reste : extraction des infos, telechargement des photos, design adapte au secteur, code complet HTML/CSS/JS.

## Ce que ca genere

- **5+ pages** : Accueil, Services, Realisations, A propos, Contact
- **Photos reelles** telechargees depuis le site de l'entreprise
- **Couleurs de marque** extraites du CSS ou du logo (+ fallback par industrie)
- **Responsive** mobile-first avec menu hamburger
- **SEO** complet (meta tags, Open Graph, schema.org)
- **Formulaire de contact** avec validation + guide email (Formspree/Netlify/EmailJS)
- **Animations** scroll reveal, micro-interactions
- **0 framework** — HTML5 / CSS3 / Vanilla JS pur

## Installation

```bash
# Cloner dans le dossier skills de Claude Code
git clone https://github.com/AdemGur662/web-builder-skill.git ~/.claude/skills/web-builder
```

C'est tout. Le skill est disponible immediatement dans Claude Code.

## Utilisation

Dans Claude Code, tapez :

```
/web-builder https://www.example.com
```

Ou avec un lien Google Business :

```
/web-builder https://www.google.com/search?q=Mon+Entreprise
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

## Structure du site genere

```
mon-entreprise-website/
  index.html
  services.html
  realisations.html
  about.html
  contact.html
  GUIDE-EMAIL.md        <- Guide debutant pour recevoir les emails
  css/
    style.css
    animations.css
  js/
    main.js
    navigation.js
  images/
    hero.jpg
    logo.png
    projet1.jpg
    ...
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
- Modele : Claude Opus (configure automatiquement dans le skill)
- Skill recommande : `ui-ux-pro-max` (pour palettes, typos et guidelines UX avancees)

## License

MIT
