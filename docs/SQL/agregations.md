# Agrégations

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d’être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    - Un cours de Quentin Fortier
    - Le tuto SQL du [W3C](https://www.w3schools.com/sql/) (indispensable)
    - Un cours en français [ici]https://sql.sh/cours

## Objectif

Les fonctions d'agrégation dans le lagage SQL permettent d'effectuer des opérations statistiques sur $\color{red}\text{un ensemble d'enrengistrment}$

Contrairement aux autres opérateurs, elles s'appliquent à des ensembles (ou _agrégats_) de lignes et pas seulement à des lignes isolées.

Récupérer des informations concernant un groupe de lignes.

!!!example "Exercice donner pour chaque ville "

    - le nombre de commandes passées par les clients habitant cette ville,
    - le prix le plux cher payé.
    - la moyenne des âges des clients.
    - la somme des totales des commandes des habitants de cette ville.

## Compter

Il s'agit de regrouper les lignes par agrégats présentant une caractéristique commune.

```SQL linenums="1"
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
-- ORDER BY column_name(s); (facultatif : trier)
```

Nombre de clients par pays :

```SQL linenums="1"
-- liste des pays et du nombre
-- de clients qui y vivent */
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country;
```

$\color{blue}\texttt{COUNT(∗)}$ est équivalent à $\color{blue}\texttt{COUNT(CustomerID)}$ car l'identifiant étant une clé primaire, il n'y a pas de $\color{blue}\texttt{NULL}$ dans la colonne.

## Agrégation

Nombre de clients par pays, rangé par ordre décroissant d'effectif

```SQL linenums="1"
SELECT COUNT (CustomerID), Country
FROM Customers
GROUP BY Country
ORDER BY COUNT(CustomerID) DESC;
```

## Les $5$ fonctions d'agrégation

- `AVG()` pour calculer la moyenne sur un ensemble d'enrengistrement
- `COUNT()` pour compter le nombre d'enrengistrement sur une table ou une colonne.
- `MAX()` pour récupérer la valeur maximum d'une colonne sur un ensemble de ligne. Cela s'applique à la fois pour des données numériques ou alphanumérique
- `MIN()` récupérer la valeur minimum de la même manière que `MAX()`
- `SUM()` calculer la somme sur un ensemble d'enrengistrement

!!!example ""

    Pour chaque catégorie de produits donner l'effectif, le prix moyen, le prix le plus bas, le plus élevé et la somme des prix :

    ```SQL linenums="1"
    SELECT CategoryID, AVG(Price) AS Moyenne ,
           Min(Price) AS Inf, Max(Price) AS Sup,
           Count(∗) AS Nb, SUM(Price) AS Somme
    FROM Products
    GROUP BY CategoryID;
    ```

## $\color{blue}\texttt{HAVING}$

Fixer des conditions sur les groupes affichés (par exemple ceux au dessus d'un certain effectif) et non pas sur les enrengistrements affichés (ce qui est le boulot de $\color{blue}\texttt{WHERE}$).

$\color{blue}\texttt{HAVING}$ permet de filtrer les agrégats de lignes en utilisant (uniquement) les fonctions d'aggrégation `SUM`, `COUNT`, `AVG`, `MIN`, `MAX`.

On filtre donc des agrégats (d'enregistrements) plutôt que des enrengistrements.

```SQL linenums="1"
SELECT column_name(s)
FROM table_name
WHERE condition sur les lignes
GROUP BY column_name(s)
HAVING condition sur les ensembles de lignes
ORDER BY column_name(s);
```

!!!example ""

    Nombres de clients par pays si il y a plus de $5$ clients dans ce pays :

    ```SQL linenums="1"
    SELECT COUNT(CustomerID), Country
    FROM Customers
    GROUP BY Country
    HAVING COUNT(CustomerID) > 5;
    ```

!!!example "Exercice"

    - Donner le pays dans lequel se trouve le plus de clients.
    - Donner le prix (unitaire) moyen des produits délivrés par les fournisseurs de chaque ville si le produit le moins cher dans cette ville côute plus de $20$ dollars (prix unitaire).
