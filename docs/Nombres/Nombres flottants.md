# Nombres flottants

!!!warning ""
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    - Informatique pour tous en classes préparatoires aux grandes écoles (Eyrolles)
    - Nombres flottants : [Wikipedia](https://fr.wikipedia.org/wiki/IEEE_754)
    - [cpprefrence](https://en.cppreference.com/w/c/numeric/math/nextafter) (en **C**)

## Présentation

### Introduction

**Objectif :** représenter une partie des nombres rationnels : des nombres avec une quantité bornée de chiffres après la virgule

**Idée :** On se donne tous les chiffres composants le nombre de la position de la virgule : $(1234,2)$ représente $12,34$ c'est à dire $1,234 \times 10^1$

**Comment :** On peut écrire tous les nombres réels non nuls sous la forme $(−1)^s(1+m)2^e$ avec

- $s∈{0,1}$ est le _signe_ du nombre
- $m∈[0, 1[$ est la _mantisse_
- $e∈ℤ$ est son _exposant_

L'exposant du nombre représente la position de la virgule dans son expression en base $2$

$1+m$ est un nombre entre $1$ et $2$ (exclu). Il s'écrit sous la forme $1.c_1...c_i...$ avec les $c_i∈{0,1}$. Comme les ordinateurs ne gère que des quantités finies, le nombre de chiffres binaires $c_i$ est borné (il est souvent égal à $23$ ou $52$)

### Pourquoi les nombres à virgule flottante ?

L'avantage de la représentation en virgule flottante par rapport à la virgule fixe est que la virgule flottabte est capable, à nombre de chiffres égal, de gérer un intervalle de nombres réels plus importants.

!!!example ""

    Considérons une représentation en virgule fiwe qui a $5$ chiffres dont un après la virgule. Elle peut exprimer $10^5$ nombres décimaux dans $[0,9999.9]$. Avec $5$ fois les chiffre 1, on ne représente que $1111.1$

    Avec une représentation en virgule flottante et $5$ chiffres $1$ : on peut représenter $11111$, $1111.1$, $111.11$, $11.111$, $1.1111$, $.11111$, donc 6 fois plus d'expressions en virgule flottante qu'en virgule fixe. De plus l'intervalle de représentation est plus grand : [0, 99999].

    Cependant, il faut alors oder la position de la virgule. Cela demande donc plus de place.

## La norme $\text{IEEE 754}$

### $\text{IEEE 754}$, présentation

$\text{IEEE}$ Standard for Binary Floating-Point Arithmetic ($\text{ANSI/IEEE}$ Std $\text{754-1985}$) (standard $\text{IEEE}$ pour l'arithmétique binaire en virgule flottante) : $\text{IEEE 754}$.

Standard le plus employé actuellement pour le calcul des nombres à virgule flottante dans le domaine informatique, avec les CPU et les FPU.

Le standard définit les formats de représentation des nombres à virgule flottante (signe mantisse, exposant, nombres dé-normalisés) et valeurs spéciales ($\text{infinis}$ et $\text{NaN}$) en même temps qu'un ensemble d'opérations sur les nombres flottants.

Il décrit aussi quatre modes d'arrondi et cinq exceptions (comprenant les conditions dans lesquelles une exception se produit, et ce qui se passe dans ce cas)

- Les quatre modes d'arrondi :
    - Vers moins l'infini : $⌊−3.−4⌋=−4$
    - Vers plus l'infini : $⌈−3.−4⌉=−3$
    - Vers zéro : $⌊−3.−4⌋=−3$ et $⌊3.4⌋=3$
    - Au plus proche (avec le cas particulier de l'équidistance : le nombre $1.5$ doit-il être arrondi à l'unité vers $1$ ou $2$?)

- La version $1985$ de la nomre $\text{IEEE 754}$ définit 4 formats pour représenter les nombres à virgule flottante :
    - Simple précision ($32$ bits : $1$ bit signe, $8$ bits d'exposant, $23$ bits de mantisse avec $1$ bit implicite)
    - Simple précision étendue (≥ $43$ bits, obsolète)
    - Double précision ($64$ bits : $1$ bit de signe, $11$ bits d'expo, $52$ bits de mantisse avec $1$ implicite)
    - Double précision étendue (>$79$ bits)

### Bit $1$, bit _implicite_

!!!quote "Bit implicite"
    La mantisse représente un nombre décimal entre $1$ et $2$ (exclu), par exemple $1.$ $1101100011$. Rendre le bit $1$ implicite consiste à écrire $1101100011$, la partie **$1.$** étant sous entendue puisque toujours la même.

### Représentation binaire des nombres flottants à précision simple

<p align="center"><img src="/images/Nbre_flottants/nbr2.png"></p>

_Figure - $\text{IEEE-754} siple précision$_

PLus généralement, les nombres sont écrits au format $(1, E, M)$ donc sur $1 + E + M$ bits :

<p align="center"><img src="/images/Nbre_flottants/nbr1.png"></p>

Ce format $(1, 8, 23)$ obsolète est privilégié ici parce qu'on peut faire tenir 32 bits sur un transparent.

- Bit de poids fort à $1$ : Négatif, à $0$ : Positif
- Exposant : Pas de représentation en complément à $2$ (car comparer des nombres serait difficile). L'exposant est _décalé_, afin de le stocker sous forme d'un nombre non signé. En notant $E$ le nombre de chiffres (toujours le même nombre) de l'exposant, on ajoute un décalage de $d=2^{E−1}−1$
- Avec $E = 8$, $d = 127$. L'exposant est dans l'intervalle $[-127 ; 128]$, donc l'exposant décalé est dans $[0, 255]$ ; $0$ et $255$ ayant une signification spéciale.

Ils sont longs de $4$ octets ($32$ bits) : $(1, 8, 23)$

La mantisse complète, le _significande_, doit être considérée comme une valeur sur $24$ bits. Si la mantisse avec bit $1$ implicite est $101000… $ alors le significande en base 2 est **$1.$** $101000…$

La quantité de nombres représentables au format $(1, E, M)$ est grande mais pas infinie (mince). L'ordinateur travaille donc avec des valeurs en général approchées :

Avec $\texttt{float x = 0.1}$, l'ordinateur travaille en interne avec $\text{0 01111011 10011001100110011001101}_2$ c'est à dire $0.100000001490116_{10}$ qui est égal à $\frac{13421773}{134217728}$

### Affichage

Quand on entre un nombre au clavier, l'orinateur en calcule une représentation en virgule flottante. Du fait des arrondis, une infinité de nombres peuvent avoir la même représentation comme flottant.

La relation "a la même représentation que" est une relation d'équivalence.

Pour le confort de lecture, l'ordinateur affiche l'unique représentant de cette classe d'équivalence qui nécessite le moins de chiffres décimaux.

C'est la raison pour laquelle l'ordinanteur affiche $\texttt{0.1}$ et nont le nombre rationnel avec lequel il travaille effectivenment.

Un nombre qui est égalt à sa représentation en flottant est dit _repésentable exactement_ en machine. $\texttt{0.1}$  n'est pas représentable exactement en machine ; $\texttt{1.0}$ et $\texttt{3.75}$ oui.  

### À propos de l'exposant

#### Format $(1, E, M)$

- En fonction de la valeur $e_d$ du champ exposant décalé ($0$, $255$, ou autre), certains nombres peuvent avoir une signification spéciale Ils peuvent être :
    - Des nombres dé-normalisés ($e_d=0$);
    - Zéro ($e_d=0$);
    - Infini ($e_d = 2^{N-1}-1$);  
    - NaN (_Not a Nunber_ ($e_d = 2^{N-1}-1$) "pas un nombre", comme $0/0$ ou $\sqrt{-1}$);
- L'exposant est décalé dans $⟦0,2^E−1⟧$ donc le décalage est de $2^{E−1}−1$.LEs nombres "normalisés" ont un exposant décalé dans $⟦1,2^E−2⟧$ .
- Le bit implicite de la mantisse est déterminé par la valeur de l'exposant décalé. Il vaut $0$ si l'exposant décalé est égal à $0$ et $1$ sinon.

#### Nombres normalisés et dé-normalisés

Soit le Format $(1, E, M)$

- Si l'exposant décalé est différent de $0$ et de $2^E−1$, le bit implicite est $1$, et le nombre est dit _normalisé_.
- Si l'exposant décalé est nul, par convention, le bit implicite vaut $0$. Le nombre est dit _dé-normalisé_. La représentation au format dé-normalisé est destinée aux très petites quantités en valeur absolue.
- La quantité de nombre à virgules flottante sur une machine et grande mais finie. Chaque nombre positif (sauf le plus grand et plus petit) à un _successeur_ et un _prédécesseur_ (s'il n'est pas nul) positif
- Le successeur du nombre dé-normalisé positif le plus grand est le plus petit nombre normalisé positif.
- Zéro n'est ni normalisé ni dé-normalisé. Il a deux écritures $+0$ et $-0$

### Nombres dé-normalisés

Pour un format $1, E, M$ :

- Mantisse non nulle, champ exposant décalé : $E$ bits à $0$.
- Tous les nombres dé-normalisés ont le même exposant.
- Si la règle était la même que pour les nombres normalisés, l'exposant serait donc de $0-\text{decalage}$ soit $-2^{E-1}+1$, donc $-1023$ pour les flottants double précision.
- Mais par convention, l'exposant pour les nombres dé-normalisés est en fait égal au plus petit exposant de nombre normalisé soit $−2^{E−1}+1+1$. Ce qui change c'est le bit implicite $0$ (dé-normalisé) ou $1$ (normalisé)
    - Plus petit normalisé positif $(1 + m)2^{-2^{E-1}+1+1}$ avec $m=0$
    - PLus petit dé-normalisé positif $(0 + m)2^{-2^{E-1}+1+1}$ avec $m$ non nul minimum. Ainsi, $m$ s'écrit avec $M-1$ bits à $0$ suivis de 1 donc $m = 2^{-M}$. Conclusion $2^{-M}2^{-2^{E-1}+1+1}$

### Exposant pour un format $(1, E, M)$

Tableau récapitulatif :

|Type|Exposant décalé|Mantisse|
| :-: | :-: | :-: |
|Zéros : $\pm 0$|$0$|$0$|
|Nombres dé-normalisés|$0$| $\neq0$, `0.` implicite|
|Nombres normalisés|1 à $2^E–2$|quelconque, `1.` implicite|
|Infinis $\pm\infty$|$2^E–1$|$0$|
|NaN (_Not a Number_)|$2^E–1$|différente de $0$|

!!!example "Exposant $e$ d'un nombre normalisé"

    Si $E=128$, alors $e∈[−126,127]$. L'exposant $-127$ (qui est décalé vers la valeur $0$) est réservé pour zéro et les nombres dé-normalisés, tandis que l'exposant $128$ (décalé vers$ 255$) est réservé pour coder les infinis et les NaN.

### Real 2 float

#### Calcul du triple $(s,e,m)$

Cas des nombres normalisés. On calcule $(s, e, m)$ en base $10$

- **Écriture de $0$** : $32$ bits à $0$ ou $1$ suivi de $31$ bits nuls
- Écriture sous forme scientifique au standard décimal : Si $X≠0$, $X=(−1)^s×2^e×(1+m)$ avec
    - $s\in\{0,1\}$
    - $e∈ℤ$
    - $m∈[0, 1[$

<p style="color:red">Il faudra exprimer ces nombres au format binaire au moyen d'un triple (s₂, e₂, m₂)</p>

- **Recherche de $s$** : $0$ si $X$ est positif ou nul, $1$ sinon.
- **Recherche de $e$** :
    - si $|X|\geq 2$, diviser par $2$ la valeur absolue de $X$ autant de fois que nécessaire jusqu'à obtenir un eniter de l'intervalle $[1;2[$. $e$ est le nombre de divisions effectuées.
    - Si $|X| < 1$, multiplier par $2$ la valeur absolue de $X$ autant de fois que nécessaire jusqu'à obtenir un entier de l'intervalle $[1, 2[$. $e$ est donc l'opposé du nombre de multiplication.
    - Si $2>|X|\geq1$ alors $e = 0$
- **Recherche de $m$**: Si on connaît $X$, $s$ et $e$ alors il est facile de trouver $m$ :

$m=\frac{(−1)^s}{2^e}X−1$

#### Triplet $(s_2, e_2, m_2)$ en base 2

!!!example "Exemple"

    $X=-9.6$ ; $s_{10} = 1$ ; $9.6\times 2^{-3} = 1.2\in[1;2[$ donc $e_{10} = 3$ ; $m_{10} =1.2 -1$

    Connaissant $s,e,m$ de la notation scientifique décimale, on veut $s_2, e_2, m_2$ tels que 
    - bits de signe $s_2 = s_{10}$
    - bits d'exposant $e_2 = bin (e+127)$ (conversion de l'exposant _décalé_ en base $2$),
    - Pour la mantisse sur $23$ bits
        - $\texttt{I}$. Multiplier $m_{10}$ par $2$,
        - $\texttt{II}$. Calculer partie enitère ($0$ ou $1$); partie décimale.
        - $\texttt{III}$. partie entière : un nouveau bit de la représentation.

    Recommencer à $\texttt{I}$ avec la partie décimale du résultat jusqu'à avoir $23$ bits de mantisse (et même un peu plus pour arrondir)

!!!tip "Exemple"

      - $X=-9.6$ ; $s_{10} = 1$ ; $e_{10} = 3$ ; $m_{10} =0.2$
      - $e_{10} + 127_{10} = 130_{10}$, en base $2$ : $e_2 = 10000010_2$
      - Pour la mantisse :
          - $0.2 \times 2 = 0.4 = 0 + 0.4$
          - $0.4 \times 2 = 0.8 = 0 + 0.8$
          - $0.8 \times 2 = 1.6 = 1 + 0.6$
          - $0.6 \times 2 = 1.2 = 1 + 0.2$ On est revenu à $0.2$ : séquence infinie. Mantisse $\texttt{0011 0011 0011 0011 0011 0011 0011...}$

    |$2^{-1}$|$2^{-2}$|$2^{-3}$|$2^{-4}$|$2^{-5}$|$2^{-6}$|$2^{-7}$|$2^{-8}$|$2^{-9}$|$2^{-10}$|
    |:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
    |$0$|$0$|$1$|$1$|$0$|$0$|$1$|$1$|$0$|$0$|
    |$2^{-11}$|$2^{-12}$|$2^{-13}$|$2^{-14}$|$2^{-15}$|$2^{-16}$|$2^{-17}$|$2^{-18}$|$2^{-19}$|$2^{-20}$|
    |$1$|$1$|$0$|$0$|$1$|$1$|$0$|$0$|$0$|$0$|
    |$2^{-21}$|$2^{-22}$|$2^{-23}$|$2^{-24}$|$2^{-25}$|$2^{-26}$|$2^{-27}$|$...$|||
    |$0$|$0$|$1$|$1$|$0$|$0$|$1$|$...$|||

    On a donc la mantisse comme une somme infinie de coefficients $\sum^{+\infty}_{i=1}{\frac{a_i}{2^i}}$ mais on veut une somme finie.

    **Question** : $\frac{1}{2^{24}} + 0\times\frac{1}{2^{25}} 0\times\frac{1}{2^{26}} 1\times\frac{1}{2^{27}}$ est il plus proche de $\frac{1}{2^{23}}$ ou de $0$ ? Donc $m$ est-il plus proche de $\sum^{23}_{i=1}{\frac{a_i}{2^i}}$ ou de $\left(\sum^{23}_{i=1}{\frac{a_i}{2^i}}+\frac{1}{2^{24}}+\frac{1}{2^{27}}\right)$?

     $\frac{1}{2^{24}} + 0\times\frac{1}{2^{25}} 0\times\frac{1}{2^{26}} 1\times\frac{1}{2^{27}}$ est plus proche de $\frac{1}{2^{23}}$ : on ajoute donc $1$ au bit de poids faible (le $23$ème) et on tient compte des retenues.

     Attention : Dans le pire cas (mais pas ici), $m = \sum_{i=1}^{23}{\frac{1}{2^i}}$ et on ajoute $\frac{1}{2^{23}}$. Alors $m + \frac{1}{2^{23}} = \frac{\frac{1}{2} - \frac{1}{2^{24}}}{1-1^{\frac{1}{2}}} +\frac{1}{2^{23}} = 1$ donc $1 + m = 2$ et il faut changer l'exposant !!

     Arrondi au plus proche sur $23$ bits :

     $$M = \text{0011 0011 0011 0011 0011 010}$$

     Finalemnt : $-9.6_{10}$ est représenté par

     $$1_2 \text{ } 10000010_2 \text{ 0011 0011 0011 0011 0011 010}_2$$

#### Arrondi si $M = 23$

Dans l'exemple étudié $X=-9.6$, on pousse le calcul des décimales jusqu'au $27$ème bit et on peut  déterminer sans erreur quel est l'arrondi au plus proche de la matrice.

Les bits $24$, $25, $26$ s'écrivent en effet $100$ et leur connaissance seule ne permet pas de trancher la question : faut-il arrondir par défaut ou par excès (exactement comme arrondir en base $10$ le nombre $9.5$ à l'unité au plus proche a $2$ réponses).

Dans nos exemples, on ne pousse pas les calculs trop loin après le dernier bit maintenu (le $23$ème ici) et on préfère calculer seulement $3$ bits supplémentaires. Le cas où ces $3$ bits s'écrivent $100$ est géré par la règle dite de l'_arrondi au plus proche pair_ (cf. plus loin).

Cas dégénéré : On peut aussi arrondir sans utiliser les bits au delà du $23$ème : (cf. arrondi au plus proche pair)

### Float 2 real

Considérons le flottant $\text{0100 0000 1011 1000 0000 0000 0000 0000}$ A quel réel correspond ce flottant ?

- |Signe|Exposant décalé|Bit caché + mantisse|
  |:-:|:-:|:-:|
  |$0$|$1000$ $0001$|$\text{(1) 011 1000 0000 0000 0000 0000}$|
- Le signe est $0$, le nombre est donc positif. Le champ exposant décalé est $e_2=10000001_2$, autrement dit $e_{10} = 129$. La valeur réelle de l'exposant est donc $e_{10} -d = 129 -127 = 2$. La significande (donc avec le bit implicite) est $1.0111000000000000000000_2$.
- Conversion : $(-1)^0\times2^2\times\left(\underset{\text{implicite}}{1}\times 2^0+0\times2^{-1}+1\times 2^{-2}+1 \times 2^{-3} + 1\times 2^{-4}\right)= 5.75$

Plus généralement, partant d'un flottant simple précision normalisé :

- L'écrire en binaire et retrouver chaque champs.
- Signe $s$ : but de poids fort
- Convertir le binaire du champs exposant en un entier $e$, lui retrancher le décalage $d=127\times(2^{8-1}-1)$.
- Parite décimale $m$ (indiquée par la mantisse $b_1b_2...b_{22}b_{23}$).

$m = \sum^{22}_{i=0}{b_i2^{-i}} = b_1 2^{-1} + b_2 2^{-2}+ ... + b_{22}2^{-22}+b_{23}2^{-23}$.

Le nombre réel correspondant est $(-1)^s(\underset{\text{implicite}}{1}+m)2^{e-127}$.

### Portée

<p align="center"><img src="/images/Nbre_flottants/nbr3.png"></p>

_Figure - Quelques nombres positifs (Wikipedia)_