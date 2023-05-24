# Complexité

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par
    Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce
    site à pour seul but d’être plus compréhensible pendant les périodes de
    révision que des diaporamas.

## Définitions

!!!quote "Définition : Analyse de la complexité"
    Étude formelle de la quantité de ressources (de temps, d'espace mémoire, ...) nécessaire à l'exécution de cet algorithme
    
    - En temps il s'agit de savoir si une fonction termine dans un temps raisonnable
    - En mémoire il s'agit de déterminer si la fonction dispose bien toutes les ressources physiques pour s'exécuter.

!!!quote "Définition utiles"
    On note $D_{n}$ l'ensemble des données de taille n et C(d) la complexité d'un certain programme pour une donnée d.

    - $C_{\max}(n) = \max_{d \in D_{n}}C(d)$ Est la complexité dans le pire
    cas
    - $C_{\min}(n) = \min_{d \in D_{n}}C(d)$ Est la complexité dans le
    meilleur des cas
    - $C_{moy}(n) = \sum_{d \in D_{n}}^{}P(d)C(d)$ Est la complexité en
    moyenne avec P la loi de proba associée à l'apparition des données de
    taille n

!!!quote "Définition: Domination / Ordre de grandeur"
    **U est dominée par **V s'il existe
    $\lambda \in {\mathbb{R}} + etN \in {\mathbb{N}}$ tels que pour tout
    $n \in {\mathbb{N}}$, si n ≥ N alors $u_{n} \leq \lambda v_{n}$. On le
    note $u_{n} = O\left( v_{n} \right)$. 
    On dit aussi que V domine U.

    **U et V sont de même ordre de grandeur** si U domine V et V domine U (i.e. $u_{n} = O\left( v_{n} \right)etv_{n} = O\left( u_{n} \right)$). On le
    note $u_{n} = \Theta\left( v_{n} \right)$ (« $u_{n}$ est en grand thêta
    de $v_{n}$»)

!!!quote "Définition : Équivalence / Grand Oméga"
    On écrit $u_{n} = \Omega\left( v_{n} \right)$ (« $u_{n}$est en grand
    Oméga de $v_{n}$ ») s'il existe$\lambda \in {\mathbb{R}} +$ et
    $N \in {\mathbb{N}}$ tels que pour tout $n \in {\mathbb{N}}$, si n ≥ N
    alors $u_{n} \geq \lambda v_{n}$.

    Observons que dans ce cas $\frac{1}{\lambda}u_{n} \geq v_{n}$ et donc
    $u_{n} = \Omega\left( v_{n} \right) \Leftrightarrow v_{n} = O\left( u_{n} \right)$.
    
    **U et V sont dites équivalentes** si et seulement si $v_{n} \neq 0$ à
    partir d'un certain rang et $\frac{u_{n}}{v_{n}}$ tend vers 1 en
    $n \rightarrow + \infty$. Cette définition (qui nous suffit) est plus
    restrictive que celle du cours du maths.

!!!tip ""
    **Remarque**
    
    Si $u_{n} \sim v_{n}$ alors $u_{n} = \Theta\left( v_{n} \right)$.

### Ordres de grandeur

Pour une donnée de taille n

  | Dénomination            | Définition              | Exemple
  | ----------------------- | ----------------------- | -------------------------
  | Logarithmique           |       $O(\log n)$       | Recherche dans une liste triée (pire cas)
  | Linéaire                |         $O(n)$          | Inversion d'une liste
  | Quasi-linéaire          |      $O(n\log n)$       | Tri fusion
  | Polynomiale             |               $O(n²)$   | Tri par insertion (pire cas)
  | Polynomiale             |       $O(n^k)$ pour k>1 | Produit matriciel naïf en $O\left( n^{3} \right)$
  | Exponentielle           |$O(\alpha^n)$ pour $\alpha >1$| Satisfiabilité d'une formule (pire cas)

### Quoi compter ?

La plupart du temps, on se contente de donner une majoration « à
constante multiplicative près » du temps de calcul. On ne compte pas
précisément le nombre d'opérations élémentaires (est-ce bien malin de
mettre dans le même sac un accès à un élément de tableau et une addition
binaire ?).

