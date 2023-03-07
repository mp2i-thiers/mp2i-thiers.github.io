#  Arbre Binaire de Recherche
##   AutoCours

``` mermaid
graph TB; 
    A((1))-->B((2))
    A-->C((3))
    B-->D((4))
    B-->E((5))
    C-->F((6))
    C-->G((7))
    D-->H((8))
    D-->I((9))
    E-->J((10))
    E-->K((11))
    F-->L((12))
    F-->M((13))
    G-->N((14))
    G-->O((15))
```

Ce cours a été automatiquement traduit des transparents de M.Noyer par
Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce
site à pour seul but d’être plus compréhensible pendant les périodes de
révision que des diaporamas.

???+ abstract "Sommaire"
    - Arbre binaire de recherche
        * ABR
        * Suppression
    - Dictionnaire
    - Tas
        * Supprimer un élément
        * Création
    - Files de priorités (une application des tas)
    - Arbre binaire de recherche

!!! tip "Crédits"
    - "Option informatique MPSI, MP/MP*", Roger Mansuy, paru chez Vuibert.
    - Wikipédia : Tas, Tas binaires, Tri par tas
    - OpenClassRoom Arbres binaires de recherche

!!! warning "Définition"
    Binary search trees (BST)
    
    Un arbre binaire de recherche (ABR) sur un type totalement ordonné est
    un arbre binaire tel que pour tout nœud interne, les étiquettes
    apparaissant dans le sous-arbre gauche (resp.droit) sont strictement¹
    inférieures (resp. supérieures) à celle la racine.
    1. Selon la mise en œuvre de l’ABR, on pourra interdire ou non des clés de valeur égale.

_Figure – Un ABR_

``` mermaid
graph TB; 
    A((8))-->B((3))
    A-->C((10))
    B-->D((1))
    B-->E((6))
    C-->H(( ))
    C-->G((14))
    E-->J((4))
    E-->K((7))
    G-->O((13))
    G-->F(( ))
    style H display:none
    style F display:none
```

Les étiquettes de gauche ont des valeurs plus petites que
celle de la racine, celle de droite sont plus grandes


### DSF
On peut facilement récupérer les clés d’un arbre binaire de recherche dans
l’ordre croissant en réalisant un parcours en profondeur infixe. Contre exemple:

_Figure – Un arbre binaire qui n’est pas un ABR_
``` mermaid
graph TB; 
    A((6))-->B((4))
    A-->C((5))
```

_Figure – Un ABR pour représenter_ ```[4,5,6]```
``` mermaid
graph TB; 
    A((6))-->B((5))
    A -->Z(( ))
    B-->C((4))
    B-->W(( ))
    style Z display:none
    style W display:none
```
_Figure – Un ABR équilibré pour représenter_ ```[4,5,6]```
``` mermaid
graph TB; 
    A((5))-->B((6))
    A-->C((4))
```
_Figure – Un ABR pour représenter_ ```[4,5,6]```

``` mermaid
graph TB; 
    A -->Z(( ))
    A((4))-->B((5))
    B-->W(( ))
    B-->C((6))
    style Z display:none
    style W display:none
```

Passage liste ordonnée/ABR
A une liste ordonnée correspondent plusieurs ABR.

### Type de données
Nous utiliserons le type suivant:

``` Ocaml linenums="1" title="Type Arbre"
--8<-- "Type Arbre"

type 'a tree =
    |Nil
    |N of 'a * 'a tree * 'a tree;;
```
!!! note ""
    * Une feuille est implémentée par ```N(x, Nil, Nil)```,
    * une nœud d’arité 1 par ```N(x, t, Nil)``` ou ```N(x, Nil, t)``` avec x et t de type
    convenable.
    * Une telle structure modélise les arbres binaires, pas seulement les ABR.
    C’est lors de la création d’un arbre que nous ferons attention à ce qu’il
    respecte la contrainte d’ordre.

