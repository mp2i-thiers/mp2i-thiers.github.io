# Boucles

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

!!!tip "Crédits"

    - [Openclassrooms](https://openclassrooms.com/fr/courses/19980-apprenez-a-programmer-en-c/14722-repetez-des-instructions-grace-aux-boucles)
    - « Langage C » -Claude Delannoy- Ed. Eyrolles
    - « Informatique - MP2I/MPI - CPGE 1re et 2e années » -Balabonski Thibaut, Conchon Sylvain, Filliˆatre Jean-Christophe, Nguyen Kim, Sartre Laurent- Ed. Ellipse

## Boucles `while`

Pour mémoire on rappelle le fonctionnement de la boucle `while` :

```C linenums="1"
while(Condition){
    // Instructions à répéter
}
```

Voici une boucle qui demande d'entrer un nombre jusqu'à ce que l'utilisateur saisisse $10$ :

```C linenums="1"
#include <stdio.h>

int main(){
    int nb = 0;
    while(nb != 10){
        printf("\nentrer un nb entier : ");
    }
    return 0;
}
```

## Boucle `do` ... `while`

Dans une boucle `while` , si la condition n'est pas réalisée alors le corps de la boucle n'est jamais exécuté.

Une façon d'éviter cela est de mettre le test de sortie de boucle à la fin :

```C linenums="1"
int compteur = 0; // le compteur
do{
    printf("le compteur vaut %i.\n", compteur);
    compteur ++; // incrémentation de 1
}while(compteur < 0); // ne pas oublier le point virgule
```

Après compilation et exécution :

```bash title="Rendu"
le compteur vaut 0.
```

## Boucle for

- Si on souhaite parcourir tous les entiers de $0$ à $n −1$, on peut le faire avec une boucle while (et gérer soi-même le compteur) :

`{int i = 0; while (i<n){...; i = i+1; }}`

- Une boucle `for(declaration, test, mise à jour)` permet de faire la même chose et gère la mise à jour :

`for (int i = 0; i<n; i = i+1) {...;}`

- Incrémentation : On peut écrire `i+=1` à la place de `i=i+1` , ou même `i++` .
- Décrémentation : `i--`

### Compléments

La boucle `for` a une forme non limitée à l'exemple du transparent précédent.

- Pour une variable x déclarée hors de la boucle, on peut écrire :

```C linenums="1"
int x = 3 ; ...
for(; x != 1; x=f(x)){...}
```

- Et si start, stop, step sont 3 fonctions définies par ailleurs

```C linenums="1"
for(start(); tests(); step()){...}
```

## Opérateurs d'incrémentation

- `i++` est une expression qui renvoie la valeur de la variable $i$ $\underline{\text{avant}}$ son incrémentation.
- `++i` est une expression qui incrémente $i$ $\underline{\text{puis}}$ renvoie la valeur de la variable $i$.

!!!example ""

    ```C linenums="1"
    int n = 3;
    while (n−− > 0) printf("n=%d", n);
    ```

    affiche $\texttt{n=2,n=1,n=0}$. On compare en effet successivement $3,2,1,0$ avec $0$ car `n-- > 0` décrémente **après** la comparaison.

    - Mais
    
    ```C linenums="1"
    int n = 3;
    while (n−− > 0) printf("n=%d", n);
    ```

- Moyen mémotechnique : mettre le `--` le plus loin possible de l'opérateur de comparaison.
- Avec `--n>0` on comprend que  
    - d'abord on décrémente et
    - ensuite on compare.
- Avec `0<n--` on comprend que
    - d'abord on compare et
    - ensuite on décrémente.
- Mais de toute façon, je déconseille fortement d'utiliser ces opérateurs dans d'autres instructions.

## Danger des opérateurs d'incrémentation

- L'ordre d'évaluation des opérandes (de gauche à droite ?) ou des arguments d'un appel de fonction n'est pas spécifié en $\texttt{C}$.
    - Il faut donc se méfier des appels comme `f(x++,x)` car la norme ne dit pas si le second paramètre est affecté ou non par l'incrémentation (ça peut dépendre des implémentations !).
    - Si `x=3` , cet appel peut être compris `f(3,3)` ou `f(3,4)`.
- Nous réservons alors sagement les expressions comme `i++` et `i--` à la partie « incrémentation » des boucles, sans les inclures dans d'autres expressions.

## Les instructions `break` et `continue`

- L'instruction `break` permet de sortir d'une boucle ; `continue` de passer à l'itération suivante.
- Ces instructions ne concernent que le bloc d'instruction courante.
- Si, par exemple, `break` est utilisée dans une double boucle imbriquée, il ne permet de sortir que de la boucle interne.

### `break`

```C linenums="1"
    int i, j;
    for(i = 0; i < 4 ; i++){
        printf("\ni = %d,", i);
        for(j = 0; j < 5 ; j++){
            if(j == 2) break; // quitter la boucle interne
            printf("j = %d,", j);
        }// f o r j
    }// for i
    printf("\nbye\n");
```

Produit l'affichage

```bash title="Rendu"
i = 0, j = 0,j = 1,
i = 1, j = 0,j = 1,
i = 2, j = 0,j = 1,
i = 3, j = 0,j = 1,
bye
```

### `continue`

Le code suivant :

```C linenums="1"
int i;
for(i = 0; i < 5 ; i++){
    if(i == 3) continue; // passer à l'itération suivante
    // cette instruction est ignorée lorsque i == 3
    printf("i = %d\n", i);
}
printf("bye\n");

```

produit l'affichage

```bash title="Rendu"
i = 0
i = 1
i = 2
i = 4
bye
```
