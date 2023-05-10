
logique
=======
!!! danger
    Ce cours n'a pas été entièrement reverifié après le passage du programme. Pensez à supprimer ce message si vous avez reverifié ce cours

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par
    Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce
    site à pour seul but d’être plus compréhensible pendant les périodes de
    révision que des diaporamas.


  
Introduction  
 Syntaxe  
 Sémantique  
 Satisfiabilité  
Tautologies  Formes normales  
 Problème SAT  
Problème SAT et n-SAT  Problème 2-SAT  

#### Crédits


Wikipedia,  
Mansuy,  
Becirspahic.  


  
Introduction  
 Syntaxe  
 Sémantique  
 Satisfiabilité  
Tautologies  Formes normales  
 Problème SAT  
Problème SAT et n-SAT  Problème 2-SAT  

#### Proposition


Notion aﬃnée au cours des siècles.  
Une proposition est une construction syntaxique censée parler de  vérité. La définition précise de ce concept est l’un des objectifs du  calcul des propositions.  
"Pourvu que je n’attrape pas le Coronavirus !" exprime un souhait  et pas un fait. Il n’est pas modélisé par une proposition.  
Il n’y a pas de quantification en calcul des propositions. Ainsi "M.  Noyer marche sur les eaux" est exprimable comme une proposition  mais pas "tous les marseillais marchent sur les eaux" ni "il existe  un marseillais qui marche sur les eaux" (d’ailleurs c’est M. Noyer !)  
Le calcul des propositions est la première étape dans la définition de  la logique et du raisonnement. ´Etape suivante : calcul des prédicats.  

#### Contenu des propositions


Une proposition donne une information évaluable sur quelque chose.  
Exemple : 2 + 2 = 4 ou "la maison de Pierre est rose" ou "si a ≤ b  et f (a)f (b) < 0 alors il existe c ∈]a, b[ tel que f (c) = 0".  
L’idée est qu’on puisse attribuer une valeur de vérité à toute  proposition.  Par exemple 2 + 2 = 4 a pour valeur Vrai, la seconde phrase dépend  des goûts de Pierre mais elle est sans doute fausse, la dernière dépend  du type et de la continuité de f .  Contre-exemple "`A quelle heure finit le cours ?" ou "Travaille  davantage !". Ces phrases n’apportent pas d’information.  Il est diﬃcile de leur attribuer une valeur.  
Les valeurs attribuées aux propositions sont en général 0 ou 1, True  ou False, Vrai ou Faux.  

#### Logique classique


C’est celle du cours de maths en CPGE.  
Quand le cardinal de l’ensemble des valeurs possibles que peuvent  prendre les propositions est 2, on parle de logique classique ou  bouléenne.  
Il existe d’autres logiques. Elles sont parfois tri-valuées. Par exemple  Vrai, Faux, Je ne sais pas.  Pourquoi faudrait-il d’autres logiques ?  En logique classique "je suis ici ou ailleurs" (ce qu’on appelle le  tiers exclu) est toujours vrai. Mais ça ne représente pas bien ce qui se  passe en physique quantique.  

#### Tiers exclu


Le principe du tiers exclu a été introduit par Aristote comme  conséquence du principe de non-contradiction. Le principe de  non-contradiction stipule que pour toute proposition P on ne peut  pas avoir P et ¬P (non P) en même temps.  
La proposition ¬(P ∧ ¬P) équivaut sémantiquement (par utilisation  de règle de De Morgan) à ¬P ∨ ¬¬P.  On retrouve le tiers exclu SI ON ACCEPTE que ¬¬P et P sont  sémantiquement équivalents.  

#### D’autres mathématiques


La logique intuitionniste qui engendre les mathématiques  constructives, admet le principe de non-contradiction mais réfute celui  du tiers exclu.  Pour un constructiviste, le raisonnement "si a ∈ A alors bla sinon  bli" n’a de sens que s’il existe un test pour déterminer l’appartenance  à A.  Les constructivistes réfutent aussi l’axiome du choix qui dit en  substance que "dans un ensemble infini, je peux choisir un  élément", au motif que "je ne peux pas choisir si je ne sais pas  comment choisir".  La plupart des théorèmes de CPGE sont adaptables en  mathématiques constructives (parfois, les énoncés sont les mêmes,  d’autre fois, il faut adapter).  Une preuve de mathématiques constructives est bien adaptée à  l’informatique en ce sens qu’elle est pratiquement donnée sous forme  d’algorithme.  

#### Structure (d’après Wikipedia)


```linenums="1"
Soit en munissant d’ une (un sens) les connecteurs
logique. On peut alors définir inductivement une interprétation pour
toute proposition complexe.
Soit par : On se donne des régles purement syntaxiques qui
permettent de déduire une proposition d’un ensemble d’autres : les
(Par exemple : ou ).
On ne se préoccupe donc pas du sens, juste de la déductibilité.
```

