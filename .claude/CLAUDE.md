# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Mémoire de référence pour Claude sur le projet **Platel Corporation** : contexte client, DA V3/V4, état des prototypes, conventions, et architecture des maquettes.

---

## Contexte projet

**Client final :** **Platel Corporation** — marque parente de Julien Platel (Nice). Site vitrine qui regroupe **trois rubriques** :

1. **Coaching** — cours, stages et préparation compétition (pickleball uniquement — voir note de positionnement plus bas)
2. **Événements** — séjours pickleball Côte d'Azur, tournois, animations clubs et corporate
3. **Diadem France** — distribution exclusive France & Monaco de la marque US Diadem Sports (lien sortant vers `diademsports.com`)

**Interlocuteur client :** Julien Platel, basé à Nice. Pickleballeur pro. Coach et manager de son frère (top FR pickleball).

> **Positionnement (décision client 2026-05-27) :** le site ne mentionne **jamais le tennis** dans le copy public, même si Julien a un passé tennis. Toute landing, eyebrow, signature, footer, meta tag doit rester strictement "pickleball". Cette règle s'applique à tous les fichiers V4+ et aux versions futures.

**Donneur d'ordre :** Théo (web designer freelance), travaille sous la responsabilité de son boss qui gère la relation client. Toute livraison passe par une **validation interne avant présentation au client**.

**Nature du site :** **Vitrine pure** — pas de e-commerce. Les CTA Diadem renvoient en externe vers `diademsports.com` ; **ne jamais créer de page produit interne**.

**Langue de travail :** Français, registre familier / peer-to-peer.

> Historique : V1 et V2 traitaient le site comme « Diadem France isolé ». Depuis la V3, le scope s'élargit à Platel Corporation (marque parente). V1/V2 sont conservés dans `archives/` (gitignored) comme référence design ; **tout nouveau travail se fait dans V3 ou V4**.

---

## Direction créative (V3/V4 — canonique)

Style : **bold sport** — accent lime énergique sur fond clair ou ink, italique éditorial DM Sans pour les accents.

### Typographies (Google Fonts)

- **Barlow Condensed** (500–900) — titres, display, eyebrows (`--f-display`)
- **DM Sans** (400–700, italic 400–500) — corps de texte ET édito (`--f-body`, `--f-edito`)

> ⚠️ Fraunces a été retiré depuis. Le token `--f-edito` pointe maintenant vers DM Sans italic (cf. V3/V4 `:root`). Ne pas réintroduire Fraunces sans validation explicite.

### Palette V3/V4 (CSS custom properties dans `:root`)

| Token              | Code      | Rôle                                                                                                                       |
| ------------------ | --------- | -------------------------------------------------------------------------------------------------------------------------- |
| `--lime`           | `#AFD639` | **Couleur primaire**, accents brand, highlights, btn-lime                                                                  |
| `--lime-hover`     | `#AEDB2B` | Hover lime                                                                                                                 |
| `--lime-blend`     | `#b7e349` | Variant blend                                                                                                              |
| `--lime-text`      | `#4d7811` | Lime sombre pour texte sur fond clair (contraste AA OK)                                                                    |
| `--orange`         | `#ec9937` | Signal urgence / promo (distinct du lime brand)                                                                            |
| `--orange-deep`    | `#c97a1c` | Orange hover                                                                                                               |
| `--ink`            | `#0F0F0F` | Texte principal, fonds sombres                                                                                             |
| `--ink-soft`       | `#1c1c1c` | Variant ink                                                                                                                |
| `--text`           | `#656565` | Texte gris secondaire                                                                                                      |
| `--text-light`     | `#898989` | Texte gris clair                                                                                                           |
| `--paper`          | `#F6F5F9` | Fond principal (blanc cassé)                                                                                               |
| `--white`          | `#FFFFFF` | Blanc pur                                                                                                                  |
| `--border`         | `#E6E4E0` | Séparateurs                                                                                                                |
| `--dark`           | `#121212` | Footer                                                                                                                     |
| `#008C95` (inline) | —         | **Teal Diadem** conservé spécifiquement pour les CTA / blocs Diadem (signature de la marque US, distincte du brand Platel) |

> Les V1/V2 utilisaient une palette monochrome blanc + teal Diadem (`--electric` `#008C95`). Cette charte est **obsolète depuis la V3** — ne pas la reprendre sauf pour les CTA Diadem explicites.

---

## Architecture des maquettes

Pages HTML **autonomes**, chacune embarque son propre `<style>` complet (pas de CSS partagé, pas de build, pas de package manager).

```
.
├── archives/              # V1/V2 historiques (gitignored)
│   ├── V1/                # 3 pages, palette teal Diadem
│   └── V2/                # 4 pages allégées, même palette teal
├── V3/                    # ✅ Version one-pager — Platel Corporation
│   ├── index.html         # ~1179 lignes, page unique
│   └── assets/            # Assets spécifiques V3
├── V4/                    # ✅ Variante multi-pages — base V3 + contact dédié
│   ├── index.html         # ~1203 lignes
│   ├── contact.html       # ~1313 lignes — page contact standalone
│   └── assets/            # Assets propres à V4 (logos, photos, dont versions WebP)
├── assets/                # Pool partagé (gitignored sauf 3 photos pickleball whitelistées)
└── .claude/CLAUDE.md      # Ce fichier
```

