---
name: "mulesoft-brand"
description: "Apply official MuleSoft/Salesforce 2026 Brand Guidelines to any visual, design, presentation, or content deliverable. Use this skill EVERY TIME the user asks to create slides, decks, HTML pages, diagrams, mockups, UI components, marketing materials, emails, reports, artifacts, or any visual content that should follow MuleSoft or Salesforce branding. Trigger on \"MuleSoft style\", \"Salesforce brand\", \"branded\", \"on-brand\", \"brand guidelines\", \"MuleSoft colors\", \"SF design\", or any creation task where MuleSoft/Salesforce visual identity should apply. Also trigger when the user mentions creating demo assets, SE materials, pitch decks, customer-facing documents, or any professional Salesforce/MuleSoft content — even without explicitly saying \"brand guidelines\"."
---

# MuleSoft / Salesforce Brand Guidelines — 2026

Ce skill définit la Direction Artistique (DA) officielle MuleSoft / Salesforce 2026.
Il sert d'**ancrage visuel** pour garantir la cohérence de marque, tout en offrant une **liberté créative totale** : tu peux adapter la disposition, créer des animations complexes, faire varier les structures ou proposer des cartes immersives tant que les règles de couleurs et de typographie restent respectées.

---

## 1. PALETTE DE COULEURS OFFICIELLE (2026)

### Palette principale — Blues

| Token | Hex | Usage principal |
|---|---|---|
| Cloud Blue 90 | `#CFE9FE` | Fond par défaut des slides claires et des cartes claires |
| Cloud Blue 68 | `#00B3FF` | Cyan vif — mots-clés dans les titres, accents, étoiles 3D, marque |
| Electric Blue 50 | `#066AFE` | Bleu Salesforce vif — CTAs, pilules d'en-tête, puces |
| Electric Blue 30 | `#022AC0` | Bleu profond — conteneurs sombres, fonds de héros, bannières |
| Electric Blue 15 | `#001E5B` | Bleu nuit — couleur principale des textes et titres sur fond clair |

### Palette secondaire — AI / Agentforce Signals

| Token | Hex | Usage |
|---|---|---|
| Teal 60 | `#00A3A3` | Indicateurs d'agents IA, flux de données, couche Process |
| Yellow 70 | `#F5C518` | Alertes, badges de mise en avant (« Limited Use ») |
| Pink 40 | `#D43FAA` | Nœuds d'intégration, fonds de portraits, accents créatifs |
| Violet 30 | `#6B3FA0` | Intelligence autonome, couche System, portraits |

**Règles d'emploi de la palette secondaire :**

- Ne jamais utiliser les couleurs secondaires comme fond de section plein.
- Les réserver aux : séries de data viz, badges d'accent, petites icônes, indicateurs de statut d'agent, marqueurs de couche d'architecture.
- Toujours les associer à un fond bleu primaire ou blanc.
- Maximum 1 à 2 couleurs secondaires par composition.

---

## 2. TYPOGRAPHIE & TITRES

### Typographies

- **Headlines & Titres (H1–H4, CTA)** : `Avant Garde for Salesforce` (fallback : `Century Gothic, Futura, sans-serif`).
- **Corps de texte (B1–B2, C1)** : `Salesforce Sans` (fallback : `Arial, Helvetica, sans-serif`).

### Règle des titres bicolores

- **Sur fond clair (`#CFE9FE`)** : texte principal en `Electric Blue 15` (`#001E5B`), mot-clé ou élément d'emphase en `Cloud Blue 68` (`#00B3FF`) ou `Electric Blue 50` (`#066AFE`).
- **Sur fond sombre (`#022AC0` / `#001E5B`)** : texte principal en blanc (`#FFFFFF`), mot-clé en `Cloud Blue 68` (`#00B3FF`).
- **Zéro gris** : ne jamais utiliser de gris pour le texte — toujours la palette bleue ou du blanc.
- **Casse** : *sentence case* systématique (majuscule uniquement en début de phrase).

---

## 3. COMPOSANTS & FORMES SIGNATURE

### A. Cartes & conteneurs

- **Cartes claires** : fond blanc (`#FFFFFF`), pas de bordure colorée prononcée, arrondis généreux (`border-radius: 16px` à `24px`).
- **Cartes concaves** : blocs dont l'un des bords verticaux est incurvé vers l'intérieur pour apporter de la dynamique.
- **Conteneurs glassmorphic** : cartes semi-transparentes avec effet de verre dépoli (`backdrop-filter: blur(10px)`) sur fond bleu nuit.
- **Trame de fond (Dot Grid)** : arrière-plan habillé d'un réseau subtil de points ou de petites croix alignées.

### B. En-têtes, tableaux et timelines

- **En-têtes en pilule (Table Banners)** : étiquettes de colonnes sous forme de pilules complètement arrondies (`border-radius: 9999px`) en `Electric Blue 50` ou `30`.
- **Timelines** : suite de pilules interconnectées progressant du cyan vif (`#00B3FF`) au bleu nuit (`#001E5B`), reliées aux textes par des lignes pointillées.

### C. Schémas d'architecture (API-led Connectivity)

