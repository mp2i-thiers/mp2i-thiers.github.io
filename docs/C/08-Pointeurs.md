# Les pointeurs

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    - Ce cours de [OpenClassRoom](https://openclassrooms.com/fr/courses/19980-apprenez-a-programmer-en-c/15417-a-lassaut-des-pointeurs) (Renvoie sur une page vide  ¯\\_(ツ)_/¯ )
    - Un autre de [Zeste de savoir](https://zestedesavoir.com/tutoriels/755/le-langage-c-1/1043_aggregats-memoire-et-fichiers/4277_les-pointeurs/)
    - Un cours d'[Anne Canteaut](https://www.rocq.inria.fr/secret/Anne.Canteaut/COURS_C/chapitre3.html)
    - Et toujours [Wikipedia](https://fr.wikipedia.org/wiki/Pointeur_(programmation))

## Pointeurs

### Accès à la mémoire

- Un entier naturel codé en binaire peut-être vu comme :
    - ce qu'il est : un nombre destiné par exemple à faire l'objet d'opérations arithmétiques,
    - une adresse mémoire. Dans ce cas l'utilisation arithmétique du nombre n'est pas ce qui nous intéresse.
- Dans les processeurs ont donc été créés des _registres d'adresses_ et dans les langages de programmation, un type dédié.
- Pointeurs : Apparus dans $\texttt{Aigol 68}$. Le langage $\texttt{C}$ y a ajouté l'_arithmétique des pointeurs_ : quand on incrémente un tel pointeur, il n'est en fait pas forcément incrémenté de un, mais de la taille du type pointé. Cette arithmétique (hors programme) permet donc de se déplacer dans la mémoire.
- L'utilisation des pointeurs permet d'avoir accès à la mémoire. On peut se déplacer de case mémoire en case mémoire. Avantage : optimisations sur l'utilisation de la mémoire ou la performance.

### Syntaxe

#### Déclaration

- Syntaxe :

```C linenums="1"
1 type *nom du pointeur;
```

- L'astérisque peut être entourée d'espaces et placée n'importe où entre le type et l'identificateur.

```C linenums="1"
int *ptr ; //autorisé
int *ptr ; //autorisé
int *ptr ; //autorisé
```

#### Initialisation

- Comme les autres variables, un pointeur ne possède pas de valeur par défaut. pour lui en attribuer une, on utilise l'opérateur d'_adressage_ (ou de _référencement_) `&`.
- Déclaration puis affectation

```C linenums="1"
int a = 10;
int *p;
p = &a;
```

- Déclaration et affectation dans la foulée

```C linenums="1"
int a = 10;
int *p = &a;
```

- Savoir faire : déclaration et initialisation d'un pointeur dans un type donné ♥

#### Affichage des adresses

Avec :

```C linenums="1"
int a =10;
int *p = &a;
printf ("a=%d, &a=%p, &p=%p\n", *p, p, &p);
```

On obtient :

```bash title="Rendu"
a=10, &a=0x7ffc7aaa612c, &p=0x7ffc7aaa6130
```

|nom| adresse| valeur|
|:-:|:-:|:-:|
|$p$| $\text{0x7ffc7aaa6130}$| $\text{0x7ffc7aaa612c}$|
||...||
|$a$| $\text{0x7ffc7aaa612c}$| $10$|

#### Indirection ♥

L'opérateur d'indirection (ou de _déréférencement_) `*` prend un pointeur comme opérande et se place juste avant celui-ci. On accède ainsi à la valeur de l'objet référencé par le pointeur, aussi bien pour la lire que pour la modifier.

```C linenums="1"
int a = 3;
int *p = &a;// '*' sert à la déclaration
printf ("a=%d\n", *p);// '*' sert au déréférencement
*p = 20;// '*' sert au déréférencement
printf ("a=%d\n", a);// a a été modifié
```

On obtient

```bash title="Rendu"
a=3
a=20
```

#### Les différentes casquettes de `*`

L'opérateur `*` sert au moins à trois choses distinctes :

- à la déclaration de pointeurs : `int *p = &i`;
- Au déréférencement : `*p=20; // i prend la valeur 20`
- à la multiplication `3*5`;

#### Pointeurs et constantes

Pour info :

- La règle du mot-clé const est qu'il s'applique sur ce qui est immédiatement à sa gauche, et que s'il n'y a rien à gauche alors ça s'applique sur l'élément immédiatement à droite
- Un pointeur peut être déclaré constant :

```C linenums="1"
int *const ptr ; /*Un pointeur constant sur int. non ct */
```

L'étoile est entre le type et le mot clé `const`

- Un pointeur peut pointer sur une constante

```C linenums="1"
int const *ptr ; /*Pointeur sur int constant. */
```

L'étoile est entre le mot clé `const` et le nom du pointeur

- Pointeur constant sur objet constant.

```C linenums="1"
int const *const ptr ; /*Pointeur constant sur int ct . */
const int *const ptr ; /*Pointeur ct sur int ct . */
```

#### Piège à la déclaration

- On l'a vu `int* p;` et `int *p;` sont compris de la même façon par le compilateur.
- `int *p;` permet au lecteur de savoir que `*p` est un entier. La forme avec l'étoile collée au nom est préférable.
- `int* x,y`; ne déclare pas deux pointeurs sur entier mais un pointeur sur entier et un entier. On le comprend mieux en écrivant `int *x,y;`

### Utilité

#### Utilité♥

- On peut utiliser les pointeurs pour faire des _effets de bords_ en les passant en paramètres de fonctions. Ex : passer un pointeur sur une variable _x_ en paramètre de _f_ , permet d'avoir accès au contenu de _x_ et donc de le modifier (voir $2$ slides suivants).
- Les pointeurs sont une des façons de contourner l'obligation qu'une fonction $\texttt{C}$ ne renvoie qu'une seule valeur. L'autre façon courante étant l'utilisation de _structure_ (ou enregistrement).
- On peut les utiliser aussi pour stocker des adresses mémoires allouées _dynamiquement_ (_i.e._ au run time) par l'application.

#### Conséquence du passage par valeur

```C linenums="1"
int incremente( int y){
    y = y+1;
    return y;
}
int main(){
    int y =3;
    printf ("y=%d,incremente(y)=%d", y,incremente(y));
    printf ("; y=%d\n",y);
    return 0;
}
```

```bash title="Rendu"
y=3,incremente(y)=4; y=3
```

- Le contenu de la variable **y** de **main** n'a pas été modifié par l'appel de **incremente**. On dit que le passage de l'argument se fait _par valeur_, ce qui signifie que c'est une copie de **y** qui est passée en
paramètre de la fonction. Pour changer **y** on utilisera son adresse, c'est à dire un _pointeur_.

```C linenums="1"
int incremente(int *y){
    *y = *y+1;
}
int main(){
    int y =3;
    printf ("y=%d\n", y);
    incremente(&y);
    printf ("Arpès incremente(y) : y=%d\n",y);
    return 0;
}
```

Après compilation/exécution :

```bash title="Rendu"
$./a.out$
y=3
Après incremente(y) : y=4
```

#### Pointeurs dans d'autres langages

- Dans les langages de plus haut niveau (ex : $\texttt{OCAML}$), l'utilisation des pointeurs est supprimée, au profit des références et des tableaux dynamiques gérés par le compilateur.
- On y gagne en simplicité, on évite de nombreux bugs mais on perd certaines possibilités d'optimisation.

#### « Retourner » plusieurs valeurs

##### Passage de pointeurs en paramètres

- On contourne l'obligation faite aux fonctions C de ne retourner
qu'une seule valeur.

!!!example ""

    On veut appliquer à un point une translation de vecteur donné et récupérer le résultat.

- On ne peut pas retourner un tuple de coordonées comme en $\texttt{Python}$ au $\texttt{OCaml}$.
- Solution : utiliser des pointeurs sur les coordonnées du résultat.

```C linenums="1"
void translate (double x, double y, double vx, double vy,
        double *rx, double *ry)
```

applique une translation de vecteur $V (v_x,v_y)$ à un point $A(x ,y )$ et met les coordonnées du résultat $R(r_x,r_y)$ dans deux variables. Les coordonnées de $A$ et $V$ sont passées en paramètres. Pour le résultat, ce sont adresses de ses coordonnées qui sont passées en paramètres.

```C linenums="1"
void translate (double x, double y, double vx, double vy,
        double *rx, double *ry){
    // translation de vecteur V(vx,vy)
    *rx = x + vx;
    *ry = y + vy;
}// fin translate

int main(void){
    double x = 3, y = 6;// coordonnées du point A(x,y)
    double rx , ry ; // ré sultat de la translation
    translate (x , y,1,2,& rx,&ry) ; // translater A de (1,2)
    printf ("rx=%.1f ry=%.1f\n",rx,ry);
    return EXIT SUCCESS;
} // fin main
```

On obtient

```bash title="Rendu"
x=4.0 y=8.0
```

### Pointeur $\text{NULL}$, mot clé $\text{void}$

#### Pointeur nul

- Un _pointeur nul_ est un pointeur contenant une adresse invalide. Cette adresse invalide dépend du système d'exploitation, mais elle est la même pour tous les pointeurs nuls. Ainsi, deux pointeurs nuls ont une valeur égale.
- La constante `NULL` est définie dans l'en-tête `<stddef.h>`

```C linenums="1"
int *p = NULL; /*Un pointeur nul */
```

- Le pointeur `NULL` est souvent utilisé comme marqueur de fin dans une structure définie récursivement (exemple : listes chaînées).
- Il sert aussi à indiquer que quelque chose s'est mal passé (ex `malloc`)

#### Le mot clé `void`

- `void` n'est pas un type, c'est un mot clé. Plus précisément, c'est un type dit « incomplet » , c'est à dire que sa taille n'est pas calculable et qu'il n'est pas utilisable dans des expressions.
- On l'utilise pour déclarer qu'une fonction ne renvoie pas de valeur : `void f(int x)`
- Pour les fonctions sans argument, préferer `int f(void)` (qui signifie explicitement que `f` ne prend pas d'argument) à `int f()` qui indique que le nombre d'arguments de `f` n'est pas connu.
- `void *` désigne un pointeur vers une valeur dont on ne connaît pas le type (c'est un pointeur _générique_). Un pointeur sur `void` est considéré comme un pointeur générique : il peut référencer n'importe quel type.

!!!example ""

    ```C linenums="1"
    void *malloc( size t memorySize );//prototype de malloc
    ```

- Les règles de typage du $\texttt{C}$ permettent d'utiliser `void*` là où une valeur d'un certain type de pointeur est attendu. On peut donc écrire

```C linenums="1"
int *p = malloc( siezof ( int )) ; // alocation d'un pointeur sur le tas.
```

- Enfin `void *` permet le _polymorphisme_. Exemple : une fonction qui agit sur un tableau dont les éléments sont de type quelconque. Nous n'utilisons pas cette fonctionnalité.
- Le spécifieur de format `%p` de `printf` attend un pointeur sur n'importe quel type et affiche sa valeur (donc une adresse), que le pointeur pointe sur un objet de type `int, double, char...`

```C linenums="1"
int a =10;
int *p= &a;
printf ("adresse=\%p",p); // affichage d'une adresse
```

### Valeur de retour

#### Pointeur comme valeur retournée

- Une fonction peut retourner un pointeur.
- Cependant : l'objet référencé par le pointeur retourné doit toujours exister au moment de son utilisation.
- Il faut se poser la question :
    - l'adresse retournée est-elle celle d'une variable locale à la fonction (donc gérée automatiquement dans la pile d'exécution) ? Si c'est le cas, cette adresse peut servir dans d'autres contextes d'exécution et son contenu risque d'échapper au programmeur distrait.
    - l'adresse retournée désigne-t-elle une zone mémoire allouée sur le tas ? Si c'est le cas, il faut bien penser à la libérer par la suite.

```C linenums="1"
int *ptr(void){ // retourne un pointeur sur int
    int n;
    return &n;
}
int main(void){
    int *p = ptr() ;
    *p = 10;
    printf ("%d\n", *p);
    return 0;
}
```

On obtient

```bash title="Rendu"
$./a.out
Erreur de segmentation (core dumped)
```

- Le problème vient de la ligne

```C linenums="1"
int n; // dans ptr ()
```

- Les variables locales comme `n` de `ptr` se voient allouer un emplacement en mémoire de façon dynamique lors de l'exécution du programme. Le segment de mémoire dans lequel sont stockées les variables temporaires est appelé _segment de pile_
- Leur emplacement en mémoire est libéré à la fin de l'exécution d'une fonction secondaire comme `ptr`.
- Donc la fonction **ptr** renvoie une adresse qui n'est plus pertinente et qui sera utilisée par d'autres appels à d'autres fonctions. On dit que c'est un _pointeur fantôme_

##### Mot clé static

- On modifie juste la ligne $3$ du code précédent :

```C linenums="1"
static int n;// dans ptr
```

On obtient

```bash title="Rendu"
$./a.out
10
```

- Une variable locale déclarée `static` occupe toujours la même adresse en mémoire à chaque appel de la fonction.
On verra plus tard que la partie de la mémoire (allouée au programme) qui contient les variables statiques (comme d'ailleurs les variables globales) est appelée _segment de données_.
- Cette solution n'est pas d'un grand intérêt sauf si on a absolument besoin que l'adresse retournée soit toujours la même ! On préfère _l'allocation dynamique_ (voir plus loin).

### Pointeur sur pointeur

- Un pointeur **p** a une adresse qu'on récupère avec `&p`
- Dans le `main`, écrivons :

```C linenums="1"
int a = 10;
int *pa = &a;// pointeur sur int
int **pp = &pa;// pointeur sur pointeur sur int
printf ("a =%d,*pa=%d,**pp=%d\n", a,*pa,**pp);
printf ("pa=%p,*pp=%p\n",pa,*pp);
```

- Avec `*pp` on récupère l'adresse de **a** et avec `**pp` , la valeur de **a**.
- Après compilation/exécution :

```bash title="Rendu"
$./a.out
a =10,*pa=10,**pp=10
pa=0x7ffc67074a04,*pp=0x7ffc67074a04
```

### Dangers

#### Complexification du code

- Très puissante, l'utilisation de pointeurs rend plus difficile le travail du développeur.
- ♥ Si on ne fait pas attention avec les pointeurs, le programme peut accéder à une zone mémoire qui ne lui est pas allouée ou dans laquelle il ne peut pas écrire. Le processeur via le système d'exploitation engendre alors une _erreur
de segmentation_ (segmentation fault) qui provoque une exception voire plante l'application.
- ♥ Si c'est le programmeur qui gère les allocations en mémoire, il ne doit pas oublier de libérer après usage la zone allouée sous peine de _fuite de mémoire_ : Il s'agit d'un bug qui résulte d'une occupation croissante non contrôlés ou non désirée de la mémoire. DANGER : saturation possible de la RAM.

#### Un exemple de segmentation fault

```C linenums="1"
#include <stdio.h>
#include <stdlib.h>// contient EXIT SUCCESS

int main(){
    int *variable entiere ; // dé claration d'un pointeur
    printf (" saisir une valeur : ") ;
    scanf ("%d", variable entiere ) ;
    return (EXIT SUCCESS);// indique que tout s'est bien passé
}
```

```bash title="Rendu"
$./a.out
entrer une valeur : 23
Erreur de segmentation (core dumped)
```

- Explication : le pointeur `variable_entiere` n'est pas initialisé donc contient n'importe quoi (possiblement, une adresse interdite en écriture). Quand `scanf` veut accèder à cette adresse l'OS le lui refuse.

## Allocation dynamique

### Allouer de la mémoire ♥

Trois manières pour l'allocation de mémoire dans un programme :

- Statiquement, au cours de la compilation lorsque le compilateur lit des mots clés comme `const` ou `static` dans le code source. Adresses mémoires connues à l'initialisation de la mémoire du programme. Ces adresses correspondent à ce qu'on appelle le _segment de données_ du programme.
- Dynamiquement, au cours de l'exécution :
    - soit de façon automatique sur la _pile d'exécution_ : variables locales déclarées dans un bloc d'instructions,
    - soit à la demande sur le _tas_ : en utilisant des fonctions d'allocation de la mémoire.
- Il y a donc trois zones de données distinctes pour le programme. On affinera leur description dans un cours à venir.

|||
|:-:|:-:|
|segment de données| var. globales ou var. locales statiques|
|pile d'exécution| var. locales|
|tas| allocation dynamique par `malloc`|

### Allouer de l'espace

- Un pointeur non initialisé est égal à la constante symbolique `NULL` de `<stddef.h>`. Le test `p == NULL` permet de savoir si le pointeur $p$ pointe vers un objet.
- On a vu comment initialiser un pointeur en lui affectant l'adresse d'une autre variable `int * p =&a`;
- Pour affecter directement une valeur à `*p`, il faut d'abord réserver à `*p` un espace-mémoire de taille adéquate avec `malloc` de `<stdlib.h>`.
- L'adresse de cet espace-mémoire est la valeur de `p`
- Cette opération est appelée _allocation dynamique_

### La fonction `malloc`

- Syntaxe

```C linenums="1"
void* malloc( size t size ) ; // allocation en nb de bytes
```

- La valeur retournée est l'adresse du premier octet de la zone mémoire allouée. Si l'allocation n'a pu se réaliser (par manque de mémoire libre), la valeur de retour est la constante **NULL**.
- Toute utilisation de `malloc` doit être suivie plus tard d'une libération de l'espace réservé

```C linenums="1"
void free (void *ptr) ;
```

Le seul paramètre à passer est l'adresse du premier octet de la zone allouée et aucune valeur n'est retournée une fois cette opération réalisée.

!!!example "Exemple ♥"

    - Réserver 32 octets et les libérer immédiatement.

    ```C linenums="1"
    #include <stdlib.h>// tiré de Wikipedia
    int main(){
        int *pointeur = malloc(8 * sizeof(int)) ; //8x4 octets

        if (pointeur == NULL)
            printf("L' allocation n'a pu ê tre ré alis ée\n");
        else {
            printf(" allocation : succès\n") ;
            printf(" valeur de p=%p\n",p);
            free(pointeur) ; //Libération des 32 octets
            pointeur = NULL; // Invalidation du pointeur
        }...
    }
    ```

    ```bash title="Rendu"
    allocation : succès; valeur de pointeur=0x55fb8de5f260
    ```

### Allocation dynamique vs automatique ♥

```C linenums="1"
int i = 3;
int *p;

p = (int*)malloc( sizeof ( int )) ; // alloc . dyn. sur le tas
*p = i ; // attention : p ne pointe pas sur i

int *r=&i;// alloc . auto. sur la pile

printf (" i =%d,*p=%d, *r=%d\n",i,*p,*r);
printf ("p=%p, r=%p\n",p,r);

free (p) ;
```

`p` ne pointe pas sur `i` mais `r` si. Modifier `*p` ne modifie pas `i`, mais modifier `*r` modifie `i`

### Assertions (rappel)

Pour diverses raisons on peut souhaiter interrompre un programme si une condition n'est pas réalisée (ex : impossibilité d'allouer de la mémoire). Les _assertions_ sont alors utiles.

!!!example "Exemple d'assertion"

    ```C linenums="1"
    #include <assert.h>

    int main(){
        int i = 0; // on veut imposer i > 1
        assert (i > 1); // si i <= 1 : arrêt du pgm
        printf("poursuite du pgm");
        return 0;
    }
    ```

    Trace d'exécution :
    
    ```bash title="Rendu"
    a.out: assertion2.c:8: main: Assertion ‘i >1‘ failed.
    Abandon (core dumped}
    ```

- On la vu que les assertions servent à se protéger d'un comportement aberrant en arrêtant le programmme.
- Elles servent aussi en phase de conception d'un programme :
    - Il faut en effet compiler régulièrement (dès qu'on ajoute un bloc d'instructions ?)
    - Si l'écriture du programme n'est pas encore terminée : ajouter des assertions dans les branches non écrites des instructions conditionnelles. Cela permet de compiler (donc de vérifier la syntaxe) et de tester les branches complétées.

```C linenums="1"
int f ( int x){
    assert (x>0);// on quitte le pgm si x<=0
    // bloc de code complètement écrit
    // ne concernant que x>0 et à tester
    ...
    return NimporteQuoi;
}
```

### Assertion et bon fonctionnement d'une allocation

#### Exemple d'assertion

- Pour diverses raisons, l'allocation dynamique peut échouer. Le pointeur est alors égal au pointeur `NULL`. On devrait toujours tester si l'allocation s'est bien déroulée.
- Il est plus prudent d'arrêter l'exécution du programme en cas d'échec d'allocation. On peut utiliser une _assertion_.

```C linenums="1"
int t [3] = {1.,2.,3.};
int *copy = NULL;// on veut copier t dans copy

/*Allocation de la mémoire */
copy = (int *) malloc( 3 *sizeof ( int ) ) ;
assert ( copy != NULL ); // si échec, arr ê ter le pgm
```

### (Hors programme)

#### Arithmétique des pointeurs

Les opérations valides sur les pointeurs sont :

- l'addition d'un entier à un pointeur. Le résultat est un pointeur de même type que le pointeur de départ;
- la soustraction d'un entier à un pointeur. Le résultat est un pointeur de même type que le pointeur de départ;
- la différence de deux pointeurs pointant tous deux vers des objets de même type. Le résultat est un entier.

```C linenums="1"
int main(){
    int i = 3;
    int *p1, *p2;
    p1 = &i;
    p2 = p1 + 1;
    printf ("p1 = %p \t p2 = %p \t p2−p1=%ld\n",p1,p2,p2−p1);
    return 0;
}
```

On obtient

```bash title="Rendu"
p1 = 0x7fff37e6dc34 p2 = 0x7fff37e6dc38 p2−p1=1
```

La différence entre les adresses est $4$, c'est à dire la taille d'un `int`. l'expression `p2 - p1` désigne en fait un entier dont la valeur est égale à `(p2 - p1)/sizeof(int)`.

```C linenums="1"
int main(){
    double i = 3.;
    double *p1, *p2;
    p1 = &i;
    p2 = p1 − 1;
    printf ("p1 = %p \t p2 = %p \t p2−p1=%ld\n",p1,p2,p2−p1);
    return 0;}
```

On obtient

```bash title="Rendu"
$./a.out
p1 = 0x7ffcadcfc8e0 p2 = 0x7ffcadcfc8d8 p2−p1=−1
```

La différence entre les adresses est

$e0_16 −d8_16 = 14_10 ×16_10 + 0 −(13_10 ×16_10 + 8_10) = 8_10$

C'est la taille d'un `double`

## Pointeur et tableaux

### Tableaux ♥

(Rappel)

- Déclaration de tableaux :

```C linenums="1"
//type nomDuTableau[taille] ;
int tableau [4] ; // tableau de 4 entiers
```

Déclaration et initialisation :

```C linenums="1"
int tableau [4] = {1,2,3,4}; // les 4 elts de tab sont initialis és
float tib [5] = {1.1,2.1}// seuls 2 elts de tib sont initialis és
```

Accès aux éléments comme en Python avec `tab[i]`

```C linenums="1"
tab [1] = 23;// l'elt 1 de tab vaut 23
printf ("tab[%d]=%d",1,tab[1]);
```

### Accès aux éléments d'un tableau

(Rappel) Considérons `int tab[2]`;

- Un nom de tableau comme tab désigne, après déclaration, par convention un pointeur constant sur le premier bloc de son premier élément.
- On peut utiliser un pointeur initialisé à `tab` pour parcourir les éléments du tableau par _arithmétique des pointeurs_ (hors programme en CPGE).
- Pour accéder à `tab[2]`, le système ajoute à l'adresse du (premier bloc) de `tab[0]` le produit $2*4$
    - le nombre $2$ pour la position dans le tableau,
    - le nombre $4$ pour la taille d'un `int` ($4$ octets / `int`).

### Passage d'un tableau en paramètre d'une fonction ♥

(Rappel)

- La fonction ci-dessous affiche les éléments d'un tableau

```C linenums="1"
void affiche ( int *tableau , int tailleTableau ){
    for (int i = 0; i < tailleTableau; i ++)
        printf ("%d\n", tab[i ]) ;
}
```

- Les tableaux sont en fait passés en paramètre comme des copies de pointeur sur leur $1$er élément.

!!!example "Exemple de **main**

    ```C linenums="1"
    int main(){
        int tab [4] = {1, 2, −12};
        // afficher le contenu du tableau
        affiche (tab , 4) ;
        return 0;
    }
    ```

### Notion de _lvalue_

- Une _lvalue_ (left value) est une expression désignant un objet modifiable.
- Les expressions qui sont des _lvalue_ :
    - identificateur de variable :
        - N'ayant pas reçu le qualifieur `const`
        - Autre qu'un nom de tableau
        - dans le cas d'une variable de type structure ou union, celle-ci ne doit pas comporter de champs constant.
    - Expression de la forme `*p` ou `*(adr)` : `p` (resp. `adr`) étant une variable (resp.expression) de type pointeur sur un objet non constant.
    - élément de tableau :
        - autre que tableau (cas des tableaux multi-indice)
        - autre que structure ou union avec champ constant
    - Champ de structure ou d'union : même précisions que pour les tableaux

### Un tableau n'est pas une _lvalue_

- Considérons : `int tab[4];` Un nom de tableau comme `tab` utilisé plus loin dans le programme désigne en fait un pointeur constant (non modifiable) dont la valeur est l'adresse du (premier bloc du) `tab[0]`. Après déclaration, `tab` a pour valeur `&tab[0]`.
- `tab=tab2` soulève une erreur à la compilation (les noms de tableau ne sont pas des _lvalue_).

### Les fonctions d'allocation en CPGE

(D'après [Koors](https://www.koors.io))
`malloc` de `<stdlib.h>`

```C linenums="1"
void *malloc(size_t memorySize);
```

- Cette fonction permet d'allouer un bloc de mémoire dans le tas (_heap_ en anglais).
- Attention : la mémoire allouée dynamiquement n'est pas automatiquement relachée. Il faudra donc, après utilisation, libérer ce bloc de mémoire via un appel à la fonction `free`

(D'après [Koors](https://www.koors.io))

`calloc `de `<stdlib.h>`

```C linenums="1"
void *calloc ( size t elementCount, size t elementSize ) ;
```

Cette fonction alloue un bloc de mémoire en initialisant tous ces octets à la valeur $0$. Bien que relativement proche de la fonction `malloc`, deux aspects les différencient :

- L'initialisation : `calloc` met tous les octets du bloc à la valeur $0$ alors que malloc ne modifie pas la zone de mémoire.
- Les paramètres d'appels : `calloc` requière deux paramètres (le nombre d'éléments consécutifs à allouer et la taille d'un élément) alors que `malloc` attend la taille totale du bloc à allouer.

### La fonction de libération

- free de stdlib.h

```C linenums="1"
void free ( void *pointer ) ;
```

- La fonction `free` libère un bloc de mémoire alloué dynamiquement dans le tas (le heap, en anglais), via un appel à la fonction malloc (ou tout autre fonction d'allocation mémoire)
- ne jamais désallouer avec la fonction `free` un bloc de mémoire obtenu autrement que par une fonction d'allocation comme `malloc`, `calloc`

### Squelette d'allocation ♥

- Puisqu'un nom de tableau désigne un pointeur constant :
    - On ne peut pas créer de tableaux dont la taille est une variable du programme (sauf à utiliser des VLA),
    - On ne peut pas créer de tableaux bidimensionnels dont les lignes n'ont pas toutes le même nombre d'éléments.
- Plutôt que de déclarer les tableaux comme des pointeurs constants, on peut manipuler des pointeurs alloués dynamiquement

```C linenums="1"
#include <stdlib.h>// exemple Anne Canteaut
int main(){
    int n;// la valeur sera par exemple saisie avec un scanf
    int *tab;
    ...
    tab = (int*)malloc(n * sizeof(int)) ;
    // cases de tab non initialis ées
    ...
    free(tab) ;
}
```

Avec `calloc`, le principe est le même mais en plus les cases du tableau sont initialisées à $0$ :

```C linenums="1"
#include <stdlib.h> // exemple Anne Canteaut
int main(){
    int n; // la valeur sera par exemple saisie avec un scanf
    int *tab;
    ...
    tab = (int*)calloc(n, sizeof(int)); // syntaxe différente
    // cases de tab initialisées à 0
    ...
    free(tab);
}
```

Dans tous les cas, les éléments de `tab` sont manipulés avec l'opérateur d'indexation `[]` , exactement comme pour les tableaux.

!!!tip "Commentaire"

    - Dans le transparent précédent, `tab` n'est pas un tableau statique, c'est un pointeur.
    - Mais on l'utilise comme un tableau : en particulier `tab[2]` désigne l'élément situé à l'adresse `&tab[0]+2*4`
    - de plus, `tab` n'est pas un pointeur constant : c'est une _lvalue_. On peut écrire par exemple `tab++` ou `tab = &n`

### Tableau VS pointeur ♥

Les deux différences principales entre un tableau et un pointeur sont

- un pointeur devrait toujours être initialisé, soit par une allocation dynamique, soit par affectation d'une expression adresse, par exemple `p = &i`;
- un tableau n'est pas une `lvalue` ; il ne peut donc pas figurer à gauche d'un opérateur d'affectation. En particulier, un tableau ne supporte pas l'arithmétique (on ne peut pas écrire `tab++;`).

### Tableaux à plusieurs dimensions ♥

- Un tableau à deux dimensions est, par définition, un tableau de tableaux. Après la déclaration, le nom du tableau est utilisé comme un pointeur constant vers un pointeur constant.
- Déclaration, initialisation et usage d'un tableau bi-dimensionnel :

```C linenums="1"
int main(){
    int mat[2][3]={{1,20,300},{4,50,600}};
    for (int i = 0; i < 2; i++){
        for (int j = 0 ; j < 3; j++) {printf("%3d", mat[i][j]);}
            printf ("\n");
    }
    return 0;
}
```

- Attention : les éléments de _mat_ ($6$ ici) sont rangés consécutivement en mémoire. Il n'y a pas de « grille » dans la mémoire.

- Avec l'exemple précédent, on obtient

```bash title="Rendu"
10 2 300
400 500 6
```

- Avec

```C linenums="1"
int mat[N][M];
```

- 
    - `mat` est utilisé comme un pointeur (constant) qui pointe vers un pointeur (constant) de type pointeur d'entiers. Le tableau `mat` est une adresse invariante égale à celle du premier élément : `&mat[0][0]`.
    - De même `mat[i]` , pour `i` entre $0$ et `N-1`, est un pointeur constant vers un objet de type entier, qui est le premier élément de la « ligne » d'indice `i`. `mat[i]` a donc une valeur constante qui est égale à `&mat[i][0]`.
    - `mat, mat[0]` et `&mat[0][0]` désignent la même adresse.

!!!example "Exercice"

    (Hors programme) Utiliser la propriété de contigu ̈ıté en mémoire pour écrire une fonction void display(int n, int m, int t[n][m]) qui affiche sur une ligne tous les coefficients du tableau bi-dimensionnel t. Une seule boucle est autorisée.

    Dans main, écrivons :
    
    ```C linenums="1"
    int t =[3][2] = {{1, 2}, {3, 4}, {5, 6}};
    display (3, 2, t);
    ```

    Après compilation/exécution :
    
    ```bash title="Rendu"
    1,2,3,4,5,6,
    ```

### Allocation dynamique de « matrice » ♥

- On déclare un pointeur qui pointe sur un objet de type `type *` (deux dimensions) de la même manière qu'un pointeur avec

```C linenums="1"
type **nom−du−pointeur;
```

Le squelette du programme devient :

```C linenums="1"
int k, n;
int **tab;
...
//tab. de k pointeurs d'entiers
tab = (int**)malloc(k * sizeof(int *));
for ( int i = 0; i <k; i ++)
    // tab. de n entiers
    tab [i] = (int*)malloc(n * sizeof(int)) ;
    ....
for (int i = 0; i < k; i++)
    free (tab[i]); // libération espaces des lignes
free (tab); // libération espace
```

```C linenums="1"
#include <stdio.h>
#include <stdlib.h>

int main(){
    int n=2, m=3;
    int **tab;
    tab = (int**)malloc(n *sizeof(int *)) ;
    for (int i = 0; i < n; i++){
        tab [i] = (int*)malloc(m * sizeof(int)) ; // tab. d'entiers
        for (int j = 0; j < m; j++)// on peut s'en passer avec calloc
            tab [i][j] = 0;
}
...

for (int i = 0; i < n; i++)
    free (tab[i]); // libération espaces des lignes
free (tab); // libération espace
return 0;
}
```

### Tableaux statiques 2D VS pointeur de pointeur

- Considérons le pointeur de pointeur utilisé comme un tableau à deux dimensions dans le transparent précédent : `int ** tab = malloc(2 * sizeof(int *))`. Chaque « ligne » est déclarée par `tab[i] = (int*)malloc(3 * sizeof(int));` .
- Considérons un tableau statique à deux dimensions : `int t[2][3]` .
- Dans `tab`, les éléments sont rangés par « lignes » , deux « lignes » n'étant pas nécessairement consécutives en mémoire.
- Dans `t`, les éléments sont tous contigus en mémoire. Le dernier octet du dernier élément de la « ligne $i$ » est voisin du premier octet du premier élément de la « ligne $i$ + 1 » .

#### Passage en paramètre

Les deux déclarations suivantes ne sont pas équivalentes :

```C linenums="1"
void foo(int n, int m, int **tab) ;
void moo(int n, int m, int t[n][m]);
```

- Pour accéder à `t[i][j]`, `moo` calcule `*(t + i * m + j)`. Rappel : le nom d'un tableau est un alias vers la localisation en mémoire de ce tableau : c.a.d. `&t[0][0]`.
- Pour accéder à `tab[i][j]`, `foo` calcule `*(*(tab + i) + j)` (observer le double déréférencement).
- Les appels `foo(2,3,t)` et `moo(2,3,tab)` risquent tous deux de mener à une erreur de segmentation (seg fault).
    - Dans `foo(2,3,t)`, la fonction traite `t+i` comme une adresse à déréférencer ; ce qu'elle n'est pas.
    - Dans `moo(2,3,tab)`, la fonction considère que les éléments `tab[i][m-1]` et `tab[i+1][0]` sont contigus; ce qu'ils ne sont pas.

### Allocation dynamique de matrice (suite)

Pour créer un tableau à 3 dimensions, utiliser un pointeur vers un objet de type `type **` :

```C linenums="1"
type ***nom−du−pointeur;
```

## Chaîne de caractères

- Une chaîne de caractères est un tableau à une dimension d'objets de type char, se terminant par le caractère nul `'\0'`.
- On peut donc manipuler toute chaîne de caractères à l'aide d'un pointeur sur un objet de type `char`.
- Les _constantes chaînes_ sont notées entre doubles quotes comme `"hello"` mais le caractère de fin de chaîne n'apparaît pas explicitement.
- Les constantes chaînes sont toujours placées en mémoire statique. Elles sont immuables.
- Le caractère immuable permet au compilateur d'allouer une seule occurrence de chaque chaîne distincte. En clair, dans

```C linenums="1"
printf("hello");
printf("hello");
```

les deux occurrences de **hello** sont stockées au même endroit en mémoire.

### Les chaînes de caractères comme tableaux de `char` ♥

Déclaration comme pointeur (constant):

```C linenums="1"
char chaine [5] ;
```

- Le mot `hello` est déclaré et initialisé avec

```C linenums="1"
char chaine [6] = {'h', 'e', 'l', 'l', 'o', '\0'};
```

- Le caractère `'\0'` représente la fin du mot. Sa présence est $\underline{\text{impérative}}$. Ainsi « hello » nécessite $6$ caractères. Les guillemets sont simples.
- `"h"` représente la chaîne de caractères `{'h',\0}` et `'h'` ben... le caractère « `h` » !
- Initialisation plus rapide :

```C linenums="1"
char chaine [] = "Salut";// taille automatiqu. calcul ée
// caractère de fin de chaîne automatiquement ajouté.
// Observer les doubles quote.
// chaine est de lg 6 ! !
```

### Accès aux éléments ♥

Comme en $\texttt{Python}$, l'accès aux constituants de la chaîne se fait avec l'opérateur `[]`.
- Lecture :

```C linenums="1"
char chaine[] = "hello";
int i =0;
while(chaine[i] !='\0'){ // lecture
    printf("%c", chaine[i]);
    i++;
}
```

Affiche « hello » .

- Modification :

```C linenums="1"
i = 0;
while(chaine [i] !='\0'){// codage de César
    chaine [i] = chaine[i]+1;
    i++;
}
```

La chaîne est devenue `ifmmp`

### Affichage et saisie au clavier ♥


Le spécifieur de format `%s` vaut pour `printf` et `scanf` :

```C linenums="1"
int main(){
    char chaine [] = "hello";
    printf ("%s\n", chaine);
    printf (" entrer un mot de 5 lettres sans espace : ") ;
    scanf ("%s", chaine) ;// chaine est un pointeur
    printf ("%s\n", chaine);
    return 0;
}
```

On obtient :

```bash title="Rendu"
hello
entrer un mot de 5 lettres sans espace : salut
salut
```

### Longueur

On dispose d'un moyen pour calculer la longueur d'une chaîne de caractères : on itère jusqu'à tomber sur le caractère nul `'\0'`.

```C linenums="1"
#include <stdio.h>

int main(){
    int i ;
    char *chaine ;

    chaine = "toto et gogo";
    for ( i = 0; chaine [ i ] !='\0'; i ++);    // observer le ";"

    printf ("nombre de caracteres = %d\n",i);
    return 0;
}
```

- Avec cette méthode, la complexité est $\underline{\text{linéaire}}$ : il y a $n$ étapes pour calculer la longueur (si $n = |\textbf{chaine}|$).
- Un autre moyen de calculer la longueur d'une chaîne de caractères est d'utiliser la fonction `strlen` de `<string.h>`.
- Son protototype est le suivant

```C linenums="1"
size t strlen ( const char *theString ) ;
```

- Elle prend donc en paramètre un pointeur sur un `char` constant (on ne modifie pas la chaîne).
- On l'utilise ainsi

```C linenums="1"
#include <stdio.h>
#include <string.h>

int main(){
    ...
    printf ("nombre de caracteres = %zu\n", strlen(chaine)) ;
    ...}
```

- Mais la complexité reste linéaire !

### Comparer

- `int strcmp(const char *first, const char *second);` de `string.h`
- Compare les deux chaînes `first`, `second`
- Si la valeur de retour est nulle, les chaînes sont égales
- Si la valeur renvoyée est négative, le premier caractère qui ne correspond pas a une valeur inférieure dans `first` que dans `second`
- Si la valeur renvoyée est positive, le premier caractère qui ne correspond pas a une valeur supérieure dans `first` que dans `second`.
- La variante `strncmp(first,second,n)` ne compare que les $n$ premiers caractères des chaînes.
- `first==second` ne doit être utilisé que pour des chaînes de caractères littérales (dans le doute, on évite !)

### Convertir en entier

- `int atoi(const char *theString);` de **strlib**
- Cette fonction permet de transformer une chaîne de caractères, représentant une valeur entière comme `"12356"` , en une valeur numérique de type int . Le terme d'« atoi » est un acronyme signifiant : ASCII to integer.
- Retourne la valeur $0$ si la chaîne de caractères ne contient pas une représentation de valeur numérique. Il n'est donc pas possible de distinguer la chaîne "0" d'une chaîne ne contenant pas un nombre entier. L'utilisation de la fonction `strtol` permet bien de distinguer les deux cas.

### Modifier une constante chaîne

- La norme n'indique pas si les constantes chaînes comme `"hello"` sont modifiables ou non. Juste qu'elles sont placées en mémoire statique.
- Pour imposer un caractère immuable indépendamment des implémentations, préférer :

```C linenums="1"
const char *a = ‘‘ hello ";
```

### Concaténation ♥

- Pour concaténer deux chaînes de caractères, allouer à une troisième chaîne autant de place que la somme des longueurs.
- Parcourir l'une après l'autre les deux chaînes et entrer leurs caractères dans le résultat.

```C linenums="1"
int main(){// exemple Anne Canteaut
    char *chaine1, *chaine2, *res ; // 3 pointeurs

    chaine1 = "chaine ";
    chaine2 = "de caracteres ";
    size_t lgc1 = strlen(chaine1) , lgc2 = strlen(chaine2);
    res = (char*)malloc((lgc1 + lgc2 + 1) * sizeof(char));
    for (int i = 0; i < lgc1; i++)
        res[i] = chaine1[i];
    for (int i = lgc1; i < lgc1 + lgc2; i++)
        res[i] = chaine2[i −lgc1];
    res[lgc1 + lgc2] = "\0";
    printf("%s\n", res); free(res);
    return 0;
}
```

### Tableau de constantes chaînes

- Une constante chaîne est convertie par le compilateur en un pointeur sur son premier caractère. On peut donc facilement constituer un tableau de constantes chaînes.
- On peut bien sûr toujours initialiser individuellement :

```C linenums="1"
char *jour [7] ;
jour [0] = "lundi";
jour [1] = "mardi"; ...
```

- Mais on peut aussi profiter de la syntaxe d'initialisation des tableaux en écrivant directement :

```C lineums="1"
char *jour [7] = {"lundi","mardi" ,...}
```