> V4 est une déclinaison de V3 avec une **page contact dédiée** (au lieu de la simple section ancre `#contact` de V3). Les deux versions cohabitent pour l'instant — choisir laquelle pousser en validation reste à arbitrer avec le boss.

### Structure de V3 (`V3/index.html`)

1. `.promo` — bandeau ink avec badge orange "Dernière place" + lien réservation
2. `nav.nav` — sticky white, logo PCORP, CTA Diadem (teal) + CTA Contact (lime)
3. `.hero` — fond ink, 3 blocs services en grille (`.hero-blocks-solo`) :
   - Bloc 01 : **Coaching** (photo bg + accent lime)
   - Bloc 02 : **Organisation d'événements** (photo bg + accent lime)
   - Bloc 03 : **Produits Diadem** (`.hero-block-diadem`, accent teal `#008C95`, lien externe `diademsports.com`)
4. `.intro` — split « À propos », titre avec highlight lime + DM Sans italic, signature Julien
5. `.contact-cta` — fond ink, gros titre, CTA mailto + Instagram, panneau info à droite
6. `.footer` — 4 colonnes (brand / Nos univers / Pratique / Légal), beaucoup d'items marqués `.muted` "bientôt"

### Conventions de code (V3/V4)

- **HTML5 + CSS inline** dans `<style>` — aucun fichier externe, aucun framework, aucun build
- **Composants** = classes BEM-souples (`.hero-block`, `.hb-photo`, `.btn-lime`, `.btn-diadem`, `.btn-ghost-light`)
- **Italique signature** : classe `.f-edito` (DM Sans italic) à utiliser ponctuellement dans les titres pour ramener la touche éditoriale
- **Photos de fond** : pattern `style="--hb-photo: url('...')"` consommé par la classe `.hb-photo`. Encoder les espaces et `©` dans les URLs (`%C2%A9-ROMAIN-BODEREAU...`)
- **Liens Diadem** : toujours `target="_blank" rel="noopener"` vers `diademsports.com` — pas de page produit interne
- **Liens internes** : ancres `#coaching`, `#evenements`, `#contact` (sur la page unique V3 ; V4 a en plus `contact.html`)
- **Animations** : `pulse` (badge promo), `reveal` + IntersectionObserver pour le scroll-in
- **Accessibilité** : `h1` masqué en `.sr-only`, focus visible lime (`:focus-visible`), `prefers-reduced-motion` respecté, `aria-label` sur nav et sections, `aria-hidden="true"` sur les flèches décoratives
- **Responsive** : 3 breakpoints (`1024px`, `900px`, `720px`) — la grille `hero-blocks-solo` passe en colonne unique, le btn-diadem se masque entre 900–1023 (accessible via bloc 03)

### Cohérence multi-fichiers (V4)

V4 a maintenant 2 pages. Quand de nouvelles pages V4 sont créées (coaching, événements dédiés), **répercuter manuellement** dans chaque fichier les composants partagés : `.promo`, `nav`, `footer`, `.btn-*`, palette, `.f-edito`. Il n'y a pas de source unique CSS — chaque page duplique son `<style>`. Vérifier la parité visuelle entre `V4/index.html` et `V4/contact.html` à chaque modif de composant partagé.

### Assets et `.gitignore`

Le `.gitignore` filtre agressivement le dossier `assets/` racine : seules **3 photos pickleball** sont whitelistées (`1000026709-1-682x1024.jpg`, les deux `©-ROMAIN-BODEREAU...`). Toute autre image rangée dans `assets/` racine sera ignorée par git — préférer `V3/assets/` ou `V4/assets/` pour les nouveaux assets référencés.

`V1/`, `V2/`, et `prez-maxime.md` sont également gitignored (sources d'historique, déplacées dans `archives/`).

### TODOs visibles dans V3/V4

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
#      ou  http://localhost:8000/V4/index.html
```

Ouvrir un fichier en double-clic fonctionne aussi (file://) — Google Fonts charge quand même.

Pour comparer V3 et V4 côte à côte :

```
http://localhost:8000/V3/index.html
http://localhost:8000/V4/index.html
http://localhost:8000/V4/contact.html
```

---

## Roadmap

1. **Arbitrage V3 vs V4** (one-pager vs multi-pages avec contact dédié)
2. **Validation interne** avec le boss de Théo
3. **Présentation visio** à Julien Platel
4. **Compléter la version retenue** : pages dédiées coaching / événements si V4 validé
5. **Build production en Webflow** (~25€/mois — recommandé) avec assets et contenus définitifs
6. WordPress conservé comme **fallback CMS**

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
- Tenir la direction artistique **bold sport / lime V3-V4** sur toute nouvelle page — **ne pas régresser sur la palette teal des V1/V2**
- Le teal Diadem `#008C95` reste autorisé uniquement pour les éléments explicitement Diadem (bloc 03, btn-diadem)
- Respecter la palette et les typos V3/V4 ci-dessus (Barlow Condensed / DM Sans + DM Sans italic) — **ne pas réintroduire Fraunces** sans validation
- Si une nouvelle page V4 est créée : répercuter `.promo`, nav, footer, `.btn-*`, palette en cohérence (pas de source unique)
- Préserver les `aria-label`, focus visible, `sr-only`, et `prefers-reduced-motion` — l'accessibilité a été soignée

---

## Ressources

- Marque US : [diademsports.com](https://diademsports.com)
- Instagram Diadem France (référence visuelle)
- Webflow — CMS de production recommandé
- WordPress — CMS de secours