Dans ce cas, la notation en O(f(n)) (si n est l'entrée) nous suffit.
Parfois, on s'intéresse à une opération spécifique, et dans ce cas un
décompte précis est favorisé.

!!!example ""
    **Exemple**

    - Le nombre d'échanges d'éléments dans un tri de tableau
    - Le nombre de produits de flottants dans un produit matriciel « optimisé »

## Opérations élémentaires

Pour mesurer le temps d'exécution d'un programme, on utilise le nombre
d'opérations élémentaires à effectuer plutôt qu'une durée en seconde.
En général on en cherche une majoration, plus rarement une minoration.

La raison est que le même programme s'exécutera plus ou moins vite
selon la machine mais que le nombre d'opérations ne changera pas.

!!!quote "Définitions: Opérations élémentaires"
    Les opérations suivantes sont dites élémentaires (non exhaustif)
    
    - Opérations sur les nombres : + - \* / %
    - Affectation, Afficher, Retourner : a = ... ; return ...
    - Accès : t[i]
    - Libération : free(t)
    - Comparaisons de nombres : ... < ... ; ... == ...

La complexité temporelle d'un programme au pire : nombre max. d'op.
elem. à effectuer (en fonction de la taille du ou des arguments).

!!!example ""
    **Exemple introductif**

    ```c linenums="1"
    void diviseurs1(int n){
        for(int i=2; i<n; i++){
            if(n%i == 0){
                printf("%d;", i);
            }
        }
    }
    ```

    Ce programme effectue exactement n - 2 calculs de restes de divisions
    euclidiennes, n - 2 comparaisons et un nombre d'affichages inférieur ou
    égal à n - 2. Au maximum, il y a donc 5(n - 2) + 1 opérations
    élémentaires plus les n - 2 incrémentations de i + comparaisons.
    L'ordre de grandeur est donc donné par une application affine : On dit
    que « la complexité temporelle est en O(n)

### Taille du problème

Déterminer le temps d'exécution en fonction de la taille du problème

!!!example ""
    **Exemples**

    - Recherche de diviseur de n. La taille est n, le temps d'exécution est proportionnel à n
    - Manipulation d'une liste. Taille du problème : nombre d'éléments.
    - Traitement d'un fichier texte. Taille du problème : nombre de caractères.
    - Addition de matrices $n\times m$. Taille du problème le couple (n, m) ou nm

Souvent la taille du problème est indiquée dans l'énoncé.

### Conditions

Si on a une condition if ... else ... on prend le nombre d'opérations
élémentaires maximum entre les deux + celui du test effectué dans la
condition

### Boucles For

Pour la boucle `for(i = 0 ; i < m ; i++) p();` son coût est m fois le
coût de l'instruction p si ce coût ne dépend pas de i (condition sur la
parité de i par exemple) :

$$1 + m \times \left( C(p) + 2 \right) = O\left( m \times C(p) \right)$$

avec +2 pour l'incrémentation et la comparaison à chaque tour de
boucle.

Si le coût dépend de i, on a comme coût total de la boucle une somme des
coûts du corps de la boucle pour chaque valeur de i +
l'incrémentation :

$$\sum_{i = 0}^{m - 1}\left( C\left( p_{i} \right) + 2 \right) = O\left( \sum_{i = 0}^{m - 1}C\left( p_{i} \right) \right)$$

Ainsi le coût d'une boucle for ne peut pas être inférieur à $O(m)$ car il
y a toujours l'incrémentation du compteur sauf si dans le corps de la
boucle se trouver une instruction d'arrêt ou de retour.

### Boucles While

Le nombre d'itération n'est pas connu à l'avance à priori. On évalue
donc le nombre d'itération et on procède comme pour la boucle for en y
ajoutant la complexité du test.

!!!example "Exemple de complexité temporelle (Ocaml)"
    On essaye d'établir une relation de récurrence entre C(n) (la
    complexité pour une donnée de taille n) et la complexité des appels
    internes plus la complexité hors appel récursif.
    Complexité C (n) en nombre de multiplications pour :

    ```ocaml linenums="1"
    let rec fact n =
        if n<2 then 1 else n * fact (n-1);;
    ```

    - C(0) = C(1) = 0
    - C(n) = C(n - 1) + 3 : une multiplication seulement dans le cas n≥2 avec
    la comparaison n<2 et le n-1.

    Alors on a :

    $$C(n) = C(n - 1) + 3 = \left( C(n - 2) + 3 \right) + 3 = \ldots = C(1) + 3(n–1) = O(n)$$
    
    En général, on ne s'encombre pas avec une trop grande précision. On
    fixe C(0) = C(1) = 1 et, si les opérations hors appels récursifs sont en
    O(f(n)), on prend le représentant le plus simple (ex : n pour O(n))

