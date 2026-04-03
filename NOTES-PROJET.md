## Structure du dépôt
formationAE/ 
├── .github/
│ └── workflows/
│ └── generate-sidebar.yml ← script automatique de génération de la sidebar
├── .nojekyll ← indispensable pour GitHub Pages + Docsify
├── index.html ← configuration Docsify
├── README.md ← page d'accueil du site
├── _sidebar.md ← généré automatiquement, ne pas modifier manuellement
├── images/ ← images globales du site
└── FORMATIONAE-2026/
  └── Module-1_EXPRESSIONS/
    └── Les bases et la syntaxe.md

## Conventions de nommage
Les dossiers dans `FORMATIONAE-2026/` suivent deux conventions :
### Modules numérotés
Format : `Module-X_NOM` 
Exemples :
- `Module-1_EXPRESSIONS`
- `Module-2_ANIMATION`
Ces dossiers sont triés automatiquement par numéro dans la sidebar.

### Autres dossiers
Exemples :
- `Ressources`
- `Glossaire`
- `Annexes`
Ces dossiers apparaissent **après** les modules numérotés dans la sidebar, triés par ordre alphabétique.

## Génération automatique de la sidebar
Le fichier `_sidebar.md` est **généré automatiquement** par GitHub Actions à chaque push qui modifie le contenu de `FORMATIONAE-2026/`.
**Ne jamais modifier `_sidebar.md` manuellement** — les modifications seraient écrasées au prochain push.
Le script est dans `.github/workflows/generate-sidebar.yml`.

## Intégration des images
Les images globales sont dans `/images/` à la racine du dépôt. Dans un fichier Markdown, les intégrer ainsi : ```markdown ![description](images/nom-du-fichier.png) ```
## Hébergement
- Dépôt GitHub : `https://github.com/jeanfredericbarre-svg/formationAE`
- Site en ligne : `https://jeanfredericbarre-svg.github.io/formationAE/#/`
- Hébergement : GitHub Pages (source : branche `main`, racine `/`)
- Moteur de rendu : Docsify
- 