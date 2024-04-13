# Récursion terminale et gestion de la mémoire en OCaml

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par
    Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce
    site à pour seul but d'être plus compréhensible pendant les périodes de
    révision que des diaporamas.

!!!tip "Crédits"
    Developpez.com

## Gestion de la mémoire en OCaml

### Un exemple

!!!example "Exemple"
    Considérons le programme suivant. Il calcule la somme des entiers de $0$ à $1 000 000$ :
    ```OCaml linenums="1"
    let rec somme n =
        if n=0 then 0 else n + somme (n-1)
    let v = somme 1_000_000
    ```
    Compilation, exécution :
    ```
    $ocamlopt somme.ml -o somme
    $./ somme
    Fatal error: exception Stack_overflow
    ```
    Il y a débordement de pile : le nombre de stack frame est trop grand.

### Analyse

On empile puis dépile les stack frame

Chaque stack frame contient une sauvegarde des registres du processeur; un espace pour stocker la valeur de retour; le paramètre; l'adresse de retour.

Entrons la commande suivante

```
$ulimit -s
8192
```

On obtient donc que la taille de la pile d'appel est de 8 octets.

Avec `somme 1 000 000` on empile donc $1 000 000 + 1$ stack frame
d'au moins $4$ bytes. On comprend que la taille allouée à la pile soit dépassée.

### Organisation de la mémoire en OCaml

La zone de données statiques contient les constantes présentes dans le code source du programme comme les
chaînes de caractères.

Toutes les valeurs en OCaml sont des pointeurs sur des données dans le tas. Les var. locales de la pile ne peuvent être que d'une des 4 catégories
suivantes :

- les entiers `int` , les caractères `char`
- le type `unit`
- les constructeurs sans arguments dans les types sommes (ils sont représentés en interne par des entiers)

<p align="center"><img src="/images/ocamlrec1.png"></p>
_Répartition de la mémoire lors de l'exécution d'un programme_

### Ramasse miette

En OCaml, la libération des zones mémoires est faite
automatiquement par un algorithme appelé _ramasse miette_ (en anglais Garbage Collector _GC_).

Le GC agit pendant l'exécution du programme. Il détermine quelles
zones mémoires dans le tas sont devenues inutiles. Il les libère
automatiquement.

### Gestion de la mémoire par l'OS

L'OS gère l'espace occupé par les segments durant l'exécution du
programme. C'est lui qui assure l'intégrité de ces différents segments et qui empêche par exemple que la pile écrive dans le tas.

S'il est besoin d'allouer plus de mémoire pour le tas que ce qui était prévu, c'est le mécanisme de _mémoire virtuelle_ du système qui s'en charge.

Il faut donner au programmeur l'impression que la RAM est infinie. Si la RAM vient à manquer, l'OS est capable d'utiliser automatiquement d'autres zones de stockage comme le disque dur ou une clé USB (évidemment, les performances se dégradent alors).

### Remédiation en force brute

On décide d'augmenter la taille de la pile :

```
$ulimit -s unlimited
$ulimit -s
unlimited
```

Cela permer d'augmenter la taille de la pile à la taille maximale du système d'exploitation. Sous $Linux$, cette taille est illimité (mais l'administrateur peut imposer une limite). Sous $MacOs$, la limite est de $65$ Mo.

Il est maintenant possible d'exécuter le programme `./somme`. Mais
l'exécution est lente du fait des empilements/dépilements successifs de stack frame.

## Récursion terminale

### Appel terminal

La _récursivité terminale_ est une forme particulière de récursivité pouvant être optimisée afin de ne pas consommer de mémoire dans la pile.

Dans le corps d'une fonction, un appel est _terminale_ s'il est la
dernière opération effectuée par la fonction.

Les fonctions suivantes font un appel terminal à `g`

```Ocaml linenums="1"
1 let f1 x = g x
2 let f2 x = if ... then g x else ...
3 let f3 x = let y = ... in g y
4 let f4 x = match x with
5 | ... -> ...
6 | ... -> g x
7 | _ -> ...
```

Appels à g non terminaux :

```Ocaml linenums="1"
1 let f5 x = x + g x
2 let f6 x = let y = g x in y+1
```

### Définition

!!!quote ""
    **Définition**
    Une fonction est dite _récursive terminale_ si tous les appels récursifs dans sa définition sont en position terminale.

!!!tip ""
    **Intérêt**
    Il n'est plus nécessaire d'empiler les stack frame lors des appels récursifs. La stack frame de départ suffit.

### Exemple 

`let rec f x = if x = 0 then 10 else f (x-1)`
Gestion de la pile d'appel :

- Pour `f 2` La case réservée pour la valeur retour est vide, l'argument `x` contient $2$.
- L'appel `f 1` peut utiliser la même stack frame que `f 2` car la
valeur 2 contenue dans la case `x` , n'est plus utilisée par la suite. On met donc 1 dans `x` .
- L'appel `f 0` utilise encore la même stack frame en mettant 0 dans la case `x`. <p style='color:crimson'>La valeur de retour (10) est mise dans <code style='color:crimson'>ret</code></p> car c'est la valeur qui est finalement renvoyée par le 1er appel (`f 2`)

<p align="center"><img src="/images/ocamlrec2.png"></p>
_Pile de mémoire lors de l'appel de `f 2`_

### Fonction somme : version en récursion terminale

On utilise une fonction auxiliaire `sum` à deux paramètres : l'entier ourant `x` et un accumulateur `acc`.

L'accumulateur grossit au fil des appels récursifs internes et il contient la valeur voulue lorsque `x` devient nul. On renvoie donc `acc`.

Code :
```Ocaml linenums="1"
let somme x =
    let rec sum x acc = (* fonction auxiliaire *)
        if x = 0 then acc else sum (x-1) acc + x
    in sum x 0

let _ = let v = somme 1_000_000 in Printf.printf "%d" v
```

D'une façon générale, une fonction auxiliaire est utile lorsqu'on a
besoin de plus de paramètres que ceux initialement prévus.