- **Couches horizontales** : *Experience layer*, *Process layer*, *System layer* représentées par de grands conteneurs translucides.
- **Marqueurs de couche** : onglets verticaux colorés à gauche (Teal pour Process, Violet pour System).
- **Composants d'API** : puces blanches à coins arrondis contenant le logo MuleSoft.

### D. Data viz & callouts

- **Graphiques radiaux / donuts** : épaisseur de trait (*stroke-width*) strictement fixée à **16 px**.
- **Badges de statistiques** : chiffres géants (`90%`, `79%`) placés au centre de donuts ou dans des pilules bleu nuit.

---

## 4. ÉLÉMENTS GRAPHIQUES 3D, LOGOS ET MASCOTTES

- **Disques pour logos tiers** : tout logo partenaire (AWS, SAP, Salesforce, Jira...) doit être inséré dans un disque blanc parfait (`border-radius: 50%`).
- **Sphère MuleSoft 3D** : sphère bleue brillante avec le « M » extrudé en blanc.
- **Étoiles 4 branches (Sparkles 3D)** : éléments flottants en `Cloud Blue 68` symbolisant l'IA et Agentforce.
- **Emojis & pictos 3D** : têtes de robots IA, fusées, boucliers, curseurs volumétriques.
- **Icônes 2D** : pictos plats bicolores ou monochromes pour les fonctionnalités de la plateforme (Anypoint, RPA, IDP, Governance).
- **Mascottes** :
  - **Max the Mule** : intégration, API, connectivité, ESB.
  - **Agent Max (robot)** : Agentforce, workflows autonomes, IA agentique.

---

## 5. STRATÉGIE DE GESTION DES ASSETS & FALLBACKS

Lors de la création d'artefacts HTML ou visuels, utiliser la logique de chemin suivante :

```text
Chemins locaux cibles par défaut :
├── ./assets/logos/[nom].svg       (Logos partenaires)
├── ./assets/icons/[nom].svg       (Pictos 2D plateforme)
├── ./assets/3d/[nom].png          (Rendus 3D & Sparkles)
└── ./assets/mascots/[nom].png     (Mascottes Max & Agent Max)
```

**Logique de fallback** (si le fichier n'existe pas, ou en mode HTML autonome) :

1. **Mode projet local** : tenter d'abord de lier le fichier via son chemin relatif (ex. `./assets/logos/aws.svg`).
2. **Mode fichier autonome / fallback SVG** :
   - **Logos & pictos 2D** : générer directement le code `<svg>` inline propre et vectoriel.
   - **Éléments 3D & sparkles** : générer la forme en SVG inline enrichi de dégradés radiaux (`<radialGradient>`), ou utiliser un composant CSS/glassmorphism.
   - **Mascottes & photos** : utiliser un encodage Base64 (`data:image/png;base64,...`) ou un conteneur stylisé élégant servant de placeholder.

**Encapsulation HTML des logos partenaires** (avec fallback SVG inline via `onerror`) :

```html
<div class="logo-disc">
  <img src="./assets/logos/aws.svg" alt="AWS"
       onerror="this.outerHTML='&lt;svg viewBox=&quot;0 0 24 24&quot; width=&quot;24&quot; height=&quot;24&quot;&gt;&lt;path fill=&quot;%23001E5B&quot; d=&quot;...&quot;/&gt;&lt;/svg&gt;'">
</div>
```

---

## 6. LIBERTÉ CRÉATIVE ET ANIMATIONS

Les règles ci-dessus fournissent le socle de marque MuleSoft 2026. Tu es encouragé à :

- Proposer des animations fluides (hover, transitions d'apparition CSS/JS) lors de la génération de code HTML.
- Expérimenter avec la disposition spatiale (cartes décalées, grilles asymétriques, effets de calques).
- Adapter la forme des composants au storytelling de l'utilisateur, tout en conservant les couleurs et typographies officielles comme fil conducteur.

---

## 7. CSS VARIABLES (QUICK REFERENCE)

```css
:root {
  /* Colors */
  --sf-cloud-90:    #CFE9FE; /* Fond clair par défaut */
  --sf-cloud-68:    #00B3FF; /* Cyan vif / Accent / Highlights */
  --sf-electric-50: #066AFE; /* Bleu vif / CTAs / Puces */
  --sf-electric-30: #022AC0; /* Bleu conteneur / Bannières */
  --sf-electric-15: #001E5B; /* Bleu nuit / Texte sombre */

  /* Secondary */
  --sf-teal:   #00A3A3;
  --sf-yellow: #F5C518;
  --sf-pink:   #D43FAA;
  --sf-violet: #6B3FA0;

  /* Fonts */
  --font-headline: 'Avant Garde for Salesforce', 'Century Gothic', Futura, sans-serif;
  --font-body:     'Salesforce Sans', 'Arial', Helvetica, sans-serif;

  /* Geometry & Borders */
  --radius-card: 20px;
  --radius-pill: 9999px;
  --stat-stroke: 16px;
}

/* Base Utility for Partner Logo Disc */
.logo-disc {
  width: 48px;
  height: 48px;
  background: #FFFFFF;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 8px rgba(0, 30, 91, 0.08);
}
```

