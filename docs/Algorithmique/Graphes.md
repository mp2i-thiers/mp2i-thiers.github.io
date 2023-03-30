# Graphes

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par
    Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce
    site à pour seul but d’être plus compréhensible pendant les périodes de
    révision que des diaporamas.

## Crédits

Wikipédia : théorie des graphes  
Wikipédia : graphes simples  
Toujours le Mansuy.  

## Historique

### Les sept ponts de Königsberg

Un article du mathématicien suisse Leonhard Euler, présenté à  l’Académie de Saint-Pétersbourg en 1735 puis publié en 1741.  

Trouver une promenade à partir d’un point donné qui fasse revenir à  ce point en passant une fois et une seule par chacun des sept ponts  de la ville de Königsberg : **Circuit eulérien**.  Euler fut sans doute le premier à proposer un traitement  mathématique de la question, suivi par Vandermonde.

<p align='center'><img src='/images/224099c80dc7aec23fc3183bd5015114.bmp'/></p>

_Figure – Abstraction du problème des 7 ponts de Königsberg_

!!! definition "Théorème"
    Un graphe connexe admet un circuit eulérien _si et  seulement si_ tous ses sommets sont de degré pair.  Ici un des sommets a 3 voisins : pas de circuit eulérien.  

## Graphes, représentation, sous-graphes

### Informellement

Un graphe est un ensemble de points dans lequel on fait apparaître  une ou plusieurs relations(s) entre deux points.  Ces relations sont en général représentées par des ﬂèches ou par des  segments. Dans le premier cas, le graphe est dit _orienté_ et les liens  sont appelés des _arcs_. Dans le second, le graphe est dit _non orienté_ et  les liens sont souvent appelés des _arêtes_.  Les points sont appelés les sommets (en référence aux polyèdres) ou  les nœuds (en références à la loi des nœuds).

<p align='center'><img src='/images/96b00195298f3c148e9e8a7b23e25d17.bmp'/></p>

_Figure – Un graphe orienté avec des arcs - un graphe non orienté et ses arêtes._

**Exemple : plan d’une ville.**  
<p align='center'><img src='/images/dc5ea5b5757bf7df068e7878c83ff666.bmp'/></p>

_Figure – Un multigraphe non orienté : ses arêtes multiples en rouge et ses
boucles en bleu_

En anglais, sommet se dit _vertice_, arête se dit _undirected edge_ et arc  se dit _directed edge_.  
Les arêtes multiples ne sont pas au programme.  

### Graphe simple non orienté

La définition suivante ne s’applique pas aux graphes avec arêtes multiples.

!!! quote "Définition"
    Un graphe (simple) non orienté G est un couple (V, E) où E ⊆ P(V ) est un ensemble de paires ou de singleton d’éléments de V. On appelle sommets les éléments de V et arcs ceux de E.

La lettre E est utilisée pour les arcs car en anglais, _arcs_ se dit _edge_.  
Certains auteurs utilisent un vocabulaire spécial pour les graphes non  orientés. Par exemple, une arête (undirected edge) désigne un arc.  
Soit a = {x, y}. On dit que :  

- a relie les sommets x et y , x et y sont adjacents ou encore **voisins**
- a est incidente avec x et y ou encore x et y sont incidents avec a.  

### Graphe simple orienté

Au programme ne figurent que les graphes avec au plus un seul arc d’un  sommet à un autre.

!!! quote "Définition"
    Un graphe simple orienté G est un couple (V , A) où :

    - V est appelé l’ensemble des sommets de G ,
    - et A ⊆ $V²$  est un ensemble de couples d’éléments de V appelé  l’ensemble des arcs de G.
  
La lettre V est utilisée pour les sommets car en anglais, sommet se  dit vertex (au pluriel vertices).  
Un _arbre_ est un cas particulier de graphe orienté simple.  
Mais pour certains auteurs, un arbre est un graphe non orienté  connexe et acyclique (voir plus loin pour les définitions).  

Un arc est noté a = (x, y) ou a = x → y et on dit que :  

- a va de x à y
- x est l’extrémité initiale de a  
- y est l’extrémité terminale de a
- y est un voisin de x. a est incident à x et y  

### Degré

Dans un graphe général (orienté ou non), on appelle degré d’un  sommet s et on note d(s), le nombre d’arcs incidents au sommet s. (=Nombre d'arc d'un sommet)
Dans un graphe général orienté, on distingue le degré sortant ou  extérieur $d^{+}(s)$ qui est égal au nombre d’arcs dont s est l’extrémité  initiale et le degré entrant ou intérieur $d^{−}(s)$ qui est égal au nombre  d’arcs dont s est l’extrémité finale.  

!!! danger
    schema et dernier point de la page 12

### Matrice d’adjacence sommets-sommets