Dans les théories de la logique mathématique, on considère deux  points de vue dits syntaxique et sémantique, c’est le cas en calcul des  propositions.  
Aspect syntaxique : la forme. Il s’agit de définir le langage du calcul des  propositions par les règles d’écriture des propositions. On décrit donc  ce qu’EST une proposition.  Aspect sémantique : le fond. Il s’agit de donner un sens aux symboles  représentant les connecteurs logiques en fonction de la valeur de vérité  des propositions de base. On décrit donc ce que SIGNIFIE une  proposition. En CPGE, ce sens est donné par des tables de vérité. .  L’aspect sémantique est décrit de deux façons  
interprétation déduction règles d’inférence calcul des séquents déduction  naturelle

#### Sémantique VS déduction


La fonction "qui donne du sens" est appelé une interprétation. On  en a déjà rencontrée une pour passer des expressions régulières aux  langages.  
On construit un arbre d’inférence et on dit qu’une formule est  prouvable si toutes les feuilles de l’arbre sont des axiomes.  On appelle théorème une proposition prouvable.  
En calcul des propositions, tout ce qui est vrai est prouvable et  réciproquement.  
Malheureusement, dès qu’on ajoute des fonctions et des  quantificateurs (calcul des prédicats), il y a des formules vraies qui ne  sont pas prouvables. En revanche, ce qui est prouvable reste vrai.  


  
Introduction  
 Syntaxe  
 Sémantique  
 Satisfiabilité  
Tautologies  Formes normales  
 Problème SAT  
Problème SAT et n-SAT  Problème 2-SAT  

#### L’alphabet


!!! quote "Définition"
    Définition
On considère un alphabet Σ constitué :


!!! quote "Définition"
    Remarque


de symboles Vrai et Faux notés V et F,  de variables propositionnelles en nombre dénombrable notées dans ce  cours en lettres romaines a, b, . . . , a, . . . , z, . . .  de deux symboles de parenthèses "(,)" (ouvrante et fermante).  
d’un connecteur unaire ¬ dit de négation  
de trois connecteurs binaires notés ∨, ∧, −→ et appelés connecteurs  logiques de disjonction, conjonction et d’implication.  
On dit aussi opérateur pour "connecteur"  
Dans certains cours, on ajoute aussi l’opérateur ⇐⇒ et le XOR.  Dans certains autres, seulement ¬, ∨, ∧, voire même ¬, ∧.  

#### Ensemble des propositions


!!! quote "Définition"
    Définition
On appelle langage des propositions le langage défini inductivement par :


les constantes et les variables propositionnelles sont dans le langage,  
si A est dans le langage, alors (A) aussi,  
si A est dans le langage (¬A) aussi,  
si A, B sont dans le langage alors (A ∨ B), (A ∧ B) et (A −→ B) aussi.  

#### Représentation arborescente


!!! quote "Définition"
    Remarque
Puisque les propositions sont construites comme des arbres binaires, il est
naturel de parler de leur taille et de leur hauteur.


(((¬a) ∨ b) ∧ (¬c)) se représente sous la forme d’un arbre binaire (dit  arbre syntaxique) :  
∨  
∧  
b  
¬  
c  
¬  
a  

#### Simplification de l’écriture


On ne note pas les parenthèses autour de la racine.  
On convient (souvent, mais je ne trouve pas ça si commode) que  l’opérateur de négation ¬ a priorité sur les autres, que la conjonction  et la disjonction ont priorité sur l’implication, et enfin que la  conjonction a priorité sur la disjonction.  C’est très classique !  
Pour passer des écritures sans parenthèses aux écritures avec  parenthèses, une règle simple : plus l’opérateur est prioritaire, plus il a  de parenthèses autour de lui.  

#### Priorités en Python
<p align='center'><img src='/images/0bfc055d3bd1bd46045a6bc736dd9175.bmp'/></p>

_Figure – Règles de priorité de la plus haute (en haut) à la plus basse (en bas).
Les opérateurs de même niveau sont évalués de gauche à droite_
#### Exemple


Remettre des parenthèses  
¬a ∨ b ∧ c  
((¬a) ∨ (b ∧ c))  
¬a ∧ b ∨ c  
(((¬a) ∧ b) ∨ c)  
a ∨ ¬b ∧ c −→ a ∧ c  
[(a ∨ ((¬b) ∧ c)) −→ (a ∧ c)]  


  
Introduction  
 Syntaxe  
 Sémantique  
 Satisfiabilité  
Tautologies  Formes normales  
 Problème SAT  
Problème SAT et n-SAT  Problème 2-SAT  

