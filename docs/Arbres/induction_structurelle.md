# Induction structurelle

!!! warning
    Ce cours a été automatiquement traduit des transparents de M.Noyer par
    Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce site à pour seul but d’être plus compréhensible pendant les périodes de révision que des diaporamas.

## Présentation

### Objets inductifs

Les objets inductifs sont des objets construits progressivement à partir d’objets de même nature. Les objets inductifs sont décrits par deux choses :

1. un ou plusieurs objets de base (qui forment le point de départ de toute construction) ;
2. une ou plusieurs règles de combinaisons (qui permettent de former des objets plus grands)

!!!example "Exemple"
      Les constructions en Lego :

      - les briques et les plaques sont des constructions de base
      - A partir de deux constructions en Lego, on obtient une troisième en les emboîtant.

!!!example "Exemple des listes en OCaml"

      Cas de base : la liste vide [ ]

      Règle de combinaison : si l est une liste et e un élément, alors e::l est un e nouvelle liste qui contient un élément de plus que la queue l .

!!!example "Exemple : les entiers de Peano"
      Cas de base : L’entier zéro, noté Z est un nombre entier naturel.

      Règle de construction : si n est un entier naturel, alors son successeur, noté S(n) est encore un entier naturel.

      3 se note S(S(S(Z))).

### **Fonction sur les termes**

Pour définir une fonction sur les termes, on se donne autant d’équations que de constructeurs. On définit l’addition + : N × N → N par Z + m = m et S(n) + m = S(n + m)