!!! quote "Définition"
    Soit G = (S, A) un graphe fini simple.
    Notons {v, . . . , $v_n$} les sommets de S.
    On appelle matrice d’adjacence sommets-sommets de G = (S, A) la
    matrice n × n A = $(a_{ij} )≤i,j≤n$ telle que
    $a_{ij}$ = 1 si il existe un arc de vi à vj
    0 sinon

Remarque

- La matrice d’adjacence dépend de la numérotation des sommets. Il  faut que cette numérotation soit connue pour comprendre la matrice.  
- A une numérotation des sommets correspond une unique matrice  d’ajacence sommets-sommets.  
- Inadaptée pour les arêtes (ou les arcs) multiples. Présence de boucle  si $a_{ii} = 1$.  Graphe non orienté =⇒ matrice symétrique.  

### Liste d’adjacence

!!! quote "Définition"
    Soit G = (S, A) un graphe fini simple.

    On appelle liste d’adjacence de G toute liste de couples (s, l) où s parcourt S et l est une liste de ses voisins.

Remarque

Si une numérotation des sommets est choisie, on peut se contenter de
donner la liste des voisins. La première liste donne les voisins du premier sommet, la seconde celle du second sommet etc...

### Exemple de repésentation

!!! danger
    Problèmes avec les images et tableaux à refaire

#### Cas non orienté

#### Cas orienté

### Matrices d’adjacence : quelle représentation ?

En Ocaml ou C, les matrices d’adjacence sont simplement  représentées par des matrices carrées c.a.d. des tableaux à deux  dimensions avec même nombre de lignes que de colonnes.  

A la place de 0 et de 1, on peut utiliser vdes bouléens.  
Implicitement on considère que les sommets sont des nombres. Ou  alors on dispose d’un tableau de correspondance entre les sommets et  leurs numéros (utile si les sommets contiennent des informations).  

Avec un tel choix :  

- il est facile de supprimer ou d’ajouter un arc entre deux sommets  existants.  
- On teste en O(1) si deux sommets sont voisins.  
- Ajouter un sommet nécessite en général une copie de la matrice  (complexité quadratique).  
- Place mémoire perdue importante si beaucoup de sommets et matrice  creuse.  

#### Listes d’adjacence en Ocaml : quelle représentation ?

En Ocaml pour un graphe G = (V , E ) :  
On peut considérer une liste L de longueur |V | de tuples (s, l) ou s  est un sommet et l la liste des voisins de s.  

- Avantages : pas de place mémoire perdue ; possibilité d’ajouter un  nouveau sommet après avoir vérifié que ce sommet n’est pas déjà dans  la liste.  
- Inconvénients : Accès à la liste d’adjacence de s en O(|V |) ; test de  voisinage entre s et x en O(|V | + deg s) ; ajout d’un arc (s, x) en  O(|V | + deg s).  

On peut préferer gérer un tableau de listes l plutôt qu’une liste de  tuples (les sommets sont alors des nombres).  

- Avantage : l’accès à la liste d’adjacence de s est en O(1) ; test de  voisinage avec x en O(deg s) ; ajout d’un arc (s, x) en O(deg s) (il faut  vérifier que l’arc n’est pas déjà présent - usage de list.mem -).  
- Inconvénient : pour ajouter un sommet, il faut recopier le tableau.  

#### Listes d’adjacence en C : quelle représentation ?  

##### Tableau de liste de chaînes

Dans le même esprit qu’en Ocaml, pour représenter G = (V , E )  
Tableau t des successeurs de chaque sommet.  t[i] pointe sur la liste des sucesseurs du sommet i.  Accès direct à un sommet via t.  Complexité spatiale optimale en O(|S| + |V |)  

<p align='center'><img src='/images/30d6807e73ede3051cac8121b52565f8.bmp'/></p>

_Figure – Un tableau de liste chaînée de successeurs. (F. Pesseaux)_

##### Partage physique des sommets

On peut aussi partager physiquement les sommets.  
Chaque sommet est représenté 1 et 1 seule fois.  Chaque sommet est associé à une liste dont les éléments pointent sur  ses successeurs.  

<p align='center'><img src='/images/49abd04711fec3df0133631668dd48ea.bmp'/></p>

_Figure – Partage physique des données (F.Pesseaux)_

!!!danger
    EN CHANTIER FFFFFFF

#### Sous-graphes

il ne faut
pas conserver d’arc dont une extrémité a été supprimée de
l’ensemble des sommetsOn garde tous les sommets, on enlève certains arcs.


Convention : V pour vertice, E pour edge.  
Un sous-graphe est un graphe contenu dans un autre graphe :  "H = (VH , EH ) est un sous-graphe de G = (VG , EG ) si VH ⊂ VG ,  EH ⊂ EG et pour tout arc (resp. arête) de EH , les extrémités sont  dans VH".  On supprime des arcs et des sommets avec la contrainte qu’.  
Un sous-graphe couvrant (ou graphe partiel) est un sous-graphe ayant  le même ensemble de sommets que le graphe qui le contient.  "H est un sous-graphe couvrant de G (ou H couvre G ) si VH = VG  et EH ⊂ EG ."  


