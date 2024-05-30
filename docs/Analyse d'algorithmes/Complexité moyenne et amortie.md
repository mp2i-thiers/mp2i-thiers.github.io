# Complexité moyenne et amortie

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d’être plus compréhensible pendant les périodes de révision que des diaporamas.

!!! tip "Crédits"
    - [Wikipedia (Analyse amortie)](https://fr.wikipedia.org/wiki/Analyse_amortie)

    - "Option informatique MPSI - MP/MP*" Roger MANSUY (Vuibert)

    - "Algorithmique - 3ème édition Cours avec 957 exercices et 158 problèmes" Thomas H. Cormen, Charles Leiserson, Ronald Rivest, Cliﬀord Stein (Dunod)

    - "Informatique - MP2I/MPI - CPGE 1re et 2e années" Balabonski Thibaut, Conchon Sylvain, Filliâtre Jean-Christophe, Nguyen Kim, Sartre Laurent (ellipse)

## Tri rapide

Dans le tri rapide, on sépare l'entrée en deux listes en comparant à un élément pivot, on les trie séparément et on les fusionne par concaténation.

- Dans le tri rapide, la fonction de division est délicate et celle de fusion triviale. C'est le contraire pour le tri fusion.

- On donne une version pour les listes. On peut en donner une version en place pour des tableaux.

- Mis au point en $1960$ par Tony Hoare, alors étudiant en visite à l'université d'Etat de Moscou.

- Possède une variante _Quick select_ pour le calcul du $k$-ième élément d'une liste sans procéder au tri préalable (application : calcul de la médiane).

### Partition

```OCaml linenums="1"
let rec partition l p = match l with
    | []   -> [] ,[]
    | t::q -> let (l1,l2) = partition q p in
        if t <= p then (t::l1,l2) else (l1,t::l2);;
```

#### Terminaison de la partition

Les cas de bases terminent et l'appel interne se fait avec une liste strictement plus courte que la liste initiale et il n'y a pas de boucle ou d'appel à une fonction qui ne termine pas.

C'est tout bon !

#### Complexité temporelle de la partition

Dans tous les cas de la forme $C_{n} = C_{n} - 1 + 1$ pour une liste de taille $n$. Linéaire.

#### Correction par induction

Montrons que la fonction retourne deux listes dont la 1ere contient tous les éléments de $l$ plus petits que le pivot, les éléments strictement plus grands étant dans la seconde.

- _Cas de base_ : **OK** de façon évidente
- _Hérédité_ : Supposons que `partition q p` partitionne correctement `q` en $l_1, l_2$. Supposons aussi $t ≤ p$. Alors dans le tuple retourné :
  - On ajoute $t$ à la liste $l_1$ des éléments de $q$ plus petits que $p$. On obtient exactement tous les éléments de $l$ plus petits que le pivot.
  - La liste $l_2$ est constituée exactement des éléments de $q$ strictement plus grands que $p$ ; donc (puisque $t ≤ p$) exactement ceux ceux de $l$.
  - Correction **OK**. Le cas $t > p$ cas est laissé au lecteur.

### Algorithme

```OCaml linenums="1"
let rec tri_rapide l = match l with
    | [] -> []
    | t::q -> let (l1,l2) = partition q t in
        (tri_rapide l1)@(t::(tri_rapide l2));;
```

### Terminaison

- Variant $|l|$.
- Le cas de base termine (envoie de la liste vide).
- L'appel à `partition` termine (déjà vu).
- Seulement deux appels internes, tous deux eﬀectués avec des listes de taille strictement inférieure à $|l|$ (puisque $|l1| + |l2| = |l| − 1$).
- Terminaison **OK**.

### Correction

!!!note "Preuve par induction"
    Cas de base : **OK**.

    Supposons que les deux appels internes retournent une version triée de
    `l1` et de `l2`.

    - Par correction de partition : `l1` contient les éléments de `q` plus petits ou égaux au pivot et `l2` les éléments strictement plus grands.
    - La concaténation retournée
        - est triée : d'abords les éléments plus petits que le pivot     rangés dans l'ordre, puis le pivot, puis les éléments plus grands que le pivot rangés dans l'ordre.
        - contient exactement tous les éléments de `q` (puisque la réunion de `l1` et `l2` les contient) plus l'élément manquant `t` .
        - La concaténation est la version triée de `l` .

    Hérédité **OK**

!!!note "Preuve par récurrence"
    Cas de base : **OK**.

    $\color{red}\text{Supposons que le tri soit correct pour des tailles de liste} ≤ n$. Par **HR**, les deux appels sur $l_1$ et $l_2$ retournent une version triée de `l1` et de `l2`.

    - Par correction de `partition` : `l1` contient les éléments de `q` plus petits ou égaux au pivot et `l2` les éléments strictement plus grands.
    - La concaténation retournée
        - est triée : d'abords les éléments plus petits que le pivot     rangés dans l'ordre, puis le pivot, puis les éléments plus grands que le pivot rangés dans l'ordre.
        - contient exactement tous les éléments de `q` (puisque la réunion de `l1` et `l2` les contient) plus l'élément manquant `t` .
        - La concaténation est la version triée de `l` .

    Hérédité **OK**

### Tri rapide : complexité en nombre de comparaisons

#### Cas où l'une des listes est toujours vide

Supposons que l'une des listes retournée par partition soit vide à chaque fois (cela se produit par exemple si la liste est déjà triée).

On désigne par $T_n$ la complexité temporelle en nombre de compraisons pour une liste triée de taille $n$.

- Alors Tn vériﬁe une relation de la forme $Tn = Tn−1 + n − 1$. ($n − 1$ : nombre de comparaisons dans `partition`). Remarquons au passage que $T_1 = 0$  (aucune comparaison) et que $T_{1-1} + 1 - 1 = 0 = T_1$
- Complexité en somme des premiers entiers soit $O(n^2)$.
- On montre dans la suite que ce cas est bien le pire, c'est à dire que la complexité $C_n$ en nombre de comparaisons est la pire si le pivot est à une extrémité et donc que $C_n = T_n$.

Hypothèse de récurrence selon n (= HR(n) ):$\forall q \leq n$ :\
→ $T_{q}$ est la pire complexité\
→ $\forall k \in \{ 1,\ldots,n\},T_{k - 1} + T_{n - k} + n–1 \leq T_{n}$ (avec égalité si $k=1$)

- _Cas de base_ :  $n = 1$, $T_0 = 0 = T_1,k \in \{ 1\}.$ Alors $T_{1 - 1} + T_{1 - 1} + 1–1 = 0 \leq T_1$ et $T_1$ est la pire complexité. **OK**

- Si $HR(n)$ est vérifiée. Soit $k \in \{ 1,\ldots,n + 1\}$
  
    $$\begin{matrix}
    T_{k - 1} + \textcolor{red}{T_{n + 1–k}} + n &\underset{\text{def. de T}}{=} & T_{k - 1} + \textcolor{red}{T_{n - k} + (n - k)} + n–1 + 1\\
    && \\
    &\underset{\text{HR(n)}}{\leq} & T_{n} + (n–k + 1) \leq T_{n} + n \\
    && \\
    & = & T_{n + 1}
    \end{matrix}
    $$
  
    C'est ce qu'on veut.

- Soit $l$ une liste $\color{red}\text{quelconque}$ de longueur $n+1$ partitionnée lors de l'appel `partition q`en deux sous-listes $l_1, l_2$ de longueur $k$ et $n-k$ ($k ≥ 0$). Notons $C^l_{n+1}$ la complexité exavte en nombre de comparaisons de l'appel `tri_rapide l`; $C^{l_1}_{n+1}$ (resp. $C^{l_2}_{n+1}$) les complexités de `tri_rapide l1` (resp. `tri_rapide l2`).
- Si $l_i$ est vide $C^l_{n+1} = C^{l_j}_{n} + n \leq T_n + n$ par **HR** donc $C^l_{n+1} = T_{n+1}$.
- Si $|l_1|\times|l_2| ≠ 0$ alors $1 ≤ k ≤ n-1$ et :

$$\begin{matrix} C^l_{n+1} & = & C^{l_1}_{k} + C^{l_1}_{n-k} +n \\ & & \\
& \underbrace{≤}_{HR(n)} & T_k + T_{n-k} + n \\ && \\
& \underbrace{=}_{\begin{matrix}k'= k+1  \\ k' \in ⟦2,n ⟧\end{matrix}} & T_{k'-1} + T_{n+1-k'} + n \\ & & \\
& ≤ & T_{n+1}
\end{matrix}$$
(d'après la preuve du transparent précédent)

- Ainsi, $\color{red}l \text{ étant quelconque}$, $T_{n+1}$ est la pire complexité possible. **IZP** !

### Complexité moyenne du tri rapide

La complexité moyenne $C(n)$ du tri rapide pour une liste de taille $n$ est la moyenne des complexités pour les diﬀérentes positions du pivot.

Elle vériﬁe (si la position ﬁnale du pivot est équiprobable) :

$$\begin{matrix}C(n) &=& \frac{1}{n} \sum_{k=0}^{n-1}(C(k)+C(n-k-1)+n-1)\\ & & \\
    &=& n-1+\frac{1}{n}\sum_{k=0}^{n-1}(C(k) + C(n-k-1))
\end{matrix}
$$

On constate que chaque terme apparaît deux fois.

Alors $C(n) = n - 1 + \frac{2}{n}\sum_{k = 0}^{n - 1}C(k)$
donc $nC(n) = 2\sum_{k = 0}^{n - 1}C(k) + n(n - 1)$ et donc ainsi :

$$
\begin{align}
nC(n) - (n-1)C(n-1) &\underbrace =_\text{def} \textcolor{red}{2\sum_{k=0}^{n-1}C(k)}+n(n-1)-(n-1)C(n-1) \nonumber\\
    &= \textcolor{red}{2C(n-1)+2\sum_{k=0}^{n-2}C(k) + (n-1)(n-2)}- (n-1)(n-2) +n(n-1) - (n-1)C(n-1) \nonumber\\
    &= 2C(n-1) +\textcolor{red}{(n-1)C(n-1)}-(n-1)(n-2) + n(n-1)-(n-1)C(n-1) \nonumber\\
    &= 2C(n-1)+n(n-1)-(n-1)(n-2) \nonumber\\
    &\Rightarrow nC(n)-(n-1)C(n-1) = 2(n-1) \nonumber
\end{align}
$$

Il vient ainsi

$\frac{C(n)}{n + 1}–\frac{C(n - 1)}{n} = \frac{2(n - 1)}{n(n + 1)} = \frac{4}{n + 1}–\frac{2}{n}$ (décomposition en éléments simples)

Par télescopage, et puisque $C(0) = 0$ :

$\begin{matrix}
\frac{C(n)}{n + 1}–\frac{C(0)}{1} &=& \frac{C(n)}{n + 1} \\ & &\\
 &=&4\sum_{k = 1}^{n}{\frac{1}{k + 1}}–2\sum_{k = 1}^{n}{\frac{1}{k}} \\ & &\\
 &=& 4\sum_{k = 2}^{n + 1}{\frac{1}{k}} - 2\sum_{k = 1}^{n}{\frac{1}{k}} \\ & &\\
 &=& 2\sum_{k = 2}^{n}\frac{1}{k} + \frac{4}{n + 1} - 2 \\ & &\\
 &=& 2\sum_{k = 1}^{n}{\frac{1}{k}} + {\frac{4}{n + 1}–4} \leq 2\underset{\color{red}\sim \ln(n)}{\underbrace{\sum_{k = 1}^{n}{\frac{1}{k}}}} + \underset{\color{red}O\left( \frac{1}{n} \right)}{\underbrace{\frac{4}{n + 1}}} \\
\end{matrix}$

Sachant que $\sum_{k = 1}^{n}{\frac{1}{k}} = \ln n + \gamma + O\left( \frac{1}{n} \right)$ où $γ$ est la constante d'Euler ($\gamma \simeq 0,577$) alors :

$$\frac{C(n)}{n+1} = (2 \ln n + 2\gamma + O(\frac{1}{n}))+\frac{4}{n+1}-4=2\ln n +2\gamma + O(\frac{1}{n}) -4$$

Donc $C(n) = O\left( n\log n \right)$.

On peut minorer de la même façon, (en exo : utiliser $\sum_{k=1}^{n}{\frac{1}{k}} > \ln(n)$ apcr) donc $C_{n} = \Theta\left( n\log n \right)$

## Complexité amortie

### Présentation

L'_analyse amortie_ est une méthode d'évaluation de la complexité temporelle des opérations sur une structure de données (tableaux, piles, ﬁles, arbres...).

Elle consiste principalement à majorer le coût cumulé d'une suite d'opérations pour attribuer à chaque opération la moyenne de cette majoration, en prenant en compte le fait que les cas chers surviennent rarement et isolément et $\color{red}\textsf{compensent les cas bon marché}$.

### Exemple du compteur binaire

On veut représenter, pour un entier $n$, tous les tableaux possibles de bouléens de longueur $n$.

Un tel tableau `t` peut être vu comme la représentation binaire d'en entier positif sur $n$ bits. Il y en a $2n − 1$. On choisit la version _little endian_ (bit de poids faible en `t[0])`, donc $1$ s'écrit

|  1  |  0  |  0  |  0  |
| --- | --- | --- | --- |

Pour avoir tous les tableaux, on part du tableau entièrement nul et on lui applique itérativement un incrément de $1$ (donc $2^n$ fois) :

```C linenums="1"
void incr(bool c[], int n){
    int i = 0;
    while(i<n && c[i]==1){ 
        c [i] = 0 ;
        i++; //propagation de la retenue
    }
    if (i<n) c[i] = 1 ;
}
```

Nous voulons mesurer la complexité de cette fonction en nombre d'accès aux éléments du tableau.

### Analyse grossière

Pour un tableau de taille $n$.

- La complexité dépend du nombre de tour dans la boucle while.
- Meileur cas : si `t[0] == 0` , alors pas de passage dans la boucle, deux
accès seulement (en lecture puis écriture).
- Pire cas : tous les bits à $1$. Pour chacun des $n$ passages, un accès en
lecture puis un en écriture. $2n$ accès .
- Dans le pire cas, la complexité est en $O(n)$ pour un tableau.
- Majorer le coût des $2^n$ appels à incr : $2n2^n = n2^{n+1}$.
- C'est trop imprécis car passer $n$ fois dans la boucle arrive
exceptionnellement.

!!!example "Exemple"
    | Numéro    | Paramètre      | Nombre de tours
    | --------- | -------------- | ---------------
    | $0$       | $0000 . . . 0$ | $0$
    | $1$       | $1000 . . . 0$ | $1$
    | $2$       | $0100 . . . 0$ | $0$
    | $3$       | $1100 . . . 0$ | $2$
    | $4$       | $0010 . . . 0$ | $0$
    | $5$       | $1010 . . . 0$ | $1$
    | $6$       | $0110 . . . 0$ | $0$
    | $7$       | $1110 . . . 0$ | $3$
    | $8$       | $0001 . . . 0$ | $0$
    | $9$       | $1001 . . . 0$ | $1$
    | $10$      | $0101 . . . 0$ | $0$
    | $11$      | $1101 . . . 0$ | $2$
    | $12$      | $0011 . . . 0$ | $0$
    | $13$      | $1011 . . . 0$ | $1$
    | $14$      | $0111 . . . 0$ | $0$
    | $15$      | $1111 . . . 0$ | $4$

    Total : $14$ passages dans while

    Sur $16$ appels consécutifs à `incr` :

    - Un appel sur $2$ : $0$ tour de boucle ;

    - Un appel sur $4$ : $1$ tours de boucle ;

    - Un appel sur $8$ : $2$ tours de boucle ;

    - Un appel sur $16$ : $3$ tours de boucle ;

### Cadre d'étude de complexité amortie

On eﬀectue une étude de complexité amortie pour des algorithmes qui

- ont une faible complexité pour de nombreuses entrées ;
- sont coûteux pour certaines entrées (peu nombreuses en proportion) ;
- vériﬁent que dans toute séquence d'appel à l'algorithme, les entrées coûteuses interviennent assez rarement pour que le coût moyen reste faible.

### Diﬀérentes méthodes

Il y a $3$ méthodes usuelles d'analyse amortie : la méthode de l'agrégat, la méthode comptable et la $\color{red}\textsf{méthode du potentiel}$ (seule étudiée ici).

Méthode du potentiel : A chaque entrée possible $x$, on associe un nombre positif ou nul $φ(x)$ dit _potentiel_. Il représente un coût latent dans l'entrée mais pas encore réalisé.

Une séquence d'un algorithme ayant de bonnes propriétés de complexité amortie alterne entre :

- de (nombreuses) opérations de faible coût faisant monter progressivement le potentiel,
- et de (peu nombreuses) opérations de coût élevé faisant diminuer brusquement le potentiel.

Dans l'exemple du compteur binaire, le potentiel est le nombre de $1$ dans le tableau.

### Coût amorti d'une opération

!!!quote "Définition : Coût amorti"
    Soit une opération $op$ dont l'exécution produit une sortie $x_{s}$ à partir d'une entrée $x_{e}$. Le _coût réel_ $C$ de $op$ est la complexité temporelle de son exécution. Son _coût amorti_ $A$ et la somme du coût réel et de la variation de potentiel : $A = C + \varphi(x_s) - \varphi(x_e)$.

Dans cette déﬁnition, les _entrées_ et _sorties_ représentent au sens large ce qui est donné à l'algorithme (paramètres, état de la mémoire au début de l'appel) et ce qu'il produit (résultats renvoyés, état de la mémoire en sortie).

Une _séquence d'opérations consécutives_ est une suite d'appels à un algorithme de sorte que chaque sortie d'un appel soit l'entrée du suivant.

On montre que le coût réel est toujours inférieur au coût amorti.

### Théorème d'amortissement

!!!danger ""
    **Théorème**

    Soit une suite de $n$ opérations $x_0\overset{op_1}→x_1\overset{op_2}→x_2 . . .\overset{op_n}→x_n$
    à partir d'une entrée $x_0$ telle que $φ(x_0) = 0$. En notant $C_i$ le coût réel de l'opération $op_i$ et $A_i$ son coût amorti, on a la relation d'amortissement suivante :

    $$\sum_{i=1}^{n}C_i \leq \sum_{i=1}^{n}A_i$$

!!!note ""
    **Preuve**

    $$ \sum_{i=1}^{n}A_i = \sum_{i=1}^{n}C_i + \sum_{i=1}^{n}(\varphi(x_i) - \varphi(x_{i-1})) = \sum_{i=1}^{n}C_i + \varphi(x_n) - \varphi(x_0) \geq \sum_{i=1}^{n}C_i$$

### Corollaire au th. d'amortissement

!!!quote ""
    **Corollaire**

    Avec les notations du th. d'amortissement, si le coût amorti est borné par une constante $k$, alors la complexité moyenne au sein d'une séquence arbitraire de $n$ opérations est bornée par $k$.

!!!note ""
    **Démonstration**

    $$\sum_{i=1}^n C_i \underbrace{\leq}_{\textsf{par th. d'amortissement}} \sum_{i=1}^n A_i \leq \sum_{i=1}^n k = k\times n$$

### Retour au compteur binaire

Complexité en nombre d'accès aux cases du tableau :

Soit $k$ le nombre de $1$ consécutifs depuis la position $0$. L'appel `incr(c,n)` réalise :

- $2(k + 1)$ accès mémoire si $k < n$.

- $2n$ accès si $k = n$.

On déﬁnit le potentiel $φ(c)$ du tableau $c$ par

$$\Phi(c) = 2 \times \underbrace{(\textsf{nombre de 1 dans c})}_{|c|_{1}} $$

Si $c_{i}$ possède $k$ bits $1$ consécutifs depuis la position $0$, alors soit $k'$ tel que $|c|_{1} = k + k’$. Une application de `incr`, transforme $c_{i}$ en $c_{i + 1}$.

Le delta de potentiel est :

$$\phi(c_{i+1}) - \phi(c_i) = \left \{ \begin{matrix}
 2(k’ + 1) - 2(k + k’) =  2 - 2k &  \text{ si k différent de n} \\
 -2k &   \text{ si } k = n \text{ car alors } c_{i+1} = 00...0
\end{matrix}\right.$$

Complexité en nombre d'accès aux cases du tableau :

$$A_{i+1} = C_{i+1} + \phi(c_{i+1}) - \phi(c_i) =\left \{ \begin{matrix}
 (2k + 1) +2 -2k = 3 &  \text{ si } k ≠ n \\
 2k -2k = 0 &   \text{ si } k = n
\end{matrix}\right.$$

On en déduit que la complexité amortie est bornée par $3$. D'après le corollaire du th. d'amortissement, toute séquence d'incrémentation commençant en $00 . . . 0$ réalise moins de $3$ opérations d'accès au tableau par appel à `incr`.

## Complexité moyen vs complexité amortie

La complexité moyenne est une moyenne (donc calculée avec des probabilités)

Elle donne :

- Une complexité supposée représentative du plus grand nombre d'entrée ;
- Sans apporter aucune garantie sur la complexité d'une opération particulière ni même sur une séquence particulière d'opérations.

La complexité amortie apporte une borne garantie à toute séquence d'opération.

- Son calcul n'utilise pas de probabilité ;
- Elle ne dit rien de la complexité d'une opération particulière mais assure un équilibre à toute séquence d'opérations.
