# Compilation séparée

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    - Ce cours d'[Anne Canteault](https://www.paris.inria.fr/secret/Anne.Canteaut/COURS_C/chapitre7.html) sur la compilation séparée.
    - Ce chapitre sur les directives préprocesseur d'un cours de [OpenClassRoom](https://openclassrooms.com/fr/courses/19980-apprenez-a-programmer-en-c/15954-le-preprocesseur)
    - Ce tutoriel sur les [Makefile](https://gl.developpez.com/tutoriel/outil/makefile/)

## Objectif et principe

- Plutôt que de mettre tout le code dans un seul fichier, on le distribue dans plusieurs endroits : des _modules_.
- Dans un fichier donné, on met par exemple des outils de calculs mathématiques, dans un autre des fonctions de manipulation de chaînes de caractères etc.
- Cela permet de bien organiser un projet, d'améliorer la lisibilité du code et de faciliter la _maintenance_ : en cas de changement de quelques lignes de code seulement, on n'a pas besoin de tout recompiler mais seulement les fichiers concernés.

## Rappel sur les directives de préprocesseur

### La directive `include`

- La syntaxe pour inclure l'interface d'un fichier de bibliothèque est de placer son nom entre chevrons comme dans `<stdio.h>`.
- Pour tout autre fichier, et en particulier pour NOS FICHIERS PERSONNELS, on écrit son nom entre guillements ; par exemple `#include "produit.h"`.
- Dans tous les cas, le préprocesseur insère le contenu du fichier cible à la place du `#include nomDuFichier`

### Rappels sur la directive `define`

- Lorsqu'il lit `#define TOTO Tata` , le préprocesseur remplace toute occurrence du premier mot par le second (mais pas dans les commentaires ni les expressions entre guillemets).
- On peut effectuer des calculs arithmétiques en utilisant des constantes introduites par cette directive.

```C linenums="1"
#define LARGEUR 60
#define LONGUEUR 80
#define AIRE ( LARGEUR ∗LONGUEUR)
```

Hors programme.

On peut se servir de `#define` pour définir des _macros_ sans paramètre

!!!example ""

    ```C linenums="1"
    #define HELLO ()    printf("Salut ! \n");\
                        printf("Hi !\n");\
                        printf("Hallo !\n");

    void main(){
        HELLO();
    }
    ```

    - Observer les passages à la ligne avec un antislash « \ » comme en $\texttt{Python}$
    - Après compilation et exécution :

    ```bash title="Rendu"
    Salut !
    Hi !
    Hallo !
    ```

On peut se servir de `#define` pour définir des _macros_ Avec paramètres :

!!!example ""

    ```C linenums="1"
    #include <stdio.h>
    #define APPRECIATION(nom, note)\
            if(note < 10)\
                printf("%s, vous n\'avez pas la moyenne.\n" , nom ) ; \
            else\
                printf("%s, vous êtes reçu !\n", nom);

    void main(){
        APPRECIATION("Dupont", 8);
        APPRECIATION("Durand", 18);
    }
    ```

    - Après compilation et exécution :

    ```bash title="Rendu"
    Dupont, vous n'avez pas la moyenne .
    Durand, vous êtes reçu !
    ```

### Définitions sans valeurs de substitutions ♥

Parfois, on écrit juste :

```C linenums="1"
#define NOM_DU_FICHIER
```

Cette déclaration, combinée avec l'usage des _conditions de préprocesseur_ (comme `# ifndef`) permet d'éviter les inclusions infinie (lorsqu'un fichier `A` importe un fichier `B` qui importe `A`).

## Compilation séparée

### Un projet en deux fichiers

- Dans un $1$er fichier $\texttt{main.c}$ :

```C linenums="1"
#include <stdio.h>
#include "produit.h" // observer la syntaxe

int main(void){
    int a, b, c;
    scanf("%d", &a); scanf("%d", &b);
    printf("\n le produit vaut %d\n", produit(a, b));
    return 0;
}
```

- Dans un $2$nd fichier $\texttt{produit.h}$ (qui n'est pas vraiment un fichier d'en-tête).

```C linenums="1"
int produit(int a, int b){
    return (a ∗ b);
}
```

### Compilation en une fois

- On peut compiler ce projet à $2$ fichiers en une seule fois avec la commande `gcc main.c -o main`
- Le préprocesseur va juste regrouper (en les concaténant et en supprimant les commentaires) les deux fichiers en un seul et produira l'exécutable à partir de ce fichier concaténé.
- Le seul avantage de cette démarche est de rendre le code plus lisible mais on n'a rien gagné en terme de maintenance du projet.

### Introduction d'un fichier d'en-tête

- Séparons les prototypes des corps des fonctions de $\texttt{produit.h}$.
- On crée pour cela un fichier d'interface $\texttt{produit.h}$ contenant les prototypes et un fichier $\texttt{produit.c}$ contenant les codes.
- Dans le fichier $\texttt{produit.c}$, incluons l'en-tête :

```C linenums="1"
#include "produit.h"
int produit(int a, int b){return( a ∗ b );}
```

- Toutes les fonctions de produit.c destinées à être appelées par d'autres fichiers que produit.c lui-même devraient avoir leur prototype listé dans $\texttt{produit.h}$.
- Comme de plus les interfaces listées dans produit.h correspondent à des fonctions dont le code est écrit ailleurs, on peut l'indiquer explicitement par le mot clé `extern`.
- Le fichier d'en-tête $\texttt{produit.h}$ contient donc :

```C linenums="1"
extern int produit(int, int);
```

### Mot clé `extern` pour les fonctions

- Pour une fonction les deux déclarations suivantes sont équivalentes `extern int fct(char c, float x)` et `int fct(char c, float x)`.
- La fonction sera utilisable dans les 2 cas à l'extérieur du fichier source. Son nom devient ce qu'on appelle un _identificateur externe_, c'est à dire qu'il est accessible à l'éditeur de lien.
- Le mot `static` empêche que l'identificateur de la fonction soit utilisé à l'extérieur du fichier source où elle est définie. On parle alors de fonction _cachée_ ou _privée_.

!!!example ""

    `static int fct(char c, float x)...`

### Mot clé `extern` pour les variables

- Le mot clé extern devrait être réservé aux _redéclaration_
- Une déclaration contenant une initialisation constitue toujours une définition. `int x = 2;` et `extern int x = 2;` sont équivalents. `extern` ne sert à rien.
- Une déclaration avec extern sans initialisation constitue toujours une redéclaration (donc jamais une définition) d'une variable pouvant , éventuellement, être définie ailleurs (donc dans le même fichier
source ou un autre). `extern int a`; fait référence à une variable `a` définie ailleurs.
- On retient la règle suivante de bonne pratique :
    - Dans les définitions on n'utilise jamais `extern` et on s'interdit les déclarations multiples au sein d'un même fichier. On peut ou non initialiser.
    - Dans les redéclarations On utilise systématiquement `extern` et on s'abstient de toute initialisation.

### Introduction d'un fichier d'en-tête

Fabriquons le fichier objet relatif au $\texttt{main.c}$ :

```bash
gcc −c main.c
```

Fabriquons le fichier objet relatif au $\texttt{produit.c}$ :

```bash
gcc −c produit.c
```

Compilons l'ensemble

```bash
gcc main.o produit.o −o prod
```

Remarquons qu'on peut toujours compiler tout en même temps :

```bash
gcc produit.c main.c −o prod
```

### Importation unique

- Comme on le constate, le fichier d'en-tête est importé deux fois :
    - une première fois par $\texttt{produit.c}$,
    - une seconde fois par $\texttt{main.c}$
- Dans le fichier exécutable généré par $\texttt{gcc}$, l'inclusion apparaît donc deux fois.
- Pour éviter cela, on crée une constante `PRODUIT_H`.
- Cette constante, partagée par tout le projet, est définie la première fois qu'on inclue le fichier $\texttt{produit.h}$.
- La seconde fois qu'on importe le fichier d'en-tête, la constante est déjà définie et on se débrouille pour empêcher une nouvelle inclusion des prototypes de $\texttt{produit.c}$.
- Il y a une convention de nommage pour cette constante : nom du fichier d'en-tête mis en majuscules (mais sans l'extension $\texttt{.h}$) ; le tout suivi de $\texttt{\_H}$.

