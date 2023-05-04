
tests
=====
!!! danger
    Ce cours n'a pas été entièrement reverifié après le passage du programme. Pensez à supprimer ce message si vous avez reverifié ce cours

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par
    Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce
    site à pour seul but d’être plus compréhensible pendant les périodes de
    révision que des diaporamas.

#### Crédits


Sur la nuance entre erreur et défaut, cet article de Cynthia Lefevre.  
Ce cours du Labri  
Cet article de Wikipedia.  
Ce cours du LRI de Paris-Saclay  
Un cours du CNAM avec des exemples de normes réclamant les  critères étudiés.  


 Présentation  
 Partitionnement, limite  
 Graphe de ﬂots de contrôle  

#### Terminologie


!!! quote "Définition"
    Erreur Action humaine produisant un résultat incorrect. "l’erreur


!!! quote "Définition"
    Défaut Une imperfection dans un composant (ou un système) qui


!!! quote "Définition"
    Défaillance ou Panne IEEE 729 : La fin de la capacité d’un système ou


!!! quote "Définition"
    d’un de ses composants d’eﬀectuer la fonction requise, ou de
l’eﬀectuer à l’intérieur de limites spécifiées. On cherche une
défaillance en manipulant le système.
Anomalie Un écart entre le résultat attendu et celui obtenu.


!!! quote "Définition"
    Remarque
Ce n’est pas parce qu’il y a anomalie qu’il y a défaut. Peut-être le testeur
a-t-il compris la spécification d’une façon et le développeur d’une autre.


est humaine".  
peut conduire à ce que ce composant (ou système) n’exécute  pas les fonctions requises. On cherche les défauts sans  manipuler le système.  

#### Défaut VS Défaillance


Il y a un rapport de causalité.  
A retenir : Un défaut (dans le code) ou une erreur (de l’utilisateur du  programme) peut causer (un jour) une défaillance.  
On peut trouver un défaut sans manipuler le système (analyse du  code).  
Mais on trouve une défaillance en manipulant le système.  

#### Test


Définition (norme IEEE 729) : Le test est un processus manuel ou  automatique, qui vise à établir qu’un système vérifie les propriétés  exigées par sa spécification, ou à détecter des diﬀérences entre les  résultats engendrés par le système et ceux qui sont attendus par la  spécification.  
Le test ne se préoccupe pas des erreurs. Ce n’est pas au testeur de  pointer les imperfections humaines.  
Le test met en évidence les défauts afin de prévenir les défaillances.  

#### Tests en général


Toute fabrication de produit suit les étapes suivantes  
 Conception   Réalisation   Tests  
Test : On s’assure que le produit final correspond à ce qui a été  demandé selon divers critères :  
esthétique,  performance,  ergonomie,  fonctionnalité,  robustesse etc.  

#### VVT


Validation, Vérification et Test logiciel.  
Démonstration automatique (Hum !) : exhaustive mais considérée  comme trop coûteuse.  
Model Checking : vérifier si le modèle d’un système satisfait une  propriété. Par exemple, on souhaite vérifier qu’un programme ne se  bloque pas, qu’une variable n’est jamais nulle, etc. Généralement, la  propriété est écrite dans un langage, souvent en logique temporelle.  La vérification est généralement faite de manière automatique.  
Test : non exhaustif mais facile à mettre en œuvre (bon rapport  qualité/temps).  

#### Cycle en V
<p align='center'><img src='/images/a1696852da3de0356121d3245c0da149.bmp'/></p>

_Figure – Le cycle en V du Génie Logiciel (d’après Irif)_

Le test commence dès le début !  


```linenums="1"
selon le niveau au cycle de développement
```

Classification  
Tests de recette : test de réception du logiciel chez le client final  
Tests intégration système : test de l’intégration du logiciel avec  d’autres logiciels  
Tests systeme : test d’acception du logiciel avant livraison (nouvelle  version par exemple). Généralement eﬀectué par le client dans ses  locaux après installation du système ou d’une unité fonctionnelle, avec  la participation du fournisseur.  
Tests Integration : test de l’intégration des diﬀérents composants  (avec ou sans hardware)  
Tests Unitaires : tests élémentaires des composants logiciels (une  fonction, un module, ...)  

