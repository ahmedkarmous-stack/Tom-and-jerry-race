# 🐭 Tom & Jerry — La Course de l'Année 🏁

> Un jeu de course arcade HTML5 inspiré du film animé **Tom & Jerry: La Course de l'Année**.  
> Joue en tant que **Jerry** et bats **Tom** sur 3 tours — collecte le fromage pour activer le turbo boost !

---

## 🎮 Aperçu

![Welcome Screen](https://img.shields.io/badge/Status-Playable-brightgreen)
![HTML5](https://img.shields.io/badge/HTML5-Canvas-orange)
![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-yellow)
![Responsive](https://img.shields.io/badge/Design-Responsive-blue)
![No Dependencies](https://img.shields.io/badge/Dependencies-None-lightgrey)

Un jeu de course entièrement **single-file HTML** — aucune installation, aucune dépendance, aucun serveur.  
Ouvre simplement le fichier dans n'importe quel navigateur moderne et joue immédiatement.

---

## ✨ Fonctionnalités

| Fonctionnalité | Description |
|---|---|
| 🖼️ **Écran d'accueil cinématique** | Photo officielle du film en fond animé, étoiles flottantes, ticker défilant |
| 🏆 **3 Niveaux de difficulté** | Facile, Moyen, Difficile — chacun avec sa propre piste et vitesse de Tom |
| 🧀 **Système Boost** | Collecte 6 fromages sur la piste pour remplir ta jauge turbo |
| 🗺️ **Mini-carte** | Suivi en temps réel de la position de Jerry et Tom |
| 💨 **Effets visuels** | Poussière, flammes de boost, particules de collecte |
| 📱 **Responsive** | Interface adaptée mobile, tablette et desktop |
| 🎨 **Design premium** | Cartes de niveaux avec aperçu de piste, animations CSS, typographie Fredoka One |
| 🏎️ **IA rubber-band** | Tom accélère pour te rattraper si tu prends trop d'avance |

---

## 🚀 Installation & Lancement

### Option 1 — Directement dans le navigateur
```bash
# Télécharge le fichier, puis double-clique dessus
tom-jerry-race.html
```

### Option 2 — Serveur local
```bash
# Python 3
python3 -m http.server 8080

# Node.js
npx serve .

# Puis ouvre : http://localhost:8080/tom-jerry-race.html
```

> ✅ **Compatible** : Chrome, Firefox, Safari, Edge (tout navigateur supportant HTML5 Canvas)  
> ❌ **Non requis** : npm, node, frameworks, APIs externes

---

## 🎯 Comment Jouer

### Contrôles clavier

| Touche | Action |
|--------|--------|
| `↑` ou `W` | Accélérer |
| `↓` ou `S` | Freiner / Reculer |
| `←` ou `A` | Tourner à gauche |
| `→` ou `D` | Tourner à droite |
| `Espace` | **Activer le Boost** (si jauge > 0) |

### Contrôles mobile / tactile
Utilise les **boutons D-pad** et le **bouton BOOST** affichés à l'écran.

### Règles
1. Fais **3 tours complets** dans le sens horaire ↻
2. Tu joues **Jerry** (kart jaune) contre **Tom** (kart gris-bleu)
3. Collecte les **🧀 fromages** répartis sur la piste pour remplir la jauge boost
4. La jauge boost t'accorde une vitesse de **×1.65** pendant quelques secondes
5. Le premier à finir 3 tours **gagne** !

---

## 🏆 Niveaux de Difficulté

```
🟢 FACILE      Tom: lent     Piste: très large   Idéal pour découvrir
🟡 MOYEN       Tom: rapide   Piste: normale      Un vrai défi
🔴 DIFFICILE   Tom: très rapide  Piste: étroite  Pour les experts !
```

### Détails techniques des niveaux

| Niveau | Vitesse Tom | Rubber-band | Largeur piste |
|--------|-------------|-------------|---------------|
| Facile | 2.4 px/f | 22% | ~230 px |
| Moyen | 3.3 px/f | 55% | ~175 px |
| Difficile | 3.95 px/f | 95% | ~130 px |

> **Rubber-band** : Tom accélère proportionnellement à ton avance pour que la course reste tendue.

---

## 🏗️ Architecture Technique

### Structure du fichier
```
tom-jerry-race.html
├── <style>          CSS complet (responsive, animations, 3 écrans)
├── DOM HTML         3 écrans : Welcome · Niveaux · Jeu
└── <script>         Moteur de jeu (pure Vanilla JS)
    ├── Screen manager      Navigation entre les écrans
    ├── Particle system     Étoiles, étincelles, poussière
    ├── Level configs       Paramètres des 3 niveaux
    ├── Track renderer      Dessin de la piste (ellipse O(1))
    ├── Car physics         Mouvement + collision murs
    ├── AI controller       Tom suit la ligne centrale avec lookahead
    ├── Lap counter         Accumulation d'angle trigonométrique
    └── HUD & minimap       Interface en temps réel
```

### Algorithmes clés

**Collision de piste** — O(1) par frame :
```js
function onTrack(x, y) {
  var dx = x - CX, dy = y - CY;
  return dx*dx/(ORX*ORX) + dy*dy/(ORY*ORY) <= 1   // dans l'ellipse extérieure
      && dx*dx/(IRX*IRX) + dy*dy/(IRY*IRY) >= 1;   // hors de l'ellipse intérieure
}
```

**Comptage des tours** — accumulation d'angle (pas de ligne d'arrivée) :
```js
var delta = currentAngle - prevAngle;  // delta normalisé dans [-π, π]
if (delta > 0) car.accum += delta;     // uniquement sens horaire
car.laps = Math.floor(car.accum / (2 * Math.PI));
```

**Voiture jamais bloquée** — push d'urgence si hors piste :
```js
if (!onTrack(nx, ny)) {
  // Essai x seulement, puis y seulement, puis nudge vers centre
  var ep = centreLine(tAngle(car.x, car.y));
  car.x += (ep.x - car.x) * 0.2;
  car.y += (ep.y - car.y) * 0.2;
}
```

---

## 📁 Structure du Projet

```
📦 tom-jerry-race/
├── 📄 tom-jerry-race.html    ← Jeu complet (tout-en-un)
└── 📄 README.md              ← Ce fichier
```

> L'image de fond est **encodée en base64** directement dans le HTML pour garantir un fichier 100% autonome.

---

## 🎨 Design

### Polices
- **Fredoka One** — titres et HUD (via Google Fonts)
- **Nunito 700/800** — descriptions et textes secondaires

### Palette de couleurs

| Couleur | Hex | Usage |
|---------|-----|-------|
| Or | `#FFD700` | Titres, Jerry, accent principal |
| Orange | `#FF8C00` | Gradient boutons, boost |
| Rouge | `#E53935` | Niveau difficile, Tom |
| Vert | `#43A047` | Niveau facile |
| Sombre | `#0a0a1a` | Fond principal |

### Écrans
```
[Welcome] ──────────► [Niveaux] ──────────► [Jeu]
  ↑ goWelcome()         ↑ goLevels()          ↑ startGame(n)
  └──────────────────────┘◄─── fin de partie ─┘
```

---

## 🐛 Bugs Connus & Limitations

- Le jeu utilise `requestAnimationFrame` — les performances dépendent du navigateur et de l'appareil
- Sur très petits écrans (< 320px), le D-pad peut déborder
- L'IA de Tom ne prend pas en compte les obstacles dynamiques — elle suit uniquement la ligne centrale

---

## 🤝 Crédits

| Élément | Source |
|---------|--------|
| 🎬 Artwork & personnages | © Warner Bros. Animation — *Tom & Jerry: The Fast and the Furry* |
| 🎮 Moteur de jeu | Développé from scratch en Vanilla JS / HTML5 Canvas |
| 🔤 Polices | Google Fonts (Fredoka One, Nunito) |
| 🛠️ Assistance au développement | Claude (Anthropic) |

---

## 📜 Licence

Ce projet est à des fins **éducatives et personnelles uniquement**.  
Les personnages Tom & Jerry et les visuels associés sont la propriété de **Warner Bros. Entertainment Inc.**

---

<div align="center">

**🐭 Que le meilleur gagne ! 🐱**

*Fais 3 tours, collecte le fromage, bats Tom !*

</div>