!!!example "Exemple"
      (en désignant Z par 0, 2 par S(S(Z)) etc :

      2 + 3 = S(1) + 3 = S(1 + 3) = S(S(0) + 3) = S(S(0 + 3)) = S(S(3)) = 5.

### **Constructeurs**

On utilise une syntaxe proche de OCaml. A chaque objet de base et chaque manière de construire un nouvel objet à partir d’objets plus petits sont associés un symbole appelé constructeur. Tout objet est alors construit comme une combinaison d’applications explicites de ces constructeurs.

### **Constructeurs, arités, termes**

!!!quote "Définition : Constructeur"

      Un constructeur est un symbole attendant un nombre fixe d’arguments. Ce nombre est l’arité du constructeur.
   
      Pour définir un ensemble E d’objets inductifs, on fournit une signature, c.a.d un ensemble de constructeurs. Les éléments de E, appelés termes sont alors construits en utilisant exclusivement la règle suivante : Si c est un constructeur d’arité n et si t_1,...,tn sont n termes alors l’application $c(t_1,...,t_n)$ forme un terme.

### **Sous-termes immédiats**

Si $t=c(t_1,...,t_n)$, on dit que $t_1$ (ou $t_2$ etc.) est un sous-terme immédiat de $t$. Dans la notation $t=c(c(t_2,t_3),t_1)t_1$ est un sous-terme immédiat de $t$ mais $t_2,t_3$ sont des sous-termes (non immédiats)

### **Cas de bases, cas de combinaisons**

Un constructeur c d’arité zéro est lui-même un terme. Un tel constructeur est appelé une constante et forme un cas de base. Un constructeur c d’arité non nulle doit nécessairement être appliqué à plusieurs termes déjà construits pour former un nouveau terme. Cela représente un cas de combinaison. Un constructeur n-aire est un constructeur d’arité n. On parle de constructeurs unaires, binaires, ternaires pour les arités 1, 2, 3

### **Structure des termes**

!!!quote "Définition : Sous termes immédiats"

      Les termes $t_1$ à $t_n$ utilisés pour construire un terme $t=c(t_1,...,t_n)$ sont appelés sous-termes immédiats de $t$. Deux termes sont égaux si et seulement si ils sont construits de la même façon, c’est à dire à partir des mêmes sous-termes, combinas par les mêmes constructeurs

!!!tip ""
      **Remarque**

      Cette égalité est syntaxique. 

      Contre-exemple : L’égalité dans Q n’est pas seulement syntaxique. En eﬀet $\frac{6}{4}$ est égal à $\frac{3}{2}$.

### **Précisions**

Sauf mention du contraire, on ne considère que des termes finis : ceux qui peuvent être formés à partir d’un nombre fini d’applications de constructeurs. Un ensemble inductif E dont la signature contient au moins un symbole de constante est non vide. Une signature peut très bien contenir une infinité de symboles (mais les termes n’en utilisent qu’un nombre fini).

### **Signature typée**

Parfois, on impose que chaque élément d’un constructeur soit d’une nature précise. Par exemple dans e::l , on s’attend à ce que e et l aient des types diﬀérents. Le premier est de type élément et le second de type liste d’éléments. On peut donc utiliser une notation de type c : $e_1$ × · · · × En → E pour indiquer qu’un constructeur c d’arité n attend des arguments pris dans les ensembles respectifs $e_1$, . . . , En. Avec cette convention, c : E désigne que c est une constante de l’ensemble E .

## **Ordre structurel**

### **Ensembles bien fondés**

!!!quote "Définition : "
      Un ensemble E est dit **bien fondé** s’il est muni d’une relation d’ordre ≼ telle qu’il n’existe pas de suite strictement décroissante de cet ensemble.

      Un ensemble est dit **bien ordonné**, si il est bien fondé et si, de plus, la relation d’ordre est totale.

      Rappel : une suite sur E est simplement une application f : N → E

### **Ensembles bien ordonnés/fondés**

!!!example "Exemples"
      ℕ muni de ≤ usuel est bien ordonné

      $ℕ^2$ muni de l’ordre lexicographique est bien ordonné $(a, b)  ≼_L (a', b') \Leftrightarrow (a < a') ∨ (a = a' ∧ b ≤ b')$

      $ℕ^2$ muni de l’ordre produit est bien fondé $(a, b)  ≼_P(a', b') \Leftrightarrow (a ≤ a') ∧ (b ≤ b')$
      Relation non totale.

      ℤ muni de l’ordre usuel n’est pas bien fondé (−1, −2, −3 . . . est infini strictement décroissante).

### **Bon ordre et élément minimal**

!!!warning ""
      **Proposition**

      Un ensemble ordonné E est bien ordonné si et seulement si toutes les parties non vides de E admettent un élément minimal unique. 
      
      Par élément minimal, on entend un élément plus petit que tous les autres.

!!!note "Preuve"
      **Si E est bien ordonné**

      Soit A ⊂ E une partie non vide sans élément minimal. Prenons $e_0$ ∈ A.

      - Puisque $e_0$ n’est pas minimal, on peut trouver $e_1$ ∈ A tel que ¬($e_1$ ≥ $e_0$), c’est à dire $e_1$ < $e_0$ puisque l’ordre est total.
      - De proche en proche on construit un n-uplet ($e_0$, $e_1$, . . . , en) de valeurs de A donc de E strictement décroissantes aussi longue qu’on veut. Contradiction avec "bien fondé". A possède donc un élément minimal.
      - L’unicité de l’élément minimal vient de l’antisymétrie. 
      Si e, e' sont minimaux dans A, e ≤ e' et e' ≤ e, donc e = e' par antisymétrie.

      **Si toute partie a un élément minimal**

      L’ordre est total car toute partie à deux éléments admet un élément minimal. L’un est donc plus petit que l’autre.

      - Considérons la suite $(u_n)_n$ d’éléments de E . Posons $U={u_n|n\in\mathbb{N}}⊂E$.
      - Comme U≠∅, U admet un élément minimal, disons, $u_k$ . Si la suite est strictement décroissante, alors $u_k>u_{k+1}$, ce qui contredit la minimalité.
      - Il n’y a pas de suite strictement décroissante dans l’ensemble : il est bien fondé.
  
### **Éléments sans prédécesseurs**

!!!quote "Définition: Elements sans prédécesseur"
      Dans toute partie d’un ensemble bien fondé, il y a un ou des éléments qui ne sont plus petits qu’aucun autre de cette partie. On les appelle éléments sans prédécesseur.

Soit E bien fondé et $e_0$, F tels que $e_0$∈ F ⊂ E . Si $e_0$ est sans prédécesseur dans F , alors on est content.
Sinon, on peut trouver $e_1$<$e_0$ dans F .

De proche en proche on construit une suite $e_0$>$e_1$>···>en dans F. Mais cette suite ne se prolonge pas indéfiniment car l’ordre est bien fondé.

Donc il existe $e_k$∈F sans prédécesseur. Réciproquement Si toute partie d’un ensemble ordonné admet des éléments sans prédécesseur, l’ensemble est bien fondé.

*Preuve : adapter le transparent précédent.*

## **Taille et ordre**

### **Taille**

!!!quote "Définition : Taille d'un terme"
      La taille d’un terme est le nombre de constructeurs qui le composent. Si t est un terme, on note |t| sa taille.

!!!example "Exemple"
      Pour les entiers de Peano, la taille de Z est 1, celle de S(S(S(Z))) est 4.

La définition de la taille peut subir quelques aménagements suivant les contextes.

Par exemple, pour les listes on a coutume de dire que [ ] est de taille 0 et non 1, et que la taille de [1;2] (qui est en fait 1::2::[ ] ) est 2 et non 3. La taille des listes est donc usuellement un translaté de -1 de la définition de la taille prise dans le transparent précédent.

### Ordre structurel

!!!quote "Définition : Ordre structurel"
      Soit E un ensemble de termes et $(t_1, t_2) ∈ E 2$. Notons $t_1 <_i t_2$ pour indiquer que $t_1$ est un sous-terme "immédiat" de $t_2$.

      L’ordre structurel sur E est la relation d’ordre ≤ engendré par $<_i$, c.a.d la clôture réﬂexive-transitive de $<_i$ (plus petite relation binaire contenant $<_i$ et à la fois réﬂexive et transitive).
      Un sous-terme de t est un terme t' tel que t' ≤ t

!!!tip ""
      **Remarque**

      Pour s’assurer que la relation ≤ engendre bien un ordre, il faut s’assurer qu’elle est anti-symétrique. Elle est, par définition de la clôture, réﬂexive et transitive.

### Clôture réﬂexive-transitive

La clôture réﬂexive-transitive de $<_i$ est par définition la plus petite relation binaire R sur E (i.e. partie de E × E ) telle que :

1. $<_i$ est incluse dans R
1. Si x∈E, alors(x,x)∈R
1. Si (x, y ) ∈ R et (y , z) ∈ R alors (x, z) ∈ R si x ∈ E , alors (x, x) ∈ R

!!!tip ""
      **Remarque**

      Idée de la preuve pour l’existence de ≤ (qu’on préfère noter de façon infixe)

      Indication : E × E contient  $<_i$ et est réﬂexive-transitive. Donc l’ensemble des relations qui vérifient les 3 points est non vide. 
      Alors l’intersection de toutes les relations R qui vérifient les points ci-dessus est bien réﬂexive-transitive. 
      Et c’est la plus petite à le vérifier : on la note ≤.

### Théorème de l’ordre bien fondé

!!!danger ""
      **Théorème**
      Soit E un ensemble de termes. Notons ≤ l’ordre structurel sur E et < l’ordre strict associé. On a les propriétés suivantes :

      - Si $t_1 < t_2$, alors $|t_1| < |t_2|$
      - La relation ≤ est une relation d’ordre bien fondée.

!!!note "Preuve"
      On suppose $t_1 < t_2$,

      Puisque $t_1 ≤ t_2$, alors par conséquence de la définition de la clôture réﬂexive-transitive, il existe $x_1,x_2,...,x_k$ tels que $x_1=t_1,x_k=t_2$ et pour tout j∈⟦1,k−1⟧, x_j <_i x_{j+1}.

      Comme $t_1 < t_2$, alors k ≥ 2.

      Pour j convenable, puisque $x_j$ est un sous-terme immédiat de $x_{j+1}$ (ce que signifie $x_j  <_i x_{j+1}$) et donc $|x_j| < |x_{j+1}|$ (puisqu’il faut au moins un constructeur de plus pour construire x_{j+1} que pour $x_j$ ). Par transitivité de < sur ℕ, on a $|t_1| < |t_2|$

      **Preuve : anti-symétrie**

      Soient $t_1, t_2$ deux termes tels que $t_1 ≤ t_2, t_2 ≤ t_1$ et $t_1 \neq  t_2$. On a donc $t_1 < t_2$ puisque $t_1 ≤ t_2$ et $t_1≠t_2$. Ainsi $|t_1| < |t_2|$ par le point précédent. De même, on obtient $|t_2| < |t_1|$. Alors $|t_2| ,     |t_1|$ sont dans N, $|t_1| < |t_2|$ et $|t_2| < |t_1|$ : ABSURDE.

      **Preuve : caractère bien fondé**

      Supposons qu’il existe une suite infinie strictement décroissante $(t_i)_{i∈ℕ}$ pour l’ordre structurel. Alors, par le point 1 du théorème, la suite des tailles $(|t_i|)_{i∈ℕ}$ est une suite infinie strictement décroissante de ℕ. Or cela est impossible puisque ℕ est bien ordonné. ABSURDE.

## Principe d’induction

### Présentation

La définition des ensembles inductifs amène naturellement à une technique de raisonnement sur les termes proche de la récurrence. On peut résumer cette technique ainsi : une propriété à propos des objets inductifs qui vaut pour toutes les constantes et est préservée par chaque construction inductive, est nécessairement vraie pour tous les objets pouvant être construits.

### Principe d’induction structurelle

!!!danger ""
      **Théorème**

      Soit E un ensemble inductif (non vide donc avec au moins une constante) et une propriété P à propos des objets de E. Si, pour chaque constructeur c d’arité n, la propriété $P(c(t_1,...,t_n))$ est satisfaite dès lors que les propriétés $P(t_1)$ à $P(t_n)$ sont satisfaites, alors P(t) est satisfaite pour tout t de E .

!!!tip ""
      **Remarque**

      Si t est une constante, alors pour tout terme $x ∈ E , x <_i t$ est faux. Donc $∀x ∈ E , (x  <_i t \Rightarrow P(x))$ est vraie. Par suite P est satisfaite pour tous les sous-termes immédiats de t. Il vient donc que P(t) est satisfaite.

      *Rappel :$x <_i t$ signifie que x est sous-terme immédiat de t.*

!!!note "Preuve"
      Supposons que le sous-ensemble A ⊂ E des termes ne satisfaisant pas P est non vide.

      A admet donc un élément sans prédécesseur $t_0$ puisque l’ordre est bien fondé. 
      Si $t_0$ est une constante, alors $P(t_0)$ est satisfaite. Ce n’est pas possible puisque $t_0 ∈ A$. 
      Si $t_0$ n’est pas une constante, il a des sous-termes immédiats. Ils ne peuvent donc être dans A puisque plus petits strictement que $t_0$.

      Mézalors, chaque sous-terme satisfait la propriété P. 
      D’après l’hypothèse sur t, cela veut dire que $P(t_0)$ est satisfaite et donc que $t_0∉A$ : ABSURDE.

!!!example "Exemple : élément neutre"

      Les équations définissant l’addition des entiers de Peano assurent que Z + n = n vaut pour tout n ∈ Z.

      Montrons que n + Z = n pour tout n également : 
      - Cas de base : Z + Z = Z par définition de + (1ere équation) 
      - Hérédité : prenons un entier de Peano n satisfaisant n + Z = n. Alors : S(n) + Z = S(n + Z )
      par déf. de + = S(n) Par HI 
      - On en déduit que pour tout entier de Peano n, n + Z = n.

### **Récurrence**

En récrivant le principe d’induction structurelle des entiers de Peano avec les notations habituelles (0 au lieu de Z , 2 au lieu de S(S(Z))), et en considérant P, un prédicat sur les entiers, nous obtenons que :

- Si P(0) est vrai
- Si pour tout n satisfaisant P(n), la propriété P(n + 1) est satisfaite ;

Alors pour tout n entier, la propriété P(n) est satisfaite. C’est précisément le principe de récurrence !!
