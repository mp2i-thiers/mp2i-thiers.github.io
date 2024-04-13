# Impératif et types

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par
    Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce
    site à pour seul but d’être plus compréhensible pendant les périodes de
    révision que des diaporamas.

???+ abstract "Sommaire"
    - Tableaux, références, boucles
    - Exceptions
    - Types personnalisés
  
        * Type enregistrement
        * Type somme
   
    - Astuce : le type option 

!!!tip "Crédits"
    Develooppez.com  
    λ OCaml Programming  
    FAQ Caml  

## Tableaux, références, boucles

### Tableaux

Déclaration d’un tableau :  

```Ocaml linenums="1"
let tab = [|3;6;8|];; (* à la main *)

let tab = Array.make 5 0;; (* un tableau de 5 zéros *)

(* Créer une matrice à la main *)
let mat = let p = Array.make 3 [||] in
    p.(0)<-[|1;2|]; p.(1) <-[|0;9|]; p.(2)<-[|-1;1|]; p;;

(* utiliser une fonction de bibliothèque *)
Array.make_matrix 3 2 0;;
```

### Accès aux éléments

```Ocaml linenums="1"
# let tab = [|3;6;8|] in
tab.(0)<-10;
Printf.printf "tab.(%d)=%d\n" 0 tab.(0);;
    tab.(0) =10
- : unit = ()
# let mat = Array.make_matrix 3 2 0 in
mat.(2).(1)<- -4; mat ;;
- : int array array = [|[|0; 0|]; [|0; 0|]; [|0; -4|]|]
```

### Parcours d’un tableau, longueur, boucle for

```Ocaml linenums="1"
# let tab = [|3;6;8|];;
val tab : int array = [|3; 6; 8|]
# let n = Array.length tab in
  for i = 0 to (n-1) do
    Printf.printf " tab.(%d)=%d; " i tab.(i) ;
  done ;;
    tab.(0)=3; tab.(1)=6; tab.(2)=8; - : unit = ()

# let n = Array.length tab in
  for i = n -1 downto 0 do
    Printf.printf "tab.(%d)=%d; " i tab.(i) ;
  done ;;
  tab.(2)=8; tab.(1)=6; tab.(0)=3; - : unit = ()
```

### Références

Une référence est un pointeur vers un objet. Cela permet de rendre des  variables mutables.

```Ocaml linenums="1"
# let n = ref 0;;
val n : int ref = {contents = 0}
# n := !n+3;;
- : unit = ()
# Printf.printf "!n = %d\n" !n ;;
!n = 3
- : unit = ()
```

### Boucle while

```Ocaml linenums="1"
# let i = ref 0 and s = ref 0 in
while !i<10 do
    s := ! s + ! i ;
    incr i ;
done ;
Printf.printf "!s = %d\n" !s ;;
!s = 45
- : unit = ()
```

## Exceptions

### Présentation

!!! note ""
    - En programmation fonctionnelle, les fonctions sont totales,  c’est-à-dire qu’elles sont applicables à tout argument qui appartient à  leur type de départ.  
    - Il faut donc être capable de traiter les cas, appelés exceptions , où cet  argument n’est pas acceptable. Par exemple : une division par zéro ou  bien la recherche de la tête d’une liste vide.  
    - Ceci peut être fait par des tests préventifs placés dans le corps des  fonctions, mais ce mécanisme est très lourd, car il implique un travail  important pour le programmeur et il altère la lisibilité d’un  programme en masquant son fonctionnement normal.  
    - C’est pourquoi les langages de programmation modernes comportent  un mécanisme spécifique pour le traitement des exceptions.  

### Exceptions prédéfinies

Les exceptions ont le type `exn`. On les soulève avec la commande `raise`.

- Une exception qui porte bien son nom :  

```Ocaml linenums="1"
# Exit ;;
- : exn = Pervasives.Exit
# raise Exit ;;
Exception : Pervasives.Exit.
```

- Une qu’on connaît bien

```Ocaml linenums="1"
# failwith " big pb ! " ;;
Exception : Failure " big pb ! ".
```

Observons que l’invocation de `failwith` déclenche le `raise`.
  
- Une exception au sens compréhensible  

```Ocaml linenums="1"
# 1/0;;
Exception : Division_by_zero.
```

