
# bdd1

====
!!! danger
    Ce cours n'a pas été entièrement reverifié après le passage du programme. Pensez à supprimer ce message si vous avez reverifié ce cours

!!! warning 
    Ce cours a été automatiquement traduit des transparents de M.Noyer par
    Lorentzo et **Elowan** et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce
    site à pour seul but d’être plus compréhensible pendant les périodes de
    révision que des diaporamas.

## Introduction

#### E. F. Codd (Wikipedié)

<p align='center'><img src='/images/c495dc3d7ad9d79fad04cbe7c178d6b3.bmp'/></p><p align='center'><img src='/images/35e48b901d58d5d8bfec88d8f2af083b.bmp'/></p>

#### Résumé

- Le Modele relationnel pour la gestion des Bases De Données (BDD)  est un modèle de BDD basé sur la logique du premier ordre proposé  et formulé pour la 1ere fois par Edgar F. Codd (1969).  
- Dans une BDD relationnelle l’information est organisée dans des  tableaux à deux dimensions appelées relations ou tables.  
- Une BDD est donc un ensemble de tables. Les lignes sont appelées  tuples, nuplets ou encore enregistrements.  
- Le modèle relationnel fournit une méthode déclarative pour spécifier  données (l’ensemble étudié) et requêtes (questions permises sur cet  ensemble).  
- L’utilisateur décrit les informations que contient la BDD et quelles  informations il souhaite connaître et laisse le système de gestion de  BDD (SGBD) gérer la description machine de la base et son stokage  ainsi que la manière dont il retrouve l’information.  

### Le modèle relationnel

#### Quelques considérations générales

- Toutes les données sont représentées comme des relations n−aires,  des sous-ensembles de produits cartésiens de n ensembles.  
- Calculs sur les données : calcul relationnel ou algèbre relationnelle.  
Le concepteur de BDD relationnelle crée un modèle logique cohérent  (sans contradiction).  

#### Attributs


!!! quote "Définition: Attribut"
    On considère donné un ensemble infini A, dont les éléments sont appelés des attributs, un ensemble D (ensemble des domaine), et une application dom de A dans D.

!!! tip ""
    **Remarque**
    Si A $\in$ A, l’élément dom(A) de D est appelé domaine de A.
    La domaine de A est lui-même un ensemble, par exemple ensemble des
    entiers, des flottants, des chaı̂nes de caractères...

!!! example ""
    **Exemple**
    Soit le lycée Pierre Dupont contenant des CPGE. Les classes sont des couples (filière,numéro) comme (MPSI,1) ; (MPSI,2) ou (PCSI,1).
    
    - filière est un attribut dont le domaine est l’ensemble fini de chaînes de caractères {MPSI,PCSI,PC,PSI,MP,BCPST,HK}. 
    - numéro est un attribut dont le domaine est l’ensemble $\mathbb{N}^∗$ ou mieux : un intervalle $[\![0,m]\!]$ où m est le nombre maximum de classes de même niveau dans le lycée.

#### Schéma relationnel

!!! quote "Définition : Schéma relationnel"
    Soit A un ensemble d’attributs et dom une application qui associe un domaine à chaque attribut.
    Un Schéma relationnel est un tuple $S = (A_1, A_2, ..., A_n) \in A_n$ où les $A_i$ sont distincts deux à deux (mais peuvent avoir les mêmes domaines).

!!! tip ""
    **Remarque**
    - Généralement, on écrit le schéma relationnel sous forme de tuples de couples (attribut, domaine) comme $S = ((A_1, dom(A_1)), ...  , (A_n , dom(A_n)))
    - Le plus souvent, on ajoute au schéma des symboles indiquant les clés primaires et clés étrangères (voir sections dédiées).

!!! example ""
    **Exemple**
    Schéma des classes du lycée :
    $$S = ((\text{filiere, {MPSI,PCSI,...}}), (\text{numéro,} \mathbb{N}^∗ ))$$

