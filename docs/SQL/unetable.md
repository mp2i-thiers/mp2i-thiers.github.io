# Requêtes sur une table

!!!tip "Crédits"

    - Un cours de Quentin Fortier
    - Le tuto SQL du [W3C](https://www.w3schools.com/sql/) (indispensable)
    - Un cours en français [ici](https://sql.sh/cours)

## Présentation

### Langages de requêtes

On accède à des informations d'une base de donnée avec un _langage de requêtes_.

On n'utilise ni variable ni boucle contrairement aux langages de programmation habituels.

On écrit (dans un certain langage) $\underline{\textsf{ce qu'on veut obtenir}}$ mais pas $\underline{\textsf{comment l'obtenir}}$. On laisse le SGBD se débrouiller.

!!!example "Somme des capacités de salles"

    D'après un exemple de Q. Fortier. On cherche le nombre de places dans des salles de cinéma climatisées à Marseille.
    
    - En **Python**
    ```Python linenums="1"
    somme = 0
    for salle in liste_des_salles:
        si climatisé(salle) and situé_à_Marseille(salle):
            somme+= capacité(salle)
    ```
    - Dans un **langage de requête** :
    ```SQL
    Calculer la somme des capacités des salles
          de cinéma climatisées à Marseille
    ```

### SQL

Le langage de requêtes le plus utilisé est **SQL** ($S$tructured $Q$uery $L$angage) Plusieurs implémentations (avec des nuances dans la syntaxe)

- En $MPI$, nous utilisons $\text{MySQL}$ open source, gratuit.
- $\text{Oracle Database}$ : propriétaire, payant
- $\text{PostgreSQL}$ : open source, gratuit.
- Pour les cours d'ITC, nous utilisons $\text{SQLite}$ et le navigateur léger [$\text{DB Browser for SQLite}$](https://sqlitebrowser.org).

### Syntaxe SQL

Chaque requête est terminée par un point virgule "$\texttt{ ; }$"

**SQL** n'est pas sensible à la casse (majuscules/minuscules) mais il est d'usage d'écrire les mots clés en majuscules et les noms de tables et colonnes en minuscules.

### Types SQL

Les attributs peuvent être de type

- $\color{blue}\texttt{INT}$ entier
- $\color{blue}\texttt{CHAR(k)}$ chaîne d'au plus $k$ caractères
- $\color{blue}\texttt{FLOAT}$ (nombre flottant)
- $\color{blue}\texttt{BOOLEAN}$ : bouléen (en fait $0$ ou $1$)
- D'autres types existent comme un type $\color{blue}\texttt{TIME}$ mais le programme se limite aux $4$ précédents.
- Pour les dates, conformément au programme, on utilise des chaînes de caractères au format $\text{AAAA-MM-JJ}$ : l'ordre lexicographique correspond alors à l'ordre chronologique. De même, les horaires sont écrits au format $\text{HH-MM-SS}$.

### Création

La syntaxe de création de table n'est pas au programme. On crée une table $\texttt{Utilisateur}$ avec une clé primaire $\texttt{id}$ qui est incrémentée automatiquement à chaque nouvel utilisateur :

!!!example "Exemple du cours de $\text{MPI}$"

    ```SQL linenums="1"
        CREATE TABLE utilisateur
    ( id INT AUTO_INCREMENT ,
      PRIMARY KEY(id),
      nom VARCHAR (100) ,
      prenom VARCHAR (100),
      date_naissance DATE ,
      pays VARCHAR (255),
      code_postal INT (5) );
    ```

    $\text{MPI}$ : Le type $\color{blue}\texttt{VARCHAR(100)}$ indique que les chaînes de caractères
    ont au plus $100$ lettres contrairement à $\color{blue}\texttt{CHAR(100)}$ dont l'occupation
    en mémoire est figée.

### Insertion

Syntaxe hors programme

```SQL linenums="1"
INSERT INTO ‘utilisateur‘
(‘nom‘, ‘prenom‘, ‘date_naissance‘, ‘pays‘,
‘ville‘, ‘habite_marseille‘)
VALUES
('DUPONT ','Pierre ','2002:11:30 ','UK','LONDON ','0'),
('CAGOLE ','MARIE ','2001:02:23 ','F','ISTRES ','1')
```

## Interrogation de la table

### Contexte

Une fois la table créée et remplie, on pose des questions sur son contenu.

Pour faire tourner les exemples, se rendre sur [W3C](https://www.w3schools.com/sql/)

### Fonction identité

Afficher toutes les colonnes de la table client

```SQL linenums="1"
SELECT * FROM Customers;
```

### Choix de colonnes

#### Projection

On ne conserve que certaines colonnes, par exemple le nom de client et sa ville :

```SQL linenums="1"
SELECT CustomerName , City FROM Customers;
```

Il s'agit d'une _projection_ sur une ou plusieurs colonnes.

### Filtrer des lignes

#### Clause $\color{blue}\texttt{WHERE}$ (Sélection)

On ne conserve que certaines lignes dont les caractéristiques sont filtrées dans la clause $\color{blue}\texttt{WHERE}$. Il s'agit d'une _Sélection_.

!!!example ""

    Donner tous les renseignement sur les clients mexicains :

    ```SQL linenums="1"
    1 SELECT * FROM Customers
    2 WHERE Country='Mexico ';
    ```

### Filtrer des colonnes et des lignes

On peut enlever des lignes ET des colonnes.

!!!example ""

    Donner toutes les villes ou vivent des clients en Grande-Bretagne :
    
    ```SQL linenums="1"
    SELECT City FROM Customers
    WHERE Country='UK';
    ```

    <p align='center'><img src='/images/unetable1.png'/></p>
    

### Enlever des doublons

#### $\color{blue}\texttt{DISTINCT}$

On a vu que le SGBD travaille avec des multi-ensembles. Il n'est donc pas rare que les requêtes de projection renvoient des doublons (contrairement aux projections du cours de maths).

Le mot clé $\color{blue}\texttt{DISTINCT}$ supprime les doublons.

!!!example ""

    Donner sans doublon les villes des clients anglais.

    ```SQL linenums="1"
    SELECT DISTINCT City FROM Customers
    WHERE Country='UK';
    ```
    
    <p align='center'><img src='/images/unetable2.png'/></p>

    _Figure – Une seule fois Londres_

### Opérateurs de comparaison

- $\color{blue}\texttt{=}$ (et surtout pas $\color{blue}\texttt{==}$)
- $\color{blue}\texttt{<}$, $\color{blue}\texttt{<=}$
- $\color{blue}\texttt{!=}$ (ou son équivalent $\color{blue}\texttt{<>}$)
- $\color{blue}\texttt{AND}$, $\color{blue}\texttt{OR}$, $\color{blue}\texttt{NOT}$
- $\color{blue}\texttt{LIKE}$ (voir plus loin)
- $\color{blue}\texttt{IS NULL}$ (pour repérer les cases vides ou non renseignées) ; $\color{blue}\texttt{IS NOT}$
- $\color{blue}\texttt{NULL}$ (pour repérer les cases non vides) ;

### Calcul avec des colonnes

#### Renommage

Mot clé $\color{blue}\texttt{AS}$

La somme de la colonne Quantité avec le numéro de produit (ce qui ne signifie rien, bien sûr)

```SQL linenums="1"
SELECT ProductID + Quantity AS Somme_DEBILE
FROM OrderDetails;
```

Le renommage permettra d'utiliser ce résultat dans des requêtes plus complexes.

```SQL linenums="1" title="Calcul de carré ou de racine"
SELECT POW(4, 2), POW(4, 0.5);
```

Observons que dans ce cas précis, on veut juste un résultat numérique sans lien avec aucune table. D'où l'absence de $\color{blue}\texttt{FROM}$.

### Comparaison

#### Opérateur $\color{blue}\texttt{LIKE}$

$\color{blue}\texttt{LIKE}$ est utilisé pour chercher un motif particulier dans une colonne dont le domaine est $\color{blue}\texttt{CHAR}$. Deux jokers sont utilisés en conjonction avec $\color{blue}\texttt{LIKE}$

- Le signe de pourcentage $\%$ représente zéro, un ou plusieurs caractères.
- L'underscore $\_$ représente un seul caractère.

```SQL linenums="1"
-- clients dont le nom commence par a
SELECT * FROM Customers
WHERE CustomerName LIKE 'a%';

/* clients dont le nom a n pour
2ème lettre et se termine par s*/
SELECT * FROM Customers
WHERE CustomerName LIKE '_n%s';
```

### Trier avec $\color{blue}\texttt{ORDER BY}$

Pour trier le résultat attendu par ordre croissant (par défaut -mot clé $\color{blue}\texttt{ASC}$) ou décroissant (mot clé $\color{blue}\texttt{DESC}$).

Quand plusieurs colonnes sont indiquées, le tri se fait par ordre lexicographique.

```SQL linenums="1"
SELECT column1 , column2 , ...
FROM table_name
ORDER BY column1 , column2 , ... ASC|DESC;
```

Trier les clients par odre décroissant de pays et croissant de nom :

```SQL linenums="1"
1 SELECT * FROM Customers
2 ORDER BY Country DESC , CustomerName ASC;
```

Coût d'un tri en $O (n \times log(n))$.

### Limiter le nombre de lignes

#### $\color{blue}\texttt{LIMIT}$ pour MYSQL

Attention, ce code **SQL** fonctionne avec $\text{MySQL}$ mais pas $\text{SQL SERVER}$. Bien indiquer sur la copie qu'on travaille en $\text{MySQL}$ $!!$ Voir ce qu'en dit le [W3SCHOOL](https://www.w3schools.com/sql/sql_top.asp)

Syntaxe en $\text{MYSQL}$ et $\text{SQLite}$ (diffère de $\text{ORACLE}$)

```SQL linenums="1"
SELECT column_name(s)
FROM table_name
WHERE condition
LIMIT number;
```

Seulement les $3$ premières lignes de la réponse :

```SQL linenums="1"
SELECT * FROM Customers LIMIT 3;
```

Affiche les lignes $0,1,2$. Essayer [ici](https://www.w3schools.com/sql/trymysql.asp?filename=trysql_select_limit).

#### Donner un point de départ : $\color{blue}\texttt{OFFSET}$

Seulement $3$ lignes après la sixième (incluse) :

```SQL linenums="1"
SELECT * FROM Customers LIMIT 3 OFFSET 6;
```

Cela affiche les lignes $6,7,8$. Noter la syntaxe : d'abord $\color{blue}\texttt{LIMIT}$ ensuite
$\color{blue}\texttt{OFFSET}$.

Même chose avec une syntaxe abrégée :

```SQL linenums="1"
SELECT * FROM Customers LIMIT 6, 3
```

Noter qu'on met alors le $\color{blue}\texttt{OFFSET}$ avant le nombre de lignes. Essayer [ici](https://www.w3schools.com/sql/trymysql.asp?filename=trysql_select_limit).

$\color{blue}\texttt{LIMIT k OFFSET 0}$ est équivalent à $\color{blue}\texttt{LIMIT k}$

### Cases vides

#### Le mot clé $\color{blue}\texttt{NULL}$

$\color{blue}\texttt{NULL}$ est un mot clé indiquant une case vide.

Deux opérateurs y sont associés :

- $\color{blue}\texttt{IS NULL}$ (pour repérer les cases vides ou non renseignées) ;
- $\color{blue}\texttt{IS NOT NULL}$ (pour repérer les cases non vides) ;

Les noms de clients, celui de leur contact et leur adresse pour les clients dont le champ Adress est non vide

```SQL linenums="1"
SELECT CustomerName , ContactName , Address
FROM Customers
WHERE Address IS NOT NULL;
```

### Application d'une fonction d'agrégation

Il y en a $5$ à connaître :

|Correspondances||
|:--:|:--:|
|Algèbre relationnelle|$\color{blue}\texttt{SQL}$|
|_comptage_ ou _cardinal_|$\color{blue}\texttt{COUNT}$|
|_max_| $\color{blue}\texttt{MAX}$|
|_min_| $\color{blue}\texttt{MIN}$|
|_somme_| $\color{blue}\texttt{SUM}$|
|_moyenne_| $\color{blue}\texttt{AVG}$ (pour "average ")|

Ces fonctions peuvent être utilisées pour des informations statistiques sur TOUTE la table ou bien les mêmes informations mais sur les éléments d'une PARTITION de la table (ce qu'on appelle des _agrégats_)

### Les fonctions $\color{blue}\texttt{MIN}$,$\color{blue}\texttt{ MAX}$,$\color{blue}\texttt{ SUM}$

Syntaxe :

```SQL linenums="1"
SELECT MAX(nom_colonne) FROM table
```

Retourne une table d'une seule ligne et une seule colonne dont le nom est $\texttt{MAX(nom}\_ \texttt{colonne)}$

Donner le prix minimum parmi les produits et renommer le résultat

```SQL linenums="1"
SELECT MIN(Price) AS SmallestPrice
FROM Products;
```

!!!example "Exercice"

    Idem avec prix maximum

Donner la somme des prix unitaires de la table produit.

```SQL linenums="1"
SELECT SUM(Price) AS Somme
FROM Products;
```

Donner les produits dont le prix unitaire est maximum (plusieurs réponses possibles) (attendre d'avoir vu les requêtes imbriquées)

### Moyenne

Le prix unitaire moyen des produits :

```SQL linenums="1"
SELECT AVG(Price) AS Moyenne
FROM Products
;
```

### Compter

La fonction $\color{blue}\texttt{COUNT}$ a un comportement particulier

- $\color{blue}\texttt{COUNT(a)}$ : Compte le nombre de fois que $a$ est différent de $\color{blue}\texttt{NULL}$.
- Souvent on compte le nombre total d'enregistrements avec $\color{blue}\texttt{COUNT(*)}$.

!!!example  "donner le nombre de clients"

    ```SQL linenums="1"
    SELECT COUNT (*) AS Nombre_de_clients
    FROM Customers;
    ```

### Le mot clé $\color{blue}\texttt{IN}$

#### Un raccourci pour éviter de multiples conditions $\texttt{OR}$

Pas explicitement au programme (mais pas explicitement interdit)

- Syntaxe $1$ :

```SQL linenums="1"
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1 , value2 , ...);
```

- Syntaxe $2$ : Dans des requêtes imbriquées (patience)

```SQL linenums="1"
SELECT column_name(s)
FROM table_name
WHERE column_name IN (SELECT STATEMENT );
```

!!!example ""

    Tous les clients allemands, français et anglais :

    ```SQL linenums="1"
    SELECT * FROM Customers
    WHERE Country IN ('Germany ', 'France ', 'UK');
    ```

    Tous les clients qui sont dans le même pays qu'au moins un
    fournisseur : (patience, on en parlera quand on verra les requêtes
    imbriquées)