###     Primitives
!!! note ""
    * Une fonction de création d’un ABR à partir d’une liste.
    * Une fonction d’insertion d’une valeur dans un ABR.
    * Une fonction de recherche d’une valeur dans un arbre.
    * Une fonction de suppression d’une valeur dans un arbre.

``` Ocaml linenums="1" title="Insérer sous une feuille"
--8<-- "Insérer sous une feuille"

let rec insert x t = match t with
    | Nil -> N (x , Nil , Nil )
    | N (y ,g , d ) when x < y -> N (y , insert x g , d )
    | N (y , g , d ) when x >y -> N (y ,g , insert x d )
    | _ -> t (* pas de doublon *) ;; (* le laisser sinon Warning 8. *)
```
!!! note ""
    * Le choix qui est fait ici est celui d’un ABR sans étiquettes de mêmes valeurs (pas de doublon).
    * On insère la nouvelle valeur sous une feuille.
    * On pourrait aussi insérer x à la racine:
        * "Couper" l’arbre en deux sous-ABR g , d contenant respectivement les éléments plus petits et plus grands que x.
        * construire l’arbre ```N(x, g, d)```

### Complexité de l’insertion sous une feuille
Description informelle pour un arbre de hauteur $h$ à $n$ nœuds.

!!! note ""
    * On descend le long d’une branche jusqu’à la feuille.
    * Il y a $O(h)$ pour cette descente. Pour chaque nœud interne, les opérations hors appel récursif sont à coût constant.
    * Dans le cas d’arrêt, le coût est également constant.
    * Donc complexité en $O(h)$.
    * On comprend l’intérêt de "contrôler" $h$. En pratique, on essaye de conserver $h \leq C \log(n)$ pour une certaine constante. Si on arrive à maintenir cette contrainte au fil des insertions, on obtient un arbre équilibré).

### Création
``` Ocaml linenums="1" title="Créer un ABR à partir d’une liste "
--8<-- "Créer un ABR à partir d’une liste "

let rec create l = match l with
    | [] -> Nil
    | e :: q -> insert e ( create q ) ;;

(*test*)
create ([1;4;2;3]) ;;
```

Figure – ABR obtenu par ```create([1;4;2;3]);;```

``` mermaid
graph TB; 
    A((3))-->B((2))
    A-->C((4))
    B-->D((1))
    B-->E(( ))
    style E display:none
```

### Liste triée
Si la liste est déjà triée, on obtient une liste chaînée.

Figure – ABR obtenu par ```create([1;4;2;3]);;```

``` mermaid
graph TB; 
    A((4))-->B((3))
    A -->Z(( ))
    B-->C((2))
    B-->W(( ))
    C-->D((1))
    C-->R(( ))
    style Z display:none
    style W display:none
    style R display:none
```
### Complexité de la création

!!! note ""
    * Si la liste est déjà triée, l’ABR n’a qu’une branche.
    * Pour une liste triée de longueur $n$, la complexité vérifie une relation de la forme

    $$ \begin{align}
    c_n &= c_{n−1} + \underbrace{n}_{\text{cpx insertion}} + \underbrace{c}_{\text{autres opération}}  \\
    &= c_{n−2} + 2c + n +  \underbrace{n-1}_{\text{cpx insertion}} = ... \\
    &= c_0 + nc + \sum\limits_{k=1}^{n}k \\
    &= \theta(n²)
    \end{align}$$

    * On voit l’intérêt de maintenir un certain équilibre dans l’arbre au moment
    des insertions.

### Utilité de l’équilibrage des arbres
!!! note ""
    * A priori, cela ne semble pas si grave d’avoir une liste chaînée et non un
    bel arbre binaire "équilibré".
    * Mais les opérations sur les ABR (insertion, suppression, recherche) ont
    une complexité au pire qui dépend de la hauteur...
    * ... d’où l’idée qu’il faut limiter la hauteur des arbres lors de l’insertion.
    * C’est le principe du rééquilibrage des ABR.