#### Classification selon la caractéristique


tests fonctionnels : destinés à s’assurer que, dans le contexte  d’utilisation réelle, le comportement fonctionnel obtenu est bien  conforme avec celui attendu.  
Tests de performance (rapidité, consommation mémoire etc.)  
Tests de robustesse (mon serveur va-t-il bien réagir à un aﬄux de  connexion ?)  
Tests de vulnérabilité : suis-je bien protégé face aux risques  d’intrusions ?  

#### Classification selon le niveau d’accessibilité


tests de type boîte noire : technique de conception de test,  fonctionnel ou non, qui n’est pas fondée sur l’analyse de la structure  interne du composant ou du système mais sur la définition du  composant ou du système.  
tests de type boîte blanche : technique de conception de test, en  général fonctionnel, fondée sur l’analyse de la structure interne du  composant ou du système.  
Pour comparer les deux.  

#### Divers


Tests de non-régression : à la suite de la modification d’un logiciel (ou  d’un de ses constituants), un test de régression a pour but de montrer que  les autres parties du logiciel n’ont pas été aﬀectées par cette modification.  

#### En CPGE


Tests unitaires en général boîte blanche et fonctionnels.  
Moyen : pour tout exercice, créer un fichier ou une fonction de tests.  
Rendu de projets : les tests sont impératifs.  

##  

#### Partitionnement du domaine d’entrée


Il s’agit de répartir les données en un nombre fini de classes  d’équivalence, sans restreindre le degré d’exigence. Cette méthode est  généralement utilisée pour réduire le nombre total de cas de test à un  ensemble fini.  Exemple : test d’une zone de saisie attendant des nombres de 1 à  1000.  
2, 999  
Classe des données valides d’entrée : sélectionner un (ou quelques)  nombre.s dans  (exemple : 50 et 800).  Classe des données inférieure à 0 : sélectionner un ou plusieurs nombres  négatifs (-1 et -100 par exemple).  Classe des données supérieures à 1001 : choisir 1050 et 2300 par  exemple.  
(cid:74)  
(cid:75)  


```linenums="1"
Test d’une zone de saisie attendant des nombres de 1 à 1000
```

Tests aux limites  
En complément du partitionnement du domaine d’entrée, on ajoute les  tests aux limites  
Ce sont les données encore valides mais au delà desquelles les entrées  ne le sont plus.  
Il faut impérativement tester 1 et 1000.  On peut y ajouter les valeurs  
juste en dessous des bornes : 0 et 999,  juste au-dessus des bornes : 2 et 1001  
Si un des tests ne donne pas le résutat attendu, le programme n’est  pas correct. En revanche, le programme peut être incorrect même si  tous les tests du scénario donnent satisfaction.  

##     


```linenums="1"
dits aussi tests boîte blanche
```

Tests structurels  
Sélection de tests à partir de l’analyse du code source du système  
Constructions des tests uniquement pour du code déjà écrit.  
Spécification  
(cid:47) Résultats  attendus  
Tests  
Programme  
(cid:47) Résultats  des tests  
Comparaison  
(cid:47) Verdict  

#### Test structurel


Utiliser la structure du code pour dériver des cas de tests.  
Complémentaire des tests fonctionnels car on étudie la réalisation et  pas seulement la spécification.  Il y a deux méthodes :  
`A partir du graphe de ﬂot de contrôle : couverture de toutes les  instructions, tous les branchements... (CPGE)  `A partir du graphe de ﬂot de données : couverture de toutes les  utilisations d’une variable, de tous les couples définition-utilisation...  

#### Graphe de ﬂot de contrôle


