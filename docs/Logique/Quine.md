# Algorithme de Quine

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d’être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    * Un cours de mon collègue Q. fortier
    * Ce cours de l’université de Montpellier : [ici](https://www.lirmm.fr/~retore/SE/propositions.pdf)

## Substitution

!!!note "Notation"

    * Si $x$ est une variable, la notation $[x ← T]$ désigne le contexte partiel qui remplace $x$ par $T$ (True) et n’affecte aucune autre variable.

    * La notation $[x ← T][y ← F]$ désigne le contexte partiel qui remplace $x$ par $T$ (True) ; $y$ par $F$ (False) et n’affecte aucune autre variable.
    De même : $\sum_{i=1}^n{[x_i ← C_i]}$ désigne contexte partiel qui remplace chaque $x_i$ par la constante $C_i$ (vrai ou faux).
 
    * Si $P$ est une proposition et $x$ une variable, $P[x ← T]$ désigne $P$ dans laquelle toutes les occurrences de $x$ sont remplacées par $T$ (True). Et $P$ $\sum_{i=1}^n{[x_i ← C_i]}...$ à votre avis ?

## Satisfiabilité par backtracking

On dispose de `evaluer (P:prop., µ:ctxt complet)`

```OCaml lnenums="1" title="Listing 1 – Algorithme de satisfiabilité"
fonction sat (P : proposition ;
              L : ensemble de variables à remplacer par T/F,
              µ : contexte partiel)
    sortie : un bouléen
    si L est vide renvoyer evaluer (P, µ)
    prendre x ∈ L
    si sat (P, L\{x}, µ ◦ [x ← T])
        alors renvoyer Vrai
    sinon renvoyer sat (P, L\{x}, µ ◦ [x ← F])
```

Cette fonction réalise donc un bactracking et teste potentiellement $2^{\text{nb de variables de P}}$ contextes.

appel initial : `evaluer(P, liste var de P)`.

## L'Algorithme de Quine

L’algorithme de Quine prend en paramètre une proposition en FNC et réalise un backtracking (comme la recherche de satisfiabilité en force brute).

```OCaml linenums="1"
prendre une variable x restante dans la formule
tester récursivement si P[x ← T] est satisfaisable
tester récursivement si P[x ← F] est satisfiable
```

En revanche, chaque substitution est l’occasion de supprimer un littéral d’une clause ou une clause de la formule.

### Suppression de littéral ou de clause

Soit $x$ une variable et $P$ une proposition en forme normale conjonctive : $P = c_1 ∧ c_2 ∧ · · · ∧ c_n$.
Lors de la substitution $P[x ← T]$ on effectue des simplifications :

- Si une clause contient le littéral $x$, on enlève cette clause car elle est de la forme $v_1 ∨ v_2 ∨ · · · ∨ \underbrace{T}_{\text{emplacement de } x} ∨ · · · ∨ v_k$ donc vraie.

- Si une clause contient $¬x$, on supprime ce littéral de cette clause car il est faux ($F$ est l’élément neutre de la disjonction). $v_1 ∨ v_2 ∨ · · · ∨ \underbrace{¬T}_{\text{emplacement de } x} ∨ · · · ∨ v_k$

Lors de la substitution $P[x ← F]$ on effectue des simplifications :

- Si une clause contient $¬x$, on enlève cette clause car elle est de la forme $v_1 ∨ v_2 ∨ · · · ∨ \underbrace{¬F}_{\text{emplacement de } x} ∨ · · · ∨ v_k$ donc vraie.
- Si une clause contient $x$, on supprime ce littéral de cette clause car il est faux. $v_1 ∨ v_2 ∨ · · · ∨ \underbrace{F}_{\text{emplacement de } x} ∨ · · · ∨ v_k$ donc vraie.

### Proposition ou clause vide

On supprime à chaque étape soit une/des clause(s) soit un/des littéral/littéraux.

Si une clause devient vide (disjonction de $0$ propositions), la clause est fausse donc la formule (qui est une conjonction de clause) est fausse aussi. On backtrack.

Si la proposition devient vide (conjonction de $0$ propositions), la formule est vraie. C’est gagné !
