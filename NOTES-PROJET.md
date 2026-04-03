## Conventions de nommage des dossiers

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

### Dossiers exclus de la sidebar
Les dossiers suivants sont ignorés par le script : `images`, `assets`, `media`

## Intégration des images

Les images sont dans `FORMATIONAE-2026/images/`.
Dans un fichier Markdown, les intégrer ainsi :
```markdown
![description](../images/nom-du-fichier.png)
```

Le `../` remonte d'un niveau depuis le dossier du module vers `FORMATIONAE-2026/images/`.

## Génération automatique de la sidebar

Le fichier `_sidebar.md` est **généré automatiquement** par GitHub Actions.
**Ne jamais modifier `_sidebar.md` manuellement** — les modifications seraient écrasées au prochain push.

Le script se déclenche automatiquement à chaque push modifiant `FORMATIONAE-2026/`.

### Déclenchement manuel
Onglet **Actions** sur GitHub.com → **Generate Sidebar** → **Run workflow**

## Hébergement

- Dépôt GitHub : `https://github.com/jeanfredericbarre-svg/formationAE`
- Site en ligne : `https://jeanfredericbarre-svg.github.io/formationAE/#/`
- Hébergement : GitHub Pages (source : branche `main`, racine `/`)
- Moteur de rendu : Docsify