Graphe simple orienté connexe avec un sommet initial (ou d’entrée)  E et un sommet de sortie S accessible depuis E .  Un sommet interne est  
Soit un bloc d’instructions élémentaires (on regroupe les séquences).  Soit un sommet de décision étiqueté par un test.  
Il y a deux sortes d’arcs :  
Les arcs de sortie de blocs d’instructions : ils représentent le passage  d’un bloc d’instructions à une instruction conditionnelle ou à un autre  bloc d’instruction. Il ne sont pas étiquetés.  les arcs de décision : ce sont les arcs de sortie des sommets de décision.  Ils sont étiquetés par les réponses aux tests des expressions  conditionnelles.  
Il y a un seul arc de sortie d’un sommet bloc d’instructions. Et, le plus  souvent, deux arcs de sortie (dits arcs de décision) des sommets de  décision correspondant aux valeurs positives ou négatives des  conditions.  

#### Exemple : calcul de X n


```linenums="1"
f l o a t power ( f l o a t x ,
i n t n )
i n t p = n ;
w h i l e ( p=1)f l o a t
s = 1 .
;
s=s x ;
p=p 1;
r e t u r n s ; ```



{  
>{  



* −
}  }  
  
  
  
  
  
  
  
  
E  
int p =n ;  ﬂoat s = 1. ;  
p>=1  
F  
S  
V(cid:15)  s=s*x ;  p=p-1 ;  

#### Exemple des triangles


```linenums="1"
v o i d t r i a n g l e ( i n t
j , i n t k , i n t
l ) l e s 3 c ô t é s d ’ un t r i a n g l e
l
:
// j , k ,
// i n d i q u e
t r i a n g l e
s i
// i s o c è l e ou é q u i l a t é r a l
i n t n = 0 ; // n p o u r
i f
l e
k+l =j
( j+k= l
p r i n t f ( ” p a s un t r i a n g l e n” ) ;
’ n a t u r e du t r i a n g l e ’
l+j = k )
e s t q u e l c o n q u e ,
e l s e i f
i f
i f
i f
( j==k ) n += 1 ;
( j==l ) n += 1 ;
( l==k ) n += 1 ;
( n == 0 )
p r i n t f ( ” q u e l c o n q u e n” ) ;
e l s e
i f
( n == 1 )
p r i n t f ( ” i s o c è l e n” ) ;
e l s e
p r i n t f ( ” é q u i l a t é r a l n” ) ;
```



{  





| | <<\
| |  
<

{  


\


\

\
}  
}  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  


```linenums="1"
True False```

E (cid:47)  
(cid:47) int n = 0 ;  
j+k≤l ou  k+l≤j ou  l+j≤k  
  
 (cid:41)  
 (cid:41)  
 (cid:40)  
 (cid:40)  
j==k  
j==l  
l==k  
n==0  
n==1  
  
printf(”pas un triangle”)  
  
  
  
n+=1  
n+=1  
n+=1  
(cid:47) S  
printf(”quelconque”)    
printf(”isocèle”)  
  printf(”équilatéral”)  
Avec T pour et F pour .  
  

#### Chemin faisable


Un chemin de contrôle est une suite x, x, . . . , xn de sommets tels  que x = E , xn = S et (xi , xi) est un arc pour tout i < n.  Un chemin est dit faisable si il existe un ensemble de conditions sur  les variables d’entrées permettant de passer par tous les sommets de  ce chemin.  
Si un chemin n’est pas faisable, il est dit infaisable.  


```linenums="1"
Chemin faisable
```

Exemple des triangles  
E (cid:47)  
int n = 0 ;  
j+k≤l ou  k+l≤j ou  l+j≤k  
F  
F (cid:41)  
 (cid:41)  
F (cid:40)  
F (cid:40)  
j==k  
j==l  
l==k  
n==0  
n==1  
T  
printf(”pas un triangle”)  
T  
T  
T  
n+=1  
n+=1  
n+=1  
(cid:47)S  
printf(”quelconque”)  T  
printf(”isocèle”)  
T  
printf(”équilatéral”)  
Pour j = 5; k = 3; l = 5.  
  


```linenums="1"
Chemin infaisable
```