### Meilleur des cas

La complexité au pire est la plus significative, mais il peut être utile
de connaître aussi la complexité dans le meilleur des cas, pour avoir
une borne inférieure du temps d'exécution d'un algorithme. En
particulier, si la complexité dans le meilleur et dans le pire des cas
sont du même ordre, cela signifie que le temps d'exécution de
l'algorithme est relativement indépendant des données et ne dépend que
de la taille du problème.

### Complexité en moyenne

Parler de moyenne des temps d'exécution n'a de sens que si l'on a une
idée de la fréquence des différentes données possibles pour un même
problème de taille n. Les calculs de complexité moyenne recourent aux
notions définies en mathématiques dans le cadre de la théorie des
probabilités et des statistiques. La complexité en moyenne n'est, en
général, pas attendue dans les sujets mais on en verra quelques
exemples.

### Complexité en espace

On appelle complexité en espace d'un algorithme la place nécessaire en
mémoire pour faire fonctionner cet algorithme en dehors des paramètres
du programme. Elle s'exprime également sous la forme d'un $O(f(n))$ où n
est la taille du problème.

### Complexité en mémoire

En général, il suffit de faire le total des tailles en mémoire des
différentes variables utilisées. La seule vraie exception à la règle est
le cas des fonctions récursives, qui cachent souvent une complexité en
espace élevée du fait de l'empilement des stack frames. La récursion
terminale est une bonne solution à ce problème. La complexité spatiale
est nécessairement toujours inférieure (ou égale) à la complexité
temporelle d'un algorithme : on suppose qu'une opération d'écriture
en mémoire prend un temps $O(1)$, donc écrire une mémoire de taille M
prend un temps $O(M)$.

## Diviser pour régner

!!!note ""
    **Méthode**

    On découpe la donnée que l'on doit traiter en deux parties de tailles
    proches (ou + que 2 parts).
    On résout le problème sur les parties plus petites
    On combine les résultats obtenus pour construire le résultat
    correspondant à la donnée initiale

Une donnée de taille n est découpée en deux autres données de tailles
$\left\lfloor \frac{n}{2} \right\rfloor et\left\lceil \frac{n}{2} \right\rceil$.
La complexité obéit alors à une relation de la forme :

$$C_{n} = {aC}_{\left\lfloor \frac{n}{2} \right\rfloor} + {bC}_{\left\lceil \frac{n}{2} \right\rceil} + f(n)$$

Avec $(a,b) \neq (0,0)$ sont des constantes associées au problème et
f(n) correspond au coût total du partage et de la recombinaison.
Cette récurrence est dite de type «** diviser pour régner** »

!!!warning ""
    **Proposition**

    Soient a,b deux entiers tels que $ab \neq 0$ et f, g deux fonctions
    croissantes de même ordre de grandeur. Alors les suites ($u_{n}$) et
    ($v_{n}$) telles que $u_{0} = v_{0}$ et pour tout
    $n \in {\mathbb{N}} \ast$ :

    $\begin{matrix}
    u_{n} = {au}_{\left\lfloor \frac{n}{2} \right\rfloor} + {bu}_{\left\lceil \frac{n}{2} \right\rceil} + f(n) \\
    v_{n} = {av}_{\left\lfloor \frac{n}{2} \right\rfloor} + {bv}_{\left\lceil \frac{n}{1} \right\rceil} + g(n) \\
    \end{matrix}$sont du même ordre de grandeur.

    Cette proposition (admise) justifie qu'on remplace le dernier terme par
    un terme plus simple de même ordre de grandeur (donc 1 pour O(1), n²
    pour O(n²) etc.)

### Croissance de la complexité

