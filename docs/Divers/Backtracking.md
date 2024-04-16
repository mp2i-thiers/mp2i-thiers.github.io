# Backtracking

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d’être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"
    Un cours de Solène Mirliaz ;
    Cette page de [Wikipedia](https://fr.wikipedia.org/wiki/Retour_sur_trace)

## Problème d'exploration

!!!quote ""
    **Définition**
    Un _problème d’exploration_ (ou de _recherche_) est un problème
    algorithmique $P$ donné par une relation binaire
    $R⊂E ×(S \sqcup  \{None\})$ telle qu’il existe deux sous-ensembles
    notés $E^+$, $E^−$ de $E$ formant une partition de $E$ et vérifiant :
    $R⊂(E^+ \times S )\sqcup (E^− \times \{None\})$

!!!tip ""
    **Remarque**
    - Les éléments de $E$ sont appelés des entrées et ceux de $S$ des solutions ;
    - Le fait que l’union soit disjointe interdit d’avoir une entrée $e$ et une solution $s$ telles que $(e, s ) ∈R$ et $(e, None) ∈R$.


## Vocabulaire : solution

Avec les notations de la définition précédente :

- si $(e, s ) ∈E ×S$ , on dit que $s$ est une _solution de l’entrée_ $e$;
- si $(e, Node) ∈R$, on dit que l’entrée $e$ _n’a pas de solution_.


## Backtracking : présentation informelle

!!!quote ""
    **Définition**
    Le _backtracking_ (ou _retour sur trace_) est une classe d’algorithmes cherchant la solution de problèmes d’exploration.

!!!example ""
    Notamment les problèmes avec _contraintes de satisfactions_ comme le **Sudoku** où les $N$-reines sont résolubles par backtracking.

Le backtracking construit incrémentalement des _candidats-solutions partiels_ qui sont abandonnés dès lors qu’on établit qu’ils ne peuvent pas être complétés en une solution.

Conceptuellement, les candidats-solutions partiels sont représentés comme des nœuds d’une structure arborescente : _l’arbre de recherche potentiel_.

Chaque candidat-solution partiel est le parent d’autres candidats-solutions qui diffèrent de lui par une simple étape d’extension (par exemple ajout d’un élément unique à une liste).

Les feuilles sont les candidats-solutions qui ne peuvent pas être développés plus avant (ce sont des _candidats-solutions complets_). Certains candidats-solutions complets sont effectivement des solutions, d’autres non.

## Backtracking : arbre de décision

L’algorithme de backtracking parcourt l’arbre de recherche potentiel par un **DFS**.
A chaque nœud $c$ (donc, un candidat-solution), l’algorithme cherche si $c$ peut être complété en une solution valide.

- Si ce n’est pas possible, le sous-arbre de racine $c$ dans l’arbre solution est supprimé.
- Par ailleurs, si $c$ est une feuille, l’algorithme cherche si $c$ lui-même peut être considéré comme une solution valide (exemple une grille complètement remplie constitue-t-elle une solution au problème de Sudoku ?).

L’arbre effectivement parcouru par le backtracking est un sous-arbre (souvent strict) de l’arbre de recherche potentiel.

## Algorithme de Backtracking

```
fonction backtrack(e, c):
    entree : e entrée du problème; c candidat−solution partiel
    sortie : une solution de e s'il en existe, None sinon
    debut
        si c est un candidat−solution complet(une feuille) alors
            si c est une solution de e alors renvoyer c
            sinon renvoyer None;
        sinon
            pour tout c' fils candidat−solution POSSIBLE de c faire
                v ⟵ backtrack(e, c');
                si v ≠ None alors renvoyer v
            renvoyerNone;
    fin
```


!!!tip ""
    **Remarque**
    Un candidat-solution _impossible_ est un candidat dont on se rend compte qu’il ne peut pas être complété en une solution.
    La détection précoce des candidats-solutions impossibles permet de limiter le coût de l’exploration.

## Complexité

La complexité d’un algorithme de backtracking de recherche de solution pour une entrée $e$ dépend en particulier de la taille de l’arbre exploré.
<p style='color:crimson'>On se place dans le cas le pire où aucune détection précoce de candidat impossible n’est détectée.</p>
Il faut alors explorer tout l’arbre de décision.

!!!note ""
    **Complexité**

    On note $h$ la hauteur de l’arbre et $p$, l’arité maximum d’un
    nœud. L’arbre est alors de taille $\sum_{i=0}^{h}{p^i} = O(p^h)$.

    Le test de détection précoce d’impossibilité pour un candidat $c$ a une complexité en $O (g(|c|))$ pour une certaine fonction $g$.
    La taille $|c|$ du candidat est dominée grossièrement par un $k (|e|)$ pour une certaine fonction $k$ ne dépendant que de $e$.
    Ainsi, la détection précoce est en $O (g◦k (|e|))$

    Lorsque le candidat $c$ est une feuille, le test de validité du candidat comme solution est en $O (l(|c |))$ donc en $O (l ◦k (|e|))$ pour une certaine fonction $l$
    
    Bref, il existe $f$, tel que pour tout candidat $c$, la détection précoce et le test de validité ont une complexité en $O (f (|e|))$ où $f = max(g ◦k , l ◦k )$.

    Ainsi, le backtracking a une complexité majorée par un $O (p^h \times f(|e|))$
