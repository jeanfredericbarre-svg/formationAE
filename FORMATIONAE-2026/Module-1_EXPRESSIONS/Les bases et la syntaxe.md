# C'est quoi une Expression ?

Une expression est TOUJOURS attachée à une propriété et une seule, et ne fera qu'une seule chose : envoyer un résultat à cette propriété.
![exemples de propriétés](../images/proprietes.png)
Dans cet exemple, la propriété "Taille" a une expression (les valeurs sont rouges). Il est essentiel de noter que la propriété "Taille" ou la propriété "Position" possèdent 2 valeurs alors que la propriété "Arrondi" n'en possède qu'une. C'est logique, la taille s'exprime en largeur et hauteur, la position s'exprime en position x ou position y, l'arrondi s'exprime lui avec une seule valeur, en pixels. Ce qui est important de comprendre c'est qu'une expression DEVRA RENVOYER UNE VALEUR SIMILAIRE. Si le résultat d'une expression appliquée à la propriété Position est par exemple 12, alors message d'erreur que nous pourrons traduire par "ah non, pas possible, pour cette propriété il me faut 2 valeurs séparées par une virgule." Ceci est valable pour toutes les propriétés.

Une expression peut récupérer la valeur d'une autre propriété mais NE PEUT PAS MODIFIER une valeur autre que celle dans laquelle elle est écrite. Une expression NE PEUT PAS créer des éléments, pour cela il faudra passer par un script et c'est une toute autre histoire.

Une expression est donc un mini programme , à chaque frame, After Effects **évalue ce code** et renvoie le résultat comme valeur de la propriété.
> 💡 Analogie : une keyframe dit : "à ce moment précis, la valeur est X" et c'est l'utilisateur qui définit cette valeur. Une expression dit _"calcule toi-même la valeur en fonction de règles"_.

# JavaScript dans AE
Imaginons que JavaScript Standard est la langur Française, AE parle un dialecte régional : la grammaire est la même mais le vocabulaire peut être différent.

Ce qui est identique (la grammaire) :
- les fonctions de base de JS fonctionnent dans AE : les variables, les conditions, les fonctions, les formules mathématiques

Ce qui est différent (le dialecte) :
-  Comme dit précédemment, une expression vit _à l'intérieur_ d'une propriété. En JS standard, tu écris un programme qui tourne une fois. Dans AE, chaque expression est **réévaluée à chaque frame**, comme si AE appelait ta fonction automatiquement 25 ou 30 fois par seconde.

-  Dans AE, un ensemble de variables et objets sont **disponibles automatiquement** dans chaque expression :
```javascript
// Ces variables existent SANS que tu les déclares :

time          // temps courant de la comp en secondes (Number)
thisComp      // la composition courante (Object)
thisLayer     // le calque qui porte l'expression (Object)
thisProperty  // la propriété elle-même (Object)
value         // la valeur actuelle de la propriété SANS expression (très utile)
```

- AE possède ses propres objets avec leurs propres méthodes :
```javascript
// Objet Layer → accéder à un autre calque
thisComp.layer("Mon Calque")         // par nom
thisComp.layer(1)                    // par index (commence à 1, pas 0 !)

// Objet Composition
thisComp.duration                    // durée totale en secondes
thisComp.width                       // largeur en pixels

// Objet Property → lire une valeur à un instant précis
thisLayer.opacity.valueAtTime(0)     // opacity à t=0

// Méthodes de transformation
wiggle(5, 30)                        // oscillation aléatoire – propre à AE
linear(t, tMin, tMax, v1, v2)        // interpolation – propre à AE
```
Ces méthodes **n'existent pas** en JavaScript standard. C'est le vocabulaire du dialecte.

- Les types de retour attendus varient selon la propriété
C'est le piège le plus courant pour un débutant JS :
```javascript
// Propriété scalaire (Opacity, Rotation) → retourner un Number
time * 10                           // ✅ un seul nombre

// Propriété vectorielle (Position, Scale) → retourner un Array
[thisComp.width / 2, time * 100]    // ✅ [x, y]

// Propriété texte (Source Text) → retourner une String
"Frame : " + timeToFrames(time)     // ✅ chaîne de caractères
```
Si tu retournes le mauvais type, AE affiche une erreur .


# 2 sources essentielles

- Référence du langage JavaScript -> https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference
- Référence du langage Expression AE -> https://ae-expressions.docsforadobe.dev/