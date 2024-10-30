# Complément sur les expressions conditionnelles

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    - [Zestedesavoir](https://zestedesavoir.com/tutoriels/755/le-langage-c-1/1042_les-bases-du-langage-c/4294_les-selections/)
    - ≪ Langage $\texttt{C}$ ≫(Claude Delannoy) Eyrolles

## Choix en cascade

Considérons les choix en cascades suivants, dans lesquels les instructions concernées peuvent être des instructions simples (c.a.d suivies d'un point-virgule) ou des blocs :

```C linenums="1"
if (x >1) instruction 1
else if (x!=0) instruction 2
else if (x <1) instruction 3
else instruction 4
```

On peut trouver cette présentation un peu lourde (pas moi). Dans ce cas, le branchement multiple `switch` a ses avantages.

## Branchement multiple

```C linenums="1"
switch (x){
    case constante−1:
        sequence_d_instructions_1
        break;
    case constante−2:
        sequence_d_instructions_2
        break;
    ...
    case constante−n:
        sequence_d_instructions_n
        break;
    default:
        sequence_d_instructions_par_defaut
        break;
}
```

Dans ce qui précède `x` est une _expression_ de type entier (ou caractère) mais pas un flottant.

## Observations

- Parenthèses autour de `x`.
- `case yyy:`, `case yyy :`, `case yyy :` sont valides. Il faut que `yyy` soit une expression CONSTANTE (par exemple `12` ou une variable déclarée constante)
- Il est conseillé de mettre `break` à la fin d'une liste d'instructions.
- Le cas par défaut n'est pas obligatoire.

!!!example ""

    ```C linenums="1"
    char note;

    printf ("Quelle note as−tu obtenue ?"); scanf("%c", &note);

    switch(note){
        /∗ Si la note est comprise entre E et C ∗/
        case 'E': case 'D': case 'C':
            printf ("No comment.\n");
            break;
        /∗ Si la note est comprise entre B et A ∗/
        case 'B':
        case 'A':
            printf ("C'est correct voire mieux\n");
            break;

        default:
            printf ("Euh... note improbable ...\n");
            break;
    }
    ```

## Opérateur ternaire

L'_opérateur conditionnel_ ou _opérateur ternaire_ est un opérateur particulier dont le résultat dépend de la réalisation d'une condition. Son deuxième nom lui vient du fait qu'il est le seul opérateur du langage $\texttt{C}$ à nécessiter $3$ opérandes : une condition et deux expressions.

```C linenums="1"
(condition) ? expression si vrai : expression si faux
```

!!!example ""

    ```C linenums="1"
    int heure;
    
    scanf("%d", &heure);
    
    (heure >8 && heure <20) ?
    printf (" Il fait jour.\n") : printf (" Il fait nuit.\n");
    ```