!!! tip ""
    **Notation**
    - On écrit $B \in S$ si $B \in {A_1, ... , A_n}$.
    - Si $X = {B_1, ... , B_m}$ est un ensemble d’attributs (distincts), on écrit  $X \subset S$ si tous les $B_i$ sont dans ${A_1, ... , A_n}$.  
    - On s’autorise aussi des notations de la forme :  
    $$\text{(nom,ville)} \subset \text{(téléphone,nom,ville,classe)}$$

#### Table

!!! quote "Définition: Relation ou table associée à un schéma relationnel"
    On appelle relation ou table associée à un schéma relationnel $(A_1, A_2, ... , A_n)$ tout ensemble fini de tuples de $dom(A_1) × dom(A_2) × . . . dom(A_n)$.

!!!tip ""
    **Notation**
    - Les relations sont souvent notées sous la forme R(S) (pour indiquer
    que R est associé au schéma S).
    - Explication : dans la colonne i d’une table de schéma S, les valeurs
    sont obligatoirement dans le domaine dom(Ai ). C’est ce qu’on appelle
    une contrainte d’intégrité (ici, on parle d’intégrité de domaine).

!!! warning "j'arrive pas avec les tableaux"

??? example "Example"

    - Si la table classe est finie on peut la représenter par un tableau :
    
        classe(filière,numéro)=   
        | Filière | Numéro |
        |---------|--------|
        |MPSI|1|
        |PC|3|
        |PCSI|2|
        |PCSI|1|


    - L’ordre des attributs et des tuples n’a pas d’importance. On a aussi :

        classe(filière,numéro)=   
        |Numéro|Filière|
        |---|---|
        |3|PC|
        |1|PCSI|
        |1|MPSI|
        |2|PCSI|

#### Représentation des schémas relationnels

!!!warning "j'arrive pas avec les tableaux"

!!! quote "Définition"
    Notation


!!! quote "Définition"
    Exemple


Nom du schéma  
Attribut 1  Attribut 2  
type 1  type 2  
élève  
Le shéma  
Nom  Année de naissance  
string  int  
possède les instances  
Nom  Hoareau  Grondin  
Année de naissance  1996  1995  
Nom  Nativel  Hoareau  Grondin  
Année de naissance  1998  1996  1997  

#### Multi-ensemble

!!!tip ""
    **Remarque**
    Un multi-ensemble est une sorte d’ensemble dans lequel un même élément  peut apparaître plusieurs fois comme dans {1, 2, 3, 2}.  
    - Notion à mi-chemin des ensembles et des listes.  
    - On peut voir les multi-ensembles comme des listes quotientées par les  permutations, i.e. des listes commutatives.  
    - Le multi-ensemble {1, 2, 2} est égal à {2, 1, 2}.  
Les relations du modèle relationnel sont en fait des multi-ensembles.  

### Clés uniques

#### Notation objet

!!! quote ""
    **Notation**
    Soit R(S) une relation, e $\in$ R(S) un enregistrement et A $\in$ S. On note e.A la composante du tuple e associée à l’attribut A. Si K $\subset$ S, on note e.K le sous-tuple de e constitué des composantes associées aux éléments de K. Il s’agit de la projection de e sur les attributs de K.

!!!example ""
    **Exemple**

    !!!warning "j'arrive pas avec les tableaux"

    classe(filière,numéro,Salle)=  
    Filière Numéro  MPSI  MPSI  PCSI  
    1  2  2  
    Salle  B.10  C.34  B.1  
    Si e = (PCSI, 2, B.1), alors e.Numéro = 2 et e.(Numéro, Salle) = (2, B.1).  On dit que e.A est la projection de e sur l’attribut A. e.$(A_1, ... , A_n)$ est la  projection de e sur $A_1 × ··· × A_n$.  

#### Clé unique

!!! quote "Définition : clé unique"
    Soit R(S) une relation de schéma S. On dit que K ⊂ S est une clé unique pour R si et seulement si
    $$∀(t_1, t_2) \in R^2 , t_1.K = t_2.K ⇐⇒ t_1 = t_2$$

