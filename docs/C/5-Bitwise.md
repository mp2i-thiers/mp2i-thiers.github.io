# Opérateurs bitwise

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    - Une page de [Dev4all](http://www.dev4all.com/articles.cgi?id=63&chapter=CHAPTER_OPBITWISE) (Page not found 🪦)
    - Cette page de [Wikipedia](https://en.wikipedia.org/wiki/Bitwise_operations_in_C)

## Présentation

Dans un ordinateur, les données et les fichiers sont constitués de bits en nombre variable selon leurs types.

En $\texttt{C}$, il existe $6$ opérateurs de gestion binaires (opérateurs _bitwise_ ou _bit à bit_) :

- `&` : Opérateur bitwise $\text{AND}$
- `|` : Opérateur bitwise $\text{OR}$
- `∧` : Opérateur bitwise $\text{XOR}$ $(\text{OU exclusif})$. Noté aussi $⊕$ en maths.
- `~` : Opérateur bitwise de complément
- `>>` : Opérateur de redirection vers la droite
- `<<` : Opérateur de redirection vers la gauche

## Principe

Un opérateur bit à bit binaire comme `&` traite ses données d'entrée (deux objets de même type) selon leur représentation binaire (une séquence de $0$ et de $1$).

Il calcule une nouvelle séquence binaire et traduit ce résultat dans le format d'origine des données.

## L'opérateur `&`

Cet opérateur binaire prend deux séquences de bits de même longueur et effectue un $\text{ET}$ logique bit-à-bit.

Il calcule le $\text{ET}$ logique du premier bit des deux séquences, puis celui des seconds bits etc. Le résultat est une séquence de bits qui est interprétée selon le format d'origine des séquences d'entrée.

!!!example ""

    - Dans la fonction **main**, entrons ceci :

    ```C linenums="1"
    int x = 9, y = 12;
    printf("%d&%d=%d\n", x, y, x&y);
    ```

    Après compilation et exécution, on obtient l'affichage :

    ```bash title="Rendu"
    x&y=8
    ```

    - L'opérateur **&** considère l'entier $9$ comme $1001$ et $12$ comme $1100$ :

    $\begin{matrix}
    & 1001 \\
    \& & 1100 \\
    \underline{\kern0.8pc} & \underline{\kern2pc} \\
    & 1000 \\
    \end{matrix}$
    
    Le résultat obtenu est $1000$ qui est transformé en **int**. On obtient $8$.

## L'opérateur `|`

Cet opérateur binaire fonctionne comme le précédent mais effectue un $\text{OU}$ logique bit-à-bit et non un $\text{ET}$.

!!!example ""

    - Dans la fonction **main**, entrons ceci :

    ```C linenums="1"
    int x = 9, y = 12;
    printf("%d|%d=%d\n", x, y, x|y);
    ```

    Après compilation et exécution, on obtient l'affichage :

    ```bash title="Rendu"
    x|y=13
    ```

    - On fait l'opération :

    $\begin{matrix}
    & 1001 \\
    | & 1100 \\
    \underline{\kern0.8pc} & \underline{\kern2pc} \\
    & 1101 \\
    \end{matrix}$
    
    Le résultat obtenu est $1101$ qui est transformé en **int**. On obtient $13$.

## L'opérateur `^`

Cet opérateur binaire réalise le $\text{XOR}$ bit-à-bit de ses deux opérandes.

!!!example ""

    - Dans la fonction **main**, entrons ceci :

    ```C linenums="1"
    int x = 9, y = 12;
    printf("%d^%d=%d\n", x, y, x^y);
    ```

    Après compilation et exécution, on obtient l'affichage :

    ```bash title="Rendu"
    x^y=5
    ```

    - On fait l'opération :

    $\begin{matrix}
    & 1001 \\
    \^{} & 1100 \\
    \underline{\kern0.8pc} & \underline{\kern2pc} \\
    & 0101 \\
    \end{matrix}$
    
    Le résultat obtenu est $0101$ qui est transformé en **int**. On obtient $5$.

## L'opérateur `~`
Cet opérateur unaire inverse tous les bits de la séquence
Dans la fonction main, entrons ceci :

!!!example ""

    - Dans la fonction **main**, entrons ceci :

    ```C linenums="1"
    uint8_t z = 5; uint8_t t = ~z; // entiers 8 buts
    printf("~%u=%u\n", z, t); // %u pour unsigned
    ```

    Après compilation et exécution, on obtient l'affichage :

    ```bash title="Rendu"
    ~5=250
    ```

    - N'oublions pas on travialle sur $8$ bits :

    $\begin{matrix}
    \widetilde{\kern0.4pc} & 0000 & 0101 \\
    \underline{\kern0.8pc} & \underline{\kern2pc}& \underline{\kern2pc} \\
    & 1111 & 1010 \\
    \end{matrix}$

    Le résultat obtenu est $1111$ $1010$ qui est transformé en `uint8_t` (entier non signé). On obtient $250$ c.a.d $255-5$. 
    
    Si on travaille avec des entiers signés cete fois ci alors 
    
     ```C linenums="1"
    int8_t u = 5; int8_t t = ~v; // entiers 8 bits
    printf("~%d=%d\n", u, v); // %d pour les entiers signés
    ```

    Après compilation et exécution, on obtient l'affichage :

    ```bash title="Rendu"
    ~5=-6
    ```

    - N'oublions pas on travialle en complément à $2$ sur $8$ bits :

    $\begin{matrix}
    \widetilde{\kern0.4pc} & 0000 & 0101 \\
    \underline{\kern0.8pc} & \underline{\kern2pc}& \underline{\kern2pc} \\
    & 1111 & 1010 \\
    \end{matrix}$
    
    Le résultat obtenu est $1111$ $1010$ qui est transformé en `int8_t` On lui retranche $2^8 = 256$ (entiers signés) et on obtient $-6$

## L'opérateur `>>`

Cet opérateur dit de _décalage à droite_, décale les bits vers la droite. Les bits qui sortent à droite sont perdus, tandis que le bit de poids fort est recopié à gauche.

!!!example

    ```C linenums="1"
    int32_t x = 13;
    printf("%d\n", x >> 2);
    ```
    
    On obtient l'affichage de $3$.

    Expliquons la raison de ce résultat. L'écriture binaire de $13$ sur $3$ bits est :

    ${\color{red}0}0000000000000000000000000001101$

    On décale les bits de deux rangs vers la droite. La séquence finale $01$ est perdue, et on obtient ($3$ en binaire) :
    ${\color{red}0}  \overrightarrow{\kern0.23pc0\kern0.23pc 0\kern0.23pc} 00000000000000000000000000011$

    - Il a fallu combler les deux cases vides laissées par le départ de $01$. On a rajouté deux zéros à gauche parce que le bit de poids fort de l'expression initiale est à zéro. Ce zéro désigne en fait le signe du nombre $13$ : ce dernier est positif comme $(−1)^0$.
    - Le résultat de $13 >>2$ est donc ${13}/{4}$ c'est à dire $3$ (la division par $4$ parce que $2^2 = 4$)

    ```C linenums="1"
    int32_t x = -13;
    printf("%d\n", x >> 2);
    ```

    Cette fois-ci le résultat est $-4$ alors qu'on se serait attendu à $-3$. 
    
    - L'écriture de $-13$ en complément à $2$ sur $32$ bits est en effet :
    $1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 0 1 1$
    - On décale de deux rangs vers la droite, et on ajoute la séquence 11 à gauche (on comble les vides par le bit de poids fort de la séquence initiale). On se retrouve donc avec :
    $111 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 0$

    C'est l'expression de $-4$ en complément à deux sur $32$ bits.

## L'opérateur `<<`

Cet opérateur de décalage vers la gauche correspond à une
multiplication par une puissance de deux.

!!!example ""

    Considérons

    ```C linenums="1"
    int8_t x = 3;
    printf("%d\n", x << 4);
    ```
    - Le codage de $3$ en complément à $2$ sur $8$ bits est $0000011$.
    - Le décalage de $4$ rangs à gauche produit $0110000$, ce qui fait $48$.
    - Il se trouve que $3 ×2^4 = 48$. On a donc multiplié $3 par 2^4$ en faisant notre décalage de $4$ bits.
