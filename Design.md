# DESIGN.md — Portfolio Mattheus Germano

> À lire AVANT toute session de code. Ce fichier définit la direction artistique, la stack et les contraintes. Ne jamais dévier sans validation.

---

## 1. Concept directeur

**Portfolio d'un étudiant en informatique passionné de bio-informatique.**

L'univers visuel s'inspire de la **cellule, de l'ADN et de l'imagerie scientifique** (microscopie à fluorescence, structures protéiques, séquençage). L'objectif : montrer que je ne suis pas un développeur générique — je veux travailler à l'intersection du code et du vivant.

**Ce qu'on cherche à éviter à tout prix :**
- Le portfolio « SaaS template » avec hero centré, gradient mauve-bleu, cartes arrondies et icônes Heroicons
- Les classes Tailwind par défaut (`bg-blue-500`, `rounded-lg`, `shadow-md`) utilisées telles quelles
- Les illustrations 3D génériques type Spline
- Le ton corporate. C'est un portfolio personnel, il doit avoir une voix.

**Ce qu'on cherche :**
- Une esthétique éditoriale scientifique — proche d'une revue type *Nature* ou d'un site comme alphafold.ebi.ac.uk, recursion.com, insitro.com
- Une typographie expressive
- Des moments « wow » subtils : une hélice d'ADN qui répond au scroll, des particules cellulaires en background, un effet de séquençage sur le texte du hero
- Du blanc (ou du noir) qui respire

---

## 2. Direction artistique

### Palette principale — « Lab Dark »