#### Contexte


!!! quote "Définition"
    Définition
Un contexte (ou une distribution de vérité) sur un ensemble de variables
propositionnelles V est une application de V dans l’ensemble des bouléens
B.


!!! quote "Définition"
    Remarque


Si |V| = n, alors il y a 2n distributions de vérité.  L’ensemble des bouléens peut être représenté de diﬀérentes façons.  
Dans ce cours, on pose B = {0, 1}, et on identifie B avec l’anneau  Z/2Z (donc 1 + 1 = 0 en particulier.)  La multiplication est l’interprétation de la conjonction, 0 celle de F, 1  celle de V, l’addition celle du XOR.  

#### Interprétation


!!! quote "Définition"
    Définition
Soit µ un contexte sur un ensemble de variables V à valeur dans
B = Z/2Z. On appelle évaluation (ou encore interprétation) associée à µ
l’application notée Eµ définie sur l’ensemble des propositions par :


!!! quote "Définition"
    Remarque
Observer l’analogie avec les fonctions caractéristiques.


Eµ(V ) = 1, Eµ(F ) = 0  pour toute variable v ∈ V, Eµ(v ) = µ(v )  pour toute expression p, Eµ(¬p) = 1 − Eµ(p)  pour toutes expressions p et p :  Eµ(p ∧ p) = Eµ(p)Eµ(p)  Eµ(p ∨ p) = Eµ(p) + Eµ(p) − Eµ(p)Eµ(p)  Eµ(p −→ p) = 1 − Eµ(p) + Eµ(p)Eµ(p).  

#### Implication


Comme on va le voir a −→ b est sémantiquement équivalent à  ¬a ∨ b, c’est à dire que les deux propositions ont la même valeur de  vérité pour toute interprétation.  
L’interprétation de l’implication donnée au transparent précédent  s’obtient par calcul :  
Eµ((¬p) ∨ p) =  
Eµ(¬p) + Eµ(p) − Eµ(¬p)Eµ(p)  
= (1 − Eµ(p)) + Eµ(p) − (1 − Eµ(p))Eµ(p)  = 1 − Eµ(p) + Eµ(p) − Eµ(p) + Eµ(p)Eµ(p)  =  =  
1 − Eµ(p) + Eµ(p)Eµ(p)  Eµ(p −→ p)  

#### Tables de vérité


Les 4 opérateurs de la logique classique  Table du ET  a ∧ b  a  1  1  0  1  0  0  0  0  
Table du OU  a ∨ b  a  1  1  1  1  1  0  0  0  
b  1  0  1  0  
b  1  0  1  0  
Table du NOT  a  1  0  
¬a  0  1  
Table d’implication  a  1  1  0  0  
a −→ b  1  0  1  1  
b  1  0  1  0  

#### Equivalence sémantique


!!! quote "Définition"
    Définition
Deux propositions p, q sont dites sémantiquement équivalentes si elles ont
même table de vérité.. On note p ≡ q.


!!! quote "Définition"
    Définition
On dit qu’une proposition p est une tautologie lorsque p ≡ V


!!! quote "Définition"
    Remarque


Par exemple a ∧ V est sémantiquement équivalente à a.  
Montrer que le tiers exclu a ∨ ¬a est une tautologie.  
Certains auteurs parlent d’équivalence logique.  

#### XOR


!!! quote "Définition"
    Exercice
Définir l’opérateur XOR au moyen des opérateurs usuels


Table du XOR  OU Exclusif  a ⊕ b  0  1  1  0  
b  1  0  1  0  
a  1  1  0  0  

#### Priorités


Les opérateurs ∧ et ∨ sont associatifs : en conséquence on peut écrire  ((a ∨ b) ∨ (c ∨ d)) comme a ∨ b ∨ c ∨ d  
−→ n’est pas associatif a −→ (b −→ c) n’est pas sémantiquement  équivalent à (a −→ b) −→ c  
Exercice : montrer ces assertions par des tables de vérité.  
L’implication n’est pas associative mais par convention  A −→ B −→ C −→ D se lit A −→ (B −→ (C −→ D)).  Observer l’analogie avec une fonction f de type a->b->c->d en  Ocaml : f x est de type b->c->d.  

#### D’autres opérateurs


!!! quote "Définition"
    Exercice


 Montrer qu’on peut se passer de l’opérateur d’implication.  
 On définit l’opérateur binaire NOR ainsi : NOR(a, b) a la table de  
¬(a ∨ b). Montrer que NOR(a, b) est sémantiquement équivalent à  ¬a ∧ ¬b (seconde loi de De Morgan).  
 L’opérateur binaire NAND est défini ainsi : NAND(a, b) a la table de  