### Création d’un arbre équilibré à partir d’une liste

!!! note ""
    * Un arbre A est dit équilibré lorsque $h(A) = O(\log_{}(|A|))$
    * Exemple arbres AVL: pour chaque nœud, la différence entre les hauteurs de ses fils (l’un éventuellement vide) est $0$, $1$ ou $−1$.
    * On peut établir que, dans un AVL de taille ${|A| = n} \implies {\frac{3}{2}\log_{2}(n+ 1) \geq h(A)}$
    (cf TD).
    * Autre exemple : arbres Rouge-Noir (cf TD).
    * Dans un arbre équilibré, insérer x se fait en $O(\log_{2}(n))$ appels internes au
    pire plus un nombre borné d’autres opérations en $\theta(1)$ .
    * Si on maintient le caractère équilibré (le rééquilibrage a un coût
    logarithmique -admis-), le coût de la création à partir d’une liste est donc
    de l’ordre de $\theta(\sum\limits_{k=1}^{n}\log_{2}(k)) = \theta(\log_{2}(n!)) = \theta(n\log_{2}(n))$

### Rechercher
```Ocaml linenums="1"
let rec search x t = match t with
(* cherche x dans t *)
| Nil -> false
| N (y ,_ , _ ) when y = x - > true
| N (y ,g , _ ) when x < y -> search x g
| N (y ,_ , d ) when x > y -> search x d
| _ -> false ;; (* le laisser sinon ’ this pattern - matching is not
exhaustive . ’ *)
```

!!! note ""
    * Si x est égal à la racine de t, c’est bon. Sinon on cherche récursivementdans le sous arbre gauche lorsque x es plus petit que la racine, et à droite sinon.
    * Si x est à la profondeur k, il y a k appels internes pour le trouver.
    * Si x n’est pas dans l’arbre, il y a au pire $h(t)$ appels internes.

### Suppression 
#### Opération de fusion
On veut "fusionner" deux ABR G et D tels que les étiquettes de G sont toutes
plus petites que celles de D. Ceci afin d’obtenir un ABR unique construit à partir
des nœuds des deux arbres.

```Ocaml linenums="1"
let rec merge a b = match a , b with
(* fusion qui privilégie l ’arbre gauche *)
| Nil , t | t , Nil -> t
| N (x , ga , da ) , N (y , gb , db ) -> (* on a max a <= min b *)
    N (x , ga , N (y , merge da gb , db ) ) ;; 
```
Dans cette fusion, la racine de l’arbre gauche devient systématiquement la
racine de l’arbre retourné. On aurait pu privilégier l’arbre droit.
```Ocaml linenums="1"
let a1 = N (3 , N (2 , Nil , Nil ) ,N (4 , Nil , Nil ) ) in let a2 = N (30 , N
(20 , Nil , Nil ) ,N (40 , Nil , Nil ) ) in merge a1 a2 ;;

(* Et on obtient : *)
N (3, N (2, Nil, Nil),
N (30, N (4 , Nil, N (20, Nil, Nil)),
N (40, Nil, Nil)))
```

_Figure – En rouge, la fusion des deux arbres bleus_