!!! tip ""
    **Remarque**
    - La connaissance des attributs dans K suffit à distinguer deux éléments.
    - K est une clé unique si et seulement si la projection sur K est injective.
    - Lorsqu’il y a une clé unique, la table ne contient pas de doublon de lignes.
    - Souvent, on impose que K soit de cardinal mi

!!!example ""
    **Exemple**
    
    !!!warning "j'arrive pas avec les tableaux"

    élève(Nom,Prénom,Année de naissance)=  
    Prénom Année de naissance  Nom  Patrice  Hoareau  Hoareau  Patrice  Dupont Marie  Patrice  Grondin  
    1996  1995  1997  1996  

    (Nom,Prénom) n’est pas une clé unique, ni (Prénom,Année) mais  (Nom,Année) est une clé unique.  

Soit R(S) une relation de schéma S.  

- Souvent, on cherche à limiter la clé unique à un seul attribut.  Le terme clé unique est trompeur : il peut y en avoir plusieurs !  
Exemple : dans la table Etudiant(id,nom, prénom,num. de  sécu) il y a deux clés uniques possibles :  
  - id (le numéro d’étudiant).  
  - num. de sécu  
- Les clés sont choisies par le développeur au moment d ela conception.  
Une clé unique peut porter sur plusieurs attributs : il peut très bien ne  pas y avoir de clé à un seul élément.  
- Et d’ailleurs, il est possible qu’il n’y ait pas de clé unique (si la table  possède des doublons de lignes). Mais on décourage d’utiliser de telles  tables.  




#### Clé primaire

MySQL fait la distinction entre les notions de clé unique et clé primaire.  

!!! quote "Définition: clé primaire"
    Une clé primaire est une clé unique particulière associée à un index.

!!!tip ""
    **Remarque**
    - On peut voir l’index comme une table des matières facilitant un accès  rapide aux enregistrement d’une table ayant une clé primaire.  
    - En particulier, la complexité des jointures est grandement diminuée  par l’usage d’une clé primaire ; les valeurs possibles étant triées dans  l’index.
    - Il peut y avoir plusieurs clés unique par table mais une seule clé  primaire.
    - Une clé unique peut prendre la valeur NULL (case vide, équivalent  Python de None) pas la clé primaire.  

#### Clé unique VS clé primaire

- Conformément au programme nous ferons désormais la confusion :  nous n’emploirons plus que l’expression "clé primaire" sans nous  soucier de la présence d’un index ou non.
- Une conséquence est que nous avons au plus une clé primaire par  table.  
- Une autre est que les cases des colonnes définissant la clé primaire ne sont jamais vide (pas de valeur NULL).  

#### Clé primaire

- On indique par un symbole dans le schéma qu’une clé est  unique/primaire.  
- Nous signalons les clés uniques en les soulignant. Sous  PHPMYADMIN, les clés primaires sont représentées par des clés  jaunes, les clés uniques par une clé grise.

!!!warning "j'arrive pas avec les tableaux"

élève  
Nom  Numéro SS  
string  int  

- Un mot clé PRIMARY indique, au moment de la création de la table  dans la plupart des SGBD, qu’une clé est primaire.  
- Si un tuple déjà défini possède une valeur v pour la clé primaire de la  table T, alors le SGBD devrait empêcher l’ajout de tout nouveau tuple  à T possédant la valeur v pour la clé.  

!!!warning "jme suis arrêté là"


### Relation entre deux tables

#### Deux schémas


Soit une BDD modélisant une bibliothèque simplifiée avec deux tables  
dont les schémas sont :  
livre  
titre  auteur  année de publication  
string  string  int  
emprunteur  
et  
Nom  Livre emprunté  
string  string  

#### Deux tables


Une table bibliothèque instanciant livre :  
titre  Harry Potter  Pensées  Marseille coquin  
auteur  J.K. Rowling  Pascal  Anonyme  
Publication  1997  1670  2016  
Une table Clients instanciant emprunteur  
Livre emprunté  Nom  Hoareau  Harry Potter  Grondin Marseille coquin  Dupont  
Maths MP  
On en conclut que Hoareau a emprunté "Harry Potter" et que  Grondin est un petit coquin !  
Dupont emprunte un ouvrage qui n’existe pas dans la bibliothèque,  ce qui concerne davantage l’administrateur de BDD que la vie privée  de Grondin.  

