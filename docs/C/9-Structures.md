# Définition de types (Structures)

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    - Ce Wiki de [Wikipedia](https://fr.wikibooks.org/wiki/Programmation_C/Types_avancés)
    - Ce cours de [OpenClassRoom](https://openclassrooms.com/fr/courses/19980-apprenez-a-programmer-en-c/16119-creez-vos-propres-types-de-variables)
    - Ce cours d'[Anne Canteaut](https://www.rocq.inria.fr/secret/Anne.Canteaut/COURS_C/chapitre2.html)

## Structures

### Définition

!!!quote "Structure"

    Une _structure_ est, comme un tableau, un objet de type agrégé, c'est à dire constitué d'un ensemble de valeurs.

- Mais à la différence d'un tableau, ces valeurs peuvent être de types différents.
- Par ailleurs, le repérage d'un objet dans la structure se fait, non plus selon un indice, mais par ce qu'on appelle un _nom de champ_, c'est à dire un identificateur.
- On ne peut pas parcourir les champs d'une structure au moyen d'une boucle (alors qu'on le fait pour les tableaux).
- Une _structure_ est donc une suite finie d'objets de type différents (les _champs_ ou _membres_). Le nombre de ces différents champs est limité à $127$ soit $2^7 −1$.

!!!example "Exemple d'utilisation"

    Une structure permet d'accroître la clarté des programmes en rassemblant dans un même objet des informations possédant un lien. Par exemple :
    
    - Les différentes informations associées à une personne : nom, prénom, adresse, sexe, numéro de SS...
    - les différentes informations associées à un point du plan : ses coordonnées, et, s'il s'agit d'un pixel, ses composantes RVB.

### Un mot sur les unions (Hors programme)

- Une _union_ est un moyen artificiel de considérer un même objet avec différents types.
- A chaque type se trouve associé, comme pour les structures, un nom de champs.
- Mais dans une structure, les champs ne s'excluent pas l'un, l'autre : ils sont juxtaposés. Dans une union, les champs ne peuvent cooexister en même temps : ils sont superposés.
- Les unions s'utilisent :
    - pour interpréter de façons différentes un même motif binaire.
    - pour économiser de la mémoire en faisant occuper à des instants différents un même emplacement par des informations de types différents.
- L'union a un caractère peu portable et reste limitée à des utilisations particulières.

### Définire de nouveaux types

Les unions et structures peuvent être considérés comme des types définis par le programmeur.

### Modéle et déclaration

Un _modèle de structure_ est la description d'une structure :

```C linenums="1"
struct modele {
    type 1 membre 1;
    type 2 membre 2;
    ...
    type n membre n;
};// ce point virgule est impératif !!
```

- Une fois le modèle déclaré, on déclare un objet de ce type avec :

```C linenums="1"
struct modele objet ;
```

- Cette syntaxe qui déclare dans la foulée le type `modele` et un objet `representant` de ce type est également possible :

```C linenums="1"
struct modele {
    type 1 membre 1;
    type 2 membre 2;
    ...
    type n membre n;
} representant ;
```

!!!example ""

    On définit une structure `temps` qui modélise un triplet (heure,minute,secondes) :

    ```C linenums ="1"
    struct temps {
        unsigned int heures ;
        unsigned int minutes;
        double secondes;
    };// ce point virgule est impé ratif !!
    ```

    La déclaration d'un objet de type `temps` se fait ainsi :

    ```C linenums ="1"
    1 struct temps t;
    ```

### Affectation

Il reste à affecter les différents champs d'un objet `obj` de type structuré modele. Il y a deux possibilités :

- Syntaxe de **type tableau** `struct modele obj = {v1, ..., vn }` pour une déclaration/affectation simultanée. Les types de $v_1,...,v_n$ sont ceux des champs de `modele` et respecte l'ordre des champs dans la structure.
- Syntaxe **pointée** `obj.membre 1 = v1` ,..., `obj.membre n = vn`

!!!example ""

- Affichage d'une structure de temps :

```C linenums="1"
void affichage ( struct temps t){
    printf ("temps=%uh %umin %fs\n",t.heures,t.minutes,t.secondes);
}
```

- Affectation

```C linenums="1"
void main() {
    struct temps t1 = { 1, 45, 30.560 };//Syntaxe type tableau
    affiche (t1) ;
    struct temps t2;
    t2.heures = 5;//Syntaxe de type pointée
    t2.minutes = 23;
    t2.secondes = 10.06;
    affiche (t2) ;
}
```

On obtient :

```bash title="Rendu"
temps =1h 45min 30.560000s
temps =5h 23min 10.060000s
```

### Alias de structure

- L'écriture `struct temps t` est un peu lourde. Il est plus agréable de faire des déclarations sous la forme temps t.
- Pour celà, on utilise l'instruction `typedef` dont voici la syntaxe :

```C linenums ="1"
typedef struct nomModele nomAlias;
struct nomModele{
// les diff é rents champs et leurs types
};
```

- `typedef` indique que nous créons un alias de structure, `struct nomModele` est le nom de la structure et `nomAlias` celui de son alias

!!!example ""

    ```C linenums ="1"
    typedef struct temps time;
    struct temps {
        unsigned int heures ;
        unsigned int minutes;
        double secondes;
        };// ce point virgule est impé ratif !!
    ```

    - Fonction d'affichage

    ```C linenums ="1"
    void affiche (time t){
        printf ("temps=%ih %imin %fs\n",t.heures,t.minutes,t.secondes);
    }
    ```

    - Affectation

    ```C linenums ="1"
    void main() {
        time t2 ;
        t2.heures = 5;
        t2.minutes = 23;
        t2.secondes = 10.06;
        affiche(t2);
    }
    ```

### Pointeur de structure

- Déclaration sans surprise d'un pointeur sur notre type structuré `temps` :

```C linenums="1"
time t ;
time∗p = &t;// ou time ∗p = &t
```

- Il y a en revanche un peu de sucre syntaxique; on peut affecter les champs de l'objet sur lequel pointe le pointeur de deux façons :`

```C linenums="1"
(∗p1). heures = 12;// façon classique , noter la parenthèse
p1−>minutes = 41;// façon plus élégante
p1−>secondes = 23.56;
```

- `*p1.heures = 15`; produit une erreur de compilation.

### Initialisation sélective

- On peut recourir à une _initialisation sélective_ en nommant bien les champs. Dans ce cas, l'ordre des membres n'a pas d'importance (comme pour le passage des paramètres nommés en $\texttt{Python}$) :

```C linenums="1"
time t = { .secondes = 30.560, . minutes = 45,
    .heures = 1 };
```

- On peut aussi mélanger les plaisirs :

```C linenums ="1"
time t = { .minutes = 45, 30.560 };
```

- Il manque la valeur pour les heures : par défaut, elle vaut 0. Ensuite on a une initialisation sélective des minutes, puis on revient à l'initialisation séquentielle pour les secondes.
- L'initialisation séquentielle reprend automatiquement à la dernière initialisation sélective.
- A retenir : Si certains membres d'une structure ne se voient pas attribuer de valeurs durant l'initialisation d'une variable, ils seront initialisés à zéro ou, s'il s'agit de pointeurs, seront des pointeurs nuls.

### Structure et mémoire (Hors programme)

Les différents champs d'une structure sont contigus en mémoire.

- Dans certaines implémentations, les int sont alignés sur des adresses multiples de 4 (leur premier octet est nécessairement sur une adresse multiple de $4$).
- Considérons, dans une telle implémentation, la déclaration

```C linenums ="1"
struct essai { int n;// 4 octets
    char c ;// 1 octet
    int p; // 4 octets
};// voir ci −dessous le stockage en mémoire :
```

$\!\underbrace{\!\square\!\square\!\square\!\square\!}_{\!n\!}\underbrace{\!\square\!}_{\!c\!}\square\!\square\!\square\!\underbrace{\square\!\square\!\square\!\square\!}_p$

On remarque l'utilisation d'_octets de remplissage_ entre les champs `.c` et `.p`. Donc la taille de la structure est $12$ octets et non $9$.

### Structures et tableaux de structures

Supposons que notre implémentation impose aux `int` de commencer à des adresses multiples de 4.

- Rappel : Les différents champs d'une structure sont contigus en mémoire.
- Considérons

```C linenums="1"
struct essai { int n;// 4 octets
    char c ;// 1 octet
};
```

- En théorie, $5$ octets suffisent pour représenter un objet de type essai. Mais un tel objet peut se retrouver dans un tableau...
- La règle de contguïté entre deux éléments successifs d'un tableau impose la violation de la règle des adresses en multiple de $4$.
- Pour cette raison, les objets de type `essai` font $4+1+3$ soit $8$ octets DANS cette implémentation.

### Structure et mémoire ♥

- On le voit, la taille des objets structurés dépend de l'implémentation.
- Pour se simplifier la vie : on considère en CPGE que la taille d'un type structuré est la somme des tailles de ses différents champs.

### Affectation et retour par copie

- Affectation par copie :

```C linenums="1"
struct S s= {...}; //déclaration et initialisation de s
struct S t ; // t non initialisée
t = s; // t reçoit une copie de s
```

- Retour d'une valeur structurée :

```C linenums="1"
struct S f (...) {
    ...
    struct S t = ... ;// t est initialisée
    return t ; // t est copié dans l'emplacement
    // qui accueille le retour de la fonction
}
```

### Un mot sur le passage de structures en paramètres

- En $\texttt{C}$, le passage des arguments s'effectue par _valeur_. Cela signifie que la fonction travaille avec une copie de l'argument. Si l'argument est de grande taille, on utilise donc beaucoup de mémoire pour stocker une information redondante.
- Il est préferable de passer en paramètre des fonctions, non pas une structure, mais un pointeur sur cette structure. Ce pointeur sert de « poignée » à l'objet et sa taille est fixe quelle que soit la structure pointée.
- Dans le même soucis de réduire le trafic dans la mémoire, on ne renvoie pas une structure mais, de préférence, un pointeur sur cette structure.

### Déclaration anticipée

La norme autorise qu'on déclare un nom de structure sans sa description :

```C linenums="1"
struct enreg ; // enreg est un nom de structure
// sa description sera donnée plus tard
```

Dans la portée de l'identificateur de structure, il est permis de fournir plus tard la description du type :

```C linenums="1"
struct enreg ;// dé claration du type
...
struct enreg {char c ; int n;}; // description du type
```

### Dépendance mutuelle

La déclaration anticipée est indispensable en cas de dépendance mutuelle entre $2$ structures.

!!!example ""

    ```C linenums="1"
    struct machin; // déclaration anticipée
    struct chose {... // definition normale de chose
        ...
        struct machin ∗adm;// pointeur sur objet type machin
    };
    struct machin {... // def. tardive de machin
        ...
        struct chose ∗adc;// pointeur sur objet type chose
    }
    ```

### Fonctions mutuellement récursives

Il n'y a pas que pour les dépendances mutuelles entre structures que la déclaration anticipée est indispensable.

- La déclaration anticipée est indispensable en cas de récursion mutuelle entre deux fonctions.

!!!example "Exercice comme exemple"

    Écrire deux fonctions mutuellement récursives bool pair(unsigned) et bool impair(unsigned) .

!!!note "Remarque de M.Noyer"

    Il faut vraiment que je place ce transparent ailleurs que dans le cours sur les structures !

    (Voir la page OCaml pour un exemple)

## Type énuméré

### Présentation

Il s'agit de définir un type par la liste exhaustive de ses habitants.

!!!example ""

    ```C linenums="1"
    enum couleur = {rouge, vert, bleu};
    ```

En fait, les valeurs représentées sont des entiers. Vérifions le avec :

```C linenums="1"
enum couleur {rouge, vert , bleu};

void main(){
    enum couleur c;
    c = bleu;
    printf ("c = %d\n",c);
}
```

Après compilation/exécution :

```bash title="Rendu"
c = 2
```

On comprend que `rouge` est représenté par défaut par $0$, `vert` par $1$ et `bleu` par $2$.

### Modifier les valeurs par défaut

- Il est possible de modifier les valeurs par défaut lors de la déclaration :

```C linenums="1"
enum couleur {rouge=11, vert=17, bleu=23};
```

- Alors, la séquence :

```C linenums="1"
enum couleur c; c = bleu;
printf ("c = %d\n",c);
```

- Affiche

```bash title="Rendu"
c=23
```