#### Correction de la fusion
!!! note "Preuve par induction" 
    On montre que si on fusionne deux ABR a, b tels que max
    a ≤ min b, alors le nouvel arbre formé est un ABR contenant toutes les
    étiquettes de a, b.

    * __Cas de base :__ si un des deux arbres est vide, on renvoie l’autre. C’est un
    ABR par hyp. et il contient bien toutes les étiquettes des deux arbres.
    * __Hérédité.__ Soient ```a = N(x, ga, da)``` et ```b = N(y , gb, db)``` non vides avec max a $\leq$ min b.
        * Notre hypothèse d’induction (HI) est que la fusion d’un sous-terme
        immédiats de a avec un sous-terme immédiat de b est un ABR
        contenant toutes les étiquettes de ses deux sous-termes.
        * La racine du nouvel arbre est x et elle est plus grande que toutes les
        étiquettes de son fils gauche ga (puisque a est un ABR).
        * Le fils droit est ```d = N(y , merge(da, gb), db)```. Par (HI) la fusion
        ```merge(da, gb)``` est un ABR avec toutes les étiquettes de da, gb.
        * Comme y est supérieur aux étiquettes de gb, elles-mêmes plus
        grandes que celles de da, il vient que y est supérieur aux étiquettes
        de l’ABR résultant de la fusion.
        * Or y est inférieur aux étiquettes de db puisque b est un ABR. Donc ```d =
        N(y , merge(da, gb), db)``` est un ABR dont toutes les étiquettes sont aumoins plus grandes que celles de la plus petite de da. De plus il contient y et toutes les étiquettes de da, gb, db.
    * Donc x est plus petit que les étiquettes de d (qui est un ABR) et plus
    grand que celles de ga (qui est un ABR) donc ```N(x, ga, d)``` est un ABR avec
    toutes les étiquettes de a, b.

#### Complexité de la fusion
* Soient deux ABR g , d :
    Il y a au plus autant d’appels internes que le minimum de hauteur des
    sous arbres.
* La complexité est en $\theta(\min(h(g), h(d)))$ 

```Ocaml linenums="1"
let rec remove x t = match t with (* on commence par chercher x dans t *)
| Nil -> failwith " x not found " (* on n’a pas trouvé x *)
| N (y ,g , d ) when x < y - > N (y , remove x g , d )
| N (y ,g , d ) when x > y - > N (y , g , remove x d ) (* A partir d ’ ici
, on a trouv é x *)
| N (y ,g , d ) when y = x -> merge g d (* fusion des deux sous - arbres
*)
| _ - > failwith " ne devrait pas arriver " ;;
```
!!! tip "Remarques"
    Lorsque le nœud A d’étiquette x n’a qu’un fils, celui-ci prend la place de son
    père (on le "remonte").
    Si A est une feuille, on se contente de la supprimer (la fusion met Empty à la
    place de A).

### Dictionnaire
!!! warning "Définition"
    Tableau associatif (aussi appelé dictionnaire ou table d’association) : type
    de données associant à un ensemble de clefs un ensemble correspondant
    de valeurs. Chaque clef est associée à une valeur unique : un dictionnaire
    correspond donc à une application en mathématiques. Il peut être vu
    comme une généralisation du tableau dont les indices ne serait pas
    nécessairement des entiers.

__Opérations usuellement fournies:__

!!! note ""
    * ajout : association d’une nouvelle valeur à une nouvelle clef ;
    * modification : association d’une nouvelle valeur à une ancienne clef ;
    * suppression : suppression d’une clef ;
    * recherche : détermination de la valeur associée à une clef, si elle existe.

    Les dictionnaires peuvent être implémentés concrètement par des ABR. Ce
    sont alors des données persistantes. L’ensemble des clés doit être totalement
    ordonné. Les étiquettes des nœuds de l’ABR sont des couples (clés, valeurs) et
    le placement d’un nœud dans l’arbre est fait selon sa clé et non sur sa valeur.
    En OCAML, explorons 3 façons de définir les dictionnaires.

#### Dictionnaires par liste de paires
Méthode la plus simple. Persistante.

```Ocaml
(* dictionnaires par liste de paires *)
let m = [ " Sally Smart " , " 555 -9999 " ;
" John Doe " , " 555 -1212 " ;
" J . Random Hacker " , " 553 -1337 " ];;
List . assoc " John Doe " m ;;
(* # - : string = "555 -1212" *)
```
#### Dictionnaires par AVL

