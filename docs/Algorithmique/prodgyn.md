# Programmation dynamique

!!! danger
    Ce cours n'a pas été entièrement reverifié après le passage du programme. Pensez à supprimer ce message si vous avez reverifié ce cours

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par
    Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce
    site à pour seul but d’être plus compréhensible pendant les périodes de
    révision que des diaporamas.

## Présentation

### Introduction

La résolution d’un problème peut parfois se faire en le décomposant en  sous-problèmes. Dans cette approche, les solutions aux sous-problèmes sont  ensuite combinées pour construire la solution au problème initial.  

- si les sous-problèmes sont indépendants les uns des autres (exemple :  décomposition en sous-ensembles disjoints comme pour le tri fusion),  on parle de méthode diviser pour régner
- si les sous-problèmes sont dépendants (exemple : si un même calcul  -avec les mêmes paramètres- est fait par chaque sous-problème), on  parle de programmation dynamique.  

### Historique

!!!definition "Programmation dynamique"
    Processus de résolution de problèmes où on trouve les meilleures décisions les unes après les autres.  Le terme était utilisé par le mathématicien Richard Bellman dès les années 40.  

En 1953, Bellman en donne la définition moderne, où les décisions à  prendre sont ordonnées par sous-problèmes. Le domaine a alors été reconnu par l’Institute of Electrical and  Electronics Engineers (IEEE) comme un sujet d’analyse de systèmes et  d’ingénierie.  

### Diviser pour régner

La méthode Diviser pour régner est un cas particulier de  programmation dynamique.  
On décompose encore un problème principal en sous-problèmes.  Cependant, les sous-problèmes sont ici indépendants les uns des autres  ce qui facilite la tâche du programmeur.  

!!!note "Principe"
    On **divise** en réduisant un problème en sous-problèmes du même type et  qui ne se chevauchent pas.  Puis on **règne** en résolvant ces sous-problèmes. Il reste à **rassembler** les solutions des sous-problèmes pour obtenir une  soution au problème initial.  

### Cadre d’application

La programmation dynamique est envisagée si le problème présente la propriété de sous-structure optimale et si les chevauchements de
sous-problèmes doivent être gérés.

#### Vocabulaire

!!!definition "Sous-structure optimale"
    Se dit d’un problème qu’on peut résoudre en le décomposant en sous-problèmes du même type,  eux-mêmes résolubles récursivement. 

!!!definition "Chevauchement de sous-problèmes"
    Se dit si des sous-problèmes ne sont pas indépendants et doivent être résolus plusieurs  fois.

En général on envisage tous les sous-problèmes comme dans une  recherche exhaustive mais on prend ses précautions pour ne pas avoir à  les résoudre tous :  

- soit parce que certains sont inutiles (ex : recherche dichotomique)  
- soit parce qu’ils ont déjà été rencontrés et résolus (ex : mémo¨ısation  dans le calcul des suites de Fibonacci)  

### Principe d’optimalité

La programmation dynamique s’applique à des problèmes  d’optimisations : il s’agit souvent d’optimiser le coût d’une suite de  décisions.  Cette suite de décisions correspond à un découpage du problème en  sous-problèmes :  

- On calcule les solutions optimales successives comme pour un algorithme  glouton à des sous problèmes liés par une relation de récurrence.  
- Puis, c’est la combinaison de ces solutions qui produit la solution au  problème initial.  

**Principe d’optimalité de Bellman :** une solution optimale pour un  problème présentant la propriété de sous-structure optimale est la  combinaison de solutions optimales locales pour les sous-problèmes.  

### Programmation dynamique et graphes

!!!note ""
    - Un théorème général énonce que tout algorithme de programmation  dynamique peut se ramener à la recherche du plus court chemin dans  un graphe.  
    - Or, les techniques de recherche heuristique basées sur l’algorithme A* permettent d’exploiter les propriétés spécifiques d’un problème pour  gagner en temps de calcul.  
    - Autrement dit, il est souvent plus avantageux d’exploiter un algorithme  A* que d’utiliser la programmation dynamique.  

## Exemples

### Suites de Fibonacci

Voirt TD dédié.  

### Partition équilibrée d'un tableau d'entiers positifs

#### Présentation du problème

On dispose d’un (multi) ensemble d’entiers positifs $E$.

On souhaite déterminer une partition de E en deux sous-ensembles $E_1, E_2$ tels que  

- $E_1 \cup E_2 = E$; $E_1 \cap E_2 = \emptyset$(partition)

- La somme des éléments de $E_1$ et celle de $E_2$ sont les plus proches possibles. Cela signifie que 
$$
|\sum_{x \in E_1} x - \sum_{y \in E_2} y| = min(\\{ |\sum_{x \in E'} x - \sum_{y \in E''} y| \text{ avec (E', E'') une partition de E} \\})
$$

On note S la somme des éléments de $E$ et S(A) la somme des éléments  d’un sous-ensemble A. S/2 désigne la division euclidienne de S par 2.  

#### Approche gloutonne

Pour E = {$e, . . . , e_n$} :  
On gère deux sous-ensembles E, E initialisés resp. en {$e_1$}, ∅.  
On place les éléments suivants dans E un à un jusqu’à ce que  §($E_2$) > §($E_1$). Les éléments suivants sont alors placés dans $E_1$ etc.  
**Malheureusement, même en triant les éléments de E , la solution  fournie n’est pas toujours optimale.  **

!!! example "Exercice"
    Implanter cet algorithme. Donner sa complexité. Exhiber un exemple où la solution n’est pas optimale.