On considère
$u_{n} = f(n) + u_{\left\lfloor \frac{n}{2} \right\rfloor} + u_{\left\lceil \frac{n}{2} \right\rceil}$
avec f croissante. On pose aussi u₀ = u₁ = 1 parce que ça nous arrange
et qu'on étudie un comportement asymptotique.

!!!note ""
    **Preuve**

    On montre que $u_{n - 1} \leq u_{n}$ pour n ≥ 1

    -   *Initialisation*
        1 = u₀ ≤ u₁ = 1

    -   *Hérédité*
        Si $u_{k} \geq u_{q}$ pour tout n \> k ≥ q.
        $u_{n} = f(n) + u_{\left\lfloor \frac{n}{2} \right\rfloor} + u_{\left\lceil \frac{n}{2} \right\rceil}\text{ et }u_{n - 1} = f(n - 1) + u_{\left\lfloor \frac{n - 1}{2} \right\rfloor} + u_{\left\lceil \frac{n - 1}{2} \right\rceil}$
        
        Or les fonctions parties entières sont croissantes donc
        $u_{\left\lfloor \frac{n - 1}{2} \right\rfloor} \leq u_{\left\lfloor \frac{n}{2} \right\rfloor}etu_{\left\lceil \frac{n - 1}{2} \right\rceil} \leq u_{\left\lceil \frac{n}{2} \right\rceil}$
        par HR et f(n-1) ≤ f(n) par hypothèse.
        Donc $u_{n} \geq u_{n - 1}$

    -   *Conclusion*
        $\forall n \in {\mathbb{N}},\left( u_{n} \right)$.

    En pratique, on refait pas cette preuve, et on admet que la complexité
    est croissante avec la taille des données.

### Dichotomie <3

```ocaml linenums="1"
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

A chaque étape, la zone de recherche est divisée par 2 (au moins) (on
note n//2 la division euclidienne par 2)

!!!example ""
    [x ; x ; x ; x ; x] : Tableau de taille **impaire** n

    Les deux sous-tableaux sont donc de taille n//2 (du début jusque
    l'élément m exclus et de l'élément m exclus jusque la fin du tableau)

    [x ; x ; x ; x] : Tableau de taille **paire** n

    Au moins un sous tableau est de taille n//2

Hors les appels récursifs, il y a un nombre borné d'autres opérations,
toutes en O(1). Le coût total à chaque tour hors appels récursifs est
donc O(1).
Pour un tableau de taille n, la complexité au pire est du type
$C_{n} = C_{\left\lfloor \frac{n}{2} \right\rfloor} + O(1);$ simplifié
en $C_{n} = C_{\left\lfloor \frac{n}{2} \right\rfloor} + 1$ (On cherche
dans le plus grand sous tableau pour que ça soit dans le pire cas)

Si $n = 2^{p}$alors
$C_{2^{p}} = C_{2^{p - 1}} + 1 = C_{2^{p - 2}} + 1 + 1 = \ldots = C_{2⁰} + p = \Theta\left( \log ₂n \right)$

Sinon pour un n quelconque,
$2^{\left\lfloor \log ₂n \right\rfloor} \leq n < 2^{\left\lfloor \log ₂n \right\rfloor + 1}$
et comme la complexité est croissante avec la taille du tableau, il
vient
$C_{n} \leq C_{2^{\left\lfloor \log ₂n \right\rfloor + 1}} = O\left( \left\lfloor \log ₂n \right\rfloor + 1 \right)$ainsi
on a $C_{n} = O\left( \log ₂n \right)$.
Idem pour la minoration :
$C_{n} \geq C_{2^{\left\lfloor \log ₂n \right\rfloor}} = \Omega\left( \left\lfloor \log ₂n \right\rfloor + 1 \right)$

Conclusion, $C_{n} = \Theta\left( \log ₂n \right)$

### Tri fusion <3

Pour raison de commodité, on étudie la complexité d'une version non
récursive terminale du tri fusion. On donne néanmoins une version
récursive terminale après cette étude.

On veut trier une liste. On sépare l'entrée en deux listes/deux
tableaux (approximativement) de même longueur, qu'on trie séparément,
puis on fusionne les deux résultats en comparant à chaque fois les plus
petits éléments.

```ocaml linenums="1"
let rec divise l = (*Sépare l en deux listes de même taille*)
    match l with
    | [] -> ([], [])
    | [x] -> ([x], [])
    | x::y::q -> let (l1, l2) = divise q in (x::l1, y::l2);;