### `ifndef`

- L'idée est de mettre tous les prototypes et déclarations diverses de $\texttt{produit.h}$ dans la branche positive d'une instruction conditionnelle du préprocesseur.
- Si la condition est vérifiée, alors ces prototypes et déclarations sont bien ajoutés au fichier généré. Sinon, ils sont ignorés.
- Le fichier produit.h devient alors :

```C linenums="1"
#ifndef PRODUIT_H // si PRODUIT_H non déjà déclarée

#define PRODUIT_H // ... alors on la déclare ! ...

// Et on liste les prototypes à inclure dans les autres fichiers:

int produit(int, int);

#endif // FIN instruction conditionnelle
```

- Dans le fichier $\texttt{produit.h}$ on a ajouté l'instruction de préprocessing `#ifndef PRODUIT_H` dont l'esprit est de signifier « si la constante PRODUIT_H n'a pas déjà été déclarée alors voici les  prototypes à inclure : ... ».
- et on compile en $3$ temps :

```bash
gcc −c main.c
gcc −c produit.c
gcc main.o produit.o −o prod
```

## Makefile

### Présentation

- Il ne rentre pas dans les compétences exigibles des étudiants de CPGE qu'ils sachent écrire un $\texttt{Makefile}$. C'est toutefois un outil très pratique pour automatiser la compilation d'un projet et optimiser le temps passé pour cette opération.
- On ne donne ici que quelques rudiments de cette technique qui nécessiterait un manuel à part entière. Consulter par exemple [developpez.com](https://gl.developpez.com/tutoriel/outil/makefile/) pour plus d'informations.

### Syntaxe

Un Makefile est constitué d'un ensemble de règles de la forme :

```bash
cible : dependance
commandes
```

### Utilisation

Supposons qu'on ait créé le fichier Makefile dans le répertoire courant. Il y a deux possibilités d'appel :

- **Exécution de la première règle rencontrée :** Cela s'obtient très facilement en tapant dans un terminal :

```bash
$ make
```

- **Exécution d'une règle spécifique :** Il suffit de saisir dans un terminal.

```bash
$ make nomDeLaRegle
```

### Évaluation des règles

L'évaluation d'une règle se fait en plusieurs étapes :

- **Analyse des dépendances :** si une dépendance est la cible d'une autre règle du Makefile, cette règle est préalablement évaluée.
- **Exécution des commandes :** Après analyse des dépendances, si
    - la cible ne correspond pas à un fichier existant
    - 2 ou si un fichier dépendance est plus récent que la règle, les différentes commandes sont exécutées.

### Un Makefile minimum

```Makefile
prod : produit.o main.o
        gcc −o prod produit.o main.o

produit.o : produit.c
        #compilation sans lien, avec messages d'erreur et optimisation
        gcc −o produit.o −c produit.c −Wall −O3

main.o : main.c
        #compilation sans lien, avec messages d'erreur et optimisation
        gcc −o main.o −c main.c −Wall −O3
```

- La commande $\texttt{make}$ lancée dans un terminal depuis le répertoire courant cherche à réaliser la première règle rencontrée : $\texttt{prod}$.
- Il y a deux dépendances $\texttt{produit.o}$ et $\texttt{main.o}$ qui sont d'ailleurs des règles.

### Dépendances de la règle $\texttt{prod}$

- On commence par exécuter la règle $\texttt{produit.o}$ ($1$ere dépendance de la règle $\texttt{prod}$). L'unique dépendance de cette règle est un fichier $\texttt{produit.c}$ qui n'est pas une règle. Le Makefile exécute la commande de cette règle $\texttt{produit.o}$ si une des conditions suivantes est réalisée :
    - le fichier $\texttt{produit.o}$ n'existe pas dans le répertoire courant,
    - ou bien le fichier $\texttt{produit.c}$ est plus récent que $\texttt{produit.o}$. Cela signifie que $\texttt{produit.c}$ a été modifié depuis la dernière création du module objet $\texttt{produit.o.}$ Il faut donc reconstruire ce dernier.
- Si une des deux conditions ci-dessus est vérifiée, on exécute la commande `gcc -o produit.o -c produit.c -Wall -O3` (compilation sans édition de lien $\texttt{-c}$, avec Warnings $\texttt{-Wall}$ et optimisation complète $\texttt{-O3}$).
- Une fois cette règle exécutée, on lance la règle $\texttt{main.o}$ qui fonctionne sur le même principe.

### Exécution de la première règle

- Toutes les dépendances de prod étant construites, on exécute la commande associée à la règle $\texttt{prod}$ si :
    - l'un des fichiers objets $\texttt{.o}$ n'existe pas dans le répertoire courant,
    - ou l'un des fichiers objets $\texttt{.o}$ est plus récent que $\texttt{prod}$.
- Dans ce cas, c'est la commande $\texttt{gcc -o prod produit.o main.o}$ qui est lancée

### Nettoyage et régénération

Avec le Makefile précédent, on ne peut pas créer plusieurs exécutables, ni supprimer les fichiers intermédiaires .o (ils restent sur le disque dur) ni forcer la régénération complète du projet.

- En effet, les fichiers objets $\texttt{produit.o}$ et $\texttt{main.o}$ sont restés sur le disque dur. Lorsqu'on relance la commande $\texttt{make}$, on obtient le message suivant :

```bash
make : << prod >> est à jour.
```

On rajoute alors trois règles qui portent, par convention, les noms $\texttt{all}$, $\texttt{clean}$, $\texttt{mrproper}$ :

- $\texttt{all}$ : on la fait suivre de la ou des règles de construction d'exécutables ; pour nous c'est seulement $\texttt{prod}$.
- $\texttt{clean}$ : on décide ici de lui faire supprimer tous les fichiers objets (mais ce pourrait être d'autres types de fchiers).
- $\texttt{mrproper}$ : un nettoyage complet. Le Makefile engendre un fichier exécutable $\texttt{prod}$ et des fichiers objets. On déclare $\texttt{clean}$ comme dépendance, ce qui a pour effet de supprimer les fichiers objets. Puis il reste à supprimer le fichier exécutable $\texttt{prod}$.

### Makefile plus complet

```Makefile
all : prod

prod : produit.o main.o
        gcc −o prod produit.o main.o

produit.o : produit.c
        gcc −o produit.o −c produit.c −W −O3

main.o : main.c
        gcc −o main.o −c main.c −W −O3

clean :
        rm −rf ∗.o

mrproper : clean
        rm prod
```