!!! note ""
    * Les arbres AVL ont été historiquement les premiers arbres binaires de
    recherche automatiquement équilibrés.
    * Dans un arbre AVL, les hauteurs des deux sous-arbres d’un même nœud
    diffèrent au plus de un.
    * La recherche, l’insertion et la suppression sont toutes en $O(\log_{2}(n))$ dans
    le pire des cas.
    * L’insertion et la suppression nécessitent d’effectuer des rotations
    (opérations de rééquilibrage).
    Le nom AVL vient des deux inventeurs Georgii Adelson-Velsky et Evguenii
    Landis (1962).

```Ocaml
(* dictionnaire applicatif réalisé par arbres équilibrés *)
include ( Map . Make ( String ) ) ;;
let m = empty
| > add " Sally Smart " " 555 -9999 "
| > add " John Doe " " 555 -1212 "
| > add " J . Random Hacker " " 553 -1337 " ;;
find " John Doe " m ;;
(* # - : string = "555 -1212" *)
```
Structure persistante basée sur les arbres équilibrés.
Ajout/Suppression/Recherche en temps logarithmique.

#### Dictionnaires par table de hachage

```Ocaml
(* dictionnaires par table de hachage polymorphe *)
let m = Hashtbl.create 3;; (* taille attendue 3 , ç a peut changer *)
Hashtbl.add m " Sally Smart " " 555 -9999 " ;
Hashtbl.add m " John Doe " " 555 -1212 " ;
Hashtbl.add m " J . Random Hacker " " 553 -1337 " ;;
Hashtbl.find m " John Doe " ;;
(* # - : string = "555 -1212" *)
```

Structure impérative. Modifications en place. Ajout/Suppression/Recherche en
temps constant (en moyenne pour ajout, et pour la recherche, ça dépend en
fait de la fonction de hash).


### Tas

!!! warning "Définition"
    Un tas de hauteur h est un arbre binaire vérifiant :

    * __Condition d’ordre :__ les fils d’un nœud ont une étiquette inférieure ou
    égale à celle du père.
    * __Condition de structure:__ Un tas est complet gauche :
        * Tous les niveaux sont remplis sauf éventuellement le dernier.
        * Le dernier niveau (éventuellement incomplet) est rempli sans trou enpartant de la gauche.

#### Précisions et conséquences
• Un arbre du type précédent est dit de type tas-max (racine=max).
• tas-min : l’étiquette du père est plus petite que celle des fils.
Les branches sont toutes de longueur h ou h − 1, enlever les feuilles de
profondeur h donne un arbre parfait, les nœuds internes de profondeur h − 1
d’arité ≥ 1 sont à gauche des feuilles de profondeur h − 1, si il y a un nœud
interne de profondeur h − 1 avec un seul fils, son fils est une feuille et c’est le
dernier nœud dans le parcours en largeur.
Exemple

Figure – Un tas. Si on enlève les feuilles de profondeur 2, l’arbre est parfait
(cf. fig. [13] pour la représentation en array)
Tous les niveaux sont remplies, sauf le dernier, lequel est partiellement rempli
en commençant par la gauche.
Contre-exemples
Les arbres suivants ne sont pas des tas :
Figure – Un nœud de hauteur h − 1 et d’arité 1 est à gauche d’un nœud de
hauteur h − 1 d’arité 2
Figure – Un père a un fils d’étiquette plus grande que la sienne
Page 13/30
Scpr de nono le 03/03/23 à 17:46
AutoCours - Arbre binaire de recherche
Hauteur d’un arbre complet gauche
Soit A un arbre complet gauche à n nœuds et de hauteur p
• L’avant dernier niveau (qui correspond à un arbre parfait) est rempli. Et
le dernier niveau contient au moins une feuille.
2 p −1< n≤ 2p+1 −1⏟
taille min. d’un arbre parfait plus gros que A
⇒ 2 p ≤ n<2p+1
⇒ p ≤ log2 n< p+1
⌊log2 n⌋= p
Exemple
Figure – Un tableau représentant un tas
BFS
Observer le lien entre le parcours en largeur et la représentation en
tableau.
Contrexemples
Les arbres suivants ne sont pas des tas :