#### Algorithme basé sur la demi-somme

On dispose d’un ensemble d’entiers positifs E .  
Si $E_1$ et $E_2$ réalisent une partition équilibrée de E , quitte à les  échanger, on peut supposer S($E_1$) ≤ S($E_2$).  
Comme S($E_1$) + S($E_2$) = S, on a $S(E_1) ≤ \frac{S}{2} \leq S(E_2)$ les éléments sont entiers, on obtient $S(E_1) ≤ S/2 ≤ (E_2)$.  

#### Distance à la demi-somme

Soit A ⊂ E tel que :  

Si S(A) ≤ S/2, soit (F, G) partition de E telle que S(F) ≤ S(G). Alors S(F) ≤ S/2.  
Puisque A réalise la meilleure distance à S/2 :  
$$S(F ) ≤ S(A) et S(E \ A) ≤ S(G ) $$
Et donc $|S(E \ A) − S(A)| ≤ |S(G ) − S(F )|$

De même si $S(A) ≥ S/2$.

On en déduit que **(A, E \ A) réalise une  partition équilibrée de E .**  

#### Solution par programmation dynamique  

##### Méthode descendante

**Maths pas encore faites**

On cherche (E, E), partition équilibrée de E  La remarque 2 du slide précédent suggère de travailler avec a) la  demi-somme des éléments de E et b) l’ensemble E (puisqu’on trouve  alors E facilement).  

Agorithme récursif : On gère un ensemble E et la demi-somme  e∈E e des éléments de E . On cherche à construire E.  S =  Prendre e ∈ E et calculer la distance |S(E) − S| dans 2 cas :  
{sum symbol}  
    - En mettant e dans E. Cela revient à ajouter e à la solution au problème  lorsque E = E \ {e} et S = S − e  
    - En ne mettant pas e dans E. On calcule la solution au problème lorsque  E \ {e} et S est inchangé.  
Choisir la meilleure des 2 options : celle qui améliore la distance de la  somme des éléments de E à la demi-somme S.  

!!! example "Exercice"
    Les multi-ensemble de nombres sont implémentés par des listes.
    1. Ecrire une fonction `partition : int list -> int list` qui  renvoie un ensemble A ⊂ E telle que |S(A) − S/2| soit minimal.  On ne cherche pas à mémoriser les résultats intermédiaires.  
    2. Estimer la complexité (on peut se contenter de la minorer) en fonction de |E|.  

##### Méthode ascendante avec tableau de bouléen

E = {e, . . . , en−}, multi-ensemble de nombres entiers positifs,  S = {sum symbol}  
e∈E e.  
On construit une matrice de bouléens T de taille (n + 1) × (S + 1)  On fait en sorte que le coeﬃcient Ti,j (i ≥ 0, j ≥ 0) soit vrai si et  seulement si il existe un sous-ensemble de {ek | k ≤ i − 1} dont la  somme des éléments vaut j.  On cherche une relation de récurrence qui construit Ti,j connaissant les  Ti',j' pour (i', j') < (i, j) au sens lexicographique.  

Ligne 0 : Pour k ≥ 0, T,k désigne la possibilité pour que la somme  des éléments de l’ensemble {ek | k ≤ 0 − 1} = ∅ vale k. Ainsi T,k est  faux sauf si k = 0.  Pour i ≥ 0, Ti, j est vrai si et seulement si il existe un sous-ensemble  de {e, . . . , ei } dont la somme des éléments vaut j. Ceci se décompose  en :  
Ou bien il existe un sous-ensemble de {e, . . . , ei−} dont la somme des  éléments vaut j. Ceci est équivalent à "Ti,j est vrai".  Ou bien, il existe un sous-ensemble de {e, . . . , ei−} dont la somme des  éléments vaut j − ei (chose impossible si j < ei ). Ceci est équivalent à  "Ti,j−ei est vrai" lorsque j ≥ ei .  
Relation de récurrence : pour i ≥ 1, j ≥ 0, Ti,j est équivalent à :  
Ti,j ou (j ≥ ei et Ti,j−ei )  

#### Code : construction du tableau de bouléens

!!! example "Exercice"
    Ecrire la fonction `tableau : int array -> bool array array * int` telle que `tableau e` renvoie le tuple T,m où t est décrit au transparent précédent et m est la plus grande somme d’éléments de E plus petite que S/2. Evaluer la complexité de votre fonction.  

#### Construction de la partition équilibrée

Une fois trouvés le tableau de bouléens T et la somme m, on construit E  récursivement en lui ajoutant ou pas l’élément courant.  
On part de Tn,m (qui est Vrai) et E = ∅.  On parcourt une suite (Ti,mi )in,n−,... de coeﬃcients avec mi ↓ et  mn = m.  C’est donc une suite dont les indices sont positifs et décroissants  strictement au sens lexicographique, ce qui assure la terminaison de la  récursion.  Invariant "Ti,mi est vrai". Critère de déplacement dans la matrice :  
Si Ti−,mi est vrai, alors on peut trouver un sous-ensemble de  {e, . . . , ei−} qui a pour somme mi . Donc E peut ne pas contenir  ei− : il reste inchangé.  Sinon c’est que Ti−,mi −ei− est vrai. On peut trouver un sous-ensemble  de {e, . . . , ei−} qui a pour somme m − ei−. On ajoute donc ei− à E.  

!!! example "Exercice"
    Ecrire la fonction `partitition : int array -> int array` telle que `partitition e` renvoie sous forme de tableau l’ensemble E. Donner sa complexité.