```

Complexité temporelle est de la forme $C_{n} = C_{n - 2} + 1$. Ainsi
pour n = 2k :

On trouve de même avec $C_{2k + 1} = C₁ + k$ pour n = 2k+1.

Comme dans les 2 hypothèses de parité,
$k = \left\lfloor \frac{n}{2} \right\rfloor$, on obtient
$C_{n} = \Theta(n)$

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

En dehors des appels récursifs, les opérations sont en O(1) et en nombre
borné.
La complexité dépend donc de la façon dont sont réparties les données
dans les listes. Meilleur cas en
$O\left( \text{min(|l1|,\ |l2|)} \right)$ : lorsque tous les éléments de
la liste la plus courte sont plus petits que ceux de la plus longue.

Pour éviter un raisonnement qui tiendrait compte de la répartition des
données, on peut étudier la complexité d'une fonction manifestement plus
coûteuse mais plus simple à étudier :

```ocaml linenums="1"
let rec fusion_couteuse l1 l2 = (*Fusionne deux listes triées*)
    match (l1, l2) with
    | ([], []) -> []
    | (x::q, []) | ([], x::q) -> x::(fusion_couteuse q [])
    | (x::q, y::r) ->
        if x < y then x::(fusion_couteuse q l2)
        else y::(fusion_couteuse l1 r);;
```

La complexité dépend seulement de la somme des tailles des listes. En
notant n, m les tailles de l1, l2 on obtient la relation suivante :
C(n+m) = C(n+m-1) + O(1) et C(0 + 0) = 1

On étudie donc C(n+m) = C(n+m-1)+1. Il s'agit d'une suite arithmétique.
On a donc C(n+m) = O(n+m)
Ainsi la complexité de fusion est aussi en O(n+m)

Et donc pour finir le tri fusion :

```ocaml linenums="1"
let rec tri_fusion l =
    match l with
    | [] -> [] 
    | [e] -> [e]
    | _ ->
        let (l1, l2) = divise l in
        fusion (tri_fusion l1) (tri_fusion l2);;
```

On note n la longueur de l, La division est en Θ(n). La fusion
s'applique aux deux moitiés triées ; elle a ici un coût en Θ(n) (même au
meilleur cas). La complexité obéit à une relation diviser-pour-régner de
la forme

$$C_n = C_{\lfloor\frac{n}{2}\rfloor} + C_{\lceil\frac{n}{2}\rceil} + \Theta(n) \text{ simplifié en } C_n = C_{\lfloor\frac{n}{2}\rfloor} + C_{\lceil\frac{n}{2}\rceil} + n$$

Si
$n = 2^{k}$ alors $\left\lfloor \frac{n}{2} \right\rfloor = \left\lceil \frac{n}{2} \right\rceil$(et k = log₂ n) :

$$C_{2^k} = 2^k + C_{2^{k-1}} = 2\times 2^k + 2\times 2C_{2^{k-2}} = \ldots = k2^k + 2^kC_1 = \Theta(n\log_2 n)$$

En général,
$2^{\left\lfloor \log ₂n \right\rfloor} \leq n < 2^{\left\lfloor \log ₂n \right\rfloor + 1}$
comme la complexité est croissante :

$$C_n \leq C_{2^{\left\lfloor \log_2 n \right\rfloor + 1}} = O((\lfloor \log_2 n \rfloor + 1)2^{\lfloor \log_2 n \rfloor + 1}) = O(n\log_2 n)$$

Idem pour une minoration : $C_n = Θ(n \log_2 n)$

## Suppléments à graille

Le tri d'un tableau est dit **en place** si l'algorithme travaille
directement sur le tableau en entrée et n'utilise pas de tableau
auxiliaire. Un tri est stable s'il conserve les positions relatives des
éléments de même valeur.

Il existe une version du tri fusion pour les tableaux. Une version
délicate (mais possible) existe pour un maintien en place mais non
stable.

Le tri fusion est en O(n log n) au pire, au meilleur et en moyenne.
On ne peut pas faire mieux en moyenne (voir cours sur les arbres) pour
un tri par comparaison (algorithmes de tri procédant par comparaisons
successives entre éléments).
