# Bonnes pratiques

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    - Un mémo du [CNRS](https://perso.liris.cnrs.fr/pierre-antoine.champin/enseignement/algo/cours/algo/bonnes_pratiques.html) pour $\texttt{Python}$
    - [Cette page](https://emmanuel-delahaye.developpez.com/tutoriels/c/bonnes-pratiques-codage-c/) D'Emmanuel Delahaye.
    - "Informatique MP2I/MPI" Ellipse.

## Code source

### Code bien écrit

Un algorithme ou code "bien écrit" doit avoir les propriétés suivantes :

- Être facile à lire, pas soi-même mais aussi par les autres.
- Avoir une organisation logique et évidente.
- Être explicite, montrer clairement les intentions du développeur.
- Être soigné et robuste au temps qui passe.

### Indentation

En **C** et **Ocaml**, l'indentation ne fait pas sens

!!!example ""

    ```C linenums="1"
    if (x==0)
        printf("x=%d\n", x);
        x++;
    ```

On peut croire que `x` n'est incrémenté que si il est nul : erreur.

C'est au programmeur de faire un effort pour que le code montre la structure.

On peut choisir d'aligner des éléments comparables pour insister sur leurs similarités

!!!example ""

```C linenums="1"
if ( c>0 ){...}
else if (c<0){...}
```

### Factorisation du code

Pour des raisons historiques, ne pas dépasser $80$ caractères par ligne.

Pour des expressions longues, _factoriser_ le travail en calculs plus petits stockés dans des variables élémentaires.

Décomposer un programme en sous-fonctions élémentaires de quelques lignes.

Non seulement on y gagne en _lisibilité_ mais aussi en _réutilisabilité_.

### Choix des noms

Fichiers, types, fonctions, variables.

- Variables locales à une fonction : utiliser des noms courts à une lettre. Cette lettre n'est pas choisie au hasard (par exemple `t` pour un
tableau, `i` pour un entier).
- Les noms de fonctions ont intérêt à être explicite : par exemple `int dichot(int a[], int n, int x)` pour une fonction qui fait une recherche dichotomique dans un tableau trié.
- On peut utiliser des underscore si le nom de fonction contient lusieurs mots `bool has cycle(graphe g)` qui indique par un bouléen si le graphe possède un cycle.
- On peut préférer séparer les mots par des majuscules : `bool hasCycle(graphe g)` (notation à-la-Java). Ocaml limite par ailleurs l'usage des majuscules en première lettre.

### Commentaires

Un commentaire doit être une valeur ajoutée. Ne pas paraphraser le code.

!!!example "Exemple de commentaire inutile"

    ```C linenums="1"
    // si x>0 , incrémenter, sinon décrémenter
    if (x>0) {x++} else {x−−}
    ```

Indiquer les entrées-sorties

!!!example ""

    ```C linenums="1"
    /*Entrées : a tableau trié d'entiers 
                n taille de a, x valeur cherchée */
    // Sortie : position de x
    int dichot (int a [], int n, int x)
    ```

    - Le premier commentaire est une _précondition_ : il sous-entend que le code ne vérifie pas ces hypothèses et peut planter en cas de non respect.
    - Le second commentaire est une _spécification_ : il précise le comportement de la fonction

#### Invariant de boucle

Si le programme contient une boucle, une propriété maintenue à chaque itération est appelée un invariant de boucle. Il est utile de préciser cet invariant pour expliquer le code et pour une future preuve de correction.

!!!example "Exemple du tri insertion"

```C linenums="1"
for (int i = 0; i <= n−1; i++) // tri insertion
    {   // Inv :a [0..i[ est trié
        x = T [i];
        j = i;
        while (j > 0 & T[j−1] > x)
            {
                T [j] = T[j−1];
                j = j−1;
        }
        T[j] = x;}
```

On peut aussi représenter l'invariant par un dessin comme dans l'algorithme du drapeau hollandais (classement des éléments d'un tableau selon trois catégories ordonnées)

```C linenums="1"
int b =0 , i =0 , r=n ;
while ( i<r ) {
// 0 b i r n
// +−−−−+−−−−+−−−−+−−−−+
// a | 0 | 1 | ? ? | 2 |
// +−−−−+−−−−+−−−−+−−−−+
}
```

On comprends que à chaque tour `a[0..b[` ne contient que la valeur $0$,
`a[b..i[` ne contient que la valeur $1$ etc..

### A propos des intervalles

Pour représenter un intervalle (dans un tableau, une chaîne etc.) on peut utiliser systématiquement un indice gauche inclus et un indice droit exclu (comme le `range` de **Python**)

Par exemple, on peut introduire une fonction

```C linenums="1"
void f ( int a [ ] , int g , int d )
```

Pour travailler sur les indices dans $[\![g, d −1]\!]$ avec l'hypothèse $0 ≤g ≤d ≤|a|$.

Dans ce cas :

- le nombre d'éléments concernés est $d−g$
- Si on doit couper cet intervalle en deux ce sera avec $[\![g, m]\!]$ et $[\![d, m]\!]$ pour un $g ≤m ≤d$
- le tableau tout entier correspond $g = 0$, $d = |a|$.

Nous respectons cette convention le plus possible.

### Invariant de structure

Un _invariant de structure_ décrit une propriété toujours vraies pour les valeurs d'une structure de données.

Il est utile de préciser cet invariant (même incomplètement) au niveau de la définition du type

```C linenums="1"
struct ArrStack{
    int capacity ;
    int size ; // 0 <= size <= capacity
    int * data ; // tableau de taille capacity
}
```

## Compilation

### Compiler aide à trouver les erreurs

On détecte les erreurs de syntaxe :

```bash
asup.c:9:1: error: expected declaration or statement at end of input
}
```

ou encore celles de typage

```bash
asup.c:9:5: error: too many arguments to function 'f'
```

En cas d'erreur, aucun exécutable n'est produit.

Le cycle de travail consiste en de $\underline{\text{fréquents}}$ aller-retours entre l'édition du fichier source et la compilation.

Compiler souvent ! La compilation n'est pas nécessairement chronophage avec un bon Makefile (cf plus tard)

### Avertissements

Si le compilateur émet un avertissement plutôt qu'une erreur, il va poursuivre jusqu'à la production de l'exécutable.

Un avertssement peut être négligé en première approche mais il devra être résolu avant le rendu du projet final.

Voici un exemple où le compilateur repère une variable non utilisée. Ce n'est pas propre !

```bash
asup.c:8:7: warning: unused variable 'x' [-Wunused -variable]
```

Utiliser l'option `-Wall` !

### Compiler des programmes incomplets

On peut n'avoir écrit que certaines fonctions du programme, et on peut très bien les compiler avec l'option `-c` qui produit un fichier objet sans édition de lien.

Avec le mécanisme de l'arrêt prématuré (`abort()` en **C**), des assertions (`assert` en **C** et **Ocaml**) ou des exceptions (`failwith` en **OCaml**), on peut ne compiler que des morceaux de codes qui ne sont que partiellement écrits :

```C linenums="1"
if (n > 100)
    n = n−10 ;
else {
// TODO : je verrai plus tard
abort () ; // quitter le programme prématurément
}
```

### Utiliser des prototypes

En **C**, l'écriture du prototype d'une fonction `f` permet d'appeler `f` avant d'avoir écrit son code.

Dans cet exemple, f n'a pas encore de corps. Mais la compilation avec `gcc -c` permet de se rendre compte que dans le corps de main , on appelle f avec trop d'arguments :

```C linenums="1"
#include <stdio.h>

int f (int x); // f sera explicitée plus tard

int main (void)
{
    f(2, 3);
    return 0;
}
```

## Exécution

### Le compilateur ne détecte pas tout

Un théorème célèbre (_th. de Rice_) a pour conséquence qu'aucun compilateur ne peut prévoir toutes les erreurs possibles.

Par exemple un compilateur ne peut pas garantir dans tous les cas qu'on ne divisera jamais par $0$, qu'on n'accèdera jamais à un tableau en dehors de ces bornes ou qu'on ne tombera jamais dans une boucle infinie.

Les raisons pour lesquelles un programme plante sont incomplètement indiquée à l'exécution. Mais cette information lacunaire est quand même utile.

Avec un _debugger_ on peut aller plus loin dans la recherche du bug.

### Erreur de segmentation

L'_erreur de segmentation_ est un plantage d'une application qui a tenté d'écrire dans une zone mémoire qui ne lui était pas allouée.

Dans cet exemple le pointeur `variable_entiere` n'est pas initialisé et contient donc une valeur quelconque qui a de forte chance d'être une zone mémoire interdite en écriture.

```C linenums="1"
#include <stdio.h>
#include <stdlib.h>

int main (void)
{
    int* variable_entiere;
    scanf("%d", variable_entiere);
    return EXIT SUCCESS ;
}
```

Après compilation et exécution

```bash
$./a.out
2
Erreur de segmentation (core dumped)
```

A noter que l'option `-Wall` détecte que le pointeur n'est pas initialisé.

En travaux.