Background dominant sombre, avec accents de fluorescence (clin d'œil à la microscopie GFP).

```
--bg-primary:    #0A0E0D  /* presque noir, légère teinte vert/bleu */
--bg-secondary:  #11161A  /* panneaux, cartes */
--bg-tertiary:   #1A2024  /* hover states */

--text-primary:  #E8EDE9  /* off-white, jamais pur blanc */
--text-secondary:#8A938E  /* gris-vert désaturé */
--text-muted:    #4A524E

--accent-primary:#7FFFB5  /* vert fluorescent type GFP — à utiliser AVEC PARCIMONIE */
--accent-warm:   #FF6B9D  /* rose fluorescent type marquage mCherry — accent rare */
--accent-cool:   #4DD0E1  /* cyan — pour liens, hover */

--border:        #1F2624
--border-subtle: #151A18
```

**Règle d'usage des accents** : le vert fluorescent est l'équivalent d'un marqueur fluo. On ne fluoresce pas tout. Réservé aux : liens actifs, hover importants, 1-2 mots dans le hero, état "actif" dans la nav. Maximum 3-4 utilisations par page visible.

### Typographie

**Une seule famille de chaque, pas de mix de 4 polices.**

- **Display / titres** : `Fraunces` (variable, italic disponible) — serif moderne avec du caractère, fonctionne en énorme
- **Body** : `Inter` — sans-serif neutre, lisible
- **Mono** : `JetBrains Mono` ou `Geist Mono` — pour les snippets, séquences ADN, métadonnées (dates, tags)

**Échelle typographique (mobile-first, augmente en desktop) :**

```
--text-hero:    clamp(3rem, 10vw, 8rem)    /* nom dans le hero */
--text-h1:      clamp(2.5rem, 6vw, 5rem)
--text-h2:      clamp(1.75rem, 4vw, 3rem)
--text-h3:      clamp(1.25rem, 2.5vw, 1.75rem)
--text-body:    1.0625rem                  /* 17px, plus lisible que 16 */
--text-small:   0.875rem
--text-mono:    0.8125rem                  /* mono est toujours plus petit */
```

**Règles** :
- Tracking serré (`-0.02em` à `-0.04em`) sur les très gros titres
- Line-height généreux sur le body : `1.6` à `1.7`
- Italic Fraunces utilisé volontairement pour 1-2 mots clés (ex: "*biology*" en italic dans une phrase autrement roman)

### Grille et espacement

- Base 8px (`--space-1: 8px`, `--space-2: 16px`, etc. jusqu'à `--space-16: 128px`)
- Padding vertical des sections : **minimum 120px desktop, 80px mobile**
- Conteneur max : `1280px` mais on n'utilise pas toujours toute la largeur — beaucoup de contenu est volontairement contraint à 720px (lecture éditoriale)
- Grille asymétrique préférée à la grille en 3 colonnes égales. Penser layout de magazine.

### Iconographie & illustration

- **Pas d'icônes Heroicons / Lucide partout.** Utiliser des symboles scientifiques ou pictogrammes custom SVG quand possible
- Glyphes utilisables : la double hélice stylisée, des bases ADN (A/C/G/T en mono), des cercles concentriques type cellule, des structures en ribbon protéique
- Les images de microscopie/cellules sont traitées : désaturation partielle, blend mode `screen` ou `lighten` sur fond sombre pour effet "fluorescence"

---

## 3. Stack technique

```
Framework      : Astro 5+ (idéal portfolio, hydratation sélective)
Styling        : Tailwind 4 avec config custom (variables CSS ci-dessus) + classes utilitaires custom dans @layer
Animations     : GSAP + ScrollTrigger pour les animations scroll-linked
                 Motion (ex-Framer Motion) pour les micro-interactions
Smooth scroll  : Lenis
3D (optionnel) : Three.js via react-three-fiber UNIQUEMENT pour l'hélice ADN du hero
Fonts          : Fontsource (self-hosted, pas Google Fonts CDN)
Deploy         : Vercel ou Netlify
```

**Contraintes** :
- Score Lighthouse Performance ≥ 90 — donc 3D limité, images optimisées en AVIF/WebP, lazy loading
- Pas de bibliothèque UI (shadcn, Radix UI, etc.) — on construit les composants à la main pour avoir une identité
- Accessibilité : contrastes AA minimum, focus visibles (anneau vert fluo), navigation clavier complète, `prefers-reduced-motion` respecté pour TOUTES les animations

---

## 3bis. Mapping des images disponibles

Les images suivantes sont dans `images/` (ou `public/images/`). Claude Code doit les utiliser **telles que mappées ci-dessous** et ne pas en inventer d'autres.

| Fichier | Description | Usage |
|---|---|---|
| `01-hero-dna-explosion.jpg` | ADN 3D qui éclate en particules, bleu profond + accents orange | **Hero principal** — moitié droite, plein écran, masque alpha pour fondre dans le `--bg-primary` à gauche |
| `02-signature-cells-fluorescent.jpg` | Cellules fluorescentes intenses bleu/magenta/jaune sur noir | **Image signature** — utilisée comme divider plein écran entre deux sections majeures (par ex. entre À propos et Projets) |
| `03-about-cells-blue.jpg` | Cellules fluorescentes bleu/blanc, ambiance sobre | Section **À propos** — image accompagnant le texte, format portrait, à gauche ou à droite du paragraphe d'ouverture |
| `04-skills-cells-green.jpg` | Cellules vert fluo + magenta, ambiance biotech | Section **Compétences** — background subtil (opacité 15-25%) ou accent visuel à côté du titre. Le vert se marie parfaitement avec `--accent-primary` |
| `05-projects-cells-red.jpg` | Bulles rouges éclatées, structure cellulaire dramatique | Section **Projets** — image d'introduction de la section, ou utilisée comme accent sur le projet le plus important |
| `06-microscopy-authentic.jpg` | Microscopie optique authentique, cellules végétales, tons gris/lavande | Section **Loisirs** ou **À propos** — apporte une touche scientifique réelle, à utiliser en plus petit format, presque comme une "preuve" |
| `07-hero-cell-isolated.jpg` | Une seule cellule/goutte sur fond noir avec aurore verte | **Section Contact** ou transition contemplative — ambiance méditative, idéal en fin de page |
| `08-texture-black-sand.jpg` | Sable noir ondulé, texture sombre subtile | **Transitions entre sections** — utilisé en background avec opacité 30-50% pour les sections qui ont besoin d'un fond non-uniforme |

### Règles d'usage des images

- **Optimisation obligatoire** : convertir toutes les images en AVIF + WebP (fallback JPEG), avec versions à plusieurs largeurs (640w, 1280w, 1920w, 2560w) via le composant `<Image>` d'Astro ou équivalent
- **Lazy loading** par défaut sauf pour les images visibles dans le viewport initial (hero)
- **Traitement uniforme** : appliquer un léger filtre sur certaines images pour les fondre dans la palette
  - Images 02, 04, 05 : laisser telles quelles, elles ont déjà la bonne intensité
  - Image 01 : potentiellement un blend-mode `screen` ou un masque gradient pour fondre dans le noir
  - Image 06 (microscopie) : désaturer légèrement (`filter: saturate(0.7)`) pour qu'elle s'accorde au reste
  - Image 08 : utiliser en background avec `mix-blend-mode: lighten` ou `screen`
- **Pas de bordures arrondies excessives** sur les images. Soit elles sont en pleine largeur (rectangles nets), soit elles ont un radius très subtil (4-8px max)
- **Pas de filtres mauves/violets génériques** par-dessus
- **Toujours alt-text descriptif** pour l'accessibilité (ex: "Visualisation 3D d'une double hélice d'ADN se désintégrant en particules")

---

## 4. Structure des sections

### 4.1 Hero
- Plein écran (`100dvh`)
- À gauche : nom énorme "Mattheus Germano" en Fraunces, avec "Germano" éventuellement en italic
- Sous-titre une ligne : "Étudiant en informatique — passionné par l'intersection du code et du *vivant*"
- En haut à droite : nav minimale (À propos · Projets · Contact)
- En bas : indicateur de scroll discret + une métadonnée mono ("Sherbrooke, QC · 2026")
- **Image de fond** : `01-hero-dna-explosion.jpg` occupe la moitié droite à 50-60% de l'écran, avec un masque gradient (de transparent à `--bg-primary`) qui la fond vers la gauche où se trouve le texte. Object-fit cover, position right center.
- **Animation de l'image au scroll** : très léger parallax (translateY ralenti de 20%) au scroll, sans dépasser pour ne pas casser les autres sections
- Effet texte : au chargement, le nom apparaît avec un effet de "séquençage" — les lettres se révèlent par paquets, comme un séquenceur qui imprime des bases. ~600ms total.
- **Note importante** : l'animation de double hélice mentionnée à l'origine est REMPLACÉE par l'image 01. Pas besoin de Three.js pour le hero. Économie de poids et de performance.

### 4.2 À propos
- Format éditorial, colonne unique max 720px pour le texte principal
- Premier paragraphe en plus gros (`text-h3`), comme un lede d'article
- Raconte : qui je suis, pourquoi la bio-informatique, ce qui m'intéresse (l'idée que le vivant est du code, etc.)
- Citation ou phrase clé mise en évidence dans la marge
- **Image** : `03-about-cells-blue.jpg` placée à droite du texte sur desktop (grille 2 colonnes, ratio 60/40 texte/image), au-dessus du texte sur mobile. L'image prend toute la hauteur du bloc texte, object-fit cover.
- En fin de section, petite vignette de `06-microscopy-authentic.jpg` (max 200px de large) avec une légende mono en-dessous : "Microscopie optique — observation que je fais en travaux pratiques" (à adapter par Mattheus selon ce qui est vrai)

### 4.2bis Divider signature (entre À propos et Formation)
- Section plein écran (`100vh`) dédiée uniquement à l'image `02-signature-cells-fluorescent.jpg`
- L'image occupe tout l'écran, object-fit cover
- Par-dessus, en bas à droite, une citation courte en Fraunces italic, max 12 mots, par exemple : *"Quatre lettres suffisent à écrire un être humain."*
- La citation a un fond très subtil (`backdrop-filter: blur(8px); background: rgba(10,14,13,0.4)`) pour rester lisible
- Effet au scroll : l'image a un léger zoom out (de scale 1.05 à scale 1) sur la durée où elle est visible
- Cette section sert de "respiration visuelle" entre le texte introductif et le contenu plus dense qui suit

### 4.3 Formation
- Pas une timeline classique (trop vu)
- Format : deux entrées en cartes horizontales asymétriques
  - **Baccalauréat en informatique (régime coopératif)** — Université de Sherbrooke, 2024–2027
  - **Certificat préparatoire aux programmes de 1er cycle** — Université de Sherbrooke, 2023–2024 (27/30 crédits)
- Dates en typo mono, titre en serif
- Hover : une barre verticale verte fluo apparaît à gauche

### 4.4 Compétences
- **NE PAS** faire une liste de logos avec des barres de pourcentage. Ringard.
- Format : nuage typographique organisé en deux groupes
  - **Techniques** : Python, Java, Excel (VBA), PostgreSQL, RStudio, HTML/CSS, Suite Office
  - **Méthodologiques** : Résolution de problèmes complexes · Approche méthodique · Planification & gestion de priorités · Travail d'équipe · Écoute active
- Les compétences techniques en font mono, taille variable selon "l'importance"
- Au hover, chaque compétence pulse légèrement (comme un battement)
- **Background** : `04-skills-cells-green.jpg` en position absolue, opacité 0.2, `mix-blend-mode: lighten`, légèrement floutée (`filter: blur(2px)`). Elle reste lisible sans submerger le texte. Le vert de l'image renforce l'identité visuelle de la section.

### 4.5 Projets

**Intro de section** : `05-projects-cells-red.jpg` en bandeau plein largeur, hauteur réduite (40-50vh), object-fit cover. Par-dessus, en bas à gauche, le titre "Projets" en très gros (Fraunces, ~clamp(3rem, 8vw, 6rem)), avec en sous-titre mono : `— 03 projets sélectionnés`. Le rouge tranche avec le reste de la page et signale visuellement que cette section est le cœur du portfolio.

**Layout** : trois projets, grille asymétrique en alternance. Sur desktop :
- Projet 01 : pleine largeur, format "showcase" avec une grande zone visuelle à gauche et le détail à droite
- Projet 02 : 60% à droite, 40% texte à gauche
- Projet 03 : 60% à gauche, 40% texte à droite

Sur mobile, tous en pleine largeur empilés.

**Composant `<ProjectCard />`** à créer avec ces props :
```
{
  index: string,        // "01", "02", "03"
  title: string,
  subtitle: string,     // une ligne, le quoi
  description: string,  // paragraphe de 2-4 phrases, le pourquoi/comment
  stack: string[],      // tags techniques
  context: string,      // ex: "Cours IFT320 — Systèmes d'exploitation"
  status: string,       // ex: "Terminé · Hiver 2025" ou "En cours"
  githubUrl?: string,   // optionnel
  liveUrl?: string,     // optionnel
  image?: string,       // chemin vers visuel ou capture (optionnel)
}
```

**Style des ProjectCard** :
- Numéro du projet en très gros en font mono, couleur `--text-muted`, positionné en haut à gauche (comme un compteur de revue scientifique : "01.", "02.", "03.")
- Titre en Fraunces, taille `--text-h2`
- Sous-titre en italic Fraunces, taille plus petite, couleur `--text-secondary`
- Description en Inter, prose éditoriale, max-width 60ch
- Stack : badges en font mono très petit (`--text-mono`), avec une bordure 1px de `--border`, padding 4px 10px, pas de fond (transparent)
- Contexte + statut en mono, `--text-muted`, séparés par un point médian
- Liens : underline animé, couleur `--accent-cool` au hover

**Contenu des trois projets** :

#### Projet 01 — Chat et transfert de fichiers sur port série simulé

```
title: "Chat & transfert de fichiers sur RS-232 simulé"
subtitle: "Programmation système en C — pthreads, tampons circulaires, protocole binaire custom"
description: "Implémentation en C d'une application de chat avec transfert de fichiers, communicant entre deux machines virtuelles via une simulation du port série RS-232. Le cœur du système repose sur un tampon circulaire (compilable en kernel-mode ou userspace via macros), surmonté par un protocole binaire à header (FILE:nom:taille) qui distingue les messages texte des transferts binaires. L'application tourne sur trois threads synchronisés par mutex : un reader qui parse les trames entrantes via une machine à états, un writer qui gère l'entrée terminal en mode raw (termios), et un watchdog qui détecte les transferts bloqués (timeout 3 secondes) et sauvegarde les données partielles. Le tout avec affichage de progression en temps réel et récupération gracieuse en cas d'interruption."
stack: ["C", "pthreads", "termios", "Linux", "Machines virtuelles", "Programmation système", "Synchronisation"]
context: "Cours IFT320 — Systèmes d'exploitation · Université de Sherbrooke"
status: "Terminé · 2025"
githubUrl: "https://github.com/GerMattheus/[à-créer]"   // PROJET À POUSSER SUR GITHUB
```

**Visuel suggéré** : un terminal avec deux fenêtres côte à côte montrant le chat en action, ou — encore mieux — un GIF du transfert de fichier avec barre de progression. Claude Code peut générer un SVG schématique : deux VMs reliées par un tuyau "tampon circulaire", avec head/tail animés.

**Note importante** : ce projet est encore plus impressionnant qu'il en a l'air en titre. La description doit *montrer* la sophistication technique (pthreads, machine à états, watchdog, mode raw) parce que c'est ce qui distingue ce projet d'un simple TP de buffer. Ne pas sous-vendre.

#### Projet 02 — LifeQuest RPG

```
title: "LifeQuest RPG"
subtitle: "Progressive Web App de développement personnel gamifié"
description: "Application web qui transforme les habitudes, objectifs et apprentissages quotidiens en système de jeu de rôle. Le personnage progresse à travers 8 compétences (Santé, Discipline, Études, Social, Relations, Finance, Maîtrise de soi, Créativité), accumule de l'XP via des quêtes (quotidiennes, hebdomadaires, mensuelles, personnalisées), répond à des quiz personnalisés, tient un journal et débloque des thèmes visuels via une monnaie virtuelle. Architecture full-frontend avec persistance locale (IndexedDB via Dexie.js), state management par stores Zustand, et installation native via PWA + Service Worker (fonctionne hors ligne après la première visite). Aucune donnée n'est envoyée à un serveur — tout reste sur l'appareil de l'utilisateur."
stack: ["React 18", "TypeScript", "Vite", "TailwindCSS", "Zustand", "Dexie.js (IndexedDB)", "React Router v6", "Workbox PWA", "Recharts"]
context: "Projet personnel · Développé avec l'assistance de Claude Code"
status: "En développement actif"
githubUrl: "https://github.com/GerMattheus/lifequest-rpg"
liveUrl: ""   // à ajouter une fois déployé (Netlify est déjà configuré dans le repo)
```

**Note importante** : le repo est actuellement **privé sur GitHub** — il doit passer en **public** pour que le portfolio puisse lier dessus, sinon le lien renvoie une 404. À faire avant la mise en ligne.

**Visuel suggéré** : capture d'écran du dashboard de l'application ou de la vue Stats (radar des compétences). Si l'app est déployée, une vraie capture vaut mille mots. Sinon, le composant `<Sidebar />` avec son ambiance "cosmos" suffit.

**Pourquoi ce projet compte dans le portfolio** : il démontre une maîtrise full-stack frontend moderne (TypeScript strict, state management, persistance locale, PWA, Service Worker, animations) bien au-delà de ce qu'on attend d'un étudiant en début de parcours. Il montre aussi une vraie capacité à concevoir un produit cohérent, pas juste à suivre un tutoriel.

#### Projet 03 — Projet de base de données PostgreSQL [À COMPLÉTER]

```
title: "[À définir une fois le projet commencé]"
subtitle: "Conception et implémentation d'une base de données relationnelle"
description: "[Placeholder à remplacer] Projet de fin de session : modélisation conceptuelle (MCD), schéma logique (MLD), implémentation PostgreSQL, requêtes SQL avancées (jointures, agrégations, sous-requêtes), procédures stockées et optimisation par index. Domaine modélisé : [à préciser]."
stack: ["PostgreSQL", "SQL", "Modélisation MCD/MLD", "Procédures stockées"]
context: "Cours de bases de données · Université de Sherbrooke"
status: "À venir · Session à préciser"
```

**Suggestion forte de domaine** : si tu as le choix du sujet, choisis un domaine biologique. Quelques pistes qui renforceraient ta narrative bio-informatique :
- **Catalogue de séquences génétiques** : gènes, mutations, organismes, annotations
- **Suivi d'essais cliniques** : patients, traitements, mesures longitudinales, effets indésirables
- **Banque de données protéomiques** : protéines, structures, interactions
- **Base de données phylogénétique** : taxonomie, arbres évolutifs

N'importe lequel transforme un TP standard en projet à thèse personnelle.

**Visuel suggéré** : une fois le projet commencé, capture du diagramme entité-association (DBDiagram.io, dbeaver, ou Mermaid). En attendant, un rectangle texturé avec `08-texture-black-sand.jpg` en placeholder, avec la mention "Projet en cours d'élaboration" en mono.

**Règle importante** : ne pas inventer de projets. Trois projets bien présentés valent mieux que sept projets vagues. Quand un nouveau projet est terminé (notamment un projet bio-informatique perso — un simple analyseur de FASTA avec Biopython ferait des merveilles ici), l'ajouter en suivant le même format.

### 4.6 Loisirs & intérêts
- Section humaine, courte, format inline
- Sports : Basketball · Soccer · Échecs
- Lecture : romans d'aventure, horreur, policier, mystère, mangas
- Cinéma : science-fiction, animation japonaise, action, mystère, aventure
- Format suggéré : trois colonnes éditoriales avec un mot-clé en gros par catégorie, puis détail en plus petit

### 4.7 Contact
- Fond légèrement plus clair (`--bg-secondary`) pour marquer la fin... OU mieux : utiliser `07-hero-cell-isolated.jpg` comme background plein écran de la section, avec un overlay sombre (`rgba(10,14,13,0.7)`) pour garder la lisibilité. L'image apporte une touche méditative en fin de parcours.
- Email cliquable en gros : `germanofataki243@gmail.com`
- Téléphone : `(438) 622-2513`
- Localisation : `Sherbrooke, QC`
- Liens : GitHub, LinkedIn (à ajouter)
- Footer minimal : nom + année + ligne mono "Built with care · Sherbrooke 2026"

---

## 5. Interactions et animations

### Principes
- Easing custom partout : `cubic-bezier(0.16, 1, 0.3, 1)` (le "smooth out" classique mais propre)
- Durées : `200ms` micro-interactions, `400-600ms` transitions de section, `800ms+` uniquement pour le hero
- TOUT respecte `prefers-reduced-motion: reduce` — animations désactivées ou très réduites

### Animations clés à implémenter
1. **Hero** : séquençage du nom au chargement (lettres apparaissent par paquets de 2-3, avec un petit délai)
2. **Hélice ADN** : rotation lente continue (60s par tour), accélère temporairement au scroll
3. **Scroll reveal** : chaque section fade-up légèrement au scroll (translation 20px max, opacité 0→1)
4. **Curseur** : optionnel, un petit point lumineux qui suit le curseur avec un léger lag, change de taille au survol des liens
5. **Texte mono** : sur les blocs de métadonnées (dates, tags), un effet "typewriter" subtil au reveal
6. **Liens** : underline animé qui se dessine de gauche à droite, couleur `--accent-cool` au hover

---

## 6. Contenu — textes prêts à utiliser

### Hero — sous-titre (à affiner)
> Étudiant en informatique à l'Université de Sherbrooke, je m'intéresse à ce qui se passe quand le code rencontre le vivant — bio-informatique, analyse de données génomiques, et tous les problèmes où la biologie devient un langage qu'on peut lire.

### À propos — premier paragraphe (suggestion à éditer)
> Le vivant est de l'information. Chaque cellule est une machine qui lit, copie et exécute du code écrit en quatre lettres. C'est cette idée qui m'a amené vers la bio-informatique : pouvoir poser sur le vivant les mêmes questions qu'on pose sur n'importe quel système — comment ça marche, où ça casse, comment l'optimiser.

(À personnaliser fortement par Mattheus — ce n'est qu'une amorce.)

---

## 7. Workflow de construction

Ordre recommandé pour Claude Code :

1. **Setup** : init Astro, config Tailwind avec variables CSS, install des polices via Fontsource, mise en place de Lenis
2. **Layout global** : nav, footer, conteneur de page, transitions de page
3. **Hero** d'abord — c'est la pièce maîtresse. Itérer dessus jusqu'à ce qu'il fasse "wow"
4. **À propos** ensuite — fixe le ton typographique du reste
5. **Formation + Compétences** — sections plus courtes
6. **Projets** — laisser un composant `<ProjectCard />` réutilisable, le contenu vient après
7. **Loisirs + Contact**
8. **Polish** : animations, micro-interactions, état de focus, mobile
9. **Perf & a11y** : audit Lighthouse, ajustements, test au clavier

**Règle d'or** : ne jamais coder une section complète d'un coup. Faire une première passe brute, montrer, ajuster, puis polir. Mieux : faire 2-3 variantes du hero avant de choisir.

---

## 8. Variantes possibles si on change d'avis

Si la direction "Lab Dark" ne plaît plus :
- **"Lab Paper"** : fond crème `#F5F1E8`, encre noire `#0E0E0E`, accent rouge sang `#8B0000`. Esthétique journal scientifique imprimé. Très différent, plus chaud.
- **"Microscopy"** : noir profond pur, avec photos de microscopie en background blur des sections. Plus immersif, moins lisible.

Garder Lab Dark sauf demande explicite.

---

## 9. Inspirations à consulter régulièrement

- alphafold.ebi.ac.uk
- recursion.com
- insitro.com
- allencell.org
- Awwwards (sites taggés "science", "biotech", "editorial")
- Godly.website
- siteinspire.com

Quand on est bloqué sur une décision de design, retourner aux références plutôt que d'inventer.