Exemple des triangles  
E (cid:47)  
int n = 0 ;  
j+k≤l ou  k+l≤j ou  l+j≤k  
F  
F (cid:41)  
F (cid:41)  
F (cid:40)  
F ~=  
j==k  
j==l  
l==k  
n==0  
n==1  
T  
printf(”pas un triangle”)  
T  
T  
T  
n+=1  
n+=1  
n+=1  
(cid:47)S  
printf(”quelconque”)  T  
printf(”isocèle”)  
T  
printf(”équilatéral”)  
Aucune entrée pour ce chemin.  
F  

#### Condition d’un chemin


La condition d’un chemin est la conjonction de tous les sommets de  décision portant sur les entrées que traverse ce chemin.  Exemple : avec j = 3, k = 5, l = 3, la condition du chemin vert  parcouru est  
¬(j +k ≤ l ∨k +l ≤ j ∨j +l ≤ k)∧j (cid:54)= k ∧j = l ∧l (cid:54)= k ∧n (cid:54)= 0∧n = 1.  
La condition du chemin rouge est  
¬(j +k ≤ l ∨k +l ≤ j ∨j +l ≤ k)∧j (cid:54)= k ∧j (cid:54)= l ∧l (cid:54)= k ∧n (cid:54)= 0∧n (cid:54)= 1.  
Or  ¬(j + k ≤ l ∨ k + l ≤ j ∨ j + l ≤ k) ∧ j (cid:54)= k ∧ j (cid:54)= l ∧ l (cid:54)= k =⇒ n = 0.  Et donc la condition du chemin rouge est  
¬(j +k ≤ l ∨k +l ≤ j ∨j +l ≤ k)∧j (cid:54)= k ∧j (cid:54)= l ∧l (cid:54)= k ∧0 (cid:54)= 0∧0 (cid:54)= 1.  
Cette formule est une antilogie : le chemin rouge est infaisable.  

#### Indécidabilité de la recherche de chemins infaisables


```linenums="1"
v o i d s y r a c u s e ( i n t x ) // on ne c o n s i d è r e
que d e s e n t i e r s 0
w h i l e ( x !=1) ( x%2==0) x=x / 2 ;
i f
e l s e x = 3x +1;
p r i n t f ( ” f i n i n” ) ;
```

  
  
  
  
  
  
  
  
{  
>
{  

* 
}  \
}  
La conjecture veut que au  bout d’un moment x = 1.  
E  
x (cid:54)= 1  
  
S  
(cid:15)  x mod 2 = 0    
  
x=x/2  On ne sait pas s’il existe des  chemins infaisables de E à S.  
x=3*x+1  


Syracuse : La discipline de test (DT) x=2 sensibilise le  chemin C  
Etant donné un chemin faisable, on sait trouver les conditions pour le  parcourir.  
soit le chemin C : E,  x!=1,  x%2==0,x=x/2,x!=1,S.  Quelle condition le  vérifie ?  
x (cid:54)= 1  x mod 2 = 0 : x est pair  
x  2  
= 1  
S  
E  
x (cid:54)= 1  
(cid:15)  x %2 ==0  
  
  
x=3*x+1  
  
x=x/2  
On trouve x = 2.  

#### Critère de couverture


On appelle critère de couverture une condition définissant un  ensemble de chemins du graphe de ﬂot de contrôle.  Génération de tests grâce à un critère de couverture :  
Sélectionner un ensemble minimal de chemins satisfaisant le critère.  Eliminer les chemins infaisables  Pour chaque chemin faisable, on définit un ensemble de conditions  associées sur les entrées (c’est à dire un objectif de test).  Pour chaque objectif de test (donc pour chaque chemin faisable)  choisir un cas de test (donc des valeurs d’entrées satisfaisant la  condition associée au chemin).  

#### Critère "tous les sommets"


Critère atteint lorsque tous les sommets du graphe de contrôle sont  parcourus.  
On définit le taux de couverture des sommets par  
τs =  
nombre de sommets parcourus  nombre de sommets  
Exigence minimale pour la certification en aéronautique (norme  EUROCAE ED-12B) : τs = 1.  Qualification niveau C : Un défaut peut provoquer un problème  majeur entraînant un dysfonctionnement des équipements vitaux de  l’appareil.  