¬(a ∧ b). Montrer que NAND(a, b) est sémantiquement équivalent à  ¬a ∨ ¬b (première loi de De Morgan).  
 L’opérateur binaire ⇐⇒ est défini ainsi : a ⇐⇒ b a la table de  (a −→ b) ∧ (b −→ a). Montrer que a XOR b est sémantquement  équivalent à ¬(a ⇐⇒ b).  

#### Calcul à partir de l’arbre syntaxique


∨  
∨  
∧  
1  
0  
b V  
−→  
1  
∨  
0  
¬  
0  
−→  
∧(cid:15)  1  
1  
−→  
¬  
c  
∨  
1  
0  
∨  
a  
b  
−→  
1  
1  


  
Introduction  
 Syntaxe  
 Sémantique  
 Satisfiabilité  
Tautologies  Formes normales  
 Problème SAT  
Problème SAT et n-SAT  Problème 2-SAT  


  
Introduction  
 Syntaxe  
 Sémantique  
 Satisfiabilité  
Tautologies  Formes normales  
 Problème SAT  
Problème SAT et n-SAT  Problème 2-SAT  

#### Tautologie


!!! quote "Définition"
    Définition


!!! quote "Définition"
    Exemple


On appelle tautologie toute expression évaluée à 1 dans tout contexte.  
Une proposition p est dite satisfiable si il existe un contexte µ tel que  (cid:15)µ(p) = 1. On dit que µ est un modèle de p et on note µ (cid:15) p.  Une proposition qui n’est pas satisfiable est apellée une antilogie. On  dit aussi que l’expression est insatisfiable.  
Le tiers exclu a ∨ ¬a est une tautologie, (a ∧ b) −→ b aussi.  
a −→ b est satisfiable mais n’est pas une tautologie.  
a ∧ ¬a est insatisfiable.  
Montrer que (¬A −→ A) −→ A est une tautologie  

#### Notation


Pour indiquer qu’une proposition a est sémantiquement équivalente à  une proposition b, il n’est pas suﬃsant d’écrire a ⇐⇒ b.  
En eﬀet a ⇐⇒ b est juste une proposition. Celle-ci peut être  satisfiable ou non (peut-être même pas, comme V ⇐⇒ F ).  
La bonne façon d’indiquer cette équivalence est de déclarer  "a ⇐⇒ b est une tautologie".  
Les mathématiciens sont parfois paresseux : il leur arrive d’écrire  "a ⇐⇒ b" au lieu de "a ⇐⇒ b est vraie (ce qui signifie vraie  pour tout contexte)".  
On écrit a ≡ b pour indiquer que a ⇐⇒ b est une tautologie.  
Attention, "≡" n’est pas un opérateur (il n’appartient pas au  langage) mais un méta-opérateur.  A ce propos, un principe empirique est que, pour exprimer des idées  sur un langage, il faut souvent "sortir" du langage.  

#### Notion de conséquence


!!! quote "Définition"
    Définition
On considère une expression logique p et χ un ensemble d’expressions
logiques. On dit que p est une conséquence de χ si toute interprétation qui
satisfait toutes les formules de l’ensemble χ satisfait p. Si c’est le cas, on
note : χ (cid:15) p.


!!! quote "Définition"
    Notation


Si χ = {h, . . . , hn}, on écrit aussi h, . . . , hn (cid:15) p.  Si χ = {h}, on écrit h (cid:15) p  Par convention, (cid:15) p indique que p est une tautologie.  

#### Propriétés de ⇐⇒


!!! quote "Définition"
    Réﬂexivité p ⇐⇒ p


!!! quote "Définition"
    Symétrie (p ⇐⇒ p) ⇐⇒ (p ⇐⇒ p)


!!! quote "Définition"
    Transitivité ((p ⇐⇒ p) ∧ (p ⇐⇒ p)) ⇐⇒ (p ⇐⇒ p)


Pour toutes propositions p, p, p, les propositions suivantes sont des  tautologies :  

#### Propriétés de ∧


!!! quote "Définition"
    Pour toutes propositions p, p, p, les propositions suivantes sont des
tautologies :
´Elément neutre (p ∧ V ) ⇐⇒ p
´Elément absorbant (p ∧ F ) ⇐⇒ F
Commutativité (p ∧ p) ⇐⇒ (p ∧ p)
Associativité (p ∧ (p ∧ p)) ⇐⇒ ((p ∧ p) ∧ p)
Idempotence (p ∧ p) ⇐⇒ p

#### Propriétés de ∨


!!! quote "Définition"
    Pour toutes propositions p, p, p, les propositions suivantes sont des
