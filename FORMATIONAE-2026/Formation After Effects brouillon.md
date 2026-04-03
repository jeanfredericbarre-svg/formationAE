# INDEX
## Module - 1. Expressions, bases et syntaxe
## Module - 2. Expressions fondamentales

- `loopOut()` / `loopIn()` – types ping-pong, cycle, offset
- `wiggle()` – paramètres et seedRandom pour résultats déterministes
- `time` et `valueAtTime()` – lire des valeurs à d'autres moments
- `linear()` / `ease()` / `easeIn()` / `easeOut()` – remapping temporel
- Lier des propriétés entre calques (`thisComp.layer()`, `effect()`)

## Module - 3. L'argument (t)

## Module - 4. Animation de texte

- Animateurs de texte (Text Animators) : Range Selector, Wiggly Selector
- `textIndex`, `textTotal` dans les Expression Selectors
- Contrôle du timing par caractère / mot / ligne
- `sourceText` : piloter le contenu dynamiquement

## Module - 5. Null objects & parenting

- Null comme contrôleur de rig
- Parent/enfant : comprendre local space vs world space
- `toWorld()` / `fromWorld()` / `toComp()` / `fromComp()` pour les conversions d'espace
- Rig de caméra avec null 3D

## Module - 6. Shapes & masks avancés

- Path expressions : modifier les points d'un masque ou shape par expression
- Trim Paths : pilotage par expressions
- Opérations booléennes et ordre des opérateurs de groupe
- Répéteurs (Repeater) et contrôle procédural

## Module - 7. Effets & précompositions

- Différence entre "Leave all attributes" et "Move all attributes in the new Comp"
- Collapse Transformations (étoile) : impact sur 3D et blending
- Pré-comps comme modules réutilisables
- `comp()` pour accéder aux calques d'une autre composition

## Module - 8. Workflow & organisation

- Essential Graphics Panel – création de MOGRT pour Premiere Pro
- Master Properties sur les précompositions
- Expressions Controllers (Slider, Angle, Checkbox, Color, Dropdown)
- Rendu et Output Module : automatisation des exports

## Module - 9. Performance & bonnes pratiques

- `posterizeTime()` pour limiter le calcul d'expressions
- Identifier et résoudre les expressions lentes
- Proxies et footage management
- Purge mémoire, préférences GPU, preview settings
## Des effets ...
- Flou encadré accéléré
## Raccourcis claviers

## Trucs et astuces

___

# Module - 1. Expressions, bases et syntaxe
- Une expression est TOUJOURS attachée à une propriété, elle peut récupérer la valeur d'une autre propriété mais NE PEUT PAS MODIFIER une valeur autre que celle dans laquelle elle est écrite. Une expression NE PEUT PAS créer des éléments, pour cela il faudra passer par un script et c'est un tout autre dossier.
- Une expression est un **fragment de code JavaScript** attaché à une **propriété animable** d'un calque (position, opacité, rotation…). À chaque frame, After Effects **évalue ce code** et utilise le résultat comme valeur de la propriété.
> 💡 Analogie : une keyframe dit _"à ce moment précis, la valeur est X"_. Une expression dit _"calcule toi-même la valeur en fonction de règles"_.

> 🔗  La bible du Javascript : https://www.w3schools.com/jsref/
> 🔗 After Effects Expression Reference : https://ae-expressions.docsforadobe.dev/

- Les types de données :
	- Nombre
	- Array
	- String
	- Boolean
- Les variables
-  Les commentaires
-  Accéder aux propriétés d'un autre calque ou d'une autre composition
- La syntaxe :
	- En JavaScript (et donc en expressions AE), chaque signe de ponctuation a une **signification précise et non interchangeable**. Une virgule à la place d'un point, ou un crochet à la place d'une parenthèse, génère une erreur immédiate.
```
( )   →  parenthèses    →  appeler une fonction, grouper un calcul
[ ]   →  crochets       →  créer/lire un tableau (Array)
{ }   →  accolades      →  regrouper des instructions
.     →  point          →  accéder à une propriété ou méthode
,     →  virgule        →  séparer des éléments (arguments, valeurs)
;     →  point-virgule  →  terminer une instruction, passer à la ligne suivante
" "   →  guillemets     →  délimiter une chaîne de texte (String)
```

> 💡 Règle d'or : si AE affiche une erreur rouge, **la ponctuation est le premier endroit à inspecter**.