```linenums="1"
i n t y
i n t
somme ( i n t x ,
) i n t
i f
s = 0 ;
( x==0)
s=x ;
e l s e
s= x+y ;
r e t u r n s ;
```

  
  
  
  
  
  
  
  


{  




}  
Il y a deux chemins. L’un  d’eux permet de détecter  le défaut.  
E  
int s=0 ;  
x ==0  
s=x  
  
return s  
 (cid:40)  
s=x+y  
S  Le chemin E;int s=0;x  ==0;s=x;return s; S est  associée à la condition x = 0  satisfaite par les entrées  x = 0, y = 6. Cela permet de  détecter le défaut.  

#### Limite du critère "tous les sommets"


```linenums="1"
f l o a t d i v ( f l o a t x ) r ;
( x ! = 0 . )
f l o a t
i f
r = 1 . ;
r = 1/ x ;
r e t u r n r ;
```

{  


}  
  
  
  
  
  
  
  
  
E  
ﬂoat r ;  
x != 0  
r=1.  
  
  
r=1/x  
return s  
S  
Le chemin E;int  r;x!=0;r=1;return r; S  (associé à x = 2) couvre bien  tous les sommets mais la  division par zéro n’est pas  détectée.  

#### Critère "Tous les arcs"


Dit aussi critère "toutes les décisions".  
Pour chaque décision, un test rend la décision vraie, un autre la rend  fausse.  
Taux de courverture des arcs :  
τa =  
nombre d’arcs parcourus  nombre d’arcs  
Norme DO 178B, qualification des systèmes embarqués au niveau B :  un défaut peut provoquer un problème dangereux  
Critère "Tous-les-arcs" entraîne critère "Tous-les-sommets".  

#### Limite du critère tous les arcs


```linenums="1"
f l o a t F ( i n t a ,
r ;
i n t
i f
( a != 0 a==b )
r =1/a ;
r =0;
e l s e
r e t u r n r ;
i n t b ) ```



