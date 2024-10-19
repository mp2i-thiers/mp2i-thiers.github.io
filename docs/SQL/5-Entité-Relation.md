# Modèle entité-association

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d’être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    - Un cours de Quentin Fortier
    - Le tuto SQLdu [W3C](https://www.w3schools.com/sql/) (indispensable)
    - Un cours en français [ici](https://sql.sh/cours#google_vignette)
    - Cette page de [Wikipedia](https://en.wikipedia.org/wiki/Entity–relationship_model)
    - Un cours de [Laurent Audbert](https://laurent-audibert.developpez.com/Cours-BD/?page=conception-des-bases-de-donnees-modele-a)

## Entités

!!!quote "Entité"

    Une _entité_ est un objet concret ou abstrait à propos duquel on souhaite conserver des informations. Exemple : Client, Facture...

!!!example "Une entité peut être"

    - un objet physique comme une voiture
    - un évènement comme une réparation de voiture
    - ou un concept comme une transaction commerciale

Une entité possède un ou plusieurs _attributs_ (propriété attachée à une
entité).

!!!example "Exemple d'entité"

    livre, attributs : nombre de page, auteur, date de publication etc.

## Identifiant d'entité

!!!quote "Identifiant d'entité"

    On appele _identifiant d'entité_ un ensemble minimal d'attributs caractérisant de façon unique chaque occurrence d'un type d'entité.

!!!example ""

    entité livre, identifiant d'entité ISBN (l'ISBN est un numéro unique attribué à un livre - mais pas à un exemplaire-).

On retrouve une notion proche de la clé primaire. Au moment du passage au modèle relationnel, chaque entité est traduite par une table dont la clé primaire se déduit de l'identifiant.

On souligne ci-après les identifiants d'entités dans la représentation graphique.

## Des attributs particuliers

Considérons une entité $\textsf{Film}$.

- Un film possède un titre. Il y donc un attribut $\color{blue}\texttt{Titre}$.
- Un film peut posséder plusieurs genre (horreur et comique par exemple). Il y a donc un attribut $\color{blue}\texttt{Genre}$ qui peut prendre plusieurs valeurs pour un même film. Un tel attribut est dit _attribut multiple_. Par exemple, on peut l'écrire en gras dans le diagramme EA.
- Un film peut avoir reçut un Oscar (et dans ce cas lors d'une unique
année). L'attribut $\color{blue}\texttt{Oscar}$ devrait donc désigner une année ou rien (analogie : type option en **OCaml**). Un tel attribut est dit _optionnel_. Il pourrait être représenté entre paranthèses dans le diagramme EA.

## Association

!!!quote "Association"

    Une _association_ est une liaison existant entre les entités. Le _degré_ d'une association est le nombre d'entités intervenant dans l'association

!!!example ""

    Commande. Les clients _commandent_ des produits

    <p align='center'><img src='/images/EntiteRelation/entrel1.png'/></p>

    - Ici $\texttt{Commande}$ est une relation binaire (donc de degré $2$).
    - Graphiquement : Entités/Rectangles ; Associations/Ellipses.
    - Autre exemple : réseau social. Entité : membre ; Association : "est ami de ".

## Relation $n\text{-aire}$

<p align='center'><img src='/images/EntiteRelation/entrel2.png'/></p>

Un client commande un produit dans un magasin. L'association $\texttt{Commande}$ est $3\text{-aire}$.

Une relation $n\text{-aire}$ peut être transformée en relation binaire en introduisant une nouvelle entité pour la relation. $\texttt{Commande}$ devient une entité. On introduit $3$ relations $\texttt{passe, dans, de}$.

<p align='center'><img src='/images/EntiteRelation/entrel3.png'/></p>

Un client $\color{blue}\texttt{passe}$ une commande $\color{blue}\texttt{de}$ produit $\color{blue}\texttt{dans}$ un magasin. Toutes les associations sont devenues binaires.

## Cardinalité de patte

On peut spécifier le lien entre une entité et une association avec un couple $(n,p)$ indiquant le nombre minimum et maximum de fois que l'entité peut apparaître dans l'association. Certains auteurs préfèrent écrire $\texttt{n-p}$ ou $\texttt{n..p}$.

Un tel couple est appelé _cardinalité de patte_. Il y a deux cardinalités
de pattes pour une association binaire, trois pour une ternaire et.

Les couples de cardinalités de pattes utilisés sont de la forme $(0,1)$ ;
$(0,n)$ ; $(1,1)$ (souvent noté $1$ plus simplement) et $(1,n)$. La notation littérale $n$ signifie "plusieurs ".

!!!example ""

    Salarié $\color{blue}\texttt{EmployéPar}$ Entreprise. Un salarié n'a qu'un employeur. Cardinalité $(1,1)$ ( notée souvent $1$) pour le lien entre l'entité Salarié et l'association $\color{blue}\texttt{EmployéPar}$.
    
    Une entreprise peut employer plusieurs salariés (un nombre indéterminé) ou aucun. Cardinalité $(0,n)$ entre $\texttt{Entreprise}$ et $\color{blue}\texttt{EmployéPar}$.
    
    On peut être très explicite : Avec un SGBD relationnel, nous pourrons contraindre des cardinalités à des valeurs comme 2, 3 ou plus en utilisant des déclencheurs (trigger).
    
    Exemple : En Andorre, un client français peut acheter au plus $1.5$ litre d'alcool fort et passer la douane pour rentrer en France sans problème. Il peut donc acheter $150$ $\text{cl}$ de whisky single malt. La cardinalité est de $(0,150)$ entre l'entité $\texttt{Client}$ et l'association $\color{blue}\texttt{achète sant problème avec la douane}$.

## Cardinalité d'association

Les valeurs prises sont $0,1$, ou _plusieurs_ qui est noté selon les cas $n$ ou parfois $∗$

- Si une cardinalité maximale est connue et vaut $2, 3$ ou plus, alors nous considérons qu'elle est indéterminée et vaut $n$. En effet, si nous connaissons $n$ au moment de la conception, il se peut que cette valeur évolue au cours du temps.
- La cardinalité **max** est supérieure à la cardinalité **min**.
- Considérons une association binaire dont les pattes sont de cardinalités $(a,b)$ et $(c ,d )$. On appelle _cardinalité d'association_ le couple $(b,d )$ (noté $\texttt{b - d}$) formé des cardinalité maximum des deux pattes (voir exemple suivant).

!!!example

    <p align='center'><img src='/images/EntiteRelation/entrel4.png'/></p>

    - Un fabricant peut fournir $0$ ou plusieurs produits au magasin, un produit n'est fabriqué que par un fabricant.
    - Les valeurs usuelles pour les cardinalités sont $0,1$ et $∗$ (plusieurs).
    - Plutôt que de caractériser les liens entre entité et associations, on peut représenter directement les liens entre deux entités : c'est la cardinalité d'association.
    - On les écrits sous la forme $\texttt{1-1}$, $\texttt{1-*}$ (et symétriquement $\texttt{*-1}$) ou $\texttt{*-*}$ et on forme ces symboles à partir des cardinalités maximales.
    - Par exemple, l'association entre $\texttt{Produit}$ et $\texttt{Fabriquant}$ est $\texttt{1-*}$ car un même fabriquant peut fournir plusieurs produits.

## Cardinalités d'associations entre entités

Types d'associations binaires entre une entité $a$ et une entité $b$

- $\color{darkblue}\texttt{1-1}$ (_one-to-one_) A chaque $a$ correspond exactement un b et réciproquement.
Exemple : l'association _dirige_ est de type $\texttt{1-1}$ entre les entités $\texttt{Proviseur}$ et $\texttt{Lycée}$
- $\color{darkblue}\texttt{1-*}$ (_one-to-many_) A chaque $a$ correspondent plusieurs $b$. Exemple : Un $\texttt{compte en banque}$ _possède_ un unique $\texttt{détenteur}$, mais chaque $\texttt{détenteur}$ peut détenir plusieurs comptes.
- $\color{darkblue}\texttt{*-*}$ (_many-to-many_) à chaque $a$ correspondent plusieurs $b$, et à chaque $b$ correspondent plusieurs $a$. Exemple : une $\texttt{pâtisserie}$ peut _proposer_ plusieurs $\texttt{types de gâteaux}$ et un $\texttt{type de gâteau}$ peut être proposé par plusieurs $\texttt{pâtisseries}$.

!!!example "association $n\text{-aire}$ inappropriée"

    <p align='center'><img src='/images/EntiteRelation/entrel5.png'/></p>

    - Le _type association_ ternaire $\color{blue}\texttt{Concerne}$ associant les _types entité_ $\texttt{Facture}$, $\texttt{Produit}$ et $\texttt{Client}$ représenté ici est inapproprié puisqu'une facture donnée est toujours adressée au même client.
    - En effet, cette modélisation implique pour les associations $\color{blue}\texttt{Concerne}$
    une répétition du numéro de client pour chaque produit d'une même
    facture.
    - La solution consiste à éclater le type association ternaire $\color{blue}\texttt{Concerne}$ en deux type association binaires $\color{blue}\texttt{Concerne}$ et $\color{blue}\texttt{Reçoit}$.

    <p align='center'><img src='/images/EntiteRelation/entrel6.png'/></p>

    (Laurent Audibert)

## Contraintes

Les _contraintes d'intégrité_ traduisent des spécifications de la situation étudiée qui ne peuvent être représentées par le modèle entité-association. Elles ne sont pas bien représentée par nos diagrammes EA sommaires.

Les contraintes d'identités _statiques_ doivent être satisfaite à chaque
instant. Par exemple :

- "âge du client $≥ 18$ "ou "Noms et prénoms sont des attributs obligatoires "(se traduira en $\color{blue}\texttt{NULL}$ interdit)
- "Numéro de téléphone "peut être facultatif (se traduira en $\color{blue}\texttt{NULL}$ autorisé)

Les contraintes _dynamiques_ : Exemple "taille d'un enfant "ne peut
que croître.

## Méthode de création d'un modèle conceptuel de données

Pour créer un schéma entité-association

- on identifie les types d'entités
- on identifie les types d'associations entre entités.
- on identifie les attributs des entités et associations
- on évalue les cardinalités des associations
- on exprime les éventuelles contraintes d'intégrité.

## Conversion en base de donnéeprocédé simplifié

Pour concevoir une base de donnée :

- Utiliser une table par entité.
- Pour chaque association entre $a$ et $b$ :
    - Si association $\texttt{1 - 1}$ : Fusionner les tables $a$ et $b$.
    - Si association $\texttt{1 - ∗}$ : Ajouter un attribut (clé étrangère) dans une table faisant référence à l'autre.
    - Si association $\texttt{∗ - ∗}$ : Ajouter une table ayant $2$ clé étrangère pour faire référence à $a$ et $b$. Une telle table est appelée _table de liaison_. Sa clé primaire est souvent l'ensemble de ces attributs.

(Q. Fortier)

## Interprétation de la cardinalité

La règle pour placer la clé étrangère pour une association $\texttt{1 - ∗}$ doit être nuancée selon la façon dont on comprend la cardinalité.

!!!example ""

    La cérémonie des Oscars est présentée par une seule personne (par an) mais une personne peut présenter plusieurs fois la cérémonie.

    Certains auteurs préconisent ceci :

    <p align='center'><img src='/images/EntiteRelation/entrel7.png'/></p>

    Le $1$ (qui signifie $(1,1)$) à côté de $\texttt{Personne}$ signifie pour ces auteurs qu'un Oscar n'est _présenté_ (forme passive) que par une personne. Mais qu'une personne peut très bien présenter plusieurs Oscar ($(1,n)$ du côté d'$\texttt{Oscar}$). On a donc une cardinalité d'association $\texttt{1 - n}$.
    
    Q : Pourquoi $(1,n)$ et pas $(0,n)$ ?

    Pour M.Noyer, il place les cardinalités de pattes ainsi :

    <p align='center'><img src='/images/EntiteRelation/entrel8.png'/></p>

    On a donc une cardinalité d'association $\texttt{n - 1}$.
    Il est de toute façon clair pour tout le monde qu'il faut mettre une clé étrangère dans $\texttt{Oscar}$ vers $\texttt{Personne}$.

## Ordre de création des tables

Les sommets sont les tables et les arcs représentent les clés étrangères.

A priori, le graphe est acyclique. On construit un _tri topologique_ pour en déduire dans quel ordre créer les tables. Si $b$ a une clé étrangère référençant un attribut de $a$, il faut créer $a$ avant $b$.
