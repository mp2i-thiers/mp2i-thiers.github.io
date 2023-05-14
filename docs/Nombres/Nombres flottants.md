# Nombres flottants

!!! warning 
    Ce cours a été automatiquement traduit des transparents de M.Noyer par
    Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce
    site à pour seul but d’être plus compréhensible pendant les périodes de
    révision que des diaporamas.

**Problème :** On veut représenter une partie des nombres avec une quantité finie de chiffres après la virgule

**Idée :** On se donne tous les chiffres composants le nombre de la position de la virgule : 1234,2 représente 12,34 c’est à dire 1,234x10¹

**Comment :** On peut écrire tous les nombres réels non nuls sous la forme $(−1)^s(1+m)2^e$ avec

- s∈{0,1} le signe du nombre
- m∈[0, 1[ la mantisse
- e∈ℤ son exposant

L’exposant du nombre représente la position de la virgule dans son expression en base 2

1+m est un nombre entre 1 et 2 (exclu). Il s’écrit sous la forme $1.c₁...c_i$ avec les $c_i∈{0,1}$. Comme les ordinateurs ne gère que des quantités finies, le nombre de chiffres binaires $c_i$ est borné (il est souvent égal à 23 ou 52)

L’avantage d’avoir une virgule flottante c’est qu’on peut exprimer *6 fois plus* d’expressions qu’en virgule fixe, mais il faut coder la position de la virgule

## IEEE754

Standard le plus employé pour le calcul des nombres à virgule flottante, il définit les formats de représentation des nombres à virgule flottante (signe mantisse, exposant, nombres dénormalisés) et valeurs spéciales (infinis et NaN) en même temps qu’un ensemble d’opérations sur les nombres flottants en plus de 4 modes d’arrondi et 5 exceptions (comprenant les conditions dans lesquelles une exception se produit, et ce qui se passe dans ce cas)

Les modes d’arrondi :

- Vers moins l’infini : ⌊−3,−4⌋=−4  
- Vers plus l’infini :⌈−3,−4⌉=−3
- Vers 0 : ⌊−3,−4⌋=−3 et ⌊3,4⌋=3
- Au plus proche (avec le cas particulier de 1,5 qu’on arrondi vers 1 ou 2?)

La version 1985 définit 4 formats pour représenter les nombres à virgule flottante :

- Simple précision (32bits : 1 bit signe, 8 d’expos, 23 de mantisse et 1 bit implicite)
- Simple précision étendue (≥ 43 bits, obsolète)
- Double précision (64bits : 1 bit de signe, 11 d’expo, 52 de mantisse, 1 implicite)
- Double précision étendue (>79 bits)

!!!quote "Bit implicite"
    La mantisse représente un nombre décimal entre 1 et 2 (exclu), par exemple **1.** 1101100011. Rendre le bit 1 implicite consiste à écrire 1101100011, la partie **1.** étant sous entendue puisque toujours la même.

Le format (1, E, M) sur 1 + E + M bits :

<p align="center"><img src="/images/Nbre_flottants/nbr1.png"></p>
<p align="center"><img src="/images/Nbre_flottants/nbr2.png"></p>

Ce format (1, 8, 23) est obsolète mais on l’utilise parce que c’est plus simple à afficher

- Bit de poids fort à 1 : Négatif, à 0 : Positif
- Exposant : Pas de représentation en complément à 2. L’exposant est décalé, afin de le stocker sous forme d’un nombre non signé. En notant E le nombre de chiffres (toujours le même nombre) de l’exposant, on ajoute un décalage de $d=2^{E−1}−1$
- Avec E = 8, d = 127. L’exposant est dans l’intervalle [-127 ; 128], donc l’exposant décalé est dans [0, 255] ; 0 et 255 ayant une signification spéciale.

Ils sont longs de 4 octets (32 bits) : (1, 8, 23)

La mantisse complète, le significande, doit être considérée comme une valeur sur 24 bits. Si la mantisse avec bit 1 implicite est 101000… alors le significande en base 2 est **1.** 101000

La quantité de nombres représentables au format (1, E, M) est grande mais pas infinie (mince). L’ordinateur travaille donc avec des valeurs en général approchées :

Avec floatx=0,1, l’ordinateur travaille en interne avec $00111101110011001100110011001101₂$ c’est à dire $0.100000001490116_{10}$ qui est égal à $\frac{13421773}{134217728}$

## Format (1,E,M)

En fonction de la valeur du champ exposant décalé, certains nombres peuvent avoir une signification spéciale *(Nombre dénormalisé, Zéro, Infini, NaN)*. L’exposant est décalé dans$⟦0,2^E−1⟧$donc le décalage est de $2^{E−1}−1$. Le bit implicite de la mantisse est déterminé par *la valeur de l’exposant décalé*.

Si l’exposant décalé est différent de 0 et de $2^E−1$, le bit implicite est 1, et le nombre est dit normalisé. Si l’exposant décalé est nul, par convention, le bit implicite vaut 0. Le nombre est dit dénormalisé. *La représentation au format dénormalisé est destinée aux très petites quantités en valeur absolue.*

La quantité de nombre à virgules flottante sur une machine et grande mais *finie*. Chaque nombre positif (sauf le plus grand et plus petit) à un successeur et un prédécesseur positif

Le successeur du nombre dénormalisé positif le plus grand est le plus petit nombre normalisé positif. *Zéro n’est ni normalisé ni dénormalisé*. Il y a deux écritures +0 et -0

Un nombre dénormalisé à une mantisse non nulle, un champ exposant décalé : *E bits à 0*. Tous les nombres dénormalisés ont le même exposant. Par convention, l’exposant pour les nombres dénormalisés est en fait égal au plus petit exposant de nombre normalisé soit $−2^{E−1}+1+1$. Ce qui change c’est le bit implicite 0 (dénormalisé) ou 1 (normalisé)

### Récap

|Type|Exposant décalé|Mantisse|
| :-: | :-: | :-: |
|Zéros : +-0|0|0|
|Nombres dénormalisés|0|Différente de 0, 0. implicite|
|Nombres normalisés|1 à $2^E–2$|Quelconque, 1. implicite|
|Infinis +- infinity|$2^E–1$|0|
|NaN|$2^E–1$|Différente de 0|

L’exposant e d’un nombre normalisé : e∈[−126,127]. L’exposant -127 (qui est décalé vers la valeur 0) est réservé pour zéro et les nombres dénormalisés, tandis que l’exposant 128 (décalé vers 255) est réservé pour coder *les infinis et les NaN*

## Des réels aux flottants

### Cas des nombres normalisés

On calcule (s, e, m) en base 10

- Ecriture de 0 : 32 bits à 0 ou un bit à 1 suivi de 31 bits nuls
- Ecriture sous forme scientifique au standard décimal : Si $X≠0$, $X=(−1)^s×2^e×(1+m)$ avec s=0 ou s=1, $e∈ℤ$ et $m∈[0, 1[$

#### Trouver s

0 si X est positif ou nul, 1 sinon

#### Trouver e

- Si |X| ≥ 2, diviser par 2 la valeur absolue de X autant de fois que nécessaire jusqu’à obtenir un entier de l’intervalle [1, 2[.
  *e est donc le nombre de divisions faites*
- Si |X| < 1, multiplier par 2 la valeur absolue de X autant de fois que nécessaire jusqu’à obtenir un entier de l’intervalle [1, 2[.
  *e est donc l’opposé du nombre de multiplicatio*n (on met un – devant le nombre de multiplication)
- Si 2>|X|>=1 alors e = 0

#### Trouver m

Si on connaît X, s et e alors il est facile de trouver m :

$m=\frac{(−1)^s}{2^e}X−1$

!!!example "Exemple"
    <p align="center"><img src="/images/Nbre_flottants/nbr3.png"></p>

    Si c’est infini, on calcule encore 3 fois la multiplication, on regarde si on est plus proche de 0,2 (dans notre cas), donc on ajoute 1 au bit de poids faible (le 23e) et on tient compte des retenues

    <p align="center"><img src="/images/Nbre_flottants/nbr4.png"></p>

    Dans cet exemple, on pousse le calcul des décimales jusqu’au 27eme bit et on peut déterminer sans erreur quel est l’arrondi au plus proche de la matrice. Le cas ou les 3 bits supplémentaires s’écrivent 100 alors on utilise la règle dite de l’arrondi au plus proche pair.

## Des flottants aux réels

On prend le flottant *0100 0000 1011 1000 0000 0000 0000₂*, à quel réel correspond-t-il ?

<p align="center"><img src="/images/Nbre_flottants/nbr5.png"></p>

Le signe est 0, le nombre est donc positif, le champ d’exposant décalé est $10000001_2$ soit $129_{10}$ donc la valeur de l’exposant est 2(=129−127). Le significande (avec le bit implicite) est donc $1.0111000000000000000000_2$. En convertissant : =$5,75_{10}$

**En général,** partant d’un flottant simple précision normalisé :

- L’écrire en binaire et retrouver chaque champs
- Signe de s : bit de poids fort
- Convertir le binaire du champs exposant en un entier e, lui retrancher le décalage $d=127(2^{8−1}−1)$
- Partie décimale m (indiqué par la mantisse $b_1b_2...b_{22}b_{23}$)

$$m=\sum_{i=0}^{22}b_i2^{-i} = b_1×2^{-1}+b_2×2^{-2}+...+b_{22}×2^{-22}+b_{23}×2^{-23}$$

- Le nombre réel correspondant est $(−1)^s(1+m)2^{e−127}$ avec 1 le bit implicite