# Arguments en ligne de commande

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d’être plus compréhensible pendant les périodes de révision que des diaporamas.

## Les arguments en ligne de commande

Comme en _C_ ou en _Python_ les arguments qui sont passés en ligne de commande d’un programme _OCaml_ sont stockés dans un tableau. Il est traditionnel de nommer ce tableau **argv**.

On le trouve dans le module **Sys** de la bibliothèque standard. Son nom complet est donc `Sys.argv`. Le nombre
d’arguments (parmi lesquels le nom du programme en position 0) est la longueur du tableau.

On verra plus tard comment travailler avec les tableaux en OCaml. Retenons simplement qu’on accède à la case $i$ du tableau `tab` par `tab[i]`.
Dans un fichier _arg.ml_, entrer :

```Ocaml linenums="1"
let boucle tab =
    let n = Array.length tab in 
    let rec aux i = match i with
        | x when x=n -> ()
        | _ -> Printf.printf "[%d] %s\n" i tab.(i);
            aux (i+1)
    in aux 0

boucle Sys.argv
```

Ce programme affiche la liste des arguments et leurs positions dans `Sys.argv`.
Compiler, le programme

``` bash
$ ocamlopt −o args args.ml
```

Dans un terminal, la commande `./args arg1 arg2 arg3` produit l’affichage :
```
[0] ./args
[1] arg1
[2] arg2
[3] arg3
```