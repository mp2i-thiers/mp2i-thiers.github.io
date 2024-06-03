
# Modèle relationnel, bases de données

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Lorentzo et **Elowan** et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

## Introduction

### E. F. Codd (Wikipedié)

<p align='center'><img src='/images/bdd1-1.png'/></p>
<p align='center'><img src='/images/bdd1-2.png'/></p>

### Résumé

- Le _Modèle relationnel_ pour la gestion des **Bases De Données** (BDD)  est un modèle de BDD basé sur la logique du premier ordre proposé  et formulé pour la $1^{ère}$ fois par Edgar F. Codd $(1969)$.  
- Dans une BDD _relationnelle_ l'information est organisée dans des  tableaux à deux dimensions appelées _relations_ ou _tables_.  
- Une BDD est donc un ensemble de tables. Les lignes sont appelées  _tuples_, _nuplets_ ou encore _enregistrements_.  
- Le _modèle relationnel_ fournit une méthode déclarative pour spécifier  _données_ (l'ensemble étudié) et _requêtes_ (questions permises sur cet  ensemble).  
- L'_utilisateur_ décrit les informations que contient la BDD et quelles  informations il souhaite connaître et laisse _le système de gestion de  BDD_ (SGBD) gérer la description machine de la base et son stokage  ainsi que la manière dont il retrouve l'information.  

## Le modèle relationnel

### Quelques considérations générales

- Toutes les données sont représentées comme des _relations $\textit{n−aires}$_,  des sous-ensembles de produits cartésiens de n ensembles.  
- Calculs sur les données : _calcul relationnel_ ou _algèbre relationnelle_.  
- Le concepteur de BDD relationnelle crée un modèle logique _cohérent_  (sans contradiction).  

### Attributs

!!! quote "Définition: Attribut"

    On considère donné un ensemble infini ${\displaystyle {\mathcal {A}}}$, dont les éléments sont appelés des _attributs_, un ensemble $D$ (ensemble des _domaine_), et une application $\text{dom}$ de ${\displaystyle {\mathcal {A}}}$ dans $D$.

!!! tip ""
    **Remarque**

    Si $A \in {\displaystyle {\mathcal {A}}}$, l'élément $\text{dom}(A)$ de $D$ est appelé _domaine_ de $A$.
    La domaine de $A$ est lui-même un ensemble, par exemple ensemble des
    entiers, des flottants, des chaı̂nes de caractères...

!!! example ""
    **Exemple**

    Soit le lycée $\texttt{Pierre Dupont}$ contenant des CPGE. Les classes sont des couples $\texttt{(filière,numéro)}$ comme $\texttt{(MPSI,1)}$ ; $\texttt{(MPSI,2)}$ ou $\texttt{(PCSI,1)}$.

    - $\textsf{filière}$ est un attribut dont le domaine est l'ensemble fini de chaînes de caractères $\textsf{ \{MPSI,PCSI,PC,PSI,MP,BCPST,HK\} }$. 
    - $\texttt{numéro}$ est un attribut dont le domaine est l'ensemble $\mathbb{N}^∗$ ou mieux : un intervalle $[\![0,m]\!]$ où $m$ est le nombre maximum de classes de même niveau dans le lycée.

### Schéma relationnel

!!! quote "Définition : Schéma relationnel"

    Soit ${\displaystyle {\mathcal {A}}}$ un ensemble d'attributs et $\text{dom}$ une application qui associe un domaine à chaque attribut.
    Un _Schéma relationnel_ est un tuple $S = (A_1, A_2, ..., A_n) \in {\displaystyle {\mathcal {A}}}^n$ où les $A_i$ sont distincts deux à deux (mais peuvent avoir les mêmes domaines).

!!! tip ""
    **Remarque**

    - Généralement, on écrit le schéma relationnel sous forme de tuples de couples ($\textsf{attribut, domaine}$) comme $S = ((A_1, \text{dom}(A_1)), ...  , (A_n , \text{dom}(A_n)))$
    - Le plus souvent, on ajoute au schéma des symboles indiquant les _clés primaires_ et _clés étrangères_ (voir sections dédiées).

!!! example ""
    **Exemple**

    Schéma des classes du lycée :
    $$S = \left( (\texttt{filiere, \{MPSI,PCSI,...\}}), (\texttt{numéro,} \mathbb{N}^∗ )\right)$$

!!! tip ""
    **Notation**

    - On écrit $B \in S$ si $B \in {A_1, ... , A_n}$.
    - Si $X = \{B_1, ... , B_m\}$ est un ensemble d'attributs (distincts), on écrit  $X \subset S$ si tous les $B_i$ sont dans ${A_1, ... , A_n}$.  
    - On s'autorise aussi des notations de la forme :  
    $$(\texttt{nom,ville}) \subset (\texttt{téléphone,nom,ville,classe})$$

### Table

!!! quote "Définition: Relation ou table associée à un schéma relationnel"

    On appelle _relation_ ou _table associée_ à un schéma relationnel $(A_1, A_2, ... , A_n)$ tout ensemble fini de tuples de $\text{dom}(A_1) × \text{dom}(A_2) × . . . \text{dom}(A_n)$.

!!!tip ""
    **Notation**

    - Les relations sont souvent notées sous la forme $R(S)$ (pour indiquer
    que $R$ est associé au schéma $S$).
    - Explication : dans la colonne $i$ d'une table de schéma $S$, les valeurs sont obligatoirement dans le domaine $\text{dom}(A_i)$. C'est ce qu'on appelle une _contrainte d'intégrité_ (ici, on parle d'_intégrité de domaine_).

!!!example "Example"

    - Si la table $\text{classe}$ est finie on peut la représenter par un tableau :
    $\text{classe}\left(\texttt{filière}, \texttt{ numéro}\right) =$

    | $\texttt{Filière}$ | $\texttt{Numéro}$ |
    |:---------:|:--------:|
    |$\text{MPSI}$|$1$|
    |$\text{PC}$|$3$|
    |$\text{PCSI}$|$2$|
    |$\text{PCSI}$|$1$|

    - L'ordre des attributs et des tuples n'a pas d'importance. On a aussi :
    $\text{classe}\left(\texttt{filière}, \texttt{ numéro}\right) =$

    | $\texttt{Numéro}$ | $\texttt{Filière}$ |
    |:---------:|:--------:|
    |$1$|$\text{MPSI}$|
    |$3$|$\text{PC}$|
    |$2$|$\text{PCSI}$|
    |$1$|$\text{PCSI}$|

### Représentation des schémas relationnels

!!! quote "Définition"

    | $\text{Nom du schéma}$||
    |:--------|:---------:|
    |$\text{Attribut 1}$|$\texttt{type 1}$|
    |$\text{Attribut 2}$|$\texttt{type 2}$|

!!!tip "Remarque"

    Deux relations distinctes peuvent avoir le même schéma.

!!!example "Exemple"

    Le schéma :

    | $\text{élève}$||
    |:--------:|:---------:|
    |$\text{Nom}$|$\texttt{string}$|
    |$\text{Année de naissance}$|$\texttt{int}$|

    Possède les instances :

    | $\texttt{Nom}$|$\texttt{Année de naissance}$|
    |:--------:|:---------:|
    |$\text{Hoareau}$|$1996$|
    |$\text{Grondin}$|$1995$|

    | $\texttt{Nom}$|$\texttt{Année de naissance}$|
    |:--------:|:---------:|
    |$\text{Nativel}$|$1998$|
    |$\text{Horeau}$|$1996$|
    |$\text{Grondin}$|$1997$|

### Multi-ensemble

!!!tip ""
    **Remarque**

    Un _multi-ensemble_ est une sorte d'ensemble dans lequel un même élément  peut apparaître plusieurs fois comme dans $\{1, 2, 3, 2\}$.  
    - Notion à mi-chemin des ensembles et des listes.  
    - On peut voir les multi-ensembles comme des listes quotientées par les  permutations, _i.e._ des listes commutatives.  
    - Le multi-ensemble $\{1, 2, 2\}$ est égal à $\{2, 1, 2\}$.  
    - Les relations du modèle relationnel sont en fait des multi-ensembles.  

## Clés uniques

### Notation objet

!!! quote ""
    **Notation**

    Soit $R(S)$ une relation, $e \in R(S)$ un enregistrement et $A \in S$. On note $e.A$ la composante du tuple $e$ associée à l'attribut $A$. Si $K \subset S$, on note $e.K$ le sous-tuple de $e$ constitué des composantes associées aux éléments de $K$. Il s'agit de la projection de $e$ sur les attributs de $K$.

!!!example ""
    **Exemple**

    $\text{classe}\left(\texttt{filière}, \texttt{ numéro}, \texttt{ salle}\right) =$

    | $\texttt{Filière}$ | $\texttt{Numéro}$ | $\texttt{Salle}$|
    |:---------:|:--------:|:----:|
    |$\texttt{MPSI}$|$1$|$\text{B.10}$|
    |$\texttt{MPSI}$|$2$|$\text{C.34}$|
    |$\texttt{PCSI}$|$2$|$\text{B.1}$|

    Si $e = (\texttt{PCSI}, 2, \text{B.1})$, alors $e.\texttt{numéro} = 2$ et $e.\texttt{(numéro, salle)} = (2,\text{B.1})$.  On dit que $e.{A}$ est la projection de $e$ sur l'attribut $A$. $e.(A_1, ... , A_n)$ est la projection de $e$ sur $A_1 × ··· × A_n$.  

### Clé unique

!!! quote "Définition : clé unique"

    Soit $R(S)$ une relation de schéma $S$. On dit que $K ⊂ S$ est une _clé unique_ pour $R$ si et seulement si
    $$∀(t_1, t_2) \in R^2 , t_1.K = t_2.K ⟺ t_1 = t_2$$

!!! tip ""
    **Remarque**

    - La connaissance des attributs dans $K$ suffit à distinguer deux éléments.
    - $K$ est une clé unique si et seulement si la projection sur $K$ est injective.
    - Lorsqu'il y a une clé unique, la table ne contient pas de doublon de lignes.
    - Souvent, on impose que $K$ soit de cardinal minimum. C'est de bon sens : nous nous en tenons à cette pratique

!!!example ""
    **Exemple**

    $\text{élève}\left(\texttt{Nom}, \texttt{ Prénom}, \texttt{ Année de naissance}\right) =$

    | $\texttt{Nom}$ | $\texttt{Prénom}$ | $\texttt{Année de naissance}$|
    |:---------:|:--------:|:----:|
    |$\text{Hoareau}$|$\text{Patrice}$|$1996$|
    |$\text{Hoareau}$|$\text{Patrice}$|$1995$|
    |$\text{Dupont}$|$\text{Marie}$|$1997$|
    |$\text{Grondin}$|$\text{Patrice}$|$1996$|

    $(\texttt{Nom},\texttt{Prénom})$ n'est pas une clé unique, ni $(\texttt{Prénom},\texttt{Année})$ mais $(\texttt{Nom},\texttt{Année})$ est une clé unique.  

Soit $R(S)$ une relation de schéma $S$.  

- Souvent, on cherche à limiter la clé unique à un seul attribut.  
- Le terme _clé unique_ est trompeur : il peut y en avoir plusieurs !  
Exemple : dans la table $\text{Etudiant}(\texttt{id},\texttt{ nom},\texttt{ prénom},\texttt{ num. de  sécu})$ il y a deux clés uniques possibles :  
    - $\texttt{id}$ (le numéro d'étudiant).  
    - $\texttt{num. de sécu}$
    Les clés sont choisies par le développeur au moment de la conception.  
- Une clé unique peut porter sur plusieurs attributs : il peut très bien ne pas y avoir de clé à un seul élément.  
- Et d'ailleurs, il est possible qu'il n'y ait pas de clé unique (si la table  possède des doublons de lignes). Mais on décourage d'utiliser de telles  tables.  

### Clé primaire

**MySQL** fait la distinction entre les notions de _clé unique_ et _clé primaire_.  

!!! quote "Définition: clé primaire"

    Une _clé primaire_ est une clé unique particulière associée à un _index_.

!!!tip ""
    **Remarque**

    - On peut voir l'index comme une table des matières facilitant un accès  rapide aux enregistrement d'une table ayant une clé primaire.  
    - En particulier, la complexité des jointures est grandement diminuée  par l'usage d'une clé primaire ; les valeurs possibles étant triées dans  l'index.
    - Il peut y avoir plusieurs clés unique par table mais une seule clé  primaire.
    - Une clé unique peut prendre la valeur `NULL` (case vide, équivalent  **Python** de `None`) pas la clé primaire.  

#### Clé unique VS clé primaire

<p style="color : red">Conformément au programme nous ferons désormais la confusion :  nous n'emploirons plus que l'expression "clé primaire" sans nous  soucier de la présence d'un index ou non.</p>

Une conséquence est que nous avons au plus une clé primaire par  table.  

Une autre est que les cases des colonnes définissant la clé primaire ne sont jamais vide (pas de valeur `NULL`).  

### Détails sur la clé primaire

- On indique par un symbole dans le schéma qu'une clé est  unique/primaire.  
- Nous signalons les clés uniques en les soulignant. Sous  **PHPMYADMIN**, les clés primaires sont représentées par des clés  jaunes, les clés uniques par une clé grise.

|élève ||
|:---|:---:|
|$\text{Nom}$ |$\texttt{string}$|
|$\underline{\text{Numéro SS}}$ |$\texttt{int}$|

- Un mot clé $\texttt{PRIMARY}$ indique, au moment de la création de la table  dans la plupart des SGBD, qu'une clé est primaire.  
- Si un tuple déjà défini possède une valeur $v$ pour la clé primaire de la table $T$, alors le SGBD devrait empêcher l'ajout de tout nouveau tuple  à $T$ possédant la valeur $v$ pour la clé.  

## Relation entre deux tables

### Deux schémas

Soit une BDD modélisant une bibliothèque simplifiée avec deux tables dont les schémas sont :  

|livre||
|:--|:--:|
|$\underline{\text{titre}}$ |$\texttt{string}$|
|$\text{auteur}$ |$\texttt{string}$|
|$\text{année de publication}$ |$\texttt{int}$|

|emprunteur||
|:--|:--:|
|$\text{Nom}$|$\texttt{string}$|
|$\text{Livre emprunté}$|$\texttt{string}$|

### Deux tables

Une table $\text{bibliothèque}$ instanciant $\text{livre}$ :  

| $\texttt{titre}$ | $\texttt{auteur}$ | $\texttt{Publication}$|
|:---------:|:--------:|:----:|
|$\text{Harry Potter}$|$\text{J.K Rowling}$|$1997$|
|$\text{Pensées}$|$\text{Pascal}$|$1670$|
|$\text{Marseille coquin}$|$\text{Anonyme}$|$2016$|

Une table $\text{Clients}$ instanciant $\text{emprunteur}$

| $\texttt{Nom}$ | $\texttt{Livre emprunté}$|
|:---------:|:--------:|
|$\text{Hoareau}$|$\text{Harry Potter}$|
|$\text{Grondin}$|$\text{Pensées}$|
|$\text{Dupont}$|$\text{Maths MP}$|

On en conclut que Hoareau a emprunté "Harry Potter" et que Grondin est un petit coquin !  

Dupont emprunte un ouvrage qui n'existe pas dans la $\text{bibliothèque}$,  ce qui concerne davantage l'administrateur de BDD que la vie privée  de Grondin.  

### Clé étrangère

!!! quote "Définition"

    Une clé étrangère (représentée dans ce cours par un `#`) est un attribut qui est la clé primaire d'une autre relation. Elle permet d'établir le lien entre plusieurs relations. Elle met en évidence les dépendances fonctionnelles entre $2$ tables.

!!! tip "Remarque"

    En **SQL**, on déclare une clé étrangère avec les mots clés $\texttt{FOREIGN KEY}$.

On peut représenter les liens entre deux relations dans un diagramme  par une ﬂèche depuis l'attribut vers la clé primaire.

Si on impose que le domaine de livre $\texttt{emprunté}$ est constitué  exactement des livres apparaissant dans la relation $\texttt{bibliothèque}$, le schéma de $\texttt{emprunteur}$ devient

Shéma référençant :
|emprunteur||
|:---|:---:|
|$\text{Nom}$|$\texttt{string}$|
|$\text{Livre emprunté \#}$|$\texttt{titre}$|

$$↓$$

Shéma référencé :
|livre||
|:--|:--:|
|$\underline{\text{titre}}$ |$\texttt{string}$|
|$\text{auteur}$ |$\texttt{string}$|
|$\text{année de publication}$ |$\texttt{int}$|

Le tuple $(\texttt{Dupont}, \texttt{Maths MP})$ ne peut plus être un enregistrement de $\texttt{Clients}$ (vu plus haut) car $\texttt{Maths MP}$ n'est pas dans la colonne "titre" de  $\texttt{Bibliothèque}$.  

La contrainte de clé étrangère est gérée par la plupart des SGBD :  $\texttt{Oracle, Microsoft SQL Server, PostgreSQL, SQLite, etc.}$  

## Modèle client-serveur

### Architecture client-serveur

#### Mode de communication

- Environnement _client-serveur_ : mode de communication à travers un  réseau entre plusieurs programmes ou logiciels :
   - le premier, le _client_, envoie des requêtes ;
   - l'autre ou les autres,les _serveurs_, attendent les requêtes des clients et y répondent.  

**Par extension**, le client désigne également l'ordinateur sur lequel est exécuté le logiciel client, et le serveur, l'ordinateur sur lequel est exécuté le logiciel serveur.  

#### Vocabulaire

!!!note "Serveurs"

    Souvent des ordinateurs dédiés au logiciel serveur qu'ils  abritent (ex : serveur Web, serveur de bases de données, d'impression  ...). Ils sont dotés de capacités supérieures à celles des ordinateurs  personnels en termes de puissance de calcul, d'entrées-sorties et de  connexions réseau.

!!!note "Clients"

    Souvent des ordinateurs personnels ou des appareils  individuels (téléphone, tablette), mais pas systématiquement.  

!!!note "Nombre de clients"

    Un serveur peut répondre aux requêtes d'un grand  nombre de clients.  

!!!example "Exemples"

    Grande variété de logiciels serveurs et de logiciels clients en fonction des  besoins à servir :

    - un serveur web publie des pages web demandées par des navigateurs  web ;  
    - un serveur de messagerie électronique envoie des mails à des clients  de messagerie ;  
    - un serveur de fichiers permet de stocker et consulter des fichiers sur le  réseau ;  
    - un serveur de données à communiquer des données stockées dans une  base de données, etc...  

#### Notion de port

La notion de _port logiciel_ permet, sur un ordinateur donné, de  distinguer diﬀérents interlocuteurs. Ces interlocuteurs sont des  programmes informatiques qui, selon les cas, écoutent ou émettent  des informations sur ces ports. Un port est distingué par son numéro.  

Image : $\texttt{PORT} = \texttt{PORTE}$ donnant accès au système d'exploitation. Pour fonctionner, un programme ouvre des portes pour accéder aux  services de l'OS. Quand on ferme le programme, la porte n'a plus  besoin d'être ouverte.

Lorsqu'un logiciel client veut dialoguer avec un logiciel serveur (le  _service_), il a besoin de connaître le port écouté par ce dernier. Par  exemple port $80$ pour un serveur web HTTP ; port $3306$ serveur de bases de données MySQL ; port $8888$ pour jupyter...  

`cat /etc/services` pour avoir la liste des services bien connus. Ou  `sudo netstat -antup | grep LISTEN` pour la liste des ports en écoute.  

#### Caractéristiques d'un processus serveur

Attend une connexion entrante sur un ou plusieurs _ports_ réseaux.  

À la connexion d'un client sur le port en écoute, ouvre un _socket local_  (interface de comunication) avec le système d'exploitation;

Suite à la connexion, le processus serveur communique avec le client  suivant le protocole prévu par la couche $\texttt{application}$ du modèle OSI.  

#### Caractéristiques d'un processus client

Établit la connexion au serveur grâce à son adresse IP et le port, qui  désigne un service particulier du serveur. Un socket est créé côté client.

Lorsque la connexion est acceptée par le serveur, les deux côtés  communiquent via les sockets comme le prévoit la couche $\texttt{application}$ du modèle OSI.

<p align='center'><img src='/images/bdd1-3.png'/></p>

_Figure – Architecture client-serveur_

!!!example "La machine à café"

    Dans une cafétéria, les cafés sont délivrés par un automate.

    Le client insère des pièces dans l'automate, sélectionne sa boisson et  attend que la machine remplisse son gobelet.

    Le serveur est la machine à café. Le couple (client, automate) est une  architecture client-serveur.

    Le client accède directement à la ressource.

    - Si le serveur est en panne, c'est au client d'en trouver un autre (pb de  maintenance)
    - Si le client est malhonnête, il peut tenter d'insérer de fausses pièces (il  ne court aucun risque).  

### Architecture trois-tiers  

!!!example "Une brasserie"

    Dans une brasserie, les garçons de café ont accès directement au  percolateur.

    Le client (couche présentation) s'assied à une table, attend que le  garçon (couche métier) vienne prendre sa commande.

    Une fois que le garçon a pris la commande, il accède au percolateur  (couche accès aux données) derrière le comptoir, prépare l'expresso et  le ramène au client.

    Le triplet (client,garçon,percolateur) est une architecture trois-tiers  (ou trois couches)  

    Le client accède n'accède plus directement à la ressource.  
    - Si le percolateur est en panne, c'est au garçon et pas au client d'en  trouver un autre (maintenance facilitée, on peut imaginer un  percolateur d'appoint en attendant la réparation du principal)  
    - Si le client est malhonnête, il lui est plus diﬃcile d'accéder au  percolateur pour se servir gratuitement (sécurité renforcée).  
    - Bien sûr, le client pourrait attendre que le garçon prenne la  commande d'une autre personne pour accéder en cachette au  percolateur. Il suﬃrait alors de mettre quelqu'un en permanence  derrière le bar (le patron) et ce problème serait résolu (mais on  passerait en architecture 4 couches).  

#### Principe

!!!quote "Définition"

    Le mot _tier_ signifie étage ou niveau en anglais. On dit aussi _couche_.

Une _application_ est composée de $3$ couches indépendantes :

- couche présentation,
- couche traitements (on dit aussi métier ou application)
- couche d'accès aux données.

Ces $3$ couches communiqueront entre elles à l'aide de fonctions  spécifiques (des _API_ : $A$pplication $P$rogramming $I$nterface ou Interfaces de programmation).  

#### SGBD et architecture trois-tiers

On répartit les rôles entre :

- Un serveur contenant la base données (non accessible par les clients)  
- Un système de gestion de base données : une interface souvent graphique entre les clients et la base.  
    - Elle transmet la demande (requête) du client au serveur de données.
    - Elle récupère la réponse du serveur de données et la transmet au client.  
- $\color{red}\text{Le point important : le client ne communique}$ $\color{red}\text{\underline{jamais} directement avec  le serveur de données}$.  

<p align='center'><img src='/images/bdd1-4'/></p>

- Seul le SGBD peut accéder aux données et les modifier.  
- Le client n'a pas besoin de connaître le SQL : souvent une interface  graphique avec des cases à cliquer lui évite de le faire.  
- Le client n'a pas besoin d'installer de logiciel : un navigateur Web lui suffit.  