#### Clé étrangère


!!! quote "Définition"
    Définition
Une clé étrangère (représentée dans ce cours par un #) est un attribut qui
est la clé primaire d’une autre relation. Elle permet d’établir le lien entre
plusieurs relations. Elle met en évidence les dépendances fonctionnelles
entre 2 tables.


!!! quote "Définition"
    Remarque
En SQL, on déclare une clé étrangère avec les mots clés FOREIGN KEY.


On peut représenter les liens entre deux relations dans un diagramme  par une ﬂèche depuis l’attribut vers la clé primaire.  Si on impose que le domaine de livre emprunté est constitué  exactement des livres apparaissant dans la relation bibliothèque, le  schéma de emprunteur devient  Shéma référençant  
emprunteur  
Nom  Livre emprunté# titre  
string  
→  
Schéma référencé  livre  
titre  auteur  année de publication  
string  string  int  
Le tuple (Dupont,Maths MP) ne peut plus être un enregistrement de  Client car Maths MP n’est pas dans la colonne "titre" de  Bibliothèque.  


La contrainte de clé étrangère est gérée par la plupart des SGBD :  Oracle, Microsoft SQL Server, PostgreSQL, SQLite, etc.  

###     


   
  
Introduction  
 Le modèle relationnel  
 Clés uniques  
 Relations entre deux tables  
 Modèle client-serveur  
Architecture client-serveur  Architecture trois-tiers  

#### Mode de communication


   
Environnement client-serveur : mode de communication à travers un  réseau entre plusieurs programmes ou logiciels  le premier, le client, envoie des requêtes ;  l’autre ou les autres, les serveurs, attendent les requêtes des clients et y  répondent.  
  
  
Par extension, le client désigne également l’ordinateur sur lequel est  exécuté le logiciel client, et le serveur, l’ordinateur sur lequel est  exécuté le logiciel serveur.  

#### Vocabulaire


   
Serveurs : souvent des ordinateurs dédiés au logiciel serveur qu’ils  abritent (ex : serveur Web, serveur de bases de données, d’impression  ...). Ils sont dotés de capacités supérieures à celles des ordinateurs  personnels en termes de puissance de calcul, d’entrées-sorties et de  connexions réseau.  
Clients : souvent des ordinateurs personnels ou des appareils  individuels (téléphone, tablette), mais pas systématiquement.  
Nombre de clients. Un serveur peut répondre aux requêtes d’un grand  nombre de clients.  

#### Exemples


   
Grande variété de logiciels serveurs et de logiciels clients en fonction des  besoins à servir :  
un serveur web publie des pages web demandées par des navigateurs  web ;  
un serveur de messagerie électronique envoie des mails à des clients  de messagerie ;  
un serveur de fichiers permet de stocker et consulter des fichiers sur le  réseau ;  
un serveur de données à communiquer des données stockées dans une  base de données, etc...  

#### Notion de port


   
La notion de port logiciel permet, sur un ordinateur donné, de  distinguer diﬀérents interlocuteurs. Ces interlocuteurs sont des  programmes informatiques qui, selon les cas, écoutent ou émettent  des informations sur ces ports. Un port est distingué par son numéro.  Image : port = porte donnant accès au système d’exploitation.  Pour fonctionner, un programme ouvre des portes pour accéder aux  services de l’OS. Quand on ferme le programme, la porte n’a plus  besoin d’être ouverte.  
Lorsqu’un logiciel client veut dialoguer avec un logiciel serveur (le  service), il a besoin de connaître le port écouté par ce dernier. Par  exemple port 80 pour un serveur web HTTP ; port 3306 serveur de  bases de données MySQL ; port 8888 pour jupyter...  cat /etc/services pour avoir la liste des services bien connus. Ou  sudo netstat -antup | grep LISTEN pour la liste des ports en  écoute.  

#### Caractéristiques d’un processus serveur


   
Attend une connexion entrante sur un ou plusieurs ports réseaux.  
à la connexion d’un client sur le port en écoute, ouvre un socket local  (interface de comunication) avec le système d’exploitation ;  
suite à la connexion, le processus serveur communique avec le client  suivant le protocole prévu par la couche application du modèle OSI.  

#### Caractéristiques d’un processus client
<p align='center'><img src='/images/1410dc90d91424781d736f097e62bcc8.bmp'/></p>

_Figure – Architecture client-serveur_

   
établit la connexion au serveur grâce à son adresse IP et le port, qui  désigne un service particulier du serveur. Un socket est créé côté  client.  
lorsque la connexion est acceptée par le serveur, les deux côtés  communiquent via les sockets comme le prévoit la couche  application du modèle OSI.  

#### La machine à café


   
Dans une cafétéria, les cafés sont délivrés par un automate.  
Le client insère des pièces dans l’automate, sélectionne sa boisson et  attend que la machine remplisse son gobelet.  
Le serveur est la machine à café. Le couple (client, automate) est une  architecture client-serveur.  Le client accède directement à la ressource.  
Si le serveur est en panne, c’est au client d’en trouver un autre (pb de  maintenance)  Si le client est malhonnête, il peut tenter d’insérer de fausses pièces (il  ne court aucun risque).  


   
  
Introduction  
 Le modèle relationnel  
 Clés uniques  
 Relations entre deux tables  
 Modèle client-serveur  
Architecture client-serveur  Architecture trois-tiers  

#### Une brasserie


   
Dans une brasserie, les garçons de café ont accès directement au  percolateur.  
Le client (couche présentation) s’assied à une table, attend que le  garçon (couche métier) vienne prendre sa commande.  
Une fois que le garçon a pris la commande, il accède au percolateur  (couche accès aux données) derrière le comptoir, prépare l’expresso et  le ramène au client.  
Le triplet (client,garçon,percolateur) est une architecture trois-tiers  (ou trois couches)  


   
Le client accède n’accède plus directement à la ressource.  
Si le percolateur est en panne, c’est au garçon et pas au client d’en  trouver un autre (maintenance facilitée, on peut imaginer un  percolateur d’appoint en attendant la réparation du principal)  
Si le client est malhonnête, il lui est plus diﬃcile d’accéder au  percolateur pour se servir gratuitement (sécurité renforcée).  
Bien sûr, le client pourrait attendre que le garçon prenne la  commande d’une autre personne pour accéder en cachette au  percolateur. Il suﬃrait alors de mettre quelqu’un en permanence  derrière le bar (le patron) et ce problème serait résolu (mais on  passerait en architecture 4 couches).  

#### Architecture trois-tiers


!!! quote "Définition"
    Définition
Le mot tier signifie étage ou niveau en anglais. On dit aussi couche.


   
Une application est composée de 3 couches indépendantes :  
 couche présentation,   couche traitements (on dit aussi métier ou application)   couche d’accès aux données.  
Ces 3 couches communiqueront entre elles à l’aide de fonctions  spécifiques (des API : Application Programming Interface ou  Interfaces de programmation).  

#### SGBD et architecture trois-tiers


   
On répartit les rôles entre  
Un serveur contenant la base données (non accessible par les clients)  Un système de gestion de base données : une interface souvent  graphique entre les clients et la base.  
Elle transmet la demande (requête) du client au serveur de données.  Elle récupère la réponse du serveur de données et la transmet au client.  
Le point important : le client ne communique jamais directement avec  le serveur de données.  


   
Base de données  
Client 1  
SGBD  
Client 2  
Client 3  
Seul le SGBD peut accéder aux données et les modifier.  
Le client n’a pas besoin de connaître le SQL : souvent une interface  graphique avec des cases à cliquer lui évite de le faire.  
Le client n’a pas besoin d’installer de logiciel : un navigateur Web lui  suﬃt  