```linenums="1"
On enlève des sommets, toutes les arêtes correspondant à ces
sommets et uniquement celles-là.
```

Convention : V pour vertice, E pour edge.  
Un sous-graphe induit est un sous-graphe défini par un sous ensemble  de sommets.  "H est un sous-graphe induit de G si, pour tout (x, y ) ∈ V   H ,  l’existence d’un lien entre x et y dans H est équivalente à l’existence  d’un lien entre x et y dans G ."  

#### Exemples


```linenums="1"
Un Le ```

S  
S  
S  S  
S  
S  
S  S  
S  
S  
S  S  
S  
S  S  
S  
S  
S  S  
S  
S  
S  S  
S  
Le graphe complet  
graphe couvrant  
sous-graphe induit  par {S, S, S, S}  

##    

####  Historique


 Graphes, représentation, sous-graphes  
 Chaînes et chemins, connexité  
Accessibilité  Connexité  
 Graphes particuliers  
Arbres et forêts  Graphes non orientés particuliers  
 Un peu de OCAML  
 Parcours de graphes  Présentation  Parcours en largeur d’abord  Parcours en profondeur d’abord  

#### Chaînes et Chemins


Soit G = (S, E ) un graphe.  
Un chemin d’un sommet x à un sommet y est une séquence de (au  moins 2) sommets x = x, x . . . , xn−, xn = y dans laquelle chaque xi  admet xi pour voisin.  Un sommet y est accessible depuis x s’il existe un chemin de x à y .  La longueur d’un chemin est égale au nombre d’arêtes qui la  constituent.  Un chemin simple est une chemin qui ne contient pas plusieurs fois  une même arête/arc (on dit aussi eulérien).  Un chemin élémentaire est une chemin qui ne passe pas plusieurs fois  par un même sommet.  élémentaire =⇒ simple.  En CPGE, les chaînes sont souvent élémentaires (pas de doublon de  sommet sauf pour définir les cycles).  Certains auteurs utilisent le mot chaîne pour désigner les chemins  dans les graphes non orientés.  

#### Cycles et circuits


Un chemin est dit simple ou eulérien si chacun de ses arcs/arêtes  n’est emprunté qu’une fois.  Un cycle x, x, . . . , xn est un chemin eulérien dont les extrémités sont  confondues.  
Un cycle est dit élémentaire si, lorsqu’on enlève un arc quelconque, le  chemin restant est élémentaire.  
Un graphe est acyclique s’il ne possède aucun cycle.  
Certains auteurs distinguent la notion de circuit (pour les graphes  orientés) de celle de cycle (pour les graphes non orientés)  

#### Distance


La distance entre deux sommets x et y d’un graphe G = (S, A)  orienté (resp. non orienté) est notée dG (x, y ) et est égale à la  longueur d’un plus court chemin (resp. chaîne) allant de x à y s’il en  existe un ou bien +∞ sinon.  
Il s’agit bien d’une distance au sens mathématiques. En particulier,  elle vérifie l’inégalité triangulaire  ∀(x, y , z) ∈ S , dG (x, z) ≤ dG (x, y ) + dG (y , z).  Le diamètre d’un graphe G est la valeur : supx,y ∈S  dG (x, y ). C’est  "la longueur du plus long plus court chemin entre deux sommets".  

####  Historique


 Graphes, représentation, sous-graphes  
 Chaînes et chemins, connexité  
Accessibilité  Connexité  
 Graphes particuliers  
Arbres et forêts  Graphes non orientés particuliers  
 Un peu de OCAML  
 Parcours de graphes  Présentation  Parcours en largeur d’abord  Parcours en profondeur d’abord  

#### Relation de connexité


La connexité dans un graphe non orienté est une relation binaire entre  deux sommets : x et y sont en relation de connexité si et seulement si  y est accessible depuis x.  Comme le graphe est non orienté, si y est accessible depuis x, alors x  est accessible depuis y .  La connexité est une relation d’équivalence.  Les classes d’équivalences sont appelées composantes connexes. La  composante connexe d’un sommet x est notée ici ˙x et vaut :  
˙x = {y ∈ V |  
il existe une chaîne de x à y } .  
Un graphe est dit connexe si il possède une seule composante  connexe.  La connexité est étendue aux graphes orientés en ne tenant pas  compte du sens des arcs.  

#### Relation de forte connexité


```linenums="1"
Il peut y avoir un chemin de à sans chemin de à .
L’inclusion réciproque est en général fausse.
```

La relation de forte connexité est une relation binaire entre sommets  d’un graphe orienté : x et y sont en relation de forte connexité si et  seulement si  
il existe un chemin de x à y et il existe un chemin de y à x,  ou bien x = y .  
x y y xLes classes d’équivalence de la relation de forte connexité sont  appelées composantes fortement connexes.  La composante fortement connexe de x, notée ici ˜x vaut :  
˜x = {y ∈ S | il existe un chemin de x à y et de y à x} .  
Elle vérifie ˜x ⊂ ˙x. On dit qu’un graphe est fortement connexe si et seulement si il est  constitué d’une seule composante fortement connexe, c’est à dire si  pour tout couple de sommet (x, y ) il existe un chemin allant de x à y  et réciproquement.  

