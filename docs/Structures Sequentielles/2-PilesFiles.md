# Piles et Files

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    - Wikipedia : [en français](https://fr.wikipedia.org/wiki/Pile_%28informatique%29) et (plus complet mais en anglais) [ici](https://en.wikipedia.org/wiki/Stack_%28abstract_data_type%29)
    - La documentation sur le module [Stack](https://ocaml.org/manual/5.2/api/Stack.html) de OCaml
    - La documentation sur le module [Queue](https://ocaml.org/manual/5.2/api/Queue.html) de OCaml

## Piles

### Généralités

#### Pile

!!!quote "Pile (_stack_ en anglais )"

    C'est une _structure de données_ fondée sur le principe « dernier arrivé, premier sorti » (ou $\texttt{LIFO}$ pour $\texttt{Last In, First Out}$), ce qui veut dire que les derniers éléments ajoutés à la pile seront les premiers à être récupérés.

Le fonctionnement est celui d'une pile d'assiettes : on ajoute des assiettes sur la pile, et on les récupère dans l'ordre inverse, en commençant par la dernière ajoutée.

<p align='center'><img src='/images/PilesFiles/pilesfiles1.png'/></p>

_Figure_ – Empiler et dépiler

!!!example ""

    <p align='center'><img src='/images/PilesFiles/pilesfiles2.png'/></p>

    _Figure_ – Empilement de $A$ puis$ B$, dépilement de $B$ puis $A$ $(\texttt{LIFO})$

#### Historique

- Turing 1946. Description théorique d'appel et de retour de sous-routines.
- Klaus Samelson et Friedrich L. Bauer de l'université Technique de M ̈unich généralisent l'idée en 1955 en présentant une modélisation du fonctionnement des ordinateurs à base de piles.
- Même concept, indépendamment développé par l'australien Charles Leonard Hamblin en 1957.

#### Les primitives indispensables

- « Empiler » : ajoute un élément sur la pile. Terme anglais correspondant : « Push » .
- « Dépiler » : enlève l'élément au sommet de la pile et le renvoie. Terme anglais correspondant : « Pop » .
- « La pile est-elle vide ? » : renvoie vrai si la pile est vide, faux sinon. $\Leftrightarrow$ « is_empty »

#### Autres primitives

Elles peuvent être obtenues à partir des 3 premières :

- « Nombre d'éléments de la pile » : renvoie le nombre d'éléments dans la pile. Complexité : Selon les implémentations, peut être fait soit en temps constant soit en temps linéaire.
- « Quel est l'élément de tête ? » : renvoie l'élément de tête sans le désempiler. Terme anglais correspondant : « Peek » .
- « Vider la pile » : dépiler tous les éléments. Complexité : Selon l'implémentation, cela peut être fait en temps constant ou linéaire. Terme anglais correspondant : « Clear » .
- « Dupliquer l'élément de tête » et « échanger les deux premiers éléments » : existe sur les calculatrices fonctionnant en notation polonaise inverse (style HP). Termes anglais correspondants : « Dup »  et « Swap » respectivement

#### Un peu de maths

En matière de structures abstraites, on peut considérer qu'une pile est un monoïde libre, c'est-à-dire un ensemble muni d'une loi de composition interne (la concaténation) associative et possédant un élément neutre (la pile vide).

#### Applications

- La plupart des microprocesseurs gèrent nativement une pile. Elle correspond alors à une zone de la mémoire, et le processeur retient l'adresse du dernier élément.
- Dans un navigateur web, une pile sert à mémoriser les pages Web visitées. L'adresse de chaque nouvelle page visitée est empilée et l'utilisateur désempile l'adresse de la page précédente en cliquant le bouton « Afficher la page précédente » .
- L'évaluation des expressions mathématiques en notation post-fixée (ou polonaise inverse) utilise une pile.
- La fonction « Annuler la frappe » (en anglais Undo) d'un traitement de texte mémorise les modifications apportées au texte dans une pile.
- Un algorithme de recherche _en profondeur d'abord_ utilise une pile pour mémoriser les nœuds visités.
- Inversion d'un tableau ou d'une chaîne de caractères.
- Les algorithmes récursifs admis par certains langages $(\texttt{LISP, C, Python,} \text{ etc...})$ utilisent implicitement une pile d'appel.

#### Listes OCaml et piles

- En **OCaml**, les listes ont un comportement de piles (fonctionnelles). Il n'y a donc pas besoin d'importer de module particulier pour bénéficier d'une structure de pile.
- Les listes OCaml ne sont pas des structures impératives : il n'y a pas d'effets de bord. 
$\color{red}\text{Pour une structure de pile impérative préférer le module Stack.}$

#### En OCaml

```OCaml linenums="1"
open Stack ;;(*charger le module Stack (pile)*)
let s = create ();;(*création d'une pile vide*)
push 3 s; push 6 s; push 7 s;;
(*-> ajouter 3 nombres dans la pile*)
let s' = copy(s);;(*faire une copie de la pile*)
for i = 0 to 2 do
    (*notre première boucle for CAML*)
    (*pos s : retire le sommet*)
    Printf.printf "%d; " (pop s);
done ;;
try
    (*On sait que le code suivant risque
    de générer une exception ...
    ... Mais on 'essaye ' quand même (try) !*)
    print_int (pop s);
    (*on risque depiler une pile vide!!*)
    with Empty ->
        print_string "pile vide : t ouf ???\n";;
```

#### Rendu (jusqu'aux empilements)

```OCaml linenums="1"
# open Stack;;
# let s = create ();;
val s : '_a Stack.t = <abstr>
# push 3 s; push 6 s; push 7 s;;
- : unit = ()
# let s' = copy(s);;
val s' : int Stack.t = <abstr>
# for i = 0 to 2 do
    Printf.printf "%d; " (pop s);
done;;
    7; 6; 3; - : unit = ()
# push 3 s; push 6 s; push 7 s;;
- : unit = ()

# let s' = copy(s);;
val s' : int Stack.t = <abstr>
# for i = 0 to 2 do
    (*notre première boucle for CAML*)
    Printf.printf "%d; " (pop s);
done;;
    7; 6; 3; - : unit = ()
# try
    (*On sait que le code suivant risque de générer une exception *)
    (*mais on 'essaye' quand même (try)*)
    print_int (pop s);(*on risque de dépiler une pile vide*)
with Empty -> print_string "vous avez voulu dépiler une pile vide\n";;
        vous avez voulu dépiler une pile vide
- : unit = ()
```

#### Des piles en C

##### À partir de tableaux redimensionnables

Dans le fichier `vector.c` (cf. cours listes), ajoutons :

```C linenums="1"
// renvoie le sommet SANS dépiler
int vector_top(vector∗ v){
// renvoie l'élément en haut depile SANS dépiler 
int n = vector_size(v) − 1;
assert(0 <= n);
return vector_get(v, n);
}

// Dépile le sommet et le renvoie
// décrémente v−>size + redimensionnement possible
int vector_pop(vector∗ v){
    int n = vector_size(v) − 1;
    assert(0 <= n);
    int r = vector_get(v, n);
    vector_resize(v, n); //
    return r;
}
```

##### À partir de listes chaînées

Dans le fichier `tplc.c` (cf TP sur les listes chaînées) ajoutons :

```C linenums="1"
Liste∗ create(){// crée une liste vide
    Liste∗ liste = malloc(sizeof(Liste));
    liste−>first = NULL;
    return liste;
}

void push(Liste∗ liste,int x){//empiler
    ajouterDebut(liste, x);
}

int pop(Liste*  liste){// dépiler
    return supprimer(liste, 0);
}
```

## Files

### Définition

!!!quote "File (_queue_ en anglais)"

    En informatique, c'est une structure de données basée sur le principe du Premier entré, premier sorti, en anglais $\texttt{FIFO}$ $\texttt{(First In, First Out)}$, ce qui veut dire que les premiers éléments ajoutés à la file seront les premiers à être récupérés.

Le fonctionnement ressemble à une file d'attente à la poste : les premières personnes à arriver sont les premières personnes à sortir de la file.

<p align='center'><img src='/images/PilesFiles/pilesfiles3.png'/></p>

_Figure_ – Une pile $(\texttt{FIFO})$

### Applications

- Usage : Mémoriser temporairement des transactions qui doivent attendre pour être traitées dans _l'ordre d'arrivée_.
- Les serveurs d'impression, qui doivent traiter les requêtes dans l'ordre dans lequel elles arrivent, et les insèrent dans une file d'attente (ou une queue).
- Certains moteurs multitâches, dans un système d'exploitation, qui doivent accorder du temps-machine à chaque tâche, sans en privilégier aucune.
- Un algorithme de _parcours en largeur d'abord_ dans un graphe utilise une file pour mémoriser les nœuds visités.
- Pour créer toutes sortes de mémoires tampons (en anglais _buffers_).

### Primitives

Voici les primitives communément utilisées pour manipuler des files. Il n'existe pas de normalisation pour les primitives de manipulation de file. Leurs noms sont donc indiqués de manière informelle.

- « Mettre dans la file » : ajoute un élément dans la file. Terme anglais correspondant : « Enqueue » .
- « Défiler » : renvoie le prochain élément de la file, et le retire de la file. Terme anglais correspondant : « Dequeue » .
- « La file est-elle vide ? » : renvoie « vrai » si la file est vide, « faux » sinon.
- « Nombre d'éléments dans la file » : renvoie le nombre d'éléments dans la file.

### Implémentation en OCaml

- On peut utiliser une liste comme une file $\texttt{FIFO}$.
- Pas efficace : alors que les ajouts et suppressions en fin de liste sont rapides, les opérations d'insertions ou de retraits en début de liste sont lentes (car tous les autres éléments doivent être décalés d'une position).
- Pour implémenter une file, utiliser le module OCaml Queue qui a été conçue pour fournir des opérations d'ajouts et de retraits rapides dans la file.

### En OCaml

```OCaml linenums="1"
open Queue ;;(* importer le module *)
let q = create ();;(*file vide*)
let q' = copy(q);;(*copy de q*)
push 3 q; push 6 q; push 7 q;;(* ajout de 3 6 7*)
for i = 0 to 2 do (*vider q*)
    print_int (take q)
done ;;(* defiler 3 fois*)
try
    print_int (take q);(*défiler une file vide*)
with Empty -> print_string "file vide\n";;
```

Une exception $\texttt{Empty}$ est soulevée quand on essaye de défiler ($\texttt{take}$) une file vide.

### Rendu

```OCaml linenums="1"
# open Queue;;
# let q = create ();;
val q : '_a Queue.t = <abstr >
# let q' = copy(q);;
val q' : '_a Queue.t = <abstr >
# push 3 q; push 6 q; push 7 q;;
 - : unit = ()
# for i = 0 to 2 do (* vider q*)
    print_int (take q); (*défiler *)
done;;
    367- : unit = ()
# try
    print_int (take q);(*défiler une file vide*)
with Empty -> print_string "file vide\n";;
    file vide
 - : unit = ()
```

### Implémentations

En **C**, on peut créer les structures suivantes :

```C linenums="1"
// ce qu'on met dans la file
typedef struct _element{// on y met ce qu'on veut :
int v; // valeur ( mais autres champs possibles)
} elt;

typedef struct _m{// liste dblt chaînée ( why not ?)
    elt val;
    struct _m∗ next; // suivant
    struct _m∗ prev; // précédent
}maillon;

typedef struct _p{// structure de contrôle
    maillon∗ first; // pointe vers le 1er maillon
    maillon∗ last; // pointe vers le dernier
    // autres infos, par exemple :
    int size; // incr avec push, decr avec take
} grip;
```