- Les parenthèses
Les parenthèses ont **trois rôles distincts** :
	Rôle 1 — Appeler une fonction
Une fonction est un bloc de code réutilisable. On l'**appelle** (on la déclenche) avec des parenthèses. Ce qui se trouve à l'intérieur s'appelle les **arguments**.
	Rôle 2 — Grouper un calcul 
C'est des maths : ce qui est entre parenthèses est calculé **en premier**.
	Rôle 3 — Accéder aux propriétés AE (syntaxe spécifique AE)
Dans After Effects, les parenthèses servent aussi à **indexer les propriétés** d'un effet par nom ou par numéro. C'est une syntaxe **propre à AE**, absente du JavaScript standard.
- Les crochets [] sur MacOS -> alt + shift + (
- Les accolades {} sur MacOS -> alt + (
-  Le "." permet d'accéder aux différents niveaux d'un layer
Ex : ***thisComp.layer("CONTROLS").transform.position*** -> D'abord "cette composition" puis dans celle ci, le layer "CONTROLS", puis dans celui ci les propriétés de Transformations puis dans ces propriété, la Position.

# Module - 2. Expressions fondamentales

## `loopOut()` / `loopIn()` 
**Syntaxe** : loopOut("type",keyframe) les 2 paramètres sont optionnels, par défaut, loopOut() est identique à loopOut("cycle",0)
les 4 types :
- cycle -> Répète la séquence de keyframes en boucle, du premier au dernier. Exercice : pas de saut visuel à la fin de la boucle, le dernier keyframe doit avoir les mêmes valeurs que le premier keyframe
- pingpong -> aller - retour
- offset -> décalage cumulatif  - Exercice : Pendule
- continue -> pas de répétition mais l'animation continue à la même vitesse
- le paramètre numKeyframes : détermine combien de keyframes sont inclus dans la boucle, en partant de la fin pour loopOut ou en partant du début pour loopIn. ATTENTION, source de confusion : numKeyframes indique le nombre de "segments" à inclure et pas le nombre de keyframes -> numKeyframes = 1 -> 1 segment -> 2 keyframes
- loopOutDuration -> la boucle est basée sur une durée en secondes et plus en nombre de keyframes. 
## `wiggle()`
Syntaxe : wiggle(freq, amp, octave, amp_mult, t)
Les propriétés :
- freq -> propriété obligatoire, nombre de wiggle par seconde
- amp -> propriété obligatoire, déplacement max de la propriété. L'amplitude s'applique autour de la valeur courante de la propriété, pas à partir de 0.
- octave -> propriété optionnelle, par défaut = 1. Nombre de couches de bruit superposées. Chaque octave ajoute un détail plus fin et plus rapide.
- amp_mult -> propriété optionnelle, par défaut = 0,5 Contrôle à quelle vitesse l'amplitude diminue à chaque octave supplémentaire.
- t -> propriété optionnelle, par défaut = time Le wiggle est calculé à partir de ce temps, par défaut time qui est le temps courant de la composition. Option pratique pour créer un wiggle qui boucle
- Wiggle sur un seul axe. Il faut commencer par comprendre la syntaxe d'un Array (un ensemble de valeurs). Pour la position par exemple, c'est un Array à 2 valeurs : position X et position Y. La syntaxe d'un array -> [valeur 1, valeur 2] -> entre crochets et différentes valeurs séparés par une virgule. Chaque élément d'un Array possède un index, le premier élément à un index = 0, l'élément suivant un index = 1, l'élément suivant un index = 2 etc ... La première valeur d'un tableau est value[0], la seconde est value[1] etc ...  ![[array.jpg]]
Dans cette exemple, on pourrait écrire une expression QUI NE SERT A RIEN : [value[0],value[1]] et qui pourrait se traduire par : fais un tableau -> [ .... ], met y la première valeur -> value[0], une virgule puis la seconde valeur -> value[1]
Il faut maintenant comprendre le principe d'une Variable. C'est un élément définit par l'utilisateur et qui peut être récupéré ailleurs dans l'expression, sans avoir à la retaper. La syntaxe est var nomDeLaVariable = contenu
Pour notre exemple de wiggle sur un seul axe :
var w = wiggle(3,40);
[value[0],w[1]]
... qui pourrait se traduire par :
1ere instruction : considère que "w" est en fait  "wiggle(3,40)",
2eme instruction : garde la première valeur de la propriété ; remplace la seconde valeur de la propriété par ma variable "w"

## `time` et `valueAtTime()` – lire des valeurs à d'autres moments
- time représente la position de la tête de lecture, exprimé en secondes. Il permet de récupérer une valeur quelconque à un instant donné.
- valueAtTime -> récupérer n'importe quelle valeur à un temps donné.
## `linear()` / `ease()` / `easeIn()` / `easeOut()` – remapping temporel
Ces quatre fonctions font **une seule chose** : mapper une valeur d'entrée vers une valeur de sortie, en contrôlant la courbe de transition (linear = de manière linéaire ; ease = avec lissage ...). 
Syntaxe : linear(a, b, c, d, e) -> a = propriété source,  b = propriété source VALEUR MINIMUM,  c = propriété source VALEUR MAXIMUM,  d = propriété modifiée VALEUR MINIMUM, e = propriété modifiée VALEUR MAXIMUM

## Lier des propriétés entre calques (`thisComp.layer()`, `effect()`)


# Module - 3.  time
Dans le système d'expressions AE, `t` est une **convention de nommage** pour un argument de type `Number` représentant **un temps en secondes de composition**. Il n'est pas réservé, ni un mot-clé AE : c'est un paramètre ordinaire que tu nommes comme tu veux, mais la convention `t` est universelle dans la doc Adobe.
`t` joue deux rôles distincts selon le contexte :
- Rôle - 1 -> repère temporel
C'est le cas le plus classique. AE évalue chaque expression à chaque image, et expose le temps courant via  ** `time`** (type `Nombre`, exprimé en secondes).
La grande majorité des méthodes qui acceptent `t` **ont `time` comme valeur par défaut** — ce qui signifie qu'on peut les appeler sans argument.

Des méthodes utilisant `t` 

| Méthode            | Signature complète                                                  | Défaut de `t`   |
| ------------------ | ------------------------------------------------------------------- | --------------- |
| `valueAtTime()`    | `valueAtTime(t)`                                                    | — (obligatoire) |
| `velocityAtTime()` | `velocityAtTime(t)`                                                 | — (obligatoire) |
| `speedAtTime()`    | `speedAtTime(t)`                                                    | — (obligatoire) |
| `nearestKey()`     | `nearestKey(t)`                                                     | — (obligatoire) |
| `wiggle()`         | `wiggle(freq, amp [, octaves=1][, amp_mult=0.5][, t=time])`         | `time`          |
| `temporalWiggle()` | `temporalWiggle(freq, amp [, octaves=1][, amp_mult=0.5][, t=time])` | `time`          |
| `smooth()`         | `smooth([width=.2][, samples=5][, t=time])`                         | `time`          |

- Rôle - 2 -> `t` comme driver d'interpolation (pas forcément du temps)
C'est là que ça devient puissant , pour les méthodes d'interpolation (`linear`, `ease`, `easeIn`, `easeOut`) `t` est ici simplement **la valeur d'entrée** du remapping. Elle peut être :
- `time` → interpolation temporelle classique
- `value` → remapping de plage à plage
- n'importe quelle expression numérique
___
Exemple -> NORMALISER time  c.f projet AE, Comp "normaliser" :
L'idée centrale : ramener `time` (valeur absolue, imprévisible) à une valeur **entre 0 et 1** qui représente la progression du calque.
```
t = 0   →   on est à inPoint   (début du calque)
t = 1   →   on est à outPoint  (fin du calque)
```
La formule de base, ça, ce n'est pas particulier à une expression AE, c'est juste des maths :
```
t = (time - inPoint) / (outPoint - inPoint)
```
`inPoint` et `outPoint` retournent des `Nombres` en secondes. La division donne un `Nombre` entre 0 et 1 (ou hors de cette plage avant/après le calque, mais `ease()` ou `linear()` clamp automatiquement).

# Raccourcis clavier

E -> effets du layer
EE -> expressions du layer

# Trucs et astuces
- Déplacer la position d'un layer déjà animer en position : se positionner sur un keyframe, sélectionner la propriété (cela sélectionne tous les points clés de cette propriété), modifier la valeur.
-  Centrer dans la Comp -> clic droit propriété position -> "Modifier la valeur" -> Unités = % de la composition -> X=50%, Y=50%
