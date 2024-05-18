# Complexité

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!! tip "Crédits"
    - " Option informatique MPSI, MP/MP* ", Roger Mansuy, paru chez Vuibert.

    - Cours  "diviser pour régner", [Becirspahic](https://info-llg.fr/option-mpsi/pdf/08.diviser_pour_regner.pdf)
    
    - Wikipédia : Analyse de la compléxité des algorithmes.

## Introduction

### Définitions

#### Définition

!!!quote "Définition : Analyse de la complexité"
    Étude formelle de la quantité de ressources (de temps, d'espace mémoire, ...) nécessaire à l'exécution de cet algorithme

    - En **temps** il s'agit de savoir si une fonction termine dans un temps raisonnable
    - En **mémoire** il s'agit de déterminer si la fonction dispose bien toutes les ressources physiques pour s'exécuter.

#### Temps et espace

Soit $f$ un programme prenant en paramètre une donnée $d$

- Complexité en espace pour une donnée $d$ : déterminer la place en mémoire en _plus_ de $d$ occupée par le programme.
- Complexité en temps pour une donnée $d$ : dénombrer toutes les
opérations élémentaires effectuées lors de l'appel $f (d )$ et faire la somme de la durée de chacune.
- Parfois, on ne s'intéresse qu'à certaines opérations bien définies ( ̧ca dépend de ce qu'on veut étudier).

#### Complexité selon une donnée $d$

!!!quote "Définition utiles"
    On note $D_{n}$ l'ensemble des données de taille $n$ et $C(d)$ la complexité d'un certain programme pour une donnée $d$.

    1. $C_{\max}(n) = \max_{d \in D_{n}}C(d)$ Est la complexité dans le pire
    cas
    2. $C_{\min}(n) = \min_{d \in D_{n}}C(d)$ Est la complexité dans le
    meilleur des cas
    3. $C_{moy}(n) = \sum_{d \in D_{n}}^{}P(d)C(d)$ Est la complexité en moyenne avec $P$ la loi de probabilité associée à l'apparition des données de taille $n$.

#### Landau

!!!quote "Définition: Domination / Ordre de grandeur"
    Soient $U = (u_n)$ et $V = (v_n)$ deux suites réelles positives. On dit que :

    1. _$U$ est dominée par_ $V$ s'il existe $\lambda \in {\mathbb{R}} + etN \in {\mathbb{N}}$ tels que pour tout $n \in {\mathbb{N}}$, si $n ≥ N$ alors $u_{n} \leq \lambda v_{n}$. On le note $u_{n} = O\left( v_{n} \right)$. 
    On dit aussi que V domine U.
    2. _$U$ et $V$ sont de même ordre (de grandeur)_ si $U$ domine $V$ et $V$ domine $U$ (i.e. $u_{n} = O\left( v_{n} \right)$ et $v_{n} = O\left( u_{n} \right)$). On le note $u_{n} = \Theta\left( v_{n} \right)$ (« $u_{n}$ est en grand thêta de $v_{n}$»)

!!!quote "Définition : Équivalence / Grand Oméga"
    Soient $U = (u_n)$ et $V = (v_n)$ deux suites réelles positives. On dit que :

    1. On écrit $u_{n} = \Omega\left( v_{n} \right)$ (« $u_{n}$est en grand Oméga de $v_{n}$ ») s'il existe$\lambda \in {\mathbb{R}}_+$ et $N \in {\mathbb{N}}$ tels que pour tout $n \in {\mathbb{N}}$, si $n ≥ N$ alors $u_{n} \geq \lambda v_{n}$.
    Observons que dans ce cas $\frac{1}{\lambda}u_{n} \geq v_{n}$ et donc $u_{n} = \Omega\left( v_{n} \right) \Leftrightarrow v_{n} = O\left( u_{n} \right)$.
    
    2. $U$ et $V$ sont dites _équivalentes_ si et seulement si $v_{n} \neq 0$ à partir d'un certain rang et $\frac{u_{n}}{v_{n}}$ tend vers $1$ en $n \rightarrow + \infty$. Cette définition (qui nous suffit) est plus restrictive que celle du cours du maths.

!!!tip ""
    **Remarque**

    Si $u_{n} \sim v_{n}$ alors $u_{n} = \Theta\left( v_{n} \right)$.

### Que compter ?

La plupart du temps, on se contente de donner une majoration" à constante multiplicative près" du temps de calcul.
On ne compte pas précisément le nombre d'opérations élémentaires (est-ce bien malin de mettre dans le même sac un accès à un élément de tableau et une addition binaire ?).

Dans ce cas, la notation en $O (f (n))$ (si $n$ est l'entrée) nous suffit.

Parfois, on s'intéresse à une opération spécifique, et dans ce cas un décompte précis est favorisé.

!!!example ""
    **Exemple :**

    - Le nombre d'échanges d'éléments dans un tri de tableau
    - Le nombre de produits de flottants dans un produit matriciel « optimisé »

### Ordres de grandeur

Pour une donnée de taille n

  | Dénomination            | Définition              | Exemple
  | ----------------------- | ----------------------- | -------------------------
  | Logarithmique           |       $O(\log n)$       | Recherche dans une liste triée (pire cas)
  | Linéaire                |         $O(n)$          | Inversion d'un tableau
  | Quasi-linéaire          |      $O(n\log n)$       | Tri fusion d'un tableau
  | Polynomiale             |               $O(n²)$   | Tri par insertion (pire cas)
  | Polynomiale             |       $O(n^k)$ pour un  $k>1$ | Produit matriciel naïf en $O\left( n^{3} \right)$
  | Exponentielle           |$O(2^{P(n)})$ $P∈ℝ[X]$ $deg(P) ≥ 1$| Satisfiabilité d'une formule (pire cas)

## Opérations élémentaires

### Présentation

Pour mesurer le temps d'exécution d'un programme, on utilise le nombre d'_opérations élémentaires_ à effectuer plutôt qu'une durée en seconde. En général on en cherche une majoration, plus rarement une minoration.

La raison est que le même programme s'exécutera plus ou moins vite
selon la machine mais que le nombre d'opérations ne changera pas.

Il reste à définir ce qu'est une opération élémentaire. C'est affaire de convention.

!!!example ""
    Exemple d'opérations considérées dans ce cours comme élémentaires : 

    ```C linenums="1"
        + − / ∗ % // opérations sur les nombres
        t [i] //accès en lecture / écriture
    ```

#### Opérations élémentaires en C

!!!quote "Définitions: Opérations élémentaires"
    Les opérations suivantes sont dites élémentaires (non exhaustif)

    - Opérations sur les nombres : `+ - \* / %`
    - Opértaions bitwise : `& << >> ^ | `
    - Affectation, Afficher, Retourner : `a = ... ; return ...`
    - Accès en lecture écriture: `t[i]`
    - Comparaisons de nombres déjà calculés : `... < ... ; ... == ...`
    - Libération : `free(t)`

Ces opérations, dites _élémentaires_ sont donc considérées comme de coût constant $O(1)$. Mais il y a des opérations à coût constant qui ne sont pas élémentaires. Par exemple `i++`.

- Dans `x < y`, la comparaison elle-même est en $O(1)$ mais l'évaluation de `x` et `y` peut être coûteuse.

La complexité temporelle d'un programme au pire : nombre max. d'op.
elem. à effectuer (en fonction de la taille du ou des arguments).

#### Dans d'autres langages

Bien que la syntaxe soit différente, les opérations en **OCaml** équivalentes à celles ci-dessus (lorsqu'elles existent) sont aussi considérées commes élémentaires. Le _filtrage_ est considéré comme le coût constant (mais n'est pas élémentaire).

En **Python**, on a coutume de considérer l'addition comme élémentaire bien que ce soit faux : en effet les entier étant non bornés, la somme de deux s'effectue sur des parties de ces nombres, Python rajoutant un traitement logiciel pour recooler les morceaux.

En MP2I, les allocations sur le tas `malloc`, `calloc` et `realloc` sont considérées comme à coût constant même si c'est en fait bien difficile à déterminer.

Les opérations d'affichage et de saisie `printf, scanf`peuvent prendre du temps. Il est préférable d'indiquer qu'elles ne sont pas comptabilisées dans le calcul de compléxité temporelle.

#### Exemples introductifs

!!!example ""
    **Exemple introductif 1**
    On cherche les diviseurs d'un entier $n$ :

    ```c linenums="1"
    void diviseurs1(int n){
        for(int i=2; i<n; i++){
            if(n%i == 0){
                printf("%d;", i); //compté comme O(1)
            }
        }
    }
    ```

    Ce programme effectue exactement $n - 2$ calculs de restes de divisions
    euclidiennes, $n - 2$ comparaisons et un nombre d'affichages inférieur ou égal à $n - 2$.
    Au maximum, il y a donc $5(n - 2) + 1$ _opérations élémentaires_ plus les $n - 2$ incrémentations de $i$ + comparaisons.
    
    L'ordre de grandeur est donc donné par une application affine : On dit
    que « la complexité temporelle est en $O(n)$.

!!!example ""
    **Exemple introductif 2**

    Si $n = pq$ avec $p ≥ \sqrt{n}$ alors $q ≤ \sqrt{n}$
    Nouveau principe : on cherche tous les diviseurs $q$ qui sont plus petits que $\sqrt{n}$; les autres valent $\frac{n}{q}$

    ```c linenums="1"
    void diviseurs2 (int n){
        int q ; int p ; // 2 op
        q = 1; // 1 op
        while (q ∗ q <= n){// 2 op
            if (n % q == 0){ // 2 ops
                printf("%d", q); // 1 op
                p = n / q; // 2 op
                if (p != q) // (ne pas afficher q 2 fois si n=pˆ2) :1 op
                    print(p); // 1 op
                q=q +1; // 2 op
            }
        }
    }
    ```

    - $\sqrt{n}$ itérations au plus, moins de $20$ opérations élémentaires par passage
    - En tout pas plus de $3 + 20\sqrt{n}$ opérations "élémentaires". $O(\sqrt{n})$

#### Taille du problème

Déterminer le temps d'exécution en fonction de la _taille_ du problème

!!!example ""
    **Exemples**

    - Recherche de diviseur de $n$. La taille est $n$, le temps d'exécution est proportionnel à $n$ ou $\sqrt{n}$
    - Manipulation d'une liste. Taille du problème : nombre d'éléments.
    - Traitement d'un fichier texte. Taille du problème : nombre de caractères.
    - Addition de matrices $n\times m$. Taille du problème le couple $(n, m)$ ou $nm$
    - Recherche d'un mot dans un fichier. Taille du problème le couple $(n,m)$ où $n$ est le nombre de carctères du texte et $m$ celui du mot.
    - Compression d'images : une image n'est rien de plus qu'une matrice de $w \times h$ de pixels. La taille du problème est $(w, h)$.

Souvent la taille du problème est indiquée dans l'énoncé.

#### Modèle de complexité temporelle impérative

La complexité d'un alogrithme est une fonction $C$ définie inductivement.

- On ne mesure pas le temps d'exécution d'un programme en secondes mais le _nombre d'opérations élémentaires_.
- Opération "de base". On considère qu'elles ont toures le même coût.
- Séquences : Le coût des instructions `p; q` en séquence est la somme des coûts de l'instruction `p` et de l'instruction `q` : $C(p) + C(q)$
- Tests : Le coûts d'une expression conditionnelle `if (b) {p;} else {q;}` est inférieur ou égal au maximum des coûts des instructions `p` et `q`, plus un test plus ou moins compliqué (temps d'évaluation de l'expression `b`). $max \left(C(p),C(q) \right ) + C(b)$.
En général on a $C(b) = O(1)$ et on le néglige. Mais ce n'est pas toujours vrai.

#### Coût d'une boucle `for`

Le coût d'un boucle `for(i = 0 ; i < m ; i++) p();` son coût est `m` fois le coût de l'instruction `p` si ce coût ne dépend pas de la valeur de `i` :

$1 + m \times \left( C(p) + 2 \right) = O\left( m \times C(p) \right)$

("$+2$" pour l'incrémentation et la comparaison à chaque tour de boucle, $1$ : initialisation).

- Quand le coût du corps de la boucle dépend de la valeur du compteur `i`, le coût total de la boucle est la somme des coûts du corps de la boucle pour chaque valeur de `i` + l'incrémentation :

$$\sum_{i = 0}^{m - 1}\left( C\left( p_{i} \right) + 2 \right) = O\left( \sum_{i = 0}^{m - 1}C\left( p_{i} \right) \right)$$

Le coût d'une boucle `for` ne peut pas être inférieur à $O(m)$ car il y a toujours l'incrémentation du compteur (sauf si dans le corps de la boucle se trouver une instruction d'arrêt ou de retour).

#### Coût d'un boucle `while`

Le nombre d'itération n'est pas connu à l'avance à priori. On évalue donc le nombre d'itération et on procède comme pour la boucle for en y ajoutant la complexité du test.

#### Complextié temporelle (cas fonctionnel)

!!!example "Exemple de complexité temporelle (OCaml)"
    On essaye d'établir une relation de récurrence entre $C(n)$ (la complexité pour une donnée de taille $n$) et la complexité des appels internes plus la complexité hors appel récursif.
    Complexité $C (n)$ en nombre de multiplications pour :

    ```OCaml linenums="1"
    let rec fact n =
        if n<2 then 1 else n * fact (n-1);;
    ```

    - $C(0) = C(1) = 0$
    - $C(n) = C(n - 1) + 3$ : une multiplication seulement dans le cas $n≥2$.

    Alors on a :

    $$C(n) = C(n - 1) + 1 = \left( C(n - 2) + 1 \right) + 1 = \ldots = \underbrace{C(1)}_{\text{cte}} + (n–1) = O(n)$$

#### Complextié temporelle (Récursion)

!!!example "Exemple de complexité temporelle (C)"
    Complexité $C (n)$ en nombre d'opérations élémentaires pour :

    ```C linenums="1"
    int fact(int n){ // n supposé positif
        if (n<2)
            return 1;
        else 
            return n * fact (n-1);
    }
    ```

    - $C(0) = C(1) = 0$ (une comparaison `n<2`). En fait, on pourrait aussi compter l'opération d'écriture à l'adresse de retour et d'autres opérations cachées, mais ce n'est pas si important car on veut un comportement assymptotique.
    - $C(n) = C(n - 1) + 3$ : (on compte `n<2`; `n-1`;`n * ...`)
    
    On obtient:

    $C(n) = C(n - 1) + 3 = \left( C(n - 2) + 3 \right) + 3 = \ldots = \underbrace{C(1)}_{\text{cte}} + 3(n–1) = O(n)$ 
    $\color{red}{\text{Il est plus simple de prendre } C(n) = C(n-1) + 1 \text{ (même ODG)}}$

#### Calculs simplifiés

En général, on ne s'encombre pas avec une trop grande précision.

Si $C(n) = C(n-1) = f(n)$ on choisit l'expression la plus simple ayant le même ODG que $f$. 

!!!example ""
     - $n²$ si $f(n) = 3n² + 5n +15$
     - $n2^n$ si $f(n) = 6n2^n + 30n^5 + log(n)$

Seuls cas où il faut être précis : lorsqu'on s'intéresse à une opération en particulier (Exemples : multiplications d'entiers; écritures dans un tableau...).

#### Limites du modèle

Le modèle de complexité présenté est une aproximation de la réalité.

Le modèle ne tient pas compte du fait que la taille des entiers peut être grande (surtout vrai en Python). La multiplication de deux très grands nombres est bien entendu plus coûteuse que celle de $2 \times 3$.
Il faudra par exemple d'abord lire tous les chiffres des opérandes et écrire ceux du résultat. Plus toutes les autres opérations logicielles que Python ajoute au traitement processeur.

Autre exemple, une très grande liste (ou matrice) ne tiendra pas en mémoire vive : il faudra faire des accès disques pour parcourir les éléments.

En CPGE, on convient que `malloc(..)` est une opération à coût constant (ce qui est faux). En revanche `realloc(..)` peut-être éventuellement en $O (1)$ dans des cas où il n'y a pas de copie à faire.

L'évaluation du temps mis par un algorithme pour s'exécuter est un domaine de recherche à part entière, car elle se révèle parfois très difficile.

#### Meilleur des cas

La complexité au pire est la plus significative, mais il peut être utile de connaître aussi la complexité dans le meilleur des cas, pour avoir une borne inférieure du temps d'exécution d'un algorithme.

En particulier, si la complexité dans le meilleur et dans le pire des cas sont du même ordre, cela signifie que le temps d'exécution de l'algorithme est relativement indépendant des données et ne dépend que de la taille du problème.

#### Complexité en moyenne

Parler de moyenne des temps d'exécution n'a de sens que si l'on a une idée de la fréquence des différentes données possibles pour un même problème de taille $n$.

Les calculs de complexité moyenne recourent aux notions définies en mathématiques dans le cadre de la théorie des probabilités et des statistiques.

La complexité en moyenne n'est, en général, pas attendue dans les sujets mais on en verra quelques exemples.

#### Complexité en espace

Il n'y a pas que le temps d'exécution des algorithmes qui intéresse les informaticiens.

Une autre ressource importante en informatique est la mémoire.

On appelle _complexité en espace_ d'un algorithme la place nécessaire en mémoire pour faire fonctionner cet algorithme _en dehors des paramètres du programme_.

Elle s'exprime également sous la forme d'un $O(f(n))$ où $n$ est la taille du problème.

#### Complexité en mémoire

En général, il suffit de faire le total des tailles en mémoire des différentes variables utilisées.

La seule vraie exception à la règle est le cas des fonctions récursives, qui cachent souvent une complexité en espace élevée du fait de l'empilement des stack frames.
La récursion terminale est une bonne solution à ce problème.

La complexité spatiale est nécessairement toujours inférieure (ou égale) à la complexité temporelle d'un algorithme : on suppose qu'une opération d'écriture en mémoire prend un temps $O(1)$, donc écrire une mémoire de taille $M$ prend un temps $O(M)$.

### Un mot sur la racine carrée

#### Objectif

Les pages qui suivent sont données pour la culture.

- Dans cette sous-section, on présente une astuce très utilisée par les cartes graphiques 3D pour calculer la racine carrée.
- Les cartes graphiques font en effet beaucoup de calculs géométriques (par exemple pour déterminer si un projectile rencontre une surface comme un mur : intersection droite plan).
- Ces calculs font intervenir des notions comme le vecteur normal à une surface (il faut diviser un produit vectoriel par une norme, donc la **racine carrée** de la somme des carrés des coordonnées du vecteur).
- D'où l'importance d'un calcul efficace de la racine carrée.

#### Méthode de Héron

Calcul de $\sqrt{A}$

- Par dichotomie avec une précision $ε$ en résolvant $x^2 −A = 0$ sur $[0; A + 1]$. Calcul en $O(log(\frac{A+1}{ε}))$. Ce n'est déjà pas si mal.
- Méthode de Héron (une spécialisation de la méthode de Newton à cet objectif particulier) :
  - Choisir $x_0$ initial puis $x_{k +1} = \frac{1}{2} (x_k + \frac{A}{x_k})$.
  - Convergence quadratique : le nombre de chiffres significatifs double à chaque itération. Au bout de quelques itérations (15 ou 20), on atteint la précision maximale possible sur un ordinateur de bureau. On peut alors considérer que le calcul de la racine carrée est quasiment en temps constant (pas tout à fait).

#### John Carmack et $\frac{1}{\sqrt{2}}$

John Carmack est un informaticien américain à l'origine de nombreux jeux
vidéos, dont Wolfenstein 3D, Doom et Quake.

- Les cartes graphiques 3D avec processeur 32 bits (la majorité des
cartes du marché en 2020) utilisent l'astuce de John Carmack et son
nombre magique pour calculer, non pas $\sqrt{A}$ mais $\frac{1}{\sqrt{A}}$.
- Le flottant $A$ codé sur 32 bits en $A$ = $(−1)^0 2^{e_1} (1 + m_1)$ est considéré comme l'entier $I_1 = 2^{23}(127 + e_1 + m_1)$ : `int i1 = *(int*) &a;`.
- Carmack utilise son nombre magique, l'entier codé en hexadécimal 0x5f3759df (on a trouvé mieux depuis) et lui soustrait $\left \lfloor\frac{I_1}{2}\right \rfloor$. Il convertit ensuite cet entier en flottant.
- Quelle que soit la valeur de $A$, on peut montrer que le flottant retourné est proche de $\frac{1}{\sqrt{A}}$ avec une erreur au pire de $\frac{2}{\sqrt{1000}}$!!

Complexité temporelle en $O (1)$.

## Exemples de complexités de fonctions récursives

### Récurrences $C_n = C_{n-1} + 1$ et $C_n = C_{n-1} + n$

#### Exponentiation naïve

```OCaml linenums="1"
let rec pow x n = 
    match n with 
    | 0 -> 1
    | _ -> x * pow x (n-1);;
```

- La complexité (dans tous les cas) est de la forme $C_n = C_{n-1} + 1$ (déjà étudié pour la facorielle)
- C'est une suite arithmétique de raison $1$.
- On a donc $C_n =Θ(n)$
- À noter que le second membre de la relation de récurrence est $1$ puisqu'il y a un nombre d'opérations élémentaires en $O(1)$ (mais pas égal à $1$, le filtrage lui-même coûte quelques opérations). On se simplifie la vie en prenant le terme le plus simple possible dans la classe de complexité.

#### Insertion dans une liste triée

```OCaml linenums="1"
let rec insert l x = 
    match l with 
    | [] -> [x]
    | y::q when y > x -> x::l
    | y::s -> y::(insert q x);;
```

- Hors appel récursif, il faut compter l'ajout en tête de liste (règle $3$) et le filtrage lui même (il n'a pas un coût nul, bien que constant).
- La relation de récurrence pour la complexité au pire est  donc $C_n = C_{n-1} = O(1)$ ce que l'on simplifie en $C_n = C_{n-1} +1$.
- Au mieux $C_n = 1$ (règle de filtrage $1$ et $2$).
- Complexité au pire $O(n)$; au mieux $O(1)$.

#### Tri par insertion

```OCaml linenums="1"
let rec tri l = 
    match l with 
    | [] -> []
    | x::q -> insert x (tri q);;
```

- Insertion dans `tri q`au pire en $O(n)$ où $n = |l|$. Complextié au pire $C_n = C_{n-1} + n$ ($n$ pour l'insertion au pire).
- On a donc $C_n - C_{n-1} = n$ Il vient par télescopage
$C_n - \underbrace{C_0}_{\text{cte}} = \sum_{i=1}^n{C_i -C_{i-1}} = \sum_{i=1}^n{i} = O(n²)$

Donc $C_n = O(n²)$

- Complexité au mieux $C_n = C_{n-1} +1$ Donc $C_n = O(n)$.

### Stratégie "diviser pour régner"

#### Diviser pour régner

!!!note ""
    **Méthode**

    - On découpe la donnée que l'on doit traiter en deux parties de tailles proches (ou plus) de tailles proches.
    - On résout le problème sur les parties plus petites
    - On combine les résultats obtenus pour construire le résultat correspondant à la donnée initiale

- Une donnée de taille $n$ est découpée en deux autres données de tailles
$\left\lfloor \frac{n}{2} \right\rfloor et\left\lceil \frac{n}{2} \right\rceil$.
- La complexité obéit alors à une relation de la forme :
$C_{n} = {aC}_{\left\lfloor \frac{n}{2} \right\rfloor} + {bC}_{\left\lceil \frac{n}{2} \right\rceil} + f(n)$
où $(a,b) \neq (0,0)$ sont des constantes associées au problème et $f(n)$ correspond au coût total du partage et de la recombinaison.
- Cette récurrence est dite de type «**diviser pour régner** »

#### Choix du premier terme

!!!warning ""
    **Proposition**

    Soient $a,b$ deux entiers tels que $ab \neq 0$ et $f, g$ deux fonctions croissantes de même ordre de grandeur. Alors les suites ($U_{n}$) et ($V_{n}$) telles que $U_{0} = V_{0}$ et pour tout $n \in {\mathbb{N}} ^\ast$ :

    $\begin{matrix}
    U_{n} = {aU}_{\left\lfloor \frac{n}{2} \right\rfloor} + {bU}_{\left\lceil \frac{n}{2} \right\rceil} + f(n) \\
    V_{n} = {aV}_{\left\lfloor \frac{n}{2} \right\rfloor} + {bV}_{\left\lceil \frac{n}{1} \right\rceil} + g(n) \\
    \end{matrix}$ sont du même ordre de grandeur.

Cette proposition (admise) justifie qu'on remplace le dernier terme par un terme plus simple de même ordre de grandeur (donc $1$ pour $O(1)$, $n²$ pour $O(n²)$ etc.)

#### Croissance de la complexité en fonction de la taille $n$ de la donnée

On considère $U_{n} = f(n) + U_{\left\lfloor \frac{n}{2} \right\rfloor} + U_{\left\lceil \frac{n}{2} \right\rceil}$ avec $f$ croissante. On pose aussi $u_0 = u_1 = 1$ parce que ça nous arrange et qu'on étudie un comportement asymptotique.

!!!note ""
    **Preuve**

    On montre que $U_{n - 1} \leq U_{n}$ pour $n ≥ 1$

    - **Initialisation**
        $1 = u₀ ≤ u₁ = 1$
    - **Hérédité**
    Si $U_{k} \geq U_{q}$ pour tout $n > k ≥ q$.
      - $U_{n} = f(n) + U_{\left\lfloor \frac{n}{2} \right\rfloor} + U_{\left\lceil \frac{n}{2} \right\rceil}$ et
      - $U_{n - 1} = f(n - 1) + U_{\left\lfloor \frac{n - 1}{2} \right\rfloor} + Uu_{\left\lceil \frac{n - 1}{2} \right\rceil}$
      - Or les fonctions parties entières sont croissantes donc
        $U_{\left\lfloor \frac{n - 1}{2} \right\rfloor} \leq U_{\left\lfloor \frac{n}{2} \right\rfloor}$ et $U_{\left\lceil \frac{n - 1}{2} \right\rceil} \leq U_{\left\lceil \frac{n}{2} \right\rceil}$ par HR. Et $f(n-1) ≤ f(n)$ par hypothèse.
      - Donc $U_{n} \geq U_{n - 1}$

    - **Conclusion**
    Donc $\left( U_{n} \right)↑$. $\color{red}\text{En pratique, on ne refait pas cette preuve et on admet}$ 
    $\color{red}\text{que la complexité est croissante avec la taille des données.}$

#### Quelques observations

Dans ce qui suit toutes les fonctions sont positives et $n ∈ ℕ$

- Pour $a > 0$, $ln(n) = Θ(log_a)$ car $log_a(x) = \frac{ln(x)}{ln(a)}$
- $n = Θ(\left \lfloor n\right \rfloor)$ car pour $n > 2$,
$\frac{n}{2} < n-1 < \left \lfloor n\right \rfloor ≤ n$ (de même $n=Θ(⌈n⌉)$)

#### Dichotomie

```OCaml linenums="1"
let dicho v x = (*Chercher x dans le tableau trié v*)
    let rec cherche_entre a b =
        if a = b then (x=v.(a))
        else
            let m = (a+b)/2 in
            if v.(m) = x then true
            else if v.(m) > x then cherche_entre a (m-1)
            else cherche_entre (m+1) b
    in
    cherche_entre 0 (Array.length v - 1);;
```

A chaque étape, la zone de recherche est divisée par $2$ (au moins) (on note $n//2$ la division euclidienne par $2$)

!!!example ""
    $x_m$ désigne l'élément où on sépare en 2 la liste.
    $[x ; x ; x_m ; x ; x]$ : Tableau de taille **impaire** $n$

    Les deux sous-tableaux sont donc de taille $n//2$ ou $n//2 +1$

    $[x ; x_m ; x ; x]$ : Tableau de taille **paire** $n$

    Au moins un sous tableau est de taille $n//2$

Hors les appels récursifs, il y a un nombre borné d'autres opérations, toutes en $O(1)$. Le coût total à chaque tour hors appels récursifs est donc $O(1)$.

Pour un tableau de taille $n$, la complexité au pire est du type
$C_{n} = C_{\left\lfloor \frac{n}{2} \right\rfloor} + O(1);$ simplifié
en $C_{n} = C_{\left\lfloor \frac{n}{2} \right\rfloor} + 1$ (On cherche
dans le plus grand sous-tableau pour que ça soit dans le pire cas)

Si $n = 2^{p}$ alors
$C_{2^{p}} = C_{2^{p - 1}} + 1 = C_{2^{p - \color{red}{2}}} + \underbrace{1 + 1}_{\color{red}{2}} = \ldots = C_{2⁰} + p = \Theta\left( \log ₂(n) \right)$

Pour un $n$ quelconque, $2^{\left\lfloor \log ₂n \right\rfloor} \leq n < 2^{\left\lfloor \log ₂(n) \right\rfloor + 1}$. Comme la complexité est croissante avec la taille du tableau, $C_{n} \leq C_{2^{\left\lfloor \log ₂(n) \right\rfloor + 1}} = O\left( \left\lfloor \log ₂(n) \right\rfloor + 1 \right)$. Il vient $C_{n} = O\left( \log ₂(n) \right)$.

Idem pour la minoration :
$C_{n} \geq C_{2^{\left\lfloor \log ₂(n) \right\rfloor}} = \Omega\left( \left\lfloor \log ₂(n) \right\rfloor + 1 \right)$

Conclusion, $C_{n} = \Theta\left( \log ₂(n) \right)$

#### Exponentation rapide

```OCaml linenums="1"
let rec puissance_rapide x n = 
    match n with 
    | 0 -> 1
    | n when n mod 2 = 0 puissance_rapide (x*x) (n/2)
    | _ -> x * puissance_rapide x (n-1) ;; 
```

- Principe $x^{2k} = (x²)^k$ et $x^{2k+1} = x \times x^{2k}$
- Complexité en nombre de multiplications
  - Lorsque $n$ est paire $C_n = C_{\frac{n}{2}} +1 = C_{\left \lfloor\frac{n}{2}\right \rfloor} +1$.
  - Si $n$ est impaire (et $> 1$)
  
- Donc récurrence de la forme $C_n = C_{\left \lfloor\frac{n}{2}\right \rfloor} +O(1)$. On étudie toujours la forme la plus simple $C_n = C_{\left \lfloor\frac{n}{2}\right \rfloor} +1$. On a vu que $C_n = O(log_2(n))$

- Il est pertinent ici d'exprimer la complexité du nombre de bits de $n$ : il faut $\left \lfloor log_2(n)\right \rfloor + 1 = Θ(log_2(n))$ Donc complexité linéaire en le nombre de bits de l'exposant.

### Tri fusion

Pour raison de commodité, on étudie la complexité d'une version non récursive terminale du tri fusion. On donne néanmoins une version récursive terminale après cette étude.

#### Division

On veut trier une liste. On sépare l'entrée en deux listes/deux tableaux (approximativement) de même longueur, qu'on trie séparément, puis on fusionne les deux résultats en comparant à chaque fois les plus petits éléments.

```OCaml linenums="1"
let rec divise l = (*Sépare l en deux listes de même taille*)
    match l with
    | [] -> ([], [])
    | [x] -> ([x], [])
    | x::y::q -> let (l1, l2) = divise q in (x::l1, y::l2);;
```

- Complexité temporelle est de la forme $C_{n} = C_{n - 2} + 1$. Ainsi pour $n = 2k$ :
$C_{2k} = C_{2k-2} + 1 = C_{2k-4} +1 +1 = ... = C_0 +k$
On trouve de même avec $C_{2k + 1} = C₁ + k$ pour $n = 2k+1$.

Comme dans les 2 hypothèses de parité et d'imparité, $k = \left\lfloor \frac{n}{2} \right\rfloor$, on obtient $C_{n} = \Theta(n)$

#### Fusion

On fusionne maintenant deux listes triées (par ordre croissant) en
ajoutant prioritairement le plus petit élément des deux listes.

```ocaml linenums="1"
let rec fusion l1 l2 = (*Fusionne deux listes triées*)
    match (l1, l2) with
    | ([], l) -> l
    | (l, []) -> l
    | (x::q, y::r) ->
        if x < y then x::(fusion q l1)
        else y::(fusion (l2) r);;
```

En dehors des appels récursifs, les opérations sont en $O(1)$ et en nombre borné.

La complexité dépend donc de la façon dont sont réparties les données dans les listes. Meilleur cas en $O\left( min(|l_1|,\ |l_2|) \right)$ : lorsque tous les éléments de la liste la plus courte sont plus petits que ceux de la plus longue.

Pour éviter un raisonnement qui tiendrait compte de la répartition des données, on peut étudier la complexité d'une fonction manifestement plus coûteuse mais plus simple à étudier : `fusion_couteuse` ci-dessous.

La fonction suivante réalise aussi la fusion. elle est plus coûteuse que la précédente mais plus facile à étudier :

```OCaml linenums="1"
let rec fusion_couteuse l1 l2 = (*Fusionne deux listes triées*)
    match (l1, l2) with
    | ([], []) -> []
    | (a::r, []) | ([], a::r) -> a::(fusion_couteuse r [])
    | (a::r, b::s) -> if a <= b 
        then a::(fusion_couteuse r l2)
        else b::(fusion_couteuse l1 s);;
```

La complexité dépend seulement de la somme des tailles des listes. En notant $n, m$ les tailles de `l1, l2` on obtient la relation suivante : $C(n+m) = C(n+m-1) + O(1)$ et $C(0, 0) = 1$

On étudie donc $C(n+m) = C(n+m-1)+1$. Il s'agit d'une suite arithmétique. On a donc $C(n+m) = O(n+m)$.

Donc la complexité de `fusion`, toujours meilleure est aussi en $O(n+m)$

#### Tri fusion

Et donc pour finir le tri fusion :

```OCaml linenums="1"
let rec tri_fusion l =
    match l with
    | [] -> [] 
    | [e] -> [e]
    | _ ->
        let (l1, l2) = divise l in
        fusion (tri_fusion l1) (tri_fusion l2);;
```

##### Complexité du tri

On note $n$ la longueur de `l`, La division est en $Θ(n)$. La fusion s'applique aux deux moitiés triées ; elle a ici un coût en $Θ(n)$ (même au meilleur cas).

La complexité obéit à une relation diviser-pour-régner de la forme

$$C_n = C_{\lfloor\frac{n}{2}\rfloor} + C_{\lceil\frac{n}{2}\rceil} + \Theta(n) \text{; simplifié en } C_n = C_{\lfloor\frac{n}{2}\rfloor} + C_{\lceil\frac{n}{2}\rceil} + n$$

Si $n = 2^{k}$ alors $\left\lfloor \frac{n}{2} \right\rfloor = \left\lceil \frac{n}{2} \right\rceil$(et $k = log₂ (n)$) :

$$C_{2^k} = 2^k + 2\times C_{2^{k-1}} = {\color{red}{2}}\times 2^k + 2^{\color{red}{2}} \times C_{2^{k-\color{red}{2}}} = \ldots = {\color{red}{k}}2^k + 2^{\color{red}{k}}C_{k-\color{red}{k}} = \Theta(n\log_2 n)$$

En général, $2^{\left\lfloor \log ₂n \right\rfloor} \leq n < 2^{\left\lfloor \log ₂n \right\rfloor + 1}$. Comme la complexité est croissante :

$$C_n \leq C_{2^{\left\lfloor \log_2 n \right\rfloor + 1}} = O\left( \overbrace{(\lfloor \log_2 n \rfloor + 1)}^{O(log_2(n))} \times \underbrace{2^{\lfloor \log_2 n \rfloor + 1}}_{O(n)} \right ) = O(n\log_2 n)$$

Idem pour une minoration : $C_n = Θ(n \log_2 n)$

#### Tri fusion en récursion terminale

On se donne ici une version totalement récursive terminale du tri fusion (on se souvient que `List.rev` est aussi en récursion terminale).

```OCaml linenums="1"
let rec split l l1 l2 = 
    match l with 
    | [] -> l1, l2
    | x::t -> split t l2 (x::l1);;

let rec merge acc l1 l2 = 
    match l1, l2 with
    | [], l' | l', [] -> (List.rev acc) @ l'
    | x1::t1, x2::t2 -> if x1 <= x2
        then merge (x1::acc) t1 l2 else merge (x2::acc) l1 t2;;

let rec sort l = 
    match l with
    | [] -> | [_] -> l
    | _ -> let l1, l2 = split l [] [] in 
        merge [] (sort l1) (sort l2) ;;
```

#### Derniers détails

Le tri d'un tableau est dit _en place_ si l'algorithme travaille directement sur le tableau en entrée et n'utilise pas de tableau auxiliaire.
Un tri est _stable_ s'il conserve les positions relatives des éléments de même valeur.

Il existe une version du tri fusion pour les tableaux.
Une version délicate (mais possible) existe pour un maintien en place mais non stable.

Le tri fusion est en $O(n \times log (n))$ au pire, au meilleur et en moyenne.

On ne peut pas faire mieux en moyenne (voir cours sur les arbres) pour un tri _par comparaison_ (algorithmes de tri procédant par comparaisons successives entre éléments).