tautologies :
´Elément neutre (p ∨ F ) ⇐⇒ p
´Elément absorbant (p ∨ V ) ⇐⇒ V
Commutativité (p ∨ p) ⇐⇒ (p ∨ p)
Associativité (p ∨ (p ∨ p)) ⇐⇒ ((p ∨ p) ∨ p)
Idempotence (p ∨ p) ⇐⇒ p

#### Relations entre ∨ et ∧


!!! quote "Définition"
    Pour toutes propositions p, p, p, les propositions suivantes sont des
tautologies :
Subsomption (p ∨ (p ∧ p)) ⇐⇒ p
Subsomption (p ∧ (p ∨ p)) ⇐⇒ p
Distributivité (p ∧ (p ∨ p)) ⇐⇒ (p ∧ p) ∨ (p ∧ p)
Distributivité (p ∨ (p ∧ p)) ⇐⇒ (p ∨ p) ∧ (p ∨ p)
Première loi de De Morgan ¬(p ∧ p) ⇐⇒ (¬p) ∨ (¬p)
Seconde loi de De Morgan ¬(p ∨ p) ⇐⇒ (¬p) ∧ (¬p)

#### Propriétés de −→


!!! quote "Définition"
    Pour toutes propositions p, p, p, les propositions suivantes sont des
tautologies :
Modus ponens (p ∧ (p −→ p)) −→ p
Double implication ((p −→ p) ∧ (p −→ p)) ⇐⇒ (p ⇐⇒ p)

#### Raisonnement en mathématiques


!!! quote "Définition"
    Pour toutes propositions p, p, les propositions suivantes sont des
tautologies :
Disjonction de cas ((p −→ p) ∧ (¬p −→ p)) ⇐⇒ p
Contraposition (p −→ p) ⇐⇒ (¬p −→ ¬p)
Raisonnement par l’absurde (¬p −→ F ) ⇐⇒ p


!!! quote "Définition"
    Exercice
Montrer qu’on a bien des tautologies en consruisant des tables de vérité.

#### ´Ecriture simplifiée


Pour travailler avec une proposition p, il est souvent utile de  considérer une expression sémantiquement équivalente à p mais plus  simple :  
Ce peut être une version standardisée, dite normale (Analogie :  
6  4  
a  
pour forme normale  
3  2  
)  
ou encore une version avec moins de connecteurs : F pour a ∧ (¬a)  (Analogie : 6 pour 1 + 2 + 3).  

#### Littéraux


!!! quote "Définition"
    Définition
On appelle littéral toute proposition de la forme v ou ¬v où v est une
variable propositionnelle.


!!! quote "Définition"
    Définition
On appelle conjonction (resp. disjonction) des propositions p, . . . , pn
(n ≥ 1), la proposition


!!! quote "Définition"
    Remarque
Par convention :


p ∧ p · · · ∧ pn (resp. p ∨ p · · · ∨ pn)  
Une conjonction de 0 proposition est V  
Une disjonction de 0 proposition est F  
Tout littéral est à la fois une conjonction et une disjonction.  

#### Clauses, Monômes


!!! quote "Définition"
    Définition
On appelle clause toute disjonction de littéraux. F , disjonction de zéro
littéral, est appelé la clause vide. (Moyen mnémotechnique :
claUse↔ ∪ ↔ ∨)


!!! quote "Définition"
    Définition
On appelle monôme toute conjonction de littéraux. V , conjonction de zéro
littéral, est appelé le monôme vide. (Moyen mnémotechnique :
mon(cid:98)ome↔ ∧)


!!! quote "Définition"
    Définition
On appelle forme normale conjonctive (FNC) toute conjonction de clauses
et forme normale disjonctive (FND) toute disjonction de monômes.

#### forme normale conjonctive/disjonctive


!!! quote "Définition"
    Proposition
Toute proposition est sémantiquement équivalente à une forme normale
conjonctive (resp. forme normale disjonctive).


!!! quote "Définition"
    Remarque


Fait : On n’a besoin que des 3 opérateurs ¬, ∨, ∧  
Convertir une proposition en forme normale conjonctive requiert  l’utilisation de règles de transformation logiques  
Un principe : ramener la négation "au contact" des variables.  
Les formes normales conjonctives ou disjonctives de littéraux ne sont  en général pas uniques !  

#### Exemples


Les propositions suivantes, dans lesquelles a, b, c, d, e, f sont des littéraux,  sont des formes conjonctives :  
a ∧ b  
a ∨ b (oui car c’est une clause - il y a zéro conjonction-)  
a  
(a ∨ b) ∧ c  
(a ∨ ¬b ∨ ¬c) ∧ (¬d ∨ e ∨ f )  

#### Contre exemples