#### Connexité : exemple


S  
(cid:99)S  
S  
S  
S  
S  
S  
S  
S  
Graphe connexe (quand on ne considère pas le sens des ﬂèches).  S est accessible depuis tous les sommets.  ˜S = {S}. Donc le graphe n’est pas fortement connexe, sinon ˜S  contiendrait tous les sommets.  Sommets accessibles depuis S : {S, S, S, S, S}.  Sommets coaccessibles depuis S : {S, S, S, S, S, S}.  ˜S = {S, S, S, S} est l’intersection des accessibles et des  coaccessibles.  

#### Isthme


Une arête u d’un graphe G non orienté est appelée un isthme si sa  suppression augmente le nombre de composante connexes du graphe.  
S  
S  
S  
S  
S  
S  
S  
Une seule composante connexe.  


S  
S  
S  
S  
S  
S  
S  
Deux composantes connexes après suppression de {S, S}.  


!!! quote "Définition"
    Proposition


Soit G un graphe non orienté.  Une arête u est un isthme si et seulement si u n’appartient à aucun cycle  de G .  

#### Preuve


Soit u = {x, y } une arête avec x (cid:54)= y . On montre que u est un isthme si et  seulement si u n’appartient à aucun cycle de G  
Supposons que u soit un isthme. Si il y a un cycle passant par u, alors  il y a un cycle allant de y à y passant par x et donc deux façons de  joindre y à x.  Supprimer u, laisserait alors x et y dans la même composante  connexe. u ne serait pas un isthme. ABSURDE.  
Si u n’appartient à aucun cycle, supposons qu’il y ait un chemin  allant de x à y ne passant pas par u. En y ajoutant u, on obtient un  cycle passant par u : ABSURDE.  Si on supprime u, on ne peut donc plus joindre y depuis x et donc on  augmente le nombre des composantes connexes =⇒ isthme.  

#### Nombre d’arêtes et de sommets


!!! quote "Définition"
    Proposition


!!! quote "Définition"
    Corollaire
Si G non orienté sans boucle est acyclique connexe, alors p = n − 1.


Soit G un graphe non orienté sans boucle de n sommets et p arêtes.  
G connexe =⇒ p ≥ n − 1,  
G acyclique (i.e. pas de cycle simple) =⇒ p ≤ n − 1.  

#### Preuve : si G = (V , A) NO est connexe, p ≥ n − 1.


Par récurrence forte :  
Vrai si n = 1. Alors p ≥ 0. Le graphe est connexe et p ≥ n − 1.  
Si n = 2, il faut qu’il y ait une arête entre les deux sommets pour que  le graphe soit connexe. Alors p ≥ 1 = n − 1.  
Cas de base : OK. (Remarque on pourrait ajouter des boucles ça ne  changerait rien).  


Par récurrence forte :  
n = 1, 2 : OK  Si P(k) pour n ≥ 2 et tout k ≤ n. Soit G connexe à n + 1 sommets.  Tout sommet possède au moins une arête incidente car G est  connexe.  
Si G possède un sommet x de degré d(x) = 1, x n’est sur aucune  chaîne simple joignant deux autres sommets. On supprime x et son  unique arrête adjacente, le sous-graphe G' obtenu est connexe à n  sommets. Par HR le nombre d’arêtes de G' est p(cid:48) ≥ n − 1. En  remettant l’arête de x, on a au moins (n + 1) − 1 arêtes dans G .  Sinon, tous les degrés sont ≥ 2. La somme des degrés dans un graphe  est {sum symbol}  x∈V d(x) = 2p car toutes les arêtes sont comptées deux fois. On  a donc  
2p =  
{sum symbol}  
x∈V  
≥ 2 |V | = 2n + 2  
d(x)    ≥  
Donc p ≥ n + 1 ≥ (n + 1) − 1. OK  


Si G NO sans boucle a au moins n arêtes, il n’est pas  acyclique  
On considère des graphes NO à au moins 3 sommets. On raisonne par  récurrence forte sur |G | = n.  Cas de base n = 3. S’il y a 3 arêtes, le graphe tout entier est un cycle.  

#### Si G NO a au moins n arêtes, il n’est pas acyclique


On considère des graphes NO à au moins 3 sommets. On raisonne par  récurrence forte sur |G | = n.  Supposons P(k) pour k ≥ 3 et tout k ≤ n. Soit G à n + 1 sommets et  p = n + 1 arêtes. On montre qu’il possède un cycle. Considérons un  sommet quelconque x.  
S’il n’y a pas d’arête incidente à x, le graphe privé de x a n sommets  et n + 1 arêtes. Il y a un cycle par HR.  
Si il existe une arête incidente à x qui n’est pas un isthme elle est  alors sur un cycle et G possède donc un cycle : OK.  