| | 
  
  
  
  
  
  
}  
E  
int r ;  
a != 0 ou a==b      
{  
r=1/a  
r=0  
return r  
S  Critère Toutes-les-Décisions  satisfait avec  {a = 0,b = 1} et  {a = 2,b = 1}.  Pourtant, division par zéro  non détectée avec  {a = 0,b = 0}.  


```linenums="1"
Toutes les conditions multiples
```

Décomposer les conditionnelles  
On a vu que le critère "tous les arcs" pouvait passer à côté  d’erreurs.  
C’est parce que ce critère est satisfait pour un sommet de décision  avec seulement deux jeux d’entrées : un jeu d’entrées rendant la  décision vraie et un autre la rendant fausse.  
Plutôt que la considérer la décision comme un seul sommet du graphe  de contrôle, on introduit les sous-decisions comme nouveaux sommets.  
Ce critère est appelé "toutes les conditions multiples"  

#### Critère toutes les conditions multiples : exemple


Le critère  Toutes-les-Décisions  n’est plus satisfait  avec seulement  {a = 0,b = 1} et  {a = 2,b = 1}.  Le critère  Toutes-les-décisions  est satisfait avec  {a = 0,b = 1} et  {a = 2,b = 2} et  enfin  {a = 0,b = 0}.  Division par zéro  détectée.  
E  
int r ;  
a != 0  
(cid:120)  
 (cid:38)  
a==b  
 ~=  
(cid:119)  
r=1/a  
  
return r  
a==b  (cid:15)  r=0  
S  
Le graphe simple devient un  multigraphe (deux arcs sortant  du a==b de gauche vers 1/a ).  


```linenums="1"
Explosion combinatoire
```

Critère "Toutes les décisions multiples"  
Exemple if (A && (B || C))  
Toutes-les-conditions-multiples  
Toutes-les-conditions  
A B C décision  1  0  1  1  
0  1  
1  1  
1  1  1  0  0  0  0  0  
A B C décision  1  1  0  1  1  1  0  1  1  0  0  0  1  0  0  0  
1  1  0  0  1  1  0  0  2  .  Comment limiter la  combinatoire ?  


```linenums="1"
MC/DC
```

Critère Toutes-les-Conditions/Décisions-modifié  
Objectif : améliorer les critères de couverture basés sur les décisions  tout en contrôlant la combinatoire  
Critère MC/DC : Modified Condition/Decision Coverage  
On ne s’intéresse à un jeu de test faisant varier une condition que s’il  inﬂue sur la décision.  

#### Critère MC/DC


Exemple if (A && (B || C))  
Principe : pour chaque test atomique, trouver deux cas de tests qui  changent la décision lorsque les autres conditions sont fixées.  Pour A :  
A = 0, B = 1, C = 1. Décision : 0  A = 1, B = 1, C = 1. Décision : 1  
Pour B :  
A = 1, B = 0, C = 0. Décision : 0  A = 1, B = 1, C = 0. Décision : 1  
Pour C :  
A = 1, B = 0, C = 1. Décision : 1  A = 1, B = 0, C = 0. Décision : 0. Déjà couvert.  
Si n conditions : critère MC/DC entraîne au plus 2 × n tests contre  2n pour le critère Toutes-les-décisions-multiple.  

#### Limite du critère Toutes-les-décisions-multiples


```linenums="1"
f l o a t
i n v s u m ( i n t n ,
f l o a t a [ ] ) i = 0 ;
s = 0 ;
i n t
f l o a t
w h i l e ( i n ) s=s+a [ i ] ;
i=i +1;
r e t u r n 1/ s ;
```



{  

<{  
}  
}  
  
  
  
  
  
  
  
  
  
  
E(cid:15)  Bloc B1  int i = 0 ;  ﬂoat s = 0 ;  
  
Condition C1  i<n  
ret. 1/s  
(cid:47) S  
  
Bloc B2  s = s+a[i] ;  i=i+1 ;  
E,B1,C1,B2,C1,ret. 1/s, S  couvre toutes les décisions.  Satisfait par n = 1,  a = {2., 3., 5.}. Défaut non  détecté si le tableau est vide.  

#### Critère "tous les chemins"


Critère Tous-les-chemins : parcourir tous les arcs dans chaque  configuration possible (et non pas au moins une fois comme dans le  critère toutes-les-décisions)  
Impraticable dès qu’il y a des boucles car existence de chemins infinis.  
Aﬀaiblissement possible : "tous les chemins de longueur au plus k"  
En pratique on se limite souvent aux cas k = 1 ou k = 2. Cela permet  au moins de détecter les erreurs produites quand on ne rentre pas  dans la boucle.  
Dans l’exemple précédent, n = 1, a = {} satisfait le chemin  E,B1,C1,ret . 1/s,S et détecte la division par zéro.  

#### Hiérarchie des critères


toutes les conditions  multiples  
MC/DC  
tous les chemins  
tous les chemins de  longueur au plus k  
  
      
        
    
 (cid:15)  
toutes les décisions  
tous les sommets  

#### Convention de notation algébrique


ε pour "chemin vide"  
A · B ou AB pour "A suivi de B"  (CB) pour "CBCBCB"  A + B pour "A ou B"  A pour "A + A + A + · · · + An + . . ."  A∗ pour "ε + A + A + A + · · · + An + . . ."  A pour ε + A + A + A + A  

#### Expression des chemins sous forme algébrique


```linenums="1"
v o i d F ( i n t x ) i f
( x=0)
x=x ;
e l s e
i f
x=1x ;
( x==1)
x=1+x ;
e l s e
x =1;
r e t u r n x ;
```

{  
  
  
  
  
  
  
  
  
  
  
  
}  

<−


−


B | x=-x  
I | In(cid:15)  A | x≤0  
D | r=1/a  
(cid:118)  
(cid:118)  
 (cid:40)  
 (cid:40)  
C | x=1-x  
E | x=1  
F | x=1+x  
G | ret. x  
O | Out  
         
IAB  C DE  F GO  
          ·  ·    ·  ·    ·     