Les propositions suivantes, dans lesquelles a, b, c, d, e, f sont des variables,  ne sont pas des formes conjonctives : :  
¬(a ∧ b) : la négation ¬ est devant la proposition (a ∧ b) qui n’est  pas atomique (ce n’est pas un littéral)  
a ∧ (b ∨ (c ∧ d)) : un ∧ est imbriqué dans un ∨.  

#### Uniquement des ¬ et des ∧


Toute proposition peut "s’exprimer" sans constante : V est  remplacé par v ∨ ¬v ; F par v ∧ ¬v où v est une variable.  
Tous les opérateurs peuvent se définir avec ¬, ∧, ∨ uniquement.  
une disjonction a ∨ b peut s’exprimer comme la négation d’une  conjonction ¬(¬a ∧ ¬b)  
En appliquant ces règles, on obtient que toute proposition est  sémantiquement équivalente à une expression ne contenant que des  variables et les connecteurs ¬, ∧. Ceci nous sert au transparent  suivant.  

#### Preuve d’existence d’une forme normale conjonctive


Récurrence sur la taille de la proposition p. HR : p est équivalente à FNC.  
Si p est un littéral : ok.  Si p = ¬q, on applique l’hypothèse de récurrence à q qui s’écrit donc  c ∧ · · · ∧ cn où les ci sont des clauses.  
Par les lois de De Morgan, chaque clause ci s’écrit comme un ¬mi où  
ni(cid:94)  
mi est un monôme mi =  
li,ki (les li,ki sont des littéraux).  
ki   
Alors par De Morgan  p ≡ ¬(¬m ∧ · · · ∧ ¬mn) ≡ (¬¬m) ∨ · · · ∨ (¬¬mn) ≡ m ∨ · · · ∨ mn.  On a bien ce qu’on veut :  
n  (cid:95)  
(cid:32) ni(cid:94)  
(cid:33)  
li,ki  
p ≡  
i  
ki   
≡      
  
(cid:94)  
      
  l,k ∨ · · · ∨ ln,kn          
,n  
k∈  (cid:74)  
...  
,nn  
kn∈  (cid:74)  
(cid:75)  
(cid:75)  

#### Preuve d’existence d’une forme conjonctive


Récurrence sur la taille de la proposition p. HR : p est équivalente à une  conjonction de clause.  
Si p est un littéral : OK.  
Si p = ¬q : OK  Si p = f ∧ f, par HR f ≡ p ∧ · · · ∧ pn et f ≡ q ∧ · · · ∧ qn où les  pk , qr sont des clauses.  
p  
≡      ∧  
p ∧ · · · ∧ pn ∧ q ∧ · · · ∧ qn            
Ce qu’on veut.  
Pas d’autre cas puisque p est exprimé avec seulement deux  connecteurs. Comme toute proposition peut s’écrire sous cette forme,  on a bien ce qu’on veut.  

#### Disjonction de conjonction


On sait que toute proposition peut s’exprimer comme conjonction de  clause.  
Soit p une proposition, alors ¬p s’écrit sous la forme  ¬p ≡ c ∧ · · · ∧ cn avec ci = li, ∨ · · · ∨ li,ni .  Donc p ≡ ¬¬p ≡ ¬c ∨ · · · ∨ ¬cn par De Morgan.  Or chaque ¬ci est équivalent sémantiquement à une conjonction de  littéraux (par De Morgan) : ¬ci ≡ ¬li, ∧ · · · ∧ ¬li,ni .  Donc toute proposition peut s’écrire comme disjonction de monômes.  
PB : ces réécritures ne sont pas uniques.  

#### Mintermes, maxtermes


!!! quote "Définition"
    Définition
Soient v, . . . , vn des variables distinctes.


!!! quote "Définition"
    Remarque
Les 4 (à l’ordre près) maxtermes de v et v sont


On appelle minterme de v, . . . , vn, tout monôme où chaque variable  vi apparaît exactement une fois (et il n’y a pas d’autre variable).  On appelle mAxterme de v, . . . , vn, une clAuse où chaque variable vi  apparaît exactement une fois (et il n’y a pas d’autre variable).  
v ∨ v, v ∨ ¬v, ¬v ∨ v, ¬v ∨ ¬v.  
On confond les maxtermes v ∨ v et v ∨ v.  

#### Forme normale conjonctive


!!! quote "Définition"
    Définition


!!! quote "Définition"
    Remarque


On dit qu’une proposition est en forme normale conjonctive avec  maxtermes (FNCM), si elle s’écrit comme une conjonction de  maxtermes tous distincts.  
On dit qu’une proposition est en forme normale disjonctive avec  mintermes (FNDM), si elle s’écrit comme une disjonction de  mintermes tous distincts.  
Mettre une proposition sous FNCM, c’est donner une expression en  FNCM sémantiquement équivalente à cette proposition.  (v ∨ ¬v ∨ v) ∧ (v ∨ v ∨ ¬v) est une FNCM sur les variables  v, v, v mais pas (v ∨ ¬v ∨ v) ∧ ¬(v ∨ v ∨ ¬v) car la négation  n’est pas au contact des variables.  
Utilisation : démonstration automatique de théorèmes ou PB SAT.      

#### Exemple


Soit p une proposition à 3 variables dont la table est :  
b  1  1  0  0  1  1  0  0  
p  0  1  1  0  1  0  0  1  
Table de p  c  1  0  1  0  1  0  1  0  
a  1  1  1  1  0  0  0  0  Lire positivement les variables  des lignes à 1 pour la FND  Lire négativement les variables  des lignes à 0 pour la FNC  

#### Forme normale conjonctive


!!! quote "Définition"
    Proposition


!!! quote "Définition"
    Remarque
Dans ce qui suit on dit que deux FNDM sont égales si elles ont les mêmes
mintermes (les mintermes sont donnés à l’ordre des littéraux près).


Toute proposition est sémantiquement équivalente à une FNCM (resp.  FNDM) unique à l’ordre des maxtermes (resp. mintermes) près (et à ordre  des littéraux près dans chaque maxterme -resp. minterme-).  

#### Existence de la FNDM


!!! quote "Définition"
    Démonstration.


On construit la table de vérité de la proposition : il y a 2n lignes  correspondants aux valeurs de vérité des n variables de p.  
Chaque ligne satisfaisant l’expression (1 dans la dernière colonne)  donne un minterme. Toutes les variables sont présentes et une  variable v y apparaît positivement (v ) si sa valeur de vérité dans la  ligne est 1, négativement (¬v ) sinon.  
On considère la disjonction de tous les mintermes ainsi obtenus. C’est  une expression q en FND.  Soit µ un contexte. Si εµ(p) = 1, alors le minterme associé à µ est  vrai pour µ donc εµ(q) = 1.  Si εµ(p) = 0, alors le minterme associé à µ est faux pour µ donc  εµ(q) = 0.  Donc p est bien équivalente sémantiquement à une expression q en  FNDM.  

#### Unicité de la FNDM à l’ordre près


Soient p ≡ q deux FND. Elles ont donc même table.  Si p (cid:54)= q et p, q ne possèdent qu’un minterme :  
Si p, q ont deux littéraux l, l(cid:48) de la même variable v distincts, l’un des  littéraux vaut ¬v l’autre v . Autrement dit l ≡ ¬l(cid:48)  Comme p est une conjonction, il n’y a qu’un seul contexte c (i.e. une  seule ligne) dans lequel p vaut 1.  Alors l vaut aussi 1 dans ce contexte (p étant une conjonction).  Donc l(cid:48) vaut 0 dans c. Et donc q vaut 0 dans c. p et q ne peuvent  avoir même table.  De ce qui précède, on déduit aussi que des mintermes (ayant les mêmes  variables) distincts ne valent jamais 1 en même temps.  

#### Unicité de la FNDM(suite)


Soient p, q deux FND de même table.  
On a vu que si p (cid:54)= q et p, q ne possèdent qu’un minterme, leurs  tables sont diﬀérentes. On en déduit que des mintermes distincts ne  valent jamais 1 en même temps.  Si p et q sont des disjonctions de mintermes, et si un minterme m de  p ne se retrouve pas dans q :  
Lorsque m vaut 1, p aussi (comme disjonction). Tous les autres  mintermes valent 0 (puisque un seul minterme vaut 1 pour un contexte  donné), ceux de p comme ceux de q.  Alors dans ce contexte, comme disjonction de 0, q vaut 0.  Et donc p et q ne peuvent avoir même table.  

#### FNCM


!!! quote "Définition"
    Existence


!!! quote "Définition"
    Unicité Si p avait deux FNCM distinctes, par De Morgan ¬p aurait
deux FNDM distinctes. Or il y a une seule FNDM possible
pour une formule.


Soit une expression p à n variables,  
¬p a une FNDM c ∨ · · · ∨ cn où les ci sont des  mintermes.  Donc p se réécrit comme (¬c) ∧ · · · ∧ (¬cn). Or les  négations de mintermes se réécrivent en maxtermes (par  De Morgan).  Et donc p se réécrit en une FNCM. Voilà pour  l’existence.  

#### Réécriture


Pour obtenir la FNCM :  
On exprime la proposition uniquement avec des ¬, ∨, ∧  
On descend ¬ par les lois de De Morgan au contact des variables.  
On applique la distributivité pour faire descendre les ∨ et remonter les  ∧.  

#### Problème SAT


Comment tester qu’une proposition est satisfiable ? Il s’agit du  problème dit SAT.  
Comment tester qu’une proposition p est une tautologie ? Si on sait  résoudre le problème SAT, il suﬃt de montrer que ¬p est insatisfiable.  
Pour le problème SAT, on peut simplement calculer sa table. Mais on  a alors une complexité en O(2n), si n est le nombre de variables.  Des algorithmes plus eﬃcaces (en pratique, c’est à dire sauf cas  pathologiques) existent (DPLL) et passent par une mise sous FNC.  Mais dans le pire des cas ce passage à la FNC est lui-même de  complexité exponentielle.  

#### Explosion combinatoire


Considérons la proposition en forme disjonctive :  (x ∧ y) ∨ (x ∧ y) ∨ · · · ∨ (xn ∧ yn) de taille linéaire  Par distributivité, sa FNC, de taille 2n, est de la forme :  
(x ∨ x ∨ · · · ∨ xn− ∨ xn) ∧ (x ∨ x ∨ · · · ∨ xn− ∨ yn) ∧ · · ·  (x ∨ y ∨ · · · ∨ yn− ∨ yn) ∧ (y ∨ y ∨ · · · ∨ yn− ∨ yn)  

##  

###  

###  

#### Présentation du problème 2-SAT


Le problème SAT qui consiste à déterminer si une proposition est  satisfiable ou non est en général de complexité exponentielle en le  nombre de variables.  
Le problème CNF-SAT est la restriction du problème SAT aux formes  normales conjonctives.  
Le problème n-SAT est la restriction du problème SAT aux formes  normales conjonctives avec au plus n littéraux par clause.  
Le problème 2-SAT consiste à déterminer si une forme conjonctive  dont les clauses ont seulement deux littéraux est satisfiable.  Il admet des solutions polynômiales.  

###  

#### 2-SAT


Cette section est laissée pour info mais sera traitée en seconde année.  

#### Problème 2-SAT (Aspvall-Plass-Tarjan)


Soit p une proposition mise sous forme conjonctive avec des 2-clauses  
On construit un graphe orienté :  
Ses sommets sont tous les littéraux formés avec les variables  apparaissant dans la proposition.  Les arcs sont des couples de littéraux de la forme (a, ¬b) ou (¬a, b)  mais pas tous ...  Pour deux littéraux a, b, l’arc (¬a, b) est présent si et seulement si la  disjonction a ∨ b est dans p. Et dans ce cas (a, ¬b) est présent aussi.  Ceci correspond à a ∨ b ≡ (¬a −→ b) ∧ (¬b −→ a)  
Une fois ce graphe construit, on examine la composante fortement  connexe de toute variable v de p. Si ¬v est dedans, il y a un chemin  d’implications v −→ . . . −→ ¬v et un autre ¬v −→ . . . −→ v .  Alors v ⇐⇒ ¬v est une conséquence de p : p (cid:15) v ⇐⇒ ¬v (ce qui  signifie que tout modèle satisfaisant p, satisfait v ⇐⇒ ¬v ). Donc si  p est satisfiable, v ⇐⇒ ¬v aussi : ABSURDE.  Donc si v et ¬v sont dans la même composante connexe, p n’est pas  satisfiable (Aspvall-Plass-Tarjan).  

#### Exemple (Mansuy)


p = (v ∨ v) ∧ (¬v ∨ v) ∧ (v ∨ ¬v) ∧ (¬v ∨ v) ∧ (¬v ∨ ¬v) donne le  graphe  
v  
(cid:47) v  
v  
¬v  
¬v  
(cid:47) ¬v  
¬v accède à v via v et  v accède à ¬v via ¬v  Donc v et ¬v sont dans la même composante connexe.  Par suite p n’est pas satisfiable.  

#### Problème SAT


Le problème SAT est le problème algorithmique qui consiste, étant  donnée une formule à décider en FNC si elle valide ou non.  Le problème est NP-complet, même pour le cas particulier 3-SAT où  on n’autorise que les clauses d’au plus trois littéraux.  Un problème P NP-complet vérifie :  
Il est possible de vérifier une solution de P eﬃcacement (en temps  polynomial) : on me donne un candidat solution, et je peux vérifier en  temps polynomial ce qu’il en est. La classe des problèmes vérifiant  cette propriété est notée NP.  tous les problèmes de la classe NP se ramènent à P via une réduction  polynomiale. Cela signifie que le problème est au moins aussi diﬃcile  que tous les autres problèmes de la classe NP (aspect "complet").  Appliqué à la satisfiabilité, cela signifie que si on me donne un  problème de la classe NP, je peux le transformer en temps polynomial  en un problème de satisfiabilité.  
Bien sûr, un problème de la classe P (polynomial) vérifie la première  condition. On a donc P ⊂ NP, mais a-t-on NP ⊂ P ?  