#### Preuve : si G NO a au moins n arêtes, il n’est pas acyclique


On considère des graphes NO à au moins 3 sommets. On raisonne par  récurrence forte sur |G | = n.  Supposons P(k) pour k ≥ 3 et tout k ≤ n. Soit G à n + 1 sommets et  p = n + 1 arêtes. On montre qu’il possède un cycle. Considérons un  sommet quelconque x.  
Si toutes arête x-incidente est un isthme, soit u = {x, y } ∈ A.  Retirons u. Alors x se retrouve dans une composante connexe  diﬀérente de celle de y .  Séparons la composante connexe de x et ses arêtes (formant un sous  graphe G) du reste du graphe (notons G ce reste).  
G possède , disons k sommets (1 ≤ k < n + 1), l’autre n + 1 − k. G  possède q arêtes et G en a q avec q + q = n.  Si q ≥ k, il y a un cycle dans G par HR donc dans G puisque G est  un sous-graphe de G : Prouvé.  Sinon, q = n − q > n − k donc q ≥ (n − k) + 1 et le sous-graphe G  par HR a un cycle donc G aussi. CQFD  

#### Caractérisation des arbres non enracinés


!!! quote "Définition"
    Définition
On appelle arbre non enraciné tout graphe non orienté sans boucle
acyclique et connexe.


!!! quote "Définition"
    Remarque
On dit en général arbre plutôt que arbre non enraciné mais cette appelation
amène des confusions avec la notion d’arbre définie inductivement.


!!! quote "Définition"
    Proposition


Soit un graphe non orienté sans boucle G de n sommets et p arêtes, les  aﬃrmations suivantes sont équivalentes :  
 G est un arbre non enraciné,  
 G est acyclique et contient n − 1 arêtes (p = n − 1),  
 G est connexe et contient p + 1 sommets.  
On a déjà vu les sens directs.  

#### Preuve


Si G acyclique et n = p − 1  
Si x, y sont deux éléments non reliés par un chemin, on ajoute l’arête  {x, y }  
Cela crée un cycle puisque le nombre d’arête est égal à celui des  sommets.  
Ce nouveau cycle passe par l’arête {x, y } (avant, il n’y en avait pas).  
Puisque cycle il y a, c’est que x et y sont joignables sans passer par  {x, y } : Absurde.  


Si G est connexe et n = p − 1  
Si G possède un cycle, soit {x, y } une arête de ce cycle.  
Alors il y a un autre chemin de x à y que cette arête. Donc on peut  enlever l’arête {x, y } en conservant le caracètre connexe.  Mais alors le nouveau graphe G' est encore connexe et possède n  sommets et n − 2 arêtes. ABSURDE  

##  

###  

#### Arbres et forêts


