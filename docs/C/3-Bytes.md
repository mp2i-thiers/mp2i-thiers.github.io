# Bytes et tailles

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par Félix qui continue le travail fait par Lorentzo et Elowan et Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d'être plus compréhensible pendant les périodes de révision que des diaporamas.

## Définition ♥

!!!quote "byte"

    On appelle _byte_ (ou _multiplets_ en français) la plus petite unité « logiquement » adressable par un programme sur un ordinateur. Aujourd'hui, le besoin d'une structure commune pour le partage des données a fait que le byte de $8$ bits, ou $1$ octet, s'est généralisé en informatique

!!!note "Remarque (Wikipedia)"

    Le langage $\texttt{C}$ ne peut pas réserver une quantité de mémoire inférieure à un byte.

## Bon à savoir

- Dans les années $70$ et auparavant, il existait des processeurs avec des bytes de tailles très variables.
- Actuellement, il existe encore des processeurs avec des tailles de bytes de $4$ bits voire moins (notamment pour la programmation des automates industriels).
- Beaucoup de microprocesseurs adressent physiquement la mémoire avec des mots de plusieurs bytes afin d'augmenter les performances.
- En $\texttt{C}$, l'opérateur unaire `sizeof` donne la taille en bytes de son opérande. L'opérande peut être un spécificateur de type ou une expression.
- Nous prendrons $1$ octet comme taille de byte pour la suite du cours.

## Utilité de `sizeof`

- Il est souvent utile de connaître la taille d'un type de donnée (en particulier d'une _structure_).
- En C la taille des types primitifs est une constante exprimée en byte. Cependant, la taille réelle en octet du byte dépend des machines et de l'OS. Pour assurer la portabilité du code, il est préférable de réserver une zone mémoire en byte plutôt qu'en octet.
- Ci-dessous, la fonction malloc alloue une zone mémoire faisant la taille (en bytes) de $10$ entiers. Elle retourne un pointeur sur cette zone :

```C linenums="1"
int∗ pointeur;
pointeur = (int∗) malloc(10 ∗ sizeof(int));
```

## Taille d'un tableau

Pour un tableau de $5$ entiers `int t[5];` :

- la commande `sizeof(t)` donne la taille en bytes. Aucune inclusion n'est nécessaire pour $\texttt{sizeof}$.
En revanche, sa valeur de retour est un entier de type `size_t` (lequel est défini dans l'entête standard $\texttt{stdio.h}$).
- la commande `sizeof (*t)` (ou encore `sizeof *t` ) donne la taille du type des éléments de `t`.On peut aussi utiliser `sizeof(int)`
- `sizeof(t[0])` renvoie la taille du $1$er élément de `t`. C'est la taille d'un int .
- Si on ne connaît pas le nombre d'éléments de `t` , on le retrouve par `sizeof t/ sizeof *t;`. $\color{red}\text{Cette technique fait bien ce qu'on veut mais uniquement dans le bloc où t est déclaré !}$
Les parenthèses ne sont pas obligatoires avec `sizeof` s'il s'agit de déterminer la taille d'une expression. Elles le deviennent pour le nom d'un type.

!!!example "Exercice"

    Dans un fichier $\texttt{taille.c}$ :
    
    - Directement dans le main, afficher la taille des types `size t, int, int* `.
    - Directement dans le main , créer maintenant un tableau de $5$ entiers et afficher sa taille avec la technique précédente.
    - Écrire une fonction `int tailletab (int t[])` qui prend en paramètre un tableau et affiche sa taille selon la technique précédente. Appeler maintenant dans le main cette fonction avec un tableau de $5$ entiers et comparer avec l'affichage précédent. Expliquer.

## Multiples de l'octet

- Un octet vaut $8$ bits.
- Les tailles de fichiers ou de variables sont données en multiples de l'octet. Les multiples décimaux sont les plus utilisés (kilo, méga, giga, téra) mais ne sont pas recommandés par la Commission électrotechnique internationale.
- Une nouvelle norme à été créée en $1988$ pour noter les multiples de $2^10 = 1024$ : les $\underline{\text{kibi}}$ (kilo binaire), $\underline{\text{mébi}}$ (méga binaire), $\underline{\text{gibi}}$ (giga binaire) etc.
- Certains systèmes d'exploitation annoncent faussement les quantités d'octets avec un suffixe décimal.

!!!example ""

    Par exemple une quantité de $32$ $\text{Go}$ peut être affichée correctement comme $29.8$ $\text{Gio}$ ou improprement comme $29.8$ $\text{Go}$ sur certains systèmes d'exploitations.

## Multiples décimaux de l'octet

Multiples décimaux de l'octet dans le SI et mauvais usage :

|Nom|Symbole|Valeur en octets|Mésusage|
|:-:|:-:|:-:|:-:|
|kilooctet| $\text{ko}$| $10^3$| $2^{10}$|
|mégaoctet| $\text{Mo}$| $10^6$| $2^{20}$|
|gigaoctet| $\text{Go}$| $10^9$| $2^{30}$|
|téraoctet| $\text{To}$| $10^{12}$| $2^{40}$|
|pétaoctet| $\text{Po}$| $10^{15}$| $2^{50}$|
|exaoctet| $\text{Eo}$| $10^{18}$| $2^{60}$|
|zettaoctet| $\text{Zo}$| $10^{21}$| $2^{70}$|
|yottaoctet| $\text{Yo}$| $10^{24}$| $2^{80}$|

$\text{ko, Mo,Go}$ (noter les majuscules et minuscules) ♥

## Multiples binaires de l'octet

|Nom|Symbole|Valeur en octets|Mésusage|
|:-:|:-:|:-:|:-:|
|kibioctet| $\text{Kio}$| $2^{10}$|$1024$ octets|
|mébioctet| $\text{Mio}$| $2^{20}$|$1024$ $\text{Kio}$|
|gibioctet| $\text{Gio}$| $2^{30}$|$1024$ $\text{Mio}$|
|tébioctet| $\text{Tio}$| $2^{40}$|$1024$ $\text{Gio}$|
|pébioctet| $\text{Pio}$| $2^{50}$|$1024$ $\text{Tio}$|
|exbioctet| $\text{Eio}$| $2^{60}$|$1024$ $\text{Pio}$|
|zebioctet| $\text{Zio}$| $2^{70}$|$1024$ $\text{Eio}$|
|yobioctet| $\text{Yio}$| $2^{80}$|$1024$ $\text{Zio}$|

$\text{Kio, Mio,Gio}$ (noter les majuscules et minuscules) ♥

## Taille d'un fichier

### La commande `du`

- Dans un terminal depuis le répertoire courant

```bash linenums="1"
$du −h
```

Donne la taille du répertoire courant et ses sous-répertoires

- Pour avoir la taille des fichiers du répertoire, utiliser `*` . Option `-h` pour « Human redable » : résultats en Kilo-octet, Mega-octet, Giga-octect...

```bash linenums="1"
$du −h ∗
```

- L'option `-s` permet de n'afficher que le total de la taille du
répertoire.

```bash linenums="1"
$du −sh
```

### La commande `ncdu`

Plus conviviale que `du` , `ncdu` permet de naviguer dans l'arborescence avec le clavier (flèches et retour chariot)

<p align='center'><img src='/images/Bytes/bytes1.png'/></p>

_Figure_ – La commande `ncdu`

Entrer `q` pour quitter
