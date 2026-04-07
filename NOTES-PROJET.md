## Conventions de nommage des dossiers et pages

Les dossiers dans `FORMATIONAE-2026/` suivent deux conventions :

### Modules numérotés

Format : `Module-X_NOM` Exemples :

- `Module-1_EXPRESSIONS`
- `Module-2_ANIMATION`

### Pages numérotés

Format : `1. Nom de la page` Exemples :

- `1. Les bases`

Ces dossiers et pages sont triés automatiquement par numéro dans la sidebar.

### Autres dossiers

Exemples :

- `Ressources`
- `Glossaire`
- `Annexes`

Ces dossiers apparaissent **après** les modules numérotés dans la sidebar, triés par ordre alphabétique.

### Dossiers exclus de la sidebar

Les dossiers suivants sont ignorés par le script : `images`, `assets`, `media`

## Intégration des images

Les images sont dans `FORMATIONAE-2026/images/`. Dans un fichier Markdown, les intégrer ainsi :

```markdown
![description](../images/nom-du-fichier.png)
```

Le `../` remonte d'un niveau depuis le dossier du module vers `FORMATIONAE-2026/images/`.

## Génération automatique de la sidebar

Le fichier `_sidebar.md` est **généré automatiquement** par GitHub Actions. **Ne jamais modifier `_sidebar.md` manuellement** — les modifications seraient écrasées au prochain push.

Le script se déclenche automatiquement à chaque push modifiant `FORMATIONAE-2026/`.

### Déclenchement manuel

Onglet **Actions** sur GitHub.com → **Generate Sidebar** → **Run workflow**

## Fichiers projets After Effects

Les fichiers `.aep` sont hébergés sur **Google Drive** (pas dans le repo GitHub — fichiers binaires trop lourds).

### Structure Google Drive

```
Formation AE 2026/
└── _exemples-ae/
    ├── Module-1_EXPRESSIONS/
    ├── Module-2_ANIMATION-TEXTE/
    └── ...
```

Partage du dossier `_exemples-ae` : **"Toute personne disposant du lien"** en mode **Lecteur**.

### Obtenir le lien de téléchargement direct

Lien Drive standard (depuis "Partager") :

```
https://drive.google.com/file/d/FILE_ID/view?usp=sharing
```

Lien à utiliser dans les leçons (force le téléchargement) :

```
https://drive.google.com/uc?export=download&id=FILE_ID
```

Remplacer `FILE_ID` par l'identifiant présent dans l'URL Drive.

### Bloc téléchargement dans les leçons

Snippet HTML à coller dans les fichiers Markdown :

```html
<div class="ae-download">
  <div class="ae-download__icon">⬇</div>
  <div class="ae-download__body">
    <span class="ae-download__label">Projet After Effects</span>
    <span class="ae-download__name">Module-X_nom-du-fichier.aep</span>
    <span class="ae-download__desc">Fichier de départ pour tester les exemples de cette leçon</span>
  </div>
  <a class="ae-download__btn" href="https://drive.google.com/uc?export=download&id=FILE_ID">Télécharger</a>
</div>
```

Le style `.ae-download` est défini dans `index.html`. La description (`ae-download__desc`) est optionnelle.

## Hébergement

- Dépôt GitHub : `https://github.com/jeanfredericbarre-svg/formationAE`
- Site en ligne : `https://jeanfredericbarre-svg.github.io/formationAE/#/`
- Hébergement : GitHub Pages (source : branche `main`, racine `/`)
- Moteur de rendu : Docsify