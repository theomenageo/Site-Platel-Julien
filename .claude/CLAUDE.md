# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Mémoire de référence pour Claude sur le projet **Platel Corporation** : contexte client, DA V3, état du prototype, conventions, et architecture des maquettes.

---

## Contexte projet

**Client final :** **Platel Corporation** — marque parente de Julien Platel (Nice). Site vitrine qui regroupe **trois rubriques** :
1. **Coaching** — cours, stages et préparation compétition (pickleball & tennis)
2. **Événements** — séjours pickleball Côte d'Azur, tournois, animations clubs et corporate
3. **Diadem France** — distribution exclusive France & Monaco de la marque US Diadem Sports (lien sortant vers `diademsports.com`)

**Interlocuteur client :** Julien Platel, basé à Nice. Ancien joueur de tennis devenu pickleballeur pro. Coach et manager de son frère (top FR pickleball).

**Donneur d'ordre :** Théo (web designer freelance), travaille sous la responsabilité de son boss qui gère la relation client. Toute livraison passe par une **validation interne avant présentation au client**.

**Nature du site :** **Vitrine pure** — pas de e-commerce. Les CTA Diadem renvoient en externe vers `diademsports.com` ; **ne jamais créer de page produit interne**.

**Langue de travail :** Français, registre familier / peer-to-peer.

> Historique : V1 et V2 traitaient le site comme « Diadem France isolé ». Depuis la V3, le scope s'élargit à Platel Corporation (marque parente). V1/V2 sont conservés sur disque comme référence design / historique de propositions, mais **tout nouveau travail se fait dans V3**.

---

## Direction créative (V3 — canonique)

Style : **bold sport** — accent lime énergique sur fond clair ou ink, italique éditorial Fraunces pour les accents.

### Typographies (Google Fonts)
- **Barlow Condensed** (500–900) — titres, display, eyebrows (`--f-display`)
- **DM Sans** (400–700) — corps de texte (`--f-body`)
- **Fraunces italic** (display 144, 400–500) — moments éditoriaux, accents signature (`--f-edito`, classe `.f-edito`)

### Palette V3 (CSS custom properties dans `:root`)
| Token | Code | Rôle |
|-------|------|------|
| `--lime` | `#AFD639` | **Couleur primaire**, accents brand, highlights, btn-lime |
| `--lime-hover` | `#AEDB2B` | Hover lime |
| `--lime-blend` | `#b7e349` | Variant blend |
| `--lime-text` | `#4d7811` | Lime sombre pour texte sur fond clair (contraste AA OK) |
| `--orange` | `#ec9937` | Signal urgence / promo (distinct du lime brand) |
| `--orange-deep` | `#c97a1c` | Orange hover |
| `--ink` | `#0F0F0F` | Texte principal, fonds sombres |
| `--ink-soft` | `#1c1c1c` | Variant ink |
| `--text` | `#656565` | Texte gris secondaire |
| `--text-light` | `#898989` | Texte gris clair |
| `--paper` | `#F6F5F9` | Fond principal (blanc cassé) |
| `--white` | `#FFFFFF` | Blanc pur |
| `--border` | `#E6E4E0` | Séparateurs |
| `--dark` | `#121212` | Footer |
| `#008C95` (inline) | — | **Teal Diadem** conservé spécifiquement pour les CTA / blocs Diadem (signature de la marque US, distincte du brand Platel) |

> Les V1/V2 utilisaient une palette monochrome blanc + teal Diadem (`--electric` `#008C95`). Cette charte est **obsolète depuis la V3** — ne pas la reprendre sauf pour les CTA Diadem explicites.

---

## Architecture des maquettes

Pages HTML **autonomes** à la racine de chaque version, chacune embarque son propre `<style>` complet (pas de CSS partagé, pas de build, pas de package manager).

```
.
├── V1/                    # Archive — 3 pages, palette teal Diadem (~3180 lignes total)
│   ├── index.html
│   ├── evenements.html
│   └── produits.html
├── V2/                    # Archive — 4 pages allégées, même palette teal
│   ├── index.html
│   ├── evenements.html
│   ├── produits.html
│   └── contact.html
├── V3/                    # ✅ Version actuelle — Platel Corporation
│   ├── index.html         # ~1006 lignes, page unique pour le moment
│   └── assets/            # Assets spécifiques V3 (logo PCORP, hero Diadem)
├── assets/                # Pool partagé (photos, logos historiques, favicon)
├── prez-maxime.md         # Brief de validation interne (comparaison V1 vs V2)
└── .claude/CLAUDE.md      # Ce fichier
```

### Structure de V3 (`V3/index.html`)
1. `.promo` — bandeau ink avec badge orange "Dernière place" + lien réservation
2. `nav.nav` — sticky white, logo PCORP, CTA Diadem (teal) + CTA Contact (lime)
3. `.hero` — fond ink, 3 blocs services en grille (`.hero-blocks-solo`) :
   - Bloc 01 : **Coaching** (photo bg + accent lime)
   - Bloc 02 : **Organisation d'événements** (photo bg + accent lime)
   - Bloc 03 : **Produits Diadem** (`.hero-block-diadem`, accent teal `#008C95`, lien externe `diademsports.com`)
4. `.intro` — split « À propos », titre avec highlight lime + Fraunces italic, signature Julien
5. `.contact-cta` — fond ink, gros titre, CTA mailto + Instagram, panneau info à droite
6. `.footer` — 4 colonnes (brand / Nos univers / Pratique / Légal), beaucoup d'items marqués `.muted` "bientôt"

### Conventions de code (V3)

- **HTML5 + CSS inline** dans `<style>` — aucun fichier externe, aucun framework, aucun build
- **Composants** = classes BEM-souples (`.hero-block`, `.hb-photo`, `.btn-lime`, `.btn-diadem`, `.btn-ghost-light`)
- **Italique signature** : classe `.f-edito` (Fraunces italic) à utiliser ponctuellement dans les titres pour ramener la touche éditoriale héritée des V1/V2
- **Photos de fond** : pattern `style="--hb-photo: url('...')"` consommé par la classe `.hb-photo`. Images stockées dans `assets/` (racine) ou `V3/assets/`. Encoder les espaces et `©` dans les URLs (`%C2%A9-ROMAIN-BODEREAU...`)
- **Liens Diadem** : toujours `target="_blank" rel="noopener"` vers `diademsports.com` — pas de page produit interne
- **Liens internes** : ancres `#coaching`, `#evenements`, `#contact` sur la page unique
- **Animations** : `pulse` (badge promo), `reveal` + IntersectionObserver pour le scroll-in
- **Accessibilité** : `h1` masqué en `.sr-only`, focus visible lime (`:focus-visible`), `prefers-reduced-motion` respecté, `aria-label` sur nav et sections, `aria-hidden="true"` sur les flèches décoratives
- **Responsive** : 3 breakpoints (`1024px`, `900px`, `720px`) — la grille `hero-blocks-solo` passe en colonne unique, le btn-diadem se masque entre 900–1023 (accessible via bloc 03)

### Cohérence multi-fichiers (V3)

Pour l'instant V3 n'a qu'`index.html`. Quand de nouvelles pages V3 seront créées (coaching, événements, contact dédiés), **répercuter** dans chaque fichier les composants partagés : `.promo`, `nav`, `footer`, `.btn-*`, palette, `.f-edito`. Il n'y a pas de source unique CSS.

### TODOs visibles dans `V3/index.html`
- Confirmer date séjour + statut "dernière place" du bandeau promo
- Ajouter vrai numéro tel pour activer le lien
- Créer pages légales (mentions / CGV / confidentialité — obligatoire RGPD/LCEN avant mise en ligne)

---

## Commandes

Pas de build, pas de package manager. Pour prévisualiser :

```powershell
# Serveur statique Python (PowerShell, depuis la racine du projet)
python -m http.server 8000
# puis http://localhost:8000/V3/index.html
```

Ouvrir un fichier en double-clic fonctionne aussi (file://) — Google Fonts charge quand même.

Pour comparer V1/V2/V3 côte à côte (workflow décrit dans `prez-maxime.md`) :
```
http://localhost:8000/V1/index.html
http://localhost:8000/V2/index.html
http://localhost:8000/V3/index.html
```

---

## Roadmap

1. **Validation interne** V3 avec le boss de Théo
2. **Présentation visio** à Julien Platel
3. **Compléter V3** : pages dédiées coaching / événements / contact si validé (ou rester en one-pager)
4. **Build production en Webflow** (~25€/mois — recommandé) avec assets et contenus définitifs
5. WordPress conservé comme **fallback CMS**

### Features à pitcher
- Carte interactive des **revendeurs Diadem en France** (héritée de V1)
- Programmes **ambassadeurs** et **clubs partenaires**
- Page éducative **pickleball** (objectif SEO)
- Intégration **newsletter**

---

## Conventions et préférences

- Échanges en **français**, ton détendu peer-to-peer
- Toujours **passer par validation interne** avant tout livrable client
- Site **vitrine pur** — ne jamais proposer de fonctionnalités e-commerce ni de catalogue produit interne
- Tous les liens produits sortent vers `diademsports.com`
- Tenir la direction artistique **bold sport / lime V3** sur toute nouvelle page — **ne pas régresser sur la palette teal des V1/V2**
- Le teal Diadem `#008C95` reste autorisé uniquement pour les éléments explicitement Diadem (bloc 03, btn-diadem)
- Respecter la palette et les typos V3 ci-dessus (Barlow Condensed / DM Sans / Fraunces italic)
- Si une nouvelle page V3 est créée : répercuter `.promo`, nav, footer, `.btn-*`, palette en cohérence (pas de source unique)
- Préserver les `aria-label`, focus visible, `sr-only`, et `prefers-reduced-motion` — l'accessibilité a été soignée en V3

---

## Ressources

- Marque US : [diademsports.com](https://diademsports.com)
- Instagram Diadem France (référence visuelle)
- Unsplash CDN — placeholders en phase mockup uniquement (V1/V2)
- Webflow — CMS de production recommandé
- WordPress — CMS de secours
- `prez-maxime.md` à la racine — brief de validation interne (comparaison V1 vs V2, utile pour comprendre l'historique des choix DA)
