# Complexité en moyenne et amortie

!!! danger
    Ce cours n'a pas été entièrement reverifié après le passage du programme. Pensez à supprimer ce message si vous avez reverifié ce cours

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par
    Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce
    site à pour seul but d’être plus compréhensible pendant les périodes de
    révision que des diaporamas.

???+ abstract "Sommaire"
    - Tri rapide  
    - Complexité amortie  

!!!tip "Crédits"
    - Wikipédia (Analyse amortie)
    - "Option informatique MPSI - MP/MP*" Roger MANSUY (Vuibert)
    - "Algorithmique - 3ème édition Cours avec 957 exercices et 158 problèmes" Thomas H. Corman, Charles Leiserson, Ronald Rivest, Clifford Stein (Dunod)
    - "Informatique - MP2I/MPI - CPGE 1re et 2e années" Balabonski Thibaut, Conchon Sylvain, Filliâtre Jean-Christophe, Nguyen Kim, Sartre Laurent (ellipse)

## Tri rapide

!!! note ""
    - Dans le tri rapide, on sépare l’entrée en deux listes en comparant a un élément pivot, on les trie séparément et on les fusionne par concaténation.
    - Dans le tri rapide, la fonction de division est délicate et celle de fusion triviale. C’est le contraire pour le tri fusion.
    - On donne une version pour les listes. On peut en donner une version en place pour des tableaux.
    - Mis au point en 1960 par Tony Hoare, alors étudiant en visite à l’université d’état de Moscou.
    - Possède une variante Quick select pour le calcul du k-ièmeélement d’une liste sans procéder au tri préalable (application : calcul de la médiane)

### Partition

```ocaml linenums="1"
Partition
let rec partition l p = match l with
    | [] -> [],[]
    | t::q -> let (l1, l2) = partition q p in
        if t <= p then (t::l1, l2) else (l1, t::l2) ;;
```

!!! note "Terminaison"  
    - Les cas de bases terminent
    - Et l’appel interne se fait avec une liste strictement plus courte que la  liste initiale
    - Et il n’y a pas de boucle ou d’appel à une fonction qui ne termine pas. C’est tout bon !
  
Complexité temporelle : dans tous les cas de la forme $Cn = C_{n-1} + 1$  pour une liste de taille n. Linéaire.  

!!! note "Correction"
    Montrons que la fonction retourne deux listes dont la 1ere contient tous les éléments de l plus petits que le pivot, les éléments strictement plus  grands étant dans la seconde.  

    - Cas de base : OK de façon évidente  
    - Supposons que `partition q p` partitionne correctement `q` en $l_1$, $l_2$. Supposons aussi t ≤ p. Alors dans le tuple retourné :  
        - On ajoute t à la liste $l_1$ des éléments de q plus petits que p. On  obtient exactement tous les éléments de l plus petits que le pivot.
        - La liste $l_2$ est constituée exactement des éléments de q strictement  plus grands que p ; donc (puisque t ≤ p) exactement ceux ceux de $l$. 
    - Correction OK. Le cas t > p cas est laissé au lecteur.  

### Tri rapide

```ocaml linenums="1"
let rec tri_rapide l = match l with
    | [] -> []
    | t::q -> let (l1, l2) = partition q t in
        (tri_rapide l1)@(t::(tri_rapide l2));;
```

!!! note "Terminaison"  
    - Variant |l|.  
    - Le cas de base termine (envoie de la liste vide).  
    - L’appel à `partition` termine (déjà vu).
    - Seulement deux appels internes, tous deux eﬀectués avec des listes de taille strictement inférieure à |$l$| (puisque |$l_1$| + |$l_2$| = |$l$| − 1).
    - Terminaison OK.  

!!! note "Correction"
    Cas de base : OK. Supposons que les deux appels internes  retournent une version triée de `l1` et de `l2`.  

    - Par correction de `partition` : `l1` contient les éléments de q plus petits ou égaux au pivot et `l2` les éléments strictement plus grands.  
    - La concaténation retournée 
      - est triée : d’abords les éléments plus petits que le pivot rangés dans  l’ordre, puis le pivot, puis les élements plus grands que le pivot rangés  dans l’ordre.  
      - contient exactement tous les éléments de `q` (puisque la réunion de `l1` et `l2` les contient) plus l’élément manquant `t`. La concaténation est la version triée de `l` . Hérédité OK  

### Complexité en nombre de comparaisons

#### Cas où l'une des listes est toujours vide

Tri rapide : complexité en nombre de comparaisons  
HR(n) : pour tout q ≤ n : 1) Tq est la pire complexité et 2)  
∀k ∈ {1, . . ., n}, Tk− + Tn−k + n − 1 ≤ Tn (égalité si k = 1)  
Cas de base n = 1. T = 0 = T, k ∈ {1}. Alors  T− + T− + 1 − 1 = 0 ≤ T. Et T est la pire cpx. HR(1) OK.  Si HR(n) est vérifiée. Soit k ∈ {1, . . ., n + 1}  
Tk− + Tn−k + n =      T  
Tk− + Tn−k + (n − k) + n − 1 + 1 ≤    HRn  
. . .  
. . . Tn + (n − k + 1) ≤ Tn + n = Tn ce qu’on veut.  Si Cn désigne la pire complexité, soit k la position réalisant le pire.  Tk− + Tn−k + n ≤ Tn.  On a Cn = Ck− + Cn−k + n ≤    n  
Donc Tn est pire que le pire !  

### Complexité moyenne du tri rapide

Il vient  C (n)  n + 1  
−  
C (n − 1)  n  
=  
2  n  Par télescopage, et puisque C (0) = 0  
4  n + 1  
−  
=  
2(n − 1)  n(n + 1)  
(décomp. en éléments simples)  
C (n)  n + 1  
−  
C (0)  1  
C (n)  n + 1  
=  
=  
=  
= 4 {sum symbol}n  
4 {sum symbol}n  k  
2 {sum symbol}n  
k  
1  k + 1  − 2 {sum symbol}n  4  n + 1  
+  
k  1  k  1  k  
= 2 {sum symbol}n  
k  
1  k  
+  
4  n + 1  
− 4 ≤ 2 ×  
1  k  
k  
k  
− 2 {sum symbol}n  1  k  − 2  n  {sum symbol}  
1  k  k      ∼n  
+  
4  n + 1      O/n  


1  on sait que {sum symbol}n  k  d’Euler (γ ~= 0.577, cf. cours de maths).  
k  
= ln n + γ + O(1/n) où γ est la constante  
= (2 ln n + 2γ + O(1/n)) +  
Alors  C (n)  n + 1  Donc C (n) = O(n log n).  On peut minorer de la même façon, donc Cn = Θ(n log n)  
4  n + 1  
− 4 = 2 ln n + 2γ + O(1/n) − 4.  

## Complexité amortie

### Le programme a buggé il manque tt une partie du diapo là

#### Retour au compteur binaire

Complexité en nombre d’accès aux cases du tableau :  
Alors  
Ai = Ci + φ(ci) − φ(ci) =  
2(k + 1) + 2 − 2k = 4 si k (cid:54)= n  2k − 2k si k = n  
On en déduit que la complexité amortie est bornée par 4. D’après le  corollaire du th. d’amortissement, toute séquence d’incrémentation  commençant en 00 . . . 0 réalise moins de 4 opérations d’accès au  tableau par appel à incr.