C’est l’évaluateur de OCaml qui déclenche cette exception.  

- Une autre qui rappelle le C  

```Ocaml linenums="1"
# let l = [3] in assert (List.mem 2 l);;
Exception : Assert_failure("//toplevel//", 1, 15).
```

### Déclarer une exception

```Ocaml linenums="1"
# exception MyException ;;
exception MyException
# MyException ;;
- : exn = MyException
# raise MyException ;;
Exception : MyException.
# exception MyException2 of string ;;
exception MyException2 of string
# raise (MyException2 "big pb") ;;
Exception : MyException2 "big pb".
# exception MyException2 of string ;;
exception MyException2 of string
# let f = function x ->
    if x <> 0 then
        365 / x
    else
        raise (MyException2 " Division par zéro ") ;;
            val f : int -> int = <fun>
# f 3;;
- : int = 121
# f 0;;
Exception : MyException2 " Division par zéro ".
```

### Récupération d’une exception

La récupération d’une exception déclenchée lors de l’évaluation d’une  expression `e` peut être réalisée en encapsulant cette expression dans une  expression `try..with`.Voici la syntaxe à utiliser :

```Ocaml linenums="1"
try expr with
| p1 -> e1 (* si le type d'exception est p1 , renvoyer e1 *)
|...
| pn -> en
(* si le type d’exception est p1 , renvoyer e1 *)
```

Dans l’exemple suivant, on met la tête de la liste `l2` dans `l1` et, si une  exception de type `Failure` est soulevée, on renvoie la liste vide :  

```Ocaml linenums="1"
# let ajouter l1 l2 =
    try List.hd l2::l1 with Failure _ -> [];;
    val ajouter : ’a list -> ’a list -> ’a list = <fun>
# ajouter [2] [3;4];;
- : int list = [3; 2]
# ajouter [2] [];;
- : int list = []
```

## Types personnalisés

### Type enregistrement

#### Présentation

!!! note ""
    L’idée est celle des `struct` de C. On accède aux champs avec la notation  pointée; on les modifie avec la notation ﬂéchée des tableaux.

```Ocaml linenums="1"
# type complex = {x : float ; y : float};;
type complex = {x : float ; y : float ;}
# let i = {x=1.; y=1.};;
val i : complex = {x = 1.; y = 1.}
# i.x , i.y ;;
- : float * float = (1., 1.)
# i.x=4.;;
- : bool = false
# i.x<-4.;;
Characters 0-7:
i.x<-4.;;
^^^^^^^
Error : The record field x is not mutable
```

#### Champs mutables

Il peut ête souhaitable de modifier certains champs. Il faut les déclarer comme mutable. Par défaut, un champ est persistant.  

```Ocaml linenums="1"
# type complex = {mutable x : float ; y : float};;
type complex = {mutable x : float ; y : float ;}
# let i = {x =1.; y =1.};;
val i : complex = {x = 1.; y = 1.}
# i.x , i.y ;;
- : float * float = (1. , 1.)
# i.x=4.;;
- : bool = false
# i.x<-4.;;
- : unit = ()
# i.y<-1.;;
Characters 0-7:
i.y<-1.;;
^^^^^^^
Error : The record field y is not mutable
```

#### Polymorphisme

```Ocaml linenums="1"
type (’a , ’b) fourre_tout = {x : ’a ; y : ’b};;
let z = {x=2; y = "toto"};;
```

### Type somme

#### Présentation

!!! note ""
    - Un type somme est formé d’une liste de cas possibles pour une valeur  de ce type, chaque cas comporte un nom de cas, le "constructeur",  et une (éventuelle) valeur associée (l’argument du constructeur).
    - Un cas dégénéré consiste à définir un type dont les constructeurs  n’ont pas d’argument (constructeurs constants). On parle alors de  type énuméré (le symbole | se lit "ou")  

```Ocaml linenums="1"
# type couleur = Bleu | Blanc | Rouge ;;
type couleur = Bleu | Blanc | Rouge
# Bleu ;;
- : couleur = Bleu
# let fleur c = match c with
    | Bleu -> " bleuet "
    | Blanc -> " marguerite "
    | Rouge -> " coquelicot "
in fleur Rouge ;;
- : string = " coquelicot "
```

#### Type somme

