# Requêtes sur plusieurs tables

!!!tip "Crédits"

    - Un cours de Quentin Fortier
    - Le tuto SQL du [W3C](https://www.w3schools.com/sql/) (indispensable)
    - Un cours en français [ici](https://sql.sh/cours)

## Opérateurs ensemblistes usuels

### Généralités

Ces opérateurs ensemblistes concernent _uniquement_ des relations ayant le même schéma.

||$\texttt{P1}$||
|:--:|:--:|:--:|
|**Nom**|**Id**|**Prénom**|
|Hoareau|$2011$|Patrice|
|Grondin|$5256$|Marie|
|Dupont|$52$|Patrice|

||$\texttt{P2}$||
|:--:|:--:|:--:|
|**Nom**| **Id** |**Prénom**|
|Dupont| $52$ |Patrice|
|Grondin| $5256$| Pierre|

Il existe la notion de _schémas compatibles_ quand deux relations ont le même nombre d'attributs et que les attributs ont le même domaine.

**PB** : deux attributs de même domaine pouvant avoir des _sémantiques_ différentes. Ex : $\texttt{Age }$et$\texttt{ Nombre de carottes}$.

### Union

!!!quote "Union"

    L'_union_ de deux relations $R_1(S )$ et $R_2(S )$ est l'ensemble des $n\text{-uplets}$ appartenant à $R_1(S )$ ou $R_2(S )$. On la note $R_1 ∪R_2(S )$ ou plus simplement $R_1 ∪R_2$.

||$\texttt{P1 ∪ P2}$||
|:--:|:--:|:--:|
|**Nom**| **Id** |**Prénom**|
|Dupont| $52$| Patrice|
|Grondin| $5256$| Pierre|
|Hoareau| $2011$| Patrice|
|Grondin| $5256$| Marie|

### Intersection

!!!quote "Intersection"

    L'_intersection_ de deux relations $R_1(S )$ et $R_2(S )$ est l'ensemble des $n\text{-uplets}$ appartenant à $R_1(S )$ et $R_2(S )$. On la note $R_1 ∩ R_2(S )$ ou plus simplement $R_1 ∩R_2$.

||$P1∩P2$||
|:--:|:--:|:--:|
|**Nom** |**Id** |**Prénom**|
|Dupont| $52$ |Patrice|

### Différence

!!!quote "Différence"

    La _différence_ de deux relations $R_1(S )$ et $R_2(S )$ est l'ensemble des $n\text{-uplets}$ appartenant à $R_1(S )$ mais pas à $R_2(S )$. On la note $R_1 −R_2(S )$ ou plus simplement $R_1 −R_2$.

||$\texttt{P1−P2}$||
|:--:|:--:|:--:|
|**Nom** |**Id**| **Prénom**|
|Grondin| $5256$| Pierre|
|Hoareau| $2011$| Patrice|

!!!tip "Remarque"

    Pour tous ces opérateurs binaires, le schéma de la nouvelle table construite est le même que celui des deux tables entrées.

### Opérations ensemblistes

#### Le mot clé $\color{blue}\texttt{UNION}$

$\color{blue}\texttt{UNION}$, $\color{blue}\texttt{INTERSECT}$ et $\color{blue}\texttt{EXCEPT}$ sur des tables de même shéma.

```SQL linenums="1"
SELECT * FROM R1 UNION SELECT * FROM R2 ;
```

$\color{blue}\texttt{UNION}$ donne la réunion sans doublon de lignes, $\color{blue}\texttt{UNION ALL}$ donne la réunion avec potentiellement des doublons de lignes.

La liste sans doublon des villes où il y a des clients ou des fournisseurs.

```SQL linenums="1"
SELECT City FROM Customers
UNION
SELECT City FROM Suppliers
```

$\color{blue}\texttt{UNION ALL}$ pour autoriser les doublons (pratique pour voir les
effectifs par ville).

#### Intersection

Malheureusement, certains SGBD ne prennent pas en charge les commande $\texttt{EXCEPT}$ et $\texttt{INTERSECT}$ : il faut se débrouiller autrement : avec une jointure ou avec le mot clé $\color{blue}\texttt{IN}$.

Avec jointure (cf plus loin)

Avec $\color{blue}\texttt{IN}$ on récupère les villes de clients qui sont aussi des villes
d'opérateurs :

```SQL linenums="1"
SELECT DISTINCT City FROM Customers
WHERE City IN(
SELECT City FROM Suppliers
ORDER BY City);
```

Noter le $\color{blue}\texttt{DISTINCT}$ pour éviter les doublons

!!!tip "Remarque"
    En $\text{SQLite}$, la commande $\color{blue}\texttt{INTERSECT}$ est bien reconnue.

#### Soustraction ensembliste

La commande $\color{blue}\texttt{EXCEPT}$ (parfois $\color{blue}\texttt{MINUS}$) existante sur d'autres SGBD n'est pas implantée en $MySQL$.

Mais on peut s'en passer. Donner sans doublon les villes de clients qui ne sont pas des villes de fournisseurs :

```SQL linenums="1"
SELECT DISTINCT CITY FROM Customers
WHERE City NOT IN(
SELECT CITY FROM Suppliers );
```

La commande $\color{blue}\texttt{MINUS}$ est bien implantée en $\text{SQLite}$.

## Produits cartésien, jointure

### Produit cartésien

#### Présentation

La concaténation des listes $S$ ,$S'$ est notée $S + S '$.

!!!example ""

    $(1,2) + (2,4) = (1,2,2,4)$. 
    
    Cet opérateur est parfois noté de façon plus précise $\uplus$.

!!!quote "Produit cartésien"

    Si $R (S )$ et $R '(S ')$ sont deux relations de schémas $\underline{\text{disjoints}}$, on appelle produit cartésien et on note $R ×R '$ la relation de schéma $S + S '$ définie
    par :
    
    $(R ×R ')(S + S ') = {(u_1,...,u_n ,v_1,...v_m ) |(u_1,...,u_n ) ∈ R(S)∧(v_1,...,v_m) ∈ R'(S')}$

