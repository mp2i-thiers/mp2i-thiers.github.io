# Rencontre au milieu

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédit"

    * Cette page sur [Quora](https://www.quora.com/What-is-meet-in-the-middle-algorithm-w-r-t-competitive-programming)

## Position du problème

Considérons un ensemble $E$ de $n$ nombres distincts et un entier $S$.

On cherche la plus grande somme d'éléments de $E$ dont la valeur est plus petite que $S$.

On peut faire une recherche en force brute. Il s'agit de déterminer la somme pour tous les sous-ensembles de $E$ et de la comparer avec $S$ puis de prendre la plus grande somme plus petite que $S$.

Malheureusement, il y a $2^n$ sous-ensemble de $E$, donc la complexité
de cette approche serait en $O(2^n)$ au moins (c'est sans compter le coût de la somme elle-même).

## Présentation

_Meet-in-the-middle_ est une technique de recherche appliquée lorsque l'entrée est petite (par exemple $40$ nombres) mais pas assez petite pour que la recherche en force brute soit envisageable ($2^{40}$ est quand même assez gros).

Comme _diviser pour régner_, le problème est divisé en $2$ sous-problèmes. Cependant ici, le travail ne se fait pas récursivement. On travaille sur deux moitiés du tableau, on ne divise pas le tableau davantage.

## Algorithme

!!!note "Notation"
    $n/2$ : division euclidienne par $2$.

Considérons un ensemble de $n$ nombres.

Diviser l'ensemble des entiers en deux sous-ensemble $A$, $B$. $A$ contient $n/2$ éléments et $B$ les autres.

Chercher toutes les sommes possibles d'éléments de $A$ et mettre le résultat dans un tableau $X$ (resp. $B$, tableau $Y$ ). Puisqu'il y a $n/2$ éléments dans $A$, alors la complexité de cette opération est majorée par $O\left (\frac{n}{2} \times 2 ^{n/2} \right )$.

Fusionner les $2$ sous-problèmes : trouver les couples $(x, y) ∈ X × Y$ tels que $x + y ≤ S$ :

- En force brute, puisque $X, Y$ sont de taille en gros $n/2$, on obtient par une double boucle toutes les sommes en $O \left ((2 ^{n/2})^2 \right ) = O(2^n)$ : $\color{red}\textsf{pas mieux que la force brute initiale}$.
- Dans un premier temps on trie $Y$ (et pas $X$) en $O\left (\frac{n}{2} \times 2 ^{n/2} \right )$
- On mémorise la meilleure somme $m$ trouvée jusqu'ici : initialisation $m ← 0$.
- Pour chaque $x ∈ X$, on recherche par dichotomie le plus grand $y ∈ Y$ tel que $x + y ≤ S$. Si $x + y > m$, alors $m ← x + y$.
- On fait donc $|X| \simeq  2^{n/2}$ fois une recherche en $\log(|Y|) \simeq  log(2^{n/2})$.
Comme $log(2^{n/2}) = \frac{n}{2}\log(2)$, La complexité totale est en $O \left ((2 ^{n/2})^2 \right ) = O(n2^{n/2})$, coûteuse mais moins que $O(2^n)$

## Rappels utiles pour l'implantation

$1L<<52$ calcule $2^{52}$ mais caste d'abord $1$ en un `long`
Le codage binaire d'un entier $i$ représente un ensemble $E_i$ d'entiers.Exemple $i = 100101$ représente $E_ i {0, 2, 5}$.
$i$ $\&$ $1l << j$ cherche si le $j$-ième bit de $i$ est à $1$, donc si $j ∈ E_i$