Pour certains auteurs, un arbre est un graphe non orienté connexe et  acyclique. S’il a n sommets, il possède donc n − 1 arêtes.  
Comme il y a conﬂit avec la définition du cours, ces graphes non  orientés connexes acycliques sont dits arbres non enracinés (on l’a  déjà vu).  `A contrario, on parle des objets du premier chapitre (définis  inductivement) comme des arbres.  
Dans un arbre non enraciné, on peut choisir une racine. Il y a alors un  chemin unique de la racine à tous les sommets (voir prop. 3.3). La  présence de la racine induit alors une orientation.  La structure ainsi construite est ce que certains auteurs appellent  arborescence ou arbre enraciné  

#### Forêts


Une forêt est un graphe non orienté acyclique, c’est une union disjointe  d’arbres non enracinés (qui en sont les composantes connexes).  

#### Exemples


```linenums="1"
Une forêt
```


Un arbre non enraciné  S  
S  
S  
S  
S  
Un autre  
X  
X  
X  
S  
S  

#### Racine, arborescence


!!! quote "Définition"
    Définition


Un sommet r d’un graphe orienté G = (S, E ) est une racine de G si  pour tout sommet x de G il existe un chemin de r à x.  
On dit qu’un graphe orienté G = (S, E ) est une arborescence s’il  possède un unique élément x de degré entrant nul, si tous les autres  sont de degré entrant 1 et si il existe un chemin de x à tous les  autres sommets.  

#### Exemples


S  
S  
S  
S  
S  
S  
S  
S  
S  
S  
S  
S  
Pas de racine  
Une arborescence (racine : S)  

###  

#### Statut de cette section


Cette section donne quelques exemples de graphes particuliers sans  qu’aucune preuve ne soit donnée.  

#### ´Etoiles, peignes, chenilles
<p align='center'><img src='/images/1f82360efa419b92666109593eee8c4d.bmp'/></p><p align='center'><img src='/images/21598397633b4a64bfa72795022014e5.bmp'/></p>

´Etoile               
Chenille         ≥            ≥   
Peigne                        

#### Graphe planaire
<p align='center'><img src='/images/598390c394c9d54c0b8f1bed3302516a.bmp'/></p>

Un graphe est planaire  si on peut le dessiner sans  qu’aucune arête n’en coupe  une autre.  4-coloriabilité :  les sommets d’un graphe planaire  peuvent être coloriés  avec 4 couleurs sans  que deux sommets adjacents  ne soient de la même couleur.  

#### Graphe complet, tournoi


Un graphe complet est un graphe  non orienté où  tous les sommets sont deux à deux adjacents.  Un tournoi est un graphe orienté  obtenu à partir d’un graphe complet  en orientant chaque arête.  

#### Graphe biparti
<p align='center'><img src='/images/bed36094a5610d0a25130a62ba143350.bmp'/></p>

Un graphe biparti G = (S, A)  est un graphe non orienté  admettant une partition {P, P}  de ses sommets telle que  {x, y } ∈ A =⇒ (x, y ) ∈ P × P ∪ P × P  Les arbres (et plus généralement les forêts)  sont des graphes bipartis.  

#### Graphe biparti complet
<p align='center'><img src='/images/3cb1e408033346b06bd97000d16d069e.bmp'/></p>

_Figure – Exemples de graphes bipartis complets_

Un graphe est dit biparti complet (ou encore est appelé une biclique) s’il  est biparti et contient le nombre maximal d’arêtes.  Si U est de cardinal m et V est de cardinal n le graphe biparti complet est  noté Km,n.  

##    OCAML

###    OCAML

#### Liste d’adjacence


```linenums="1"
type graphe = int list array ;;
(* graphe orient é *)
let g1 = [| [1; 2]; [2] ;[ 0]|];;
(* graphe non orient é *)
let g2 = [ |[ 1 ; 2 ]; [ 2 ; 0 ] ; [0 ;1 ] |] ; ;
```

  
  
  
  

Les sommets sont numérotés de 0 à |g | − 1.  
0  
g  
2  
1  
0  
g  
2  
1  
Voir TD pour les exercices  
  

#### Matrice d’adjacence


```linenums="1"
type graphe = int array array ;;
let g = Array . make_matrix 4 4 0;;
g .(0) .(1) < -1; g .(0) .(2) < -1; g .(1) .(3) < -1; g .(2) .(1) < -1;;
```

  
  
  


(cid:47) 3  
0  
1  
2  
Voir TD pour les exercices  
  

##   

####  Historique


 Graphes, représentation, sous-graphes  
 Chaînes et chemins, connexité  
Accessibilité  Connexité  
 Graphes particuliers  
Arbres et forêts  Graphes non orientés particuliers  
 Un peu de OCAML  
 Parcours de graphes  Présentation  Parcours en largeur d’abord  Parcours en profondeur d’abord  

#### Définition


En théorie des graphes, un parcours de graphe est un algorithme  consistant à explorer les sommets d’un graphe de proche en proche à  partir d’un sommet initial. Un cas particulier important est le parcours  d’arbre.  Un parcours d’un graphe permet de choisir, à partir des sommets  visités, le sommet suivant à visiter.  Le problème consiste à déterminer un ordre sur les visites des  sommets.  Une fois le choix fait, l’ordre des visites induit une numérotation des  sommets visités (l’ordre de leur découverte) et un choix sur l’arc ou  l’arête utilisé pour atteindre un nouveau sommet à partir des sommets  déjà visités.  Les arcs ou arêtes distingués forment une arborescence ou un arbre, et  les numéros des sommets sont croissants sur les chemins de  l’arborescence ou les chaînes de l’arbre depuis la racine.  

#### Finalité


Les algorithmes de parcours ne sont pas une fin en eux-mêmes. Ils servent  comme outil pour étudier une propriété globale du graphe, par exemple :  
Connexité et forte connexité  
Existence d’un circuit ou d’un cycle (ce qu’on appelle tri topologique)  
Calcul des plus courts chemins (notamment l’algorithme de Dijkstra)  
Calcul d’un arbre recouvrant (notamment l’algorithme de Prim)  
Algorithmes pour les ﬂots maximums (comme l’algorithme de  Ford-Fulkerson).  

#### Analyse


La diﬃculté de l’exploration consiste à éviter de visiter plusieurs fois  un même sommet. Pour cela on met en oeuvre un marquage des  sommets par des couleurs.  Lors d’une exploration, chaque sommet passe par trois couleurs :  
bleu tant que la visite du sommet n’a pas commencée  vert dès que sa visite commence et tant le traitement n’est pas terminé  rouge dès que le traitement est terminé  
L’exploration à partir d’un sommet s ne permet pas nécessairement  d’explorer tout le graphe (il peut y avoir plusieurs composantes  connexes). Pour eﬀectuer une exploration complète il faut relancer le  parcours à partir d’un sommet bleu tant qu’il en existe.  

#### Parcours de tous les sommets


```linenums="1"
t a n t q u e                
f a i r e
```

On gère trois listes (ou piles ou files) Rouges (déjà traités), Verts  (traitement en cours), Bleus (à traiter). Colorer un sommet consiste à  l’ajouter à une de ces listes/piles/files et à le retirer des autres.  
  
  
  
  
                 
    
  

         s      ∗                    s  ∗          
    
                       s ∗                   ∗  
Dès qu’un sommet bleu est abordé, il devient vert. Suivant les traitements,  on peut choisir de le rendre rouge avant ses successeurs ou après.  
  

#### Parcours à partir d’un sommet s0


```linenums="1"
p r o c e d u r e          
t a n t q u e
f a i r e
f i n f a i r e
```

  
  
  
  
  
  
  
  
  

 s  ∗                 s ∗  
               
             s      ∗              ∗  
                  

  s ∈      ∗              ∗  
        ∗                     t  s ∗           O∗          ∗   
                       
∗              ∗  
s           
       ∗  
t        
        
            ∗  

On peut choisir de traiter le sommet s après ses successeurs. Idéalement,  les tests sont tous en O(1), les colorations ainsi que le choix d’un sommet  vert aussi. Le traitement est plus ou moins coûteux (souvent O(1) mais  pas toujours).  
  

#### Graphe de liaison induit


Soit G = (S, A) un graphe et s ∈ S. On appelle graphe de liaison  induit par l’exploration de G à partir de x, le sous-graphe de G  engendré par les arêtes {u, v } ∈ A (resp. les arcs) par lesquelles  passent l’exploration de G , (l’exploration passe par {u, v } (resp.  (u, v )) si celle-ci provoque le coloriage du sommet v en vert).  Pour un parcours depuis s :  
on débute avec le graphe (s, ∅)  lors du traitement d’un sommet s vert on ajoute chaque voisin bleu t  et l’arête (resp. arc) {s, t} (resp. (s, t)) au graphe induit.  On construit ainsi un graphe connexe ayant k sommets et k − 1 arêtes,  autrement dit un arbre ou une arborescence.  
Le graphe de liaison induit par une exploration complète de G est un  ensemble d’arbre ou d’arborescence.  

#### Tableau de couleurs


On colorie tous les sommets en bleu (O(n)) puis on lance  l’exploration de n’importe quel sommet.  
Lors d’un parcours, chaque sommet entre au plus une fois dans  l’accumulateur Verts , et n’en sort qu’au plus une fois (quand il  devient rouge).  
On suppose que ces opérations d’entrée et de sortie dans la liste  soient de coût constant. Pour réaliser cette condition, la solution que  nous adopterons consistera à utiliser un tableau de couleurs R,V,B  pour connaître en temps constant le statut d’un sommet.  

###   

#### Algorithme


L’ensemble des sommets Verts est représenté par une file  (bibliothèque OCAML queue par exemple)  Principe : on explore le graphe à partir d’un sommet en visitant  d’abord tous les sommets voisins (à une distance 1), puis tous les  sommets voisins de ses voisins (à une distance 2)....  


```linenums="1"
p r o c e d u r e                
t a n t que      
dans 
f a i r e
p o u r                   
s i         a l o r s
dans 
p r o c e d u r e                      
a l o r s
s i
```


         
       
          
 

         

               
          
                

          
                      

  
  
  
  
  
  
  
  
  
  
  
  
  
         
     
    
       
          
               
             ∗         
      ∗  
     
  

#### Propriétés du parcours en largeur d’abord


s est le premier sommet rouge. Un sommet devient rouge avant ses  sucesseurs dans l’ordre de parcours.  Un sommet rouge n’a que des sommets adjacents verts ou rouges  Si un sommet x est rouge alors il existe une chaîne/un chemin allant de  s à x constituée uniquement de sommets rouge.  A la fin du parcours tous les sommets sont soit bleus, soit rouges (et la  file des verts est vide).  
Conséquence : à la fin de l’appel de Largeur les sommets rouges sont  tous les sommets accessibles à partir de s.  


Voir cette animation  

####  Historique


 Graphes, représentation, sous-graphes  
 Chaînes et chemins, connexité  
Accessibilité  Connexité  
 Graphes particuliers  
Arbres et forêts  Graphes non orientés particuliers  
 Un peu de OCAML  
 Parcours de graphes  Présentation  Parcours en largeur d’abord  Parcours en profondeur d’abord  

#### Présentation


Principe : on explore le graphe à partir d’un sommet x en visitant l’un  de ses sommets successeurs y et en poursuivant l’exploration d’abord  par les successeurs de ce dernier avant les autres successeurs de x.  
Ainsi l’exploration s’eﬀectue en suivant le plus loin possible une  chaîne issue de x. Lorsque tous les successeurs d’un sommet ont été  visités, on continu l’exploration en remontant dans la chaîne au  premier sommet ayant encore des successeurs non visités.  
On gère une pile des sommets verts (bibliothèque OCAML listes ou  stack, python : listes ou classe deque)  

#### Algorithme


```linenums="1"
p r o c e d u r e                   
t a n t que   
f a i r e
dans  
```

On utilise une pile pour gérer les sommets verts.  

  
         
    ∗                              
                                               
           
          ∗  
      
                         
  
  
  
  
  
  
  
  
  
  
         
                 ∗           
       

       
    
      ∗  
             

  
  
  
  
                                                        
                 
        
   
     
  

#### En pratique


Un même nœud s apparaît plusieurs fois au sommet de la pile. Il faut  donc gérer un marqueur de progression dans sa liste de voisins pour  éviter de reprendre cette liste depuis le début à chaque passage de s  au sommet de la pile.  
Solution : considérer les listes de voisins comme des piles. On dépile  jusqu’à trouver un sommet bleu. Les voisins dépilés ne reviennent  jamais dans la pile de voisins.  Cela impose de faire une copie du graphe (O(n) si le graphe est un  tableau de listes de voisins.)  

#### Nombre d’opérations due à un sommet


On analyse le nombre d’opérations engendrées par la présence d’un  sommet donné s en haut de la pile. Il suﬃra de sommer sur tous les  sommets pour obtenir la complexité totale. (Ajouter aussi le coût  d’initialisation en O(n).  
Le sommet s entre toujours dans la pile (soit du fait de la boucle tant  que, soit du fait de Profondeur). Le coloriage assure que le sommet  ne revient pas dans la pile verte quand il l’a quittée.  Le sommet s apparaît au plus deg s  + 1 fois au sommet de la pile  (s’en convaincre avec le graphe x → y ).  A chaque apparition au sommet de la pile verte, il y a une recherche  d’un voisin bleu (qui peut être faite en O(1) si pile de voisins) un  coloriage/empilement dans la pile verte parfois (au plus deg s  fois).  s est dépilé puis colorié en rouge une seule fois.  
La présence de s en haut de la pile engendre donc un nombre  d’opérations (majoré par un nombre) proportionnel à son nombre de  présences au sommet, soit 1 + deg s .  

#### Complexité totale


Pour un graphe G = (V , E ) avec |E | = p et |V | = n  
On somme les nombres d’opérations occasionnées par chaque sommet  pour obtenir la complexité totale.  
Le nombre total d’opération est (majoré par un nombre) proportionnel  à  
{sum symbol}  
s∈V 1 + deg s = n + {sum symbol}  
i∈V deg s = n + p = O(n + p)  
Même si on ajoute le coût d’initialisation en O(n), l’ensemble reste en  O(n + p)  

#### Observations


Les invariants suivants sont maintenus  
A la fin d’un passage dans la boucle tant que : un sommet est vert  si et seulement s’il est dans la pile  
A tout moment, la pile décrit un chemin entre s (la base) et le haut  de la pile.  
Lorsqu’on colorie un sommet x en vert, tous les sommets bleus  accessibles à partir de x seront coloriés en rouges avant que x ne le  soit.  
A la fin du parcours, l’ensemble des sommets rouges est l’ensemble  des sommets accessibles à partir de s.  


Voir cette animation  

#### Vocabulaire


S  
S  
(cid:47) S  
S  
(cid:47) S  
(cid:47) S  
S  
S  
S  
S  
Soit Gl = (V , L) le graphe de liaison induit par le parcours en  profondeur d’un graphe G . Un arc u → v est :  un arc de liaison si et seulement si u → v ∈ L  un arc retour si et seulement si u est un descendant de v dans L.  un arc avant si et seulement si v est un descendant de u dans L et  u → v /∈ L (on prend de l’avance par rapport au cheminement normal  dans L)  un arc couvrant sinon (tous les autres arcs).  

#### Variante


Certains auteurs, gèrent le parcours en profondeur comme le parcours  en largeur mais utilisent juste une pile et non une file.  
Dans ce type de parcours, un même sommet n’apparaît qu’une seule  fois au sommet de la pile verte.  
Dès qu’un sommet apparaît au sommet de la pile, il est dépilé (une  fois pour toute) et on ajoute en une seule fois tous ses voisins bleus  au sommet de la pile. Un sommet devient alors rouge avant ses  successeurs.  
L’écriture du programme est alors plus simple, puisqu’une même liste  de voisins n’est parcourue qu’une fois pour toute (alors qu’avec ma  version, on la parcourt "par bouts" en y revenant plusieurs fois).  
En revanche, on perd une propriété importante : la pile ne décrit plus  exactement un chemin de la base au sommet mais est un simple  accumulateur de sommets rencontrés.  


```linenums="1"
t a n t que      
f a i r e
p o u r                   
s i         a l o r s
dans 
```

          
        
                     
         
       
                      

  
  
  
  
  
  
  
  
          ∗            
        ∗  
∗      
    
                        
    ∗  
               
                     
               ∗          
    
       ∗  
A noter qu’il n’y a plus vraiment de raison d’utiliser trois couleurs ; deux  suﬃsent.  
     
  