!!!tip "Remarque"

    $\text{card}(R ×R ') = \text{card}(R ) × \text{card}(R ')$ et $\text{card}(S + S ') = \text{card}(S ) + \text{card}(S ')$
    (si $S$ ,$S'$ ont des schémas disjoints).

!!!example ""

    ||$\texttt{Elève}$||
    |:--:|:--:|:--:|
    |**Nom**| **Prénom**| **Classe**|
    |Dupont| Jean |$\text{MPSI}$    $1$|
    |Michel| Jacques |$\text{MPSI}$ $1$|

    |$\texttt{Prof}$||
    |:--:|:--:|
    |**Nom**| **Matière**|
    |Tartempion| Maths|
    |Duchmol| Anglais|
    |Schprountz| Allemand|

    Pour faire le produit cartésien il faut d'abord renommer pour avoir des schémas disjoints :

    ||$\texttt{Elève}$ |$×$| $ρ_{\text{Nom←Lehrer}} \texttt{(Prof)}$||
    |:--:|:--:|:--:|:--:|:--:|
    |**Nom**| **Prénom**| **Classe**| **Lehrer**| **Matière**|
    |Dupont| Jean |$\text{MPSI}$ $1$| Tartempion| Maths|
    |Michel| Jeacques| $\text{MPSI}$ $1$| Duchmol| Anglais|
    |Dupont| Jean |$\text{MPSI}$ $1$| Duchmol| Anglais|
    |Michel| Jeacques| $\text{MPSI}$ $1$| Tartempion| Maths|
    |Dupont| Jean |$\text{MPSI}$ $1$ |Schprountz| Allemand|
    |Michel| Jeacques| $\text{MPSI}$ $1$ |Schprountz| Allemand|

#### Jointure croisée

Il existe un opérateur $\color{blue}\texttt{CROSS JOIN}$ (pas universellement reconnu) mais la syntaxe suivante est universelle :

```SQL linenums="1"
--produit cartésien de deux tables
SELECT * FROM table1 , table2
```

Récupérer tous les tuples (fournisseur,expéditeur) :

```SQL linenums="1"
SELECT * FROM Suppliers , Shippers;
--87 résultats
```

!!!tip "Remarque"

    Le cardinal du produit cartésien est le produit des cardinaux :

    ```SQL linenums="1"
    1 SELECT COUNT (*) FROM Suppliers
    2 UNION
    3 SELECT COUNT (*) FROM Shippers
    4 -- donne 3 et 29
    ```

!!!example "Exercice"

    Y a t'il des produits qui ont été achetés dans la même quantité que d'autres ?

???tip "Correction"

    ```SQL linenums="1"
    SELECT Q2-Q1 <> 0 FROM
    (SELECT COUNT(*) AS Q2 FROM 
    (SELECT DISTINCT Quantity FROM OrderDetails) AS T2) AS R2,
    (SELECT COUNT(*) AS Q1 FROM
    (SELECT Quantity FROM OrderDetails) AS T1) AS R1
    
    ```

#### Complément de notations

$π_A(R )$ représente la projection de la relation $R$ sur les colonnes de l'ensemble d'attributs $A$ (on conserve toutes les lignes, on ne garde que certaine colonnes).

$σ_T (R )$ est la sélection de $R$ selon le test $T$ (on conserve toutes les colonnes, on ne garde que les lignes qui vérifient le test).

!!!example "Exercice"

    On suppose que toutes les personnes (client ou fournisseur) ont renseigné l'attribut $\texttt{City}$ dans la BDD $\texttt{Northwind}$ du W3C. Calculer la moyenne de personnes (client ou fournisseur) qui vivent dans une même ville (client ou fournisseur).

???example "Indication"

    Il s'agit de faire la différence entre le cardinal de la réponse d'un $\color{blue}\texttt{UNION ALL}$ avec celui de la réponse d'un $\color{blue}\texttt{UNION}$

    Penser à un produit cartésien.

???tip "Correction"

    ```SQL linenums="1"
    SELECT C1/C2 FROM
    (SELECT Count(*) AS C1 FROM
    (SELECT City FROM Customers
    UNION ALL
    SELECT City FROM Suppliers) AS Ville1) AS R1,
    (SELECT Count(*) AS C2 FROM
    (SELECT City FROM Customers UNION
    SELECT City FROM Suppliers) AS Ville2) AS R2
    ```

### Division cartésienne

#### Présentation

$\color{blue}\texttt{Notion non explicitement au programme ITC et MPII}$. (laissée pour info). Soient deux tables $R$ ,$S$ de schéma $A$ et $B$ telles que $B ⊂A$. On pose $C = A −B$ (en notant la soustraction ensembliste comme la différence des entiers).

!!!quote "Division cartésienne"

    On appelle _division cartésienne_ de $R$ par $S$ , la plus grande table $T$ de schéma $C$ telle que $S ×T ⊂R$ . On note $T = R ÷S$

En notant $π_C$ la projection sur $C$ :

- On pose $T_1 = π_C (R )$. $T_1$ a $C$ pour schéma.
- Soit $T_2 = π_C ((S ×T_1) −R )$.
- Alors $T = T_1 −T_2$
- La division cartésienne est donc plus petite que la projection sur $C$
- La division est utilisée pour répondre à des requêtes de type : " quels sont les produits achetés par tous les clients ? "

!!!example ""

    |$\texttt{R'}$|||$\texttt{R}$|||
    |:--:|:--:|:--:|:--:|:--:|:--:|
    |**Nom**|**Classe**||**Nom**| **Classe**| **Nom-Prof**|
    |Dupont | $\text{MPSI 1}$||Dupont| $\text{MPSI 1}$| Tartempion|
    |Martin | $\text{MPSI 1}$||Martin| $\text{MPSI 1}$| Tartempion|
    |Bernard| $\text{PCSI 2}$||Bernard| $\text{PCSI 2}$| Tartempion|
    ||||Dupont| $\text{MPSI 1}$| Duchmol|
    ||||Bernard| $\text{PCSI 2}$| Duchmol|

    Chercher la division cartésienne (notée $\texttt{DC}$) de $R$ par $R'$.

    $R÷R'$ a pour schéma le seul attribut qui est dans le schéma de $R$ et
    pas dans celui de $R$'.

    Posons $\texttt{DC}= \{\text{Tartempion}, \text{Duchmol}\}$, $\{(\text{Dupont, MPSI 1}),(\text{Martin, MPSI 2}),(\text{Bernard, PCSI 2})\}×\texttt{DC}$ a $6$ lignes, donc plus que nécessaire. Alors $\texttt{DC}$ est trop gros.

    Avec $\texttt{DC}= \{\text{Duchmol}\}$, $\{(\text{Dupont, MPSI 1}),(\text{Martin, MPSI 2}),(\text{Bernard, PCSI 2})\}×\texttt{DC}$ contient la ligne $(\text{Martin, MPSI 1, Duchmol})$ qui n'est pas dans $\texttt{Eleve-Prof}$.

    Réponse : $\texttt{DC}$ vaut

    |$\texttt{DC=R÷R'}$||$\texttt{R''}$|||
    |:--:|:--:|:--:|:--:|:--:|
    |**Nom-Prof**||**Nom-Eleve**|**Classe**| **Nom-Prof**|
    |Tartempion||Dupont| $\text{MPSI 1}$| Duchmol|
    |||Bernard| $\text{PCSI 2}$| Duchmol|
    
    On a $(R÷R'×R')∪R'' = R$.

#### Un exemple plus élaboré

<p align='center'><img src='/images/deuxtable1.png'/></p>

_Figure – Une table_

**Question** : quels sont les acteurs qui ont tourné dans tous les films de Hitchcock ? (on ne dispose pas du quantificateur universel).

Films tournés par Hitchcock :

$T_H = π_{Titre,Directeur} (σ_{Directeur ='Hitchcock'} (Film))$. La table $T_H$ admet (Titre, Directeur ) pour schéma et est constituée de couples (titre, Hitchcock).

Tous les acteurs : $A = π_{Acteur} (Film)$. On cherche $A_2$ : l'ensemble des acteurs qui ont tourné dans tous les films de Hitchcock. C'est le plus grand sous-ensemble de $A$ tel que $T_H ×A_2 ⊂Film$, donc $A_2 = Film ÷T_H$ .

!!!tip "Remarque"

    Tous les acteurs dans $A_2$ ont tourné dans tous les films de Hitchcock et un acteur qui n'est pas dans $A_2$ a manqué au moins un film de Hitchcock.

$T_H ×A$ : toutes les associations (titre, Hitchcock, acteur) possibles de titres de Hitchcock avec un acteur, même celles qui n'existent pas.

$(T_H ×A) −Film$ : seulement les associations (titre, Hitchcock, acteur) qui n'existent pas. Si un nom d'acteur et un film figurent dans une même ligne de cette table, cet acteur n'a pas tourné dans ce film d'Hitchcock.

$A' = π_{Acteur} ((T_H ×A) −Film)$ : les acteurs qui n'ont pas tourné dans au moins un film d'Hitchcock.

$A_2 = A −A'$ : les acteurs qui ont tourné dans tous les films d'Hitchcok. $A_2 = Film ÷π_{Titre,Directeur} (σ_{Directeur ='Hitchcock '}(Film))$

#### Jointure symétrique

##### Recollement de deux relations

!!!quote ""

    Soient $R (S )$ et $R '(S ')$ deux relations de schémas disjoints et $A ∈S$ , $A' ∈S '$ deux attributs de même domaine. On appelle jointure symétrique de $R$ et $R '$ selon $(A,A')$ et on note $R\Join_{[A=A']}R'$  les éléments $e$ de $R ×R '$ tels que $e.A = e.A'$
    
    $R\Join_{[A=A']}R ' := σ_{A=A'}R ×R ' = \left \{e ∈R ×R ' |e.A = e.A' \right \}$

De la même façon qu' on peut se passer en logique classique de l'opérateur $⇒$ (défini avec $∨$ et $¬$), on n'a pas besoin en théorie de l'opérateur de jointure $\Join_{[*=□]}$ car il est défini comme composée d'autres opérateurs.

Mais se serait une mauvaise idée en terme de complexité : coût en $O (|R |×|R '|)$ tests alors qu'il existe des implantations utilisant des index avec un coût linéaire en nombre de tests.

Quel est le schéma de $R\Join_{[A=A']}R '$ ?

La jointure est une sélection du produit cartésien $R ×R '$ donc : $S + S '$.

##### Exemple

|$\texttt{Livre}$|||$\texttt{Auteur}$||
|:-:|:-:|:-:|:-:|:-:|
|**Titre**| **Nom-auteur**|| **Nom**| **Prénom**|
|Madame Bovary| Flaubert|| Flaubert |Gustave|
|Le père Goriot| Balzac (de)|| Balzac (de) |Honoré|

Réaliser la jointure symétrique selon ($\texttt{Nom-auteur},\texttt{ Nom}$).

|$\texttt{Livre}\Join_{[\texttt{Nom-auteur=Nom}]}\texttt{Auteur}$||||
|:-:|:-:|:-:|:-:|
|**Titre**| **Nom-auteur**| **Nom**| **Prénom**|
|Madame Bovary |Flaubert |Flaubert| Gustave|
|Le père Goriot |Balzac (de)| Balzac (de)| Honoré|

Il y a des doublons de valeurs. Comment éviter cela ?

|$\pi_{\texttt{Titre,Nom,Prénom}}(\texttt{Livre}\Join_{[\texttt{Nom-auteur=Nom}]}\texttt{Auteur})$|||
|:-:|:-:|:-:|
|**Titre**| **Nom**| **Prénom**|
|Madame Bovary |Flaubert | Gustave|
|Le père Goriot |Balzac (de)| Honoré|

#### Opérations ensemblistes

##### Jointure symétrique

Pour croiser des informations en provenance de plusieurs tables :

- Principe : récupérer les lignes de deux tables lorsque ces lignes ont une caractéristique commune.
- En théorie, ces tables ont des shémas disjoints.
- Syntaxe :

```SQL linenums="1" 
SELECT * FROM table1
INNER JOIN table2
ON table1.colt1 = table2.colt2
```

Noms des clients et leurs numéros d'achats :

```SQL linenums="1"
SELECT Orders.OrderID , Customers.CustomerName
FROM Orders
INNER JOIN Customers
ON Orders.CustomerID = Customers.CustomerID;
```

!!!example "Exercice (improvisé)"

    Pour chaque achat, donner le prix payé.

???tip "Correction"

    ```SQL linenums="1"
    SELECT OrderDetailID, P.Price * OD.Quantity FROM OrderDetails AS OD
    INNER JOIN Products AS P
    ON OD.ProductID = P.ProductID
    ```

#### Jointure et intersection

On peut se servir d'une jointure pour obtenir l'intersection de deux tables de même schéma.

Donner les villes où il y a des clients et des fournisseurs :

```SQL linenums="1"
SELECT C.City FROM Customers AS C
INNER JOIN Suppliers AS S
ON S.City = C.City
```

#### Jointures $\color{blue}\texttt{LEFT JOIN}$

Hors programme pour l'**ITC**

- La jointure que nous avons vue $\color{blue}\texttt{R1 JOIN R2 ON A=B}$ est symétrique : les enregistrements de $\texttt{R1}$ qui ont une valeur de $\texttt{A}$ qui n'existe pas dans $\texttt{D}$ n'apparaissent pas dans le résultat.
- Parfois on a besoin de faire apparaître, en plus du résultat de la jointure symétrique, les valeurs de $\texttt{R1}$ : c'est la _jointure gauche_ ($\color{blue}\texttt{LEFT JOIN}$)

!!!example ""

    Donner le nom et la ville de chaque client et compléter l'information par le nom des fournisseurs qui vivent dans la même ville que lui.

    ```SQL linenums="1"
    SELECT C.CustomerName , C.City , S.SupplierName
    FROM Customers AS C LEFT JOIN Suppliers as S
    ON C.City=S.City
    ```

!!!tip "Remarque"

    Certaines des lignes obtenues sont complètes, d'autres pas.

#### Jointure RIGHT JOIN

Hors programme pour l'**ITC**

Il existe de même une _jointure droite_ de mot clé $\color{blue}\texttt{RIGHT JOIN}$

!!!example ""

    Donner toutes les informations sur les employés et les ventes qu'ils
    ont éventuellement assurées.

    ```SQL linenums="1"
    SELECT Orders.OrderID , Employees.LastName ,
    Employees.FirstName
    FROM Orders
    RIGHT JOIN Employees
    ON Orders.EmployeeID = Employees.EmployeeID
    ORDER BY Orders.OrderID;
    ```

    On constate que le pauvre Adam West n'a effectué aucune vente.

#### Auto-jointure

!!!example "Exercice"

    Donner tous les couples de clients qui vivent dans la même ville. On n'accepte pas les " doublons " comme (Dupont, Durand) et (Durand, Dupont) ni les identifiants identiques comme (Dupont, Dupont) sauf si il s'agit de personnes différentes (le nom n'est pas une clé primaire).

???tip "Solution"

    ```SQL linenums="1"
    SELECT C1.CustomerName, C2.CustomerName, C1.City FROM 
    Customers AS C1 
    INNER JOIN Customers AS C2 
    ON C1.City = C2.City WHERE C1.CustomerID < C2.CustomerID
    ```

#### Jointure sur plus de deux tables

Pour chaque client, indiquer ses achats et l'entreprise qui a livré ces achats :

```SQL linenums="1"
SELECT Orders.OrderID , Customers.CustomerName ,
    Shippers.ShipperName
FROM (( Orders
INNER JOIN Customers
ON Orders.CustomerID = Customers.CustomerID)
INNER JOIN Shippers
ON Orders.ShipperID = Shippers.ShipperID );
```

!!!tip "Remarque"

    Les SGBD peuvent être plus ou moins permissifs sur le parenthésage. Mais l'opération de jointure interne est associative.
