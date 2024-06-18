# Tests

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"
    - Sur la nuance entre _erreur_ et _défaut_, cet [article](https://latavernedutesteur.fr/2020/11/10/difference-entre-erreur-defaut-defaillance-et-anomalie-cynthia-lefevre/) de Cynthia Lefevre.  
    - Ce cours du [Labri](https://dept-info.labri.fr/~felix/Annee2008-09/S4/McInfo4_ASR%20Tests/1.pdf).  
    - Cet article de [Wikipedia](https://fr.wikipedia.org/wiki/Test_(informatique)).  
    - Ce cours du [LRI de Paris-Saclay](https://www.lri.fr/~longuet/Enseignements/16-17/L3-GLA/L3-TestStructurel1.pdf)
    - Un cours du [CNAM](https://cedric.cnam.fr/~taktaks/GLG101/teststructurel.pdf) avec des exemples de normes réclamant les  critères étudiés.  

## Présentation

### Terminologie

!!! quote "Définition: Erreur"

    Action humaine produisant un résultat incorrect $\color{red}\text{"l'erreur est humaine"}$

!!! quote "Définition: Défaut"

    Une imperfection dans un composant (ou un système) qui peut conduire à ce que ce composant (ou système) n'exécute pas les fonctions requises. $\color{red}\text{On cherche les défauts sans manipuler le système}$.

!!! quote "Définition: Défaillance ou Panne"

    $\text{IEEE 729}$ : La fin de la capacité d'un système ou d'un de ses composants d'effectuer la fonction requise, ou de l'effectuer à l'intérieur de limites spécifiées. $\color{red}\text{On cherche une défaillance en manipulant le système}$.

!!! quote "Définition : Anolamie"

    Un écart entre le résultat attendu et celui obtenu.

!!! tip "Remarque"

    Ce n'est pas parce qu'il y a anomalie qu'il y a défaut. Peut-être le testeur a-t-il compris la spécification d'une façon et le développeur d'une autre.

### Défaut VS Défaillance

Il y a un rapport de causalité.  

- **A retenir** : Un défaut (dans le code) ou une erreur (de l'utilisateur du  programme) peut causer (un jour) une défaillance.  
- On peut trouver un défaut sans manipuler le système (analyse du  code).  
- Mais on trouve une défaillance en manipulant le système.  

### Test

Définition (norme $\text{IEEE 729}$ ) : Le _test_ est un processus manuel ou automatique, qui vise à établir qu'un système vérifie les propriétés  exigées par sa spécification, ou à détecter des diﬀérences entre les  résultats engendrés par le système et ceux qui sont attendus par la  spécification.  

Le test ne se préoccupe pas des erreurs. Ce n'est pas au testeur de pointer les imperfections humaines.  

Le test met en évidence les défauts afin de prévenir les défaillances.  

### Tests en général

Toute fabrication de produit suit les étapes suivantes  

- Conception
- Réalisation
- Tests  

Test : On s'assure que le produit final correspond à ce qui a été  demandé selon divers critères :  

- esthétique
- performance
- ergonomie
- fonctionnalité
- robustesse etc.  

!!!example ""
    **Des Bogues aux conséquences désastreuses**

    - **Suède - $1980$** : 
    $776$ millions d'euros ($275$ millions de dollars de $1980$).
    $104$ camions semi-remorques de $35$ tonnes, c'est la cargaison
    que le Zenobia emporta par le fond lors de son naufrage, $1$
    mois seulement après son lancement inaugural.
    Pourquoi ? Un bug dans le logiciel de contrôle des ballasts
    surremplissait les réservoirs par rapport aux indications qui
    lui étaient envoyées.

    - **EADS - $1996$** :
    $266$ millions d'euros ($200$ millions d'euros de $1996$)
    Le $4$ juin $1996$ ($1$er vol), la fusée Ariane $5$ s'est élevée
    jusqu'à la moitié du ciel, avant que son logiciel ne coupe les
    moteurs et que le lanceur explose en plein vol.
    Pourquoi ? L'accélération maximale de la fusée a dépassé la
    valeur admissible par le logiciel, générant une erreur générale
    et donc l'autodestruction. Valeurs recopiées du programme
    Ariane $4$ (moins puissante). La simulation avait été annulée
    afin d'économiser $800.000$ francs ($119000$ euros).

### Coût des tests

- Budget IT : Budget alloué aux technologies de l'information (ou IT pour Information Technology).
- Dans les entreprises, la part du budget IT consacrée aux tests et à l'assurance qualité augmente de $9$ points d'une année sur l'autre selon Capgemini.
- De $26$% en $2014$, elle passe à $35$ % en $2015$.
- Il était prévu qu'en $2018$, la part des tests et de la qualité passe à $40$ % du total. (Source [Silicon](https://www.silicon.fr))

### VVT

**$V$alidation, $V$érification et $T$est logiciel.**

Démonstration automatique (Hum !) : exhaustive mais considérée comme trop coûteuse.  

Model Checking : vérifier si le modèle d'un système satisfait une  propriété. Par exemple, on souhaite vérifier qu'un programme ne se  bloque pas, qu'une variable n'est jamais nulle, etc. Généralement, la  propriété est écrite dans un langage, souvent en _logique temporelle_.  La vérification est généralement faite de manière automatique.  

Test : non exhaustif mais facile à mettre en œuvre (bon rapport qualité/temps).  

### Cycle en V

Le test commence dès le début !  

<p align='center'><img src='/images/a1696852da3de0356121d3245c0da149.bmp'/></p>

_Figure – Le cycle en V du Génie Logiciel (d'après [Irif](https://www.irif.fr/~eleph/Enseignement/2010-11/CoursTests.pdf))_

### Classification

#### Selon le niveau au cycle de développement

- Tests de recette : test de réception du logiciel chez le client final  
- Tests intégration système : test de l'intégration du logiciel avec  d'autres logiciels  
- Tests systeme : test d'acception du logiciel avant livraison (nouvelle  version par exemple). Généralement eﬀectué par le client dans ses  locaux après installation du système ou d'une unité fonctionnelle, avec  la participation du fournisseur.  
- Tests Integration : test de l'intégration des diﬀérents composants  (avec ou sans hardware)  
- $\color{red}\text{Tests Unitaires : tests élémentaires des composants logiciels (une fonction, un module, ...)  }$

#### Selon la caractéristique

- $\color{red}\text{Tests fonctionnels : destinés à s'assurer que, dans le contexte d'utilisation}$ $\color{red}\text{réelle, le comportement fonctionnel obtenu est bien  conforme avec celui attendu.}$
- Tests de performance (rapidité, consommation mémoire etc.)  
- Tests de robustesse (mon serveur va-t-il bien réagir à un aﬄux de  connexion ?)  
- Tests de vulnérabilité : suis-je bien protégé face aux risques  d'intrusions ?  

#### Selon le niveau d'accessibilité

- Tests de type boîte noire : technique de conception de test,  fonctionnel ou non, qui n'est pas fondée sur l'analyse de la structure  interne du composant ou du système mais sur la définition du  composant ou du système.  
- Tests de type boîte blanche : technique de conception de test, en  général fonctionnel, fondée sur l'analyse de la structure interne du  composant ou du système.  

Pour [comparer](https://testingbasicinterviewquestions.blogspot.com/search/label/Black%20Box%20vs%20White%20Box%20Testing) les deux.

### Divers

Tests de non-régression : à la suite de la modification d'un logiciel (ou  d'un de ses constituants), un test de régression a pour but de montrer que  les autres parties du logiciel n'ont pas été aﬀectées par cette modification.  

### En CPGE

- Tests unitaires en général boîte blanche et fonctionnels.  
- Moyen : pour tout exercice, créer un fichier ou une fonction de tests.  
- Rendu de projets : les tests sont impératifs.  

### Copmlémentarité des tests fonctionnels et structurels

En examinant ce qui a été réalisé, on ne prend pas forcément en compte ce qui aurait du être fait

- Les approches structurelles (boîte blanche) détectent plus facilement les erreurs commises
- Les approches fonctionnelles (boîte noire) détectent plus facilement les erreurs d'omission

## Partitionnement, limite

### Partitionnement du domaine d'entrée

- Il s'agit de répartir les données en un nombre fini de classes  d'équivalence, sans restreindre le degré d'exigence. Cette méthode est  généralement utilisée pour réduire le nombre total de cas de test à un  ensemble fini.  
- Exemple : test d'une zone de saisie attendant des nombres de $1$ à $1000$.  
  - Classe des données valides d'entrée : sélectionner un (ou quelques)  nombre.s dans $[\![ 2, 99]\!]$ (exemple : $50$ et $800$).  
  - Classe des données inférieure à $0$ : sélectionner un ou plusieurs nombres  négatifs ($-1$ et $-100$ par exemple).  
  - Classe des données supérieures à $1001$ : choisir $1050$ et $2300$ par  exemple.  

### Tests aux limites

#### Test d'une zone de saisie attendant des nombres de 1 à 1000

En complément du partitionnement du domaine d'entrée, on ajoute les  _tests aux limites_

- Ce sont les données encore valides mais au delà desquelles les entrées  ne le sont plus.  
- Il faut impérativement tester $1$ et $1000$. On peut y ajouter les valeurs  
    - juste en dessous des bornes : $0$ et $999$  
    - juste au-dessus des bornes : $2$ et $1001$  
- Si un des tests ne donne pas le résutat attendu, le programme n'est  pas correct. En revanche, le programme peut être incorrect même si  tous les tests du scénario donnent satisfaction.  

## Graphe de flots de contrôle

### Rappel : Tests unitaires ; tests fonctionnels

En CPGE on s'intéresse uniquement aux $\color{red}\text{Test Unitaires : tests élémentaires des composants logiciels (une fonction, un module, ...)}$;

La plupart des tests que nous avons écrits jusqu'à présent étaient des $\color{red}\text{Tests fonctionnels ("boîte noire") : destinés à s'assurer que, dans le contexte d'utilisation}$ $\color{red}\text{réelle, le comportement fonctionnel obtenu est bien conforme avec celui attendu}$.
On crée des tests sans ce soucier du code écrit.

### Tests structurels
_dits aussi tests boîte blanche_

- Sélection de tests à partir de l'analyse du code source du système  
- Constructions des tests uniquement pour du code déjà écrit.  

<p align='center'><img src='/images/tests1.png'/></p>

### Test structurel

- Utiliser la structure du code pour dériver des cas de tests.  
- Complémentaire des tests fonctionnels car on étudie la réalisation et  pas seulement la spécification.  
- Il y a deux méthodes :  
    - $\color{red}\text{À partir du }\textit{graphe de ﬂot de contrôle}$ $\color{red}\text{ : couverture de toutes les  instructions, tous les branchements...}$ (CPGE)  
    - À partir du _graphe de ﬂot de données_ : couverture de toutes les  utilisations d'une variable, de tous les couples définition-utilisation...  

### Graphe de ﬂot de contrôle

Graphe simple orienté connexe avec un sommet initial (ou d'entrée) `E` et un sommet de sortie `S` accessible depuis `E`.

- Un sommet interne est  
    - Soit un bloc d'instructions élémentaires (on regroupe les séquences).  
    - Soit un sommet de décision étiqueté par un test.  
- Il y a deux sortes d'arcs :  
    - Les arcs de sortie de blocs d'instructions : ils représentent le passage  d'un bloc d'instructions à une instruction conditionnelle ou à un autre  bloc d'instruction. Il ne sont pas étiquetés.  
    - Les arcs de décision : ce sont les arcs de sortie des sommets de décision.  Ils sont étiquetés par les réponses aux tests des expressions  conditionnelles.  

Il y a un seul arc de sortie d'un sommet bloc d'instructions. Et, le plus  souvent, deux arcs de sortie (dits _arcs de décision_) des sommets de décision correspondant aux valeurs positives ou négatives des conditions.  

### Exemple : calcul de $X^n$

```C linenums="1"
float power (float x, int n){
    int p = n ; float s = 1.;
    while(p>=l){
        s=s*x;
        p=p-1;
    }
    return s;
}
```

<p align='center'><img src='/images/tests2.png'/></p>

### Exemple des triangles

```C linenums="1"
void triangle (int j, int k, int l) {
    // j, k, l : les 3 côtés d'un triangle
    // indique si le triangle est quelconque,
    // isocèle ou équilatéral
    int n = 0; // n pour 'nature du triangle
    if (j+k<=l || k+l <= j || l+j <= k) {
        printf("pas un triangle");
    } else {
        if (j==k) n = n+1;
        if (j==l) n = n+1;
        if (l==k) n = n+1;
        if (n==0)
            printf("quelconque");
        else if (n==1)
            printf("isocèle");
        else
            printf("équilatéral");
    }
}
```

<p align='center'><img src='/images/tests3.png'/></p>

_Avec $T$ pour **True** et $F$ pour **False**_

### Chemin faisable

Un _chemin de contrôle_ est une suite $x_0, x_1, . . . , x_n$ de sommets tels que $x_0 = E$ , $x_n = S$ et $(x_i , x_{i+1})$ est un arc pour tout $i < n$.  

Un _chemin_ est dit _faisable_ si il existe un ensemble de conditions sur les variables d'entrées permettant de passer par tous les sommets de ce chemin.

Si un chemin n'est pas faisable, il est dit _infaisable_. La recherche de infaisable est _indécidable_ : on ne sait pas en entrée un GFC quelconque, dire dans tous les cas s'il existe un chemin infaisable.

???example "Exemple des triangles"
    <p align='center'><img src='/images/tests4.png'/></p>
    _Figure - Chemin faisable_
    <p align='center'><img src='/images/tests5.png'/></p>
    _Figure - Chemin infaisable_

### Condition d'un chemin

La _condition d'un chemin_ est la conjonction de tous les sommets de décision portant sur les entrées que traverse ce chemin.  

Exemple : avec $j = 3, k = 5, l = 3$, la condition du chemin vert  parcouru est  

$$¬(j +k ≤ l ∨k +l ≤ j ∨j +l ≤ k)∧j \neq k ∧ j = l ∧ l \neq k ∧ n \neq 0 ∧ n = 1$$

La condition du chemin rouge est  

$$¬(j + k ≤ l ∨ k + l ≤ j ∨ j + l ≤ k) ∧ j \neq k ∧ j \neq l ∧ l \neq k ∧ n \neq 0 ∧ n \neq 1$$

Or  

$$¬(j + k ≤ l ∨ k + l ≤ j ∨ j + l ≤ k) ∧ j \neq k ∧ j \neq l ∧ l \neq k \Rightarrow n = 0$$

Et donc la condition du chemin rouge est  

$$¬(j +k ≤ l ∨k +l ≤ j ∨j +l ≤ k)∧j \neq k ∧j \neq l ∧l \neq k ∧ {\color{red}0\neq 0} ∧ 0 \neq 1$$

Cette formule est une _antilogie_ : le chemin est rouge est infaisable.  

### Suite de Syracuse

```C linenums="1"
void syracuse(int x) {
    // on ne considère que des entiers > 0
    while (x!=1){
        if (x%2==0)
            x=x/2;
        else
            x=3*x+1;
    }
    printf(" fini\n");
```

La conjecture veut que au bout d'un moment $x = 1$.  

<p align='center'><img src='/images/tests6.png'/></p>

On ne sait pas s'il existe des chemins infaisables de **$E$** à **$S$**.  

### Syracuse : La discipline de test (DT) $x=2$ sensibilise le chemin $C$

Etant donné un chemin faisable, on sait trouver les conditions pour le  parcourir.

<p align='center'><img src='/images/tests7.png'/></p>

- soit le chemin $C$ : $E$,  
$\begin{matrix}
 x & ≠ & 1 \\
 x\text{ \% } 2 & == & 0 \\
 x & = & \frac{x}{2} \\
 x & ≠ & 1, S
\end{matrix}$
  
- Quelle condition le vérifie ?  

- Celle-ci :
$\begin{matrix} x & ≠1 &  \\ x\text{ mod } 2 & =0 & \text{: x est pair}  \\ \frac{x}{2} & = 1 & \end{matrix}$

On trouve $x = 2$.  

### Critère de couverture

On appelle _critère de couverture_ une condition définissant un  ensemble de chemins du graphe de ﬂot de contrôle.  

Génération de tests grâce à un critère de couverture :  

- Sélectionner un ensemble minimal de chemins satisfaisant le critère.  
- Eliminer les chemins infaisables  
- Pour chaque chemin faisable, on définit un ensemble de conditions associées sur les entrées (c'est à dire un _objectif de test_). $\color{red}\text{Par exemple : entrée }x > 0$
- Pour chaque objectif de test (donc pour chaque chemin faisable) choisir un cas de test (donc des valeurs d'entrées satisfaisant la condition associée au chemin). $\color{red}\text{Par exemple : entrée }x = 3$

### Critère "tous les sommets"

Critère atteint lorsque tous les sommets du graphe de contrôle sont  parcourus.  

On définit le **taux de couverture des sommets** par  

$$ \tau_s = \frac{\text{nombre de sommets parcourus}}{\text{nombre de sommets}}$$

Exigence minimale pour la certification en aéronautique (norme [EUROCAE ED-12B](https://www.eurocae.net/media/1705/airborne-software-training-12-2020.pdf)) : $\tau_s = 1$.  

Qualification niveau **C** : Un défaut peut provoquer un problème _majeur_ entraînant un dysfonctionnement des équipements vitaux de l'appareil.  

```C linenums="1"
int somme (int x, int y){
    int s = 0;
    if (x==0) s=x;
    else s=x+y;
    return s;
}
```

Il y a deux chemins. L'un d'eux permet de détecter le défaut.  

<p align='center'><img src='/images/tests8.png'/></p>

Le chemin **$E$ ; $int$ $s=0$ ; $x==0$ ; $s=x$ ; $return$ $s$ ; $S$** est associée à la condition $x = 0$  satisfaite par les entrées  $x = 0$, $y = 6$. Cela permet de détecter le défaut.  

### Limite du critère "tous les sommets"

```C linenums="1"
float div(float x){
    float r;
    if (x!=0) 
        r=1.;
    r = 1/x;
    return r;
}
```

<p align='center'><img src='/images/tests9.png'/></p>

Le chemin **$E$ ; $int$  $r$ ; $x≠0$ ; $r=1$ ; $return$ $r$; $S$**  (associé à $x = 2$) couvre bien tous les sommets mais la division par zéro n'est pas détectée.  

### Critère "Tous les arcs"

Dit aussi critère "_toutes les décisions_".  

Pour chaque décision, un test rend la décision vraie, un autre la rend  fausse.  

Taux de courverture des arcs :

$$\tau_a = \frac{\text{nombre d'arcs parcourus}}{\text{nombre d'arcs}}$$  

[Norme DO 178B](https://www.verifysoft.com/fr_do-178b.html), qualification des systèmes embarqués au niveau B :  un défaut peut provoquer un problème _dangereux_

Critère "Tous-les-arcs" entraîne critère "Tous-les-sommets".(puisque le graphe est connexe, chaque sommet est adjacent à un arc au moins).

### Limite du critère tous les arcs


```C linenums="1"
float F(int a, int b){
    int r;
    if (a!=0 || a==b) 
        r=1/a;
    else 
        r=0;
    return r;
}

```

<p align='center'><img src='/images/tests10.png'/></p>

Critère Toutes-les-Décisions  satisfait avec **$\{a = 0,b = 1\}$ $et$ $\{a = 2, b = 1\}$**.  

Pourtant, division par zéro non détectée avec **$\{a = 0,b = 0\}$**.  

### Décomposer les conditionnelles

#### Toutes les conditions multiples

On a vu que le critère "tous les arcs" pouvait passer à côté  d'erreurs.  

C'est parce que ce critère est satisfait pour un sommet de décision  avec seulement deux jeux d'entrées : un jeu d'entrées rendant la  décision vraie et un autre la rendant fausse.  

Plutôt que la considérer la décision comme un seul sommet du graphe  de contrôle, on introduit les sous-decisions comme nouveaux sommets.  

Ce critère est appelé "**toutes les conditions multiples**"  

!!!example ""

    Le critère  Toutes-les-décisions  n'est plus satisfait  avec seulement **$\{a = 0,b = 1\}$ $et$ $\{a = 2, b = 1\}$**.  
    
    Le critère  Toutes-les-décisions  est satisfait avec  **$\{a = 0,b = 1\}$ $et$ $\{a = 2, b = 1\}$ $et$ $\{a = 2, b = 2\}$** et enfin **$\{a = 0,b = 0\}$**. Division par zéro détectée.  

    <p align='center'><img src='/images/tests11.png'/></p>

    Le graphe simple devient un  multigraphe (deux arcs sortant  du $a==b$ de gauche vers $\frac{1}{a}$ ).  

#### Critère "Toutes les décisions multiples" : Explosion combinatoire

!!!example "Exemple `if (A && (B || C))`"

    Toutes-les-conditions  

    $$\begin{array}{|c c c | c |}
    A & B & C & \text{Décision} \\
    \hline
    0 & 1 & 1 & 0 \\
    1 & 1 & 1 & 1 \\
    \end{array}$$
    
    Toutes-les-conditions-multiples

    $$\begin{array}{|c c c | c |}
    A & B & C & \text{Décision} \\
    \hline
    1 & 1 & 1 & 1 \\
    1 & 1 & 0 & 1 \\
    1 & 0 & 1 & 1 \\
    1 & 0 & 0 & 0 \\
    0 & 1 & 1 & 0 \\
    0 & 1 & 0 & 0 \\
    0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 0 \\
    \end{array}$$

    $2^{\text{nb de variables }}$: Comment limiter la  combinatoire ?  

### Critère Toutes-les-Conditions/Décisions-modifié : MC/DC

!!!note ""

    Objectif : améliorer les critères de couverture basés sur les décisions  tout en contrôlant la combinatoire  

!!!quote "Définition: Critère MC/DC"

    MC/DC : _Modified Condition/Decision Coverage_

On ne s'intéresse à un jeu de test faisant varier une condition que s'il  inﬂue sur la décision.  

### Critère MC/DC

!!!example "Exemple `if (A && (B || C))`"
 
    Principe : pour chaque test atomique, trouver deux cas de tests qui  changent la décision lorsque les autres conditions sont fixées.  
    
    - Pour $A$ :  
        - $A = 0$ , $B = 1$ , $C = 1$. Décision : $0$  
        - $A = 1$ , $B = 1$ , $C = 1$. Décision : $1$ 
    - Pour $B$ :  
        - $A = 1$ , $B = 0$ , $C = 0.$ Décision : $0$  
        - $A = 1$ , $B = 1$ , $C = 0.$ Décision : $1$  
    - Pour $C$ :  
        - $A = 1$ , $B = 0$ , $C = 1.$ Décision : $1$  
        - $\color{red}A = 1 \text{ , } B = 0 \text{ , } C = 0. \text{ Décision : 0. Déjà couvert}$.  
  
    Si $n$ conditions : critère $MC/DC$ entraîne au plus $2 × n$ tests contre $2^n$ pour le critère Toutes-les-décisions-multiple.  

### Limite du critère Toutes-les-décisions-multiples

```C linenums="1"
float invsum(int n, float a[]){
    int i = 0;
    float s = 0;
    while(i<n){
        s = s+a[i];
        i++;
    }
    return 1/s;
}
```

Toutes-les-décisions $=$ toutes-les-décisions-multiples puisque test atomique.

<p align='center'><img src='/images/tests12.png'/></p>

$\color{red}\textbf{E,B1,C1,B2,C1,ret. }\frac 1 s\textbf{ , S}\text{ couvre toutes les décisions.}$
$\color{red}\text{Satisfait par } n = 1,  a = \{2., 3., 5.\}.$
$\color{red}\text{Défaut non  détecté si le tableau est vide.}$

### Critère "tous les chemins"

!!!quote "Définition: Critère Tous-les-chemins"

    Parcourir tous les arcs dans chaque configuration possible (et non pas au moins une fois comme dans le  critère toutes-les-décisions)  

Impraticable dès qu'il y a des boucles car existence de chemins infinis.  

Aﬀaiblissement possible : "tous les chemins qui passent au plus $k$ fois dans les boucles"

En pratique on se limite souvent aux cas $k = 1$ ou $k = 2$. Cela permet au moins de détecter les erreurs produites quand on ne rentre pas dans la/les boucle.s.  

Dans l'exemple précédent, $n = 1$, $a = \{\}$ satisfait le chemin  $\textbf{E,B1,C1,ret. }\frac 1 s\textbf{ , S}$ et détecte la division par zéro.  

### Hiérarchie des critères

<p align='center'><img src='/images/tests13.png'/></p>

### Convention de notation algébrique

!!!note ""
    - $ε$ pour "chemin vide"  
    - $A · B$ ou $AB$ pour "$A$ suivi de $B$"  
    - $(CB)^3$ pour "$CBCBCB$"  
    - $A + B$ pour "$A ou B$"  
    - $A^{+}$ pour "$A + A^2 + A^3 + · · · + A^n + . . .$"  
    - $A^{∗}$ pour "$ε + A + A^2 + A^3 + · · · + A^n + . . .$"  
    - $A^{\underline{4}}$ pour $ε + A + A^2 + A^3 + A^4$

### Expression des chemins sous forme algébrique

```C linenums="1"
void F(int x){
    if (x <= 0)
        x = -x;
    else
        x = 1 + x;
    
    if (x==1)
        x=1-x;
    else
        x=1;
    return x;
}
```

<p align='center'><img src='/images/tests14.png'/></p>

- Les chemins de contrôle possibles sont décrits par

$$IA(B+C)D(E+F)GO$$

- Le nombre de chemins de contrôle possible est $1 · 1 ·(1+1)· 1 ·(1+1)· 1 = 4$