On peut passer des paramètres aux constructeurs.  
Ci-dessous on définit un type pour les piles d’entiers. Une pile non  vide posséde deux éléments : une étiquette entière et une pile (vide  éventuellement). On écrit une fonction qui fait la somme du contenu  de la liste :  

```Ocaml linenums="1"
# type stack_of_int = Vide | P of int * stack_of_int;;
type stack_of_int = Vide | P of int * stack_of_int
# let rec somme p = match p with
    | Vide -> 0
    | P (x , q) -> x + somme q ;;
        val somme : stack_of_int -> int = <fun>
```

On crée ensuite une pile dont le sommet est 3, l’élément intermédiaire  2 et la base 1. On lui applique la fonction `somme` :  

```Ocaml linenums="1"
# let p = P (3 , P (2 , P (1 , Vide))) ;;
val p : stack_of_int = P (3 , P (2 , P (1 , Vide)))
# somme p ;;
- : int = 6
```

#### Type somme polymorphe

Ci-dessous on définit un type pour les piles polymorphes. On écrit une  fonction qui prend en paramètre une pile, une addition adaptée et une valeur de départ.  

```Ocaml linenums="1"
# type ’a stack = Vide | P of ’a * ’a stack;;
type ’a stack = Vide | P of ’a * ’a stack
# let rec somme_polymorphe p add start = match p with
    | Vide -> start
    | P (x , q) -> add x (somme_polymorphe q add start) ;;
        val somme_polymorphe : ’a stack -> (’a -> ’b -> ’b)
            -> ’b -> ’b = <fun>
```

On crée ensuite une pile de ﬂottant. On lui applique la fonction  `somme_polymorphe` à laquelle on passe l’addition des ﬂottants  
`(+.)` et le point de départ `0.` :  

```Ocaml linenums="1"
# let p = P (0.3 , P (0.2 , P (0.1 , Vide))) ;;
val p : float stack = P (0.3 , P (0.2 , P (0.1 , Vide)))
# somme_polymorphe p (+.) 0.;;
- : float = 0.600000000000000089
```

## Astuce : le type option

### Utilité du type option

Nous voulons écrire une fonction qui retourne en général une valeur  mais, parfois, ne retourne rien.  
Par exemple, la fonction `list_max` renvoie le maximum d’une liste si  elle n’est pas vide. Mais on ne sait trop quoi faire avec la liste vide :

```ocaml linenums="1"
let rec list_max = function
    | [] -> ???
    | h :: t -> max h (list_max t)
```

!!! note ""
    Pour la liste vide, on peut :

    * renvoyer une `min_int` ? mais le code ne fonctionnerait qu’avec une  liste d’entiers
    * Soulever une exception ? mais il faudra que l’utilisateur se souvienne  d’encapsuler ses appels dans un `try..with`  
    * Renvoyer `NULL` ? mais cette notion existe en C pas en OCaml  

    **La meilleure solution est d’employer le type `option`.**

### Le type option

Le type `’a option` possède deux constructeurs `Some` et `None`.
un élément du type `option` peut être vu comme une boîte qui est soit vide, soit contenant un objet d’un certain type.  

```Ocaml linenums="1"
# None ;;
- : ’a option = None
# Some 45;;
- : int option = Some 45
# Some " toto " ;;
- : string option = Some " toto "
```

### Type option : Accès au contenu

On accède au contenu de la boîte par filtrage. Ci-dessous on écrit une fonction qui extrait un entier d’une option (s’il y en a un dedans) et le convertit en `string` :  

```Ocaml linenums="1"
# let extract o =
    match o with
    | Some i -> string_of_int i
    | None -> "";;
        val extract : int option -> string = <fun>
# extract (Some 42) ;;
- : string = "42"
# extract None ;;
- : string = ""
```

#### Maximum d’une liste

On revient au programme de recherche du maximum. On ne renvoie rien  (donc `None`) dans le cas où la liste est vide.

```ocaml linenums="1"
# let rec list_max = function
    | [] -> None
    | h :: t -> begin
        match list_max t with
            | None -> Some h
            | Some m -> Some (max h m)
        end ;;
                val list_max : ’a list -> ’a option = <fun>
# list_max [5;1;9;2];;
- : int option = Some 9
# list_max [];;
- : ’a option = None
```
