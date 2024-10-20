# Chaîne de compilation

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    - Un cours de Christophe Rippert à l'ENSIMAG
    - Un tuto sur [KOOR.fr](https://koor.fr/C/Tutorial/Compilation.wp)

!!!warning "Avertissment"

    Nous utilisons toujours le langage **C** tel que défini dans la norme « C99 » .

!!!note "Historique"

    - $\texttt{C}$ : langage de programmation impératif généraliste, de _bas niveau_ (il gère les échanges $\texttt{RAM}$/processeur). Mais pas de _très bas niveau_ sinon il gérerait aussi ce qui se passe dans le processeur. Toujours très utilisé.
    - Création début des années $70$ pour réécrire $\texttt{UNIX}$
    - A inspiré de nombreux langages plus modernes comme $\texttt{C++}$, $\texttt{C\#}$, $\texttt{Java}$ et $\texttt{PHP}$ ou $\texttt{Javascript}$.
    - Utilisé par exemple pour réaliser les bases (compilateurs, interpréteurs...) des langages plus modernes.
    - C offre au développeur une marge de contrôle importante sur la machine (notamment sur la gestion de la mémoire)

## Compilation

### Du fichier source au langage machine

La traduction d'un (ou plusieurs) fichiers sources en $\texttt{C}$ vers le langage machine se fait en 2 étapes indépendantes et souvent enchaînées automatiquement (donc difficile à distinguer pour le débutant).

!!!quote"Prétraitement"

    Opération purement textuelle $\color{red}\text{consistant par exemple à supprimer les } \textit{commentaires}$

- Compilation . En $3$ sous-étapes :
    - Compilation proprement dite
    - Assemblage
    - Édition de liens

!!!quote "Compilation proprement dite"

    $\color{red}\text{Transforme le fichier généré par le préprocesseur en }\textit{assembleur}$ (une suite d'instructions du microprocesseur lisibles -- avec de l'entraînement -- par un humain),

!!!quote "Assemblage"

    $\color{red}\text{Transforme le code assembleur en instructions binaires}$ (donc directement compréhensibles par le processeur) Cette instruction et la précédente sont généralement effectuées dans la foulée l'une de l'autre sauf précision contraire passée en argument du compilateur.. Fichier binaire produit appelé $\color{red}\textit{fichier objet}\text{ (ou }\textit{module objet})$.

!!!quote "Édition de liens"

    $\color{red}\text{Génération des liens entre les différents fichiers objets du projet}$. En effet, un programme complexe est en général écrit sur plusieurs fichiers différents. L'éditeur de lien produit un fichier _exécutable_.

### Environnement

#### Choix pour la MP2I à Thiers

- Un éditeur de texte **emacs** tournant sous Linux couplé au compilateur $\texttt{C}$ **gcc** lancé depuis un terminal
- Avantage : s'installe sans (trop) de difficultés pour la plupart des configurations Linux.
- Par exemple, sous Ubuntu, puisque **gcc** et le terminal sont fournis
par défaut, il ne reste qu'à installer **emacs** :

```bash linenums="1"
$sudo apt−get install emacs
```

### Un code

```C linenums="1"
// Le fichier hello . c Mon premier code C

#include <stdio.h>// pour communiquer avec des fichier
#include <stdlib.h>// pour gérer la mémoire

int main(){
    int i = 10;
    printf (”Hello world!\n”);// affichage puis passage à la ligne
    printf (”%i\n”,i) ;// affichage de i
    return 0;// renvoyer 0 : c.a.d tout s'est bien passé
}
```

### Compiler puis exécuter

```bash linenums="1"
$gcc hello.c −o hello
$./hello
Hello world!
i=10
```

## GCC

### Présentation

!!!quote "$\texttt{GCC}$"

    $\texttt{GNU}$ $\texttt{C}$ompiler $\texttt{C}$ollection, abrégé en $\texttt{GCC}$, est un ensemble de compilateurs créés par le projet $\texttt{GNU}$. Nous l'utilisons pour $\texttt{C}$ mais il peut aussi compiler $\texttt{Java}$, $\texttt{Ada}$, $\texttt{Fortran}$...

Trois composantes $\texttt{GCC}$ désigne :

- la collection complète de compilateurs (le « projet
$\texttt{GCC}$ » )
- la partie commune à tous les compilateurs (« $\texttt{GCC}$ » ) ;
- le compilateur $\texttt{C}$ lui-même (le frontend `gcc` ), écrit en
minuscule).

frontend `gcc` est un une application _frontale_ (en anglais « frontend » ), ce qui signifie que l'utilisateur interragit directement avec lui.

### L'application `gcc` ♥

`gcc` permet d'appeler dans l'ordre le préprocesseur, le compilateur, l'assembleur et le _linker_ (lieur) pour le langage $\texttt{C}$ (et aussi $\texttt{C++}$). Opérations réalisées par **gcc** :

- appel du préprocesseur $\texttt{C}$ (nommé **cpp**) ;
- appel du compilateur $\texttt{C}$ (nommé **cc**). Le compilateur génère un fichier assembleur;
- appel de l'assembleur (**as**) pour générer le fichier objet ;
- appel de l'éditeur de liens (**ld**) pour générer l'exécutable ou la bibliothèque. La librairie $\texttt{C}$ standard est incluse par défaut dans la ligne de commande de l'édition de liens.

## Pré-processeur

### Prétraitement

Le pré-processeur est un module de pré-traitement qui effectue des transformations sur le code écrit avant qu'il ne soit traité par le compilateur.

- Consiste en la modification du texte d'un fichier source basée sur l'interprétation des directives à destination du préprocesseur. ♥
- On les reconnaît parce qu'elles commencent par un hastag #. Elle ne se terminent pas par un point-virgule `;` .
- Les deux directives les plus importantes sont :
    - `#include` : inclusion d'autres fichiers source. Récursif : si **A** inclut **B** qui inclut **C**, après pré-processing, **A** contient **C**. ♥
    - `#define` : définition de macros ou symboles (c'est à dire des bout de code qui seront recopiées tels-quels dans le code $\texttt{C}$). ♥
- D'autres directives `#if`, `#ifndef` et `#endif`, utilisées lors de l'écriture de fichiers d'en-tête, seront présentées ultérieurement (pour la compilation de projets sur plusieurs fichiers).

### Pré-processeur

- Effectue des remplacements textuels (et non sémantiques) : le pré-processeur $\underline{\text{n'est pas le compilateur}}$, il n'interprète pas ce qu'il manipule. Analogie : fonction **Remplacer** d'un traitement de texte. ♥
- Avec `#define NOM val` , le pré-processeur remplace chaque occurence de la chaine **NOM** par la valeur **val** dans tout le texte du fichier. ♥
- Entrer `cpp hello.c hello.i` puis vérifier l'explosion de la taille de **hello.i**.

## Compilation

### Compilateur

Traduire le code $\texttt{C}$ en code assembleur (programme `cc`).

- Compilation séparée : chaque fichier source est compilé sans regarder les autres fichiers, même s'ils font partie du même programme
- Le compilateur a juste besoin de connaitre les _prototypes_ (on dit aussi _signatures_) de toutes les fonctions utilisées dans le fichier compilé, mais il n'a pas besoin d'avoir à sa disposition le code des fonctions externes.
- Les prototypes des fonctions sont définies dans des fichiers `.h`, qui sont inclus par le pré-processeur (via la directive `#include`) dans le fichier $\texttt{C}$ qui les utilise.
- Entrer `cc -S hello.i` . On obtient **hello.s**. L'ouvrir et vérifier par exemple la mention du registre $32$ bits `%eax`, du pointeur de sommet de pile `%rsp` et du pointeur de cadre de pile `%rbp`

### L'assembleur

- L'assembleur `as` prend un fichier écrit en langage assembleur et le traduit en code binaire (langage machine).
- Avec `as -o hello.o hello.s`, l'assembleur produit un fichier objet `hello.o` illisible pour un humain et non encore exploitable tel quel (il manque les _liens_).

### Édition de liens

L'éditeur de lien `ld` a pour rôle de fusionner les différents fichiers objets pour produire l'exécutable final. Il fusionne :

- les codes contenus dans les différentes fonctions de tous les fichiers objets du programme, pour obtenir une seule grosse zone de code ;
- les données statiques allouées dans tous les fichiers objets du programme, pour obtenir une grosse zone de données statiques commune à tout le programme.

Un programme $\texttt{C}$ utilise toujours d'autres fichiers objets contenus dans la bibliothèque standard **libc.a**. Par exemple le fichier objet contenant le code de `printf` mais aussi d'autres fichiers qui permettent de rendre le code exécutable.

### Édition de liens `ld`

Dans la compilation de **hello.c**, l'assembleur a laissé un « trou » à l'endroit de l'appel à `printf`. C'est une indication pour insérer à cet endroit un véritable appel à la fonction `printf` (on trouve sur ma machine le fichier objet `stdio.o` dans l'archive **/usr/lib/x86 64-linux-gnu/libc.a**)

Si on appelle dans un fichier .c une fonction mal orthographiée ou qui n'existe pas, c'est l'éditeur de lien qui repère l'erreur.

Oublions d'écrire un `main` dans le projet. Un des fichiers inclus automatiquement par l'éditeur de lien inclu un appel à `main` lequel est cherché dans les fichiers sources qu'on lui donne. `ld` déclenche une erreur.

## Retour sur GCC

### Fichiers provisoires ♥

Au cours de la compilation, des fichiers provisoires sont produits. Ils sont effacés par le compilateur quand ce dernier termine sa tâche :

- $\color{blue}\text{.c}$ : fichier source contenant le programme (et ses commentaires) écrit par le programmeur,
- $\color{blue}\text{.i}$ : fichiers générés par le préprocesseur (ce sont des fichiers textes),
- $\color{blue}\text{.s}$ : fichiers assembleurs (donc lisibles par un humain),
- $\color{blue}\text{.o}$ : fichiers objets (binaires) produits après l'assemblage.

### Bibliothèques

« Bibliothèque » est la traduction de l'anglais « library » . On utilise improprement en français le mot « librairie » .

Les fichiers **.a** : Archives constituées de fichiers objets correspondant aux librairies précompilées (par exemple la librairie standard d'entrées-sorties).

Exemple **libc.a** : archive contenant notamment **stdio.o**.

### gcc et bibliothèques

Option `-l` de `gcc`

- Les éventuelles librairies précompilées utilisées sont déclarées par la chaîne **-lbibliothèqueDésirée** et se situent souvent dans un sous-répertoire de **/usr/lib/**.

!!!example ""

    la librairie mathématique **libm.a** est située sur mon ordinateur dans **/usr/lib/x86 64-linux-gnu/**

    Pour la trouver, entrer :
    
    ```bash linenums="1"
    $find /usr/lib −name libm.a # affiche localisation de libm.a
    ```

Un nom de librairie commence par **lib**. A la suite de `-l` on n'écrit que ce qui est après « **lib** » et avant le point.

Pour utiliser **libm.a** et ses fonctions dans le programme **myprog** : ♥

```bash linenums="1"
$gcc myprog.c −lm −o myprog
```

### Encore plus sur libm.a

Un certain nombre de fonctions de la librairie **libm.a** sont des fonctions intégrées du compilateur (built-in functions).

Pour des fonctions comme `cos` , la directive `#include <math.h>` est toujours nécessaire mais pas (ou pas toujours) l'option `-lm` de **gcc**.

En revanche, d'autres fonctions comme `nextafterf` ne sont toujours pas des fonctions intégrées du compilateur et nécessite encore l'option `-lm` de **gcc**. Du moins sur ma machine...

### Accès aux bibliothèques

Les fonctions dont les prototypes sont dans **stdlib.h** et **stdio.h** sont écrites dans la librairie **libc** laquelle est importée par défaut par `gcc` . `gcc truc.c` se comprend donc comme `gcc truc.c -lc`. L'usage de `-lc` est donc implicite.

Les librairies précompilées sont cherchées par défaut dans le répertoire
**/usr/lib** et ses descendants.

Mais il peut se produire qu'elles ne se trouvent pas dans ce répertoire usuel, on spécifie alors leur chemin d'accès par l'option `-L

### Options de production de fichiers

On peut demander à gcc de produire les fichiers intermédiaires manipulés par les différents programmes qu'il appelle si on veut les observer :

- `gcc -E -o hello.i hello.c` produit le fichier en sortie du pré-processeur,
- `gcc -S hello.c` produit le fichier en sortie du compilateur (après avoir appelé aussi le pré-processeur)
- `gcc -c hello.c` produit le fichier en sortie de l'assembleur (après avoir appelé aussi le pré-processeur et le compilateur)
- `gcc -o hello hello.c` appelle tous les programmes nécessaires pour produire directement le binaire hello.

### Application de gcc à un format de fichier

Le programme `gcc` se base simplement sur l'extension du fichier qu'on lui donne pour déterminer quelles opérations il doit effectuer. Par exemple, si le fichier se termine par :

- `.c` , il appelle dans l'ordre : le pré-processeur, le compilateur, l'assembleur et l'éditeur de lien.
- `.i` il appelle dans l'ordre : le compilateur, l'assembleur et l'éditeur de lien.
- `.s` : il appelle dans l'ordre : l'assembleur et l'éditeur de lien.
- `.o` : il appelle l'éditeur de lien.
- `gcc -o hello hello.c` appelle tous les programmes nécessaires pour produire directement le binaire **hello**. Détruit en outre les fichiers intermédiaires.

### Schéma du processus de compilation

Chaîne de compilation pour un programme composé d'un programme principal `prog.c` qui utilise un module `mod.c` (Christophe Rippert).

<p align='center'><img src='/images/ChaineCompil/chainecompil1.png'/></p>

**crt0.o** : chargé par défaut par `ld` ; permet de rendre le fichier objet exécutable

### Options

Quelques options du compilateur **gcc** :

- `-c` : supprime l'édition de liens ; produit un fichier objet.
- `-E` : n'active que le préprocesseur (le résultat est envoyé sur la sortie standard).
- `-I nom-de-répertoire` : spécifie le répertoire dans lequel doivent être recherchés les fichiers en-têtes à inclure (en plus du répertoire courant).
- `-L nom-de-répertoire` : spécifie le répertoire dans lequel doivent être recherchées les librairies précompilées (en plus du répertoire usuel).
- `-o nom-de-fichier` spécifie le nom du fichier produit. Par défaut, l'exécutable fichier s'appelle **a.out**.
- `-S` : n'active que le préprocesseur et le compilateur ; produit un fichier assembleur.
- `-v` : pour _verbose_. Affiche la liste des commandes exécutées par les différentes étapes de la compilation.
- `-W` : imprime des messages d'avertissement (warning) supplémentaires.
- `-Wall` imprime tous les messages d'avertissement. Attention, cela peut-être très verbeux !
- ...

**Le manuel** : Pour plus d'informations sur **gcc**, entrer :

```bash linenums="1"
$man gcc
```

Pour quitter la _manpage_, taper $q$ puis « Entrée » .
