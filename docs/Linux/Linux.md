
Linux
=====
!!! danger
    Ce cours n'a pas été entièrement reverifié après le passage du programme. Pensez à supprimer ce message si vous avez reverifié ce cours

!!! warning 
    Ce cours a été automatiquement traduit des transparents de M.Noyer par
    Lorentzo et Elowan et mis en forme par Mehdi, nous ne nous accordons en aucun cas son travail, ce
    site à pour seul but d’être plus compréhensible pendant les périodes de
    révision que des diaporamas.

 Système d’exploitation  
 Présentation de Linux  
 Système de fichiers  Vue logique  Les principaux répertoires systèmes  
 Le shell bash  

#### Avant-propos


Ces transparents constituent une toute petite introduction forcément non  exhaustive au système Linux. Ils sont présentés pour faciliter le quotidien  des étudiants de MP2I dans l’élaboration de leurs programmes en cours  d’année. On y donne notamment  
des précisions sur les principaux répertoires sous Linux  
les commandes de survie dans le shell bash.  
On n’y trouve pas d’explication sur ce qu’est un disque logique ni sur la  notion d’inode. Un futur document traitera des droits et redirections.  

#### Crédits


Un historique de Linux  
Un cours sur Linux très complet : Linux France et un autre : Telecom  Paris  
Des précisions sur l’architecture des fichiers sous Linux ici et la  nomenclature des systèmes de fichiers là.  
Un cours du MIT  

##  

#### Présentation
<p align='center'><img src='/images/d5c87ac1434fe878c1a6506bdc52c42c.bmp'/></p>

_Figure – Le système d’exploitation est un intermédiaire entre les logiciels
d’application et le matériel (WIKIPEDIA)._

Un système d’exploitation (en anglais operating system (OS) ) est un  ensemble de programmes qui dirige l’utilisation des ressources d’un  ordinateur par des logiciels applicatifs.  L’OS fournit aux programmes utilisateurs un accès unifié aux  ressources matérielles et logicielles (périphériques, mémoire, autres  programmes). Il oﬀre un ensemble de fonctions  "primitives" permettant d’interagir avec le matériel.  

#### Composants logiciels d’un OS


On trouve notamment :  
l’ordonnanceur : décide quel programme s’exécute à un moment  donné sur le processeur ;  
le gestionnaire de mémoire, qui répartit la mémoire vive entre les  diﬀérents programmes en cours d’exécution ;  
les diﬀérents systèmes de fichiers qui définissent la manière de stocker  les fichiers sur les supports physiques (disque dur, clé USB, disque  optique etc.) ;  
la pile réseau qui implémente des protocoles de communication  comme TCP/IP ;  
les pilotes de périphériques (driver) qui gèrent les périphériques  matériels (souris, clavier, carte 3D etc.)  

#### Standard POSIX


La palette des services oﬀerts et la manière de s’en servir diﬀèrent  d’un système d’exploitation à l’autre.  
Le standard industriel POSIX du IEEE définit une suite d’appels  systèmes standard. POSIX est largement inspiré de UNIX.  
Un logiciel applicatif qui eﬀectue des appels système selon POSIX  pourra être utilisé sur tous les systèmes d’exploitation conformes à ce  standard.  
Tous les systèmes de type LINUX sont compatibles POSIX de même  que Android, macOS, iOS mais pas Windows : alors que les premiers  dérivent de UNIX, Windows dérive de MS-DOS dont la philosophie  est diﬀérente.  

#### Extension des noms de fichiers


Dans les systèmes POSIX, l’extension du nom de ficheir (tous les  caractères à droite du "·" dans le nom du fichier) n’a pas de  signification particulière pour le système.  
Mais elle est pratique pour l’utilisateur car il voit bien à quel type de  fichier il a à faire.  
Un fichier exécutable n’a pas nécéssairement un nom se terminant par  .exe  L’utilisation de l’extension pour déterminer le type de fichier est une  caractéristique de Windows  

##   

#### Historique (PI)


Linux ou GNU/Linux : famille de systèmes d’exploitation open source  de type Unix fondé sur le noyau Linux, créé en 1991 par Linus  Torvalds.  
GNU : projet informatique créé en janvier 1984 par Richard  Stallman pour développer le système d’exploitation GNU. Chaque  brique du projet est un logiciel libre utilisable en tant que tel mais  dont l’objectif est de s’inscrire dans une logique cohérente.  GNU/Linux c’est donc le noyau Linux plus les composantes de GNU.  
Linux équipe une faible part des ordinateurs PC mais beaucoup de  serveurs, téléphones portables, systèmes embarqués ou encore  superordinateurs.  
Android : système d’exploitation pour téléphones portables. Utilise le  noyau Linux mais pas GNU. Equipe 85 % des tablettes tactiles et  smartphones.  

#### Logiciel libre (PI)


Distributions Linux (Ubuntu, Debian, Linux Mint, CentOS ...) :  systèmes d’exploitation libres.  Les 4 libertés d’un logiciels libres telles que définies par la Free  Software Foundation. On peut :  
utiliser le logiciel sans restriction,  étudier le logiciel  le modifier pour l’adapter à ses besoins  le redistribuer sous certaines conditions précises  
le non respect de ces règles peut conduire à des condamnations.  Certaines licences sont conçues selon le principe du copyleft : une  œuvre dérivée d’un logiciel sous copyleft doit à son tour être libre.  Licence GNU GPL : logiciel libre + copyleft. Le noyau Linux utilise  cette licence.  Un logiciel libre n’est pas nécessairement gratuit, et inversement un  logiciel gratuit n’est pas forcément libre.  

#### Caractéristiques (PI)


Linux est multi-tâches : plusieurs processus peuvent s’exécuter  "simultanément".  
Linux est multi-utilisateur : Chaque personne utilisant le système  dispose d’un compte, qui peut être vu comme une certaine zone qui  lui est allouée, accessible par un nom et un mot de passe. Un  mécanisme de droits un peu contraignant empêche un utilisateur  d’accéder à des données dont il n’a pas les droits.  
Les droits peuvent être modifiés.  
Un utilisateur spécial a tous les droits le super-utilisateur ou  administrateur (super user en anglais).  

#### Utilisateur


Chaque utilisateur possède un identifiant de connexion (login). On lui  associe un mot de passe.  
L’ensemble des données de l’utilisateur (identifiant, mot de passe et  autres métadonnées), ainsi que ses fichiers personnels constituent le  compte de l’utilisateur.  
L’ensemble des interractions de l’utilisateur authentifié avec le  système est appelé une session. Quand l’utilisateur se déconnecte, on  dit qu’il ferme sa session.  


A l’identifiant de connexion correspond un numéro d’utilisateur  nommé UID (User IDentifier). L’OS n’utilise que l’UID, l’identifiant  n’est là que par soucis de convivialité.  
Les utilisateurs peuvent être réunis en groupes. Ces derniers ont un  nom symbolique et un identifiant numérique GID (Group IDentifier).  
Chaque utilisateur a un groupe principal et éventuellement des  groupes secondaires. La commande id aﬃche les identifiants  numériques et les groupes de l’utilisateur courant.  $ id  uid =1000( ivan ) gid =1000( ivan ) groupes =1000( ivan ) ,  4( adm ) ,24( cdrom ) ,27( sudo ) ,30( dip ) ,46( plugdev ) ,  111( lxd ) ,116( lpadmin ) ,131( libvirtd ) ,132( sambashare )  

#### Noyau


Noyau de système d’exploitation (ou noyau, ou kernel en anglais) :  partie fondamentale de certains systèmes d’exploitation. Gère les  ressources de l’ordinateur et permet aux diﬀérents composants (  matériels et logiciels) de communiquer entre eux.  Le noyau assure :  
la communication entre les logiciels et le matériel  la gestion des divers logiciels (tâches) d’une machine (lancement des  programmes, ordonnancement...)  la gestion du matériel (mémoire, processeur, périphérique, stockage...).  
Le noyau oﬀre ses fonctions (l’accès aux ressources qu’il gère) au  travers des appels système. Il transmet ou interprète les informations  du matériel via des interruptions (appellées les entrées/sorties).  

#### Processus ♥


Processus : programme en cours d’exécution par un ordinateur.  Régulièrement, il a besoin d’accéder à des ressources protégées (comme  une écriture en mémoire). Le noyau prend alors le relai du processus pour  rendre le service demandé et lui rend le contrôle lorsque les actions voulues  ont été réalisées.  


 Système d’exploitation  
 Présentation de Linux  
 Système de fichiers  Vue logique  Les principaux répertoires systèmes  
 Le shell bash  

###   

#### Système de fichier



**Système de fichier : ensemble de conventions et structures permettant
de stocker des données sur un support physique ♥
Chaque système de fichier implémente (à sa façon) les primitives de
manipulation de fichiers génériques oﬀertes par le système
d’exploitation :**

Création d’un fichier de nom donné ;  Ouverture d’un fichier en lecture ou écriture ;  Lecture ou écriture de portion d’un fichier ;  Suppression d’un fichier ;  Copie ou renommage d’un fichier.  

#### Systèmes de fichier


Chaque système de fichier possède ses propres caractéristiques et  objectifs. Certains (Samba ou NFS) sont par exemple destinés à  exposer à l’utilisateur des fichiers et répertoires se trouvant  physiquement sur une/des machine(s) distante(s).  
Quelques exemples :  
Système de fichier  FAT  
OS  MSDOS  Windows 95/98 FAT32 (File Allocation Table 32bits)  Windows NT  MAC OS  Linux  
NTFS (New Technology File System) standard pour les ordinateurs sous Windows depuis VISTA.  HFS+ (High Performance File System)  ext4 (Extended File System)  

#### Système de fichier


Fichier : suite d’octets constituant un ensemble cohérent (en principe)  d’information. Sous Linux, tout est fichier (même le clavier est  considéré comme un fichier).  
Règles de nommage Linux : 255 caractères au plus, principalement  des lettres et des chiﬀres mais . -  ˜ + % sont aussi autorisés.  Les espaces ne sont pas interdits, mais je les déconseille de même que  les accents.  

#### Répertoires ou dossiers


Si le système de fichier était une commode, les répertoires seraient  des tiroirs contenant des fichiers ou des sous-tiroirs.  

**Un répertoire est un fichier dans lequel se trouve la liste de son
"contenu" sous la forme (nom du fichier, numéro d’inode) ♥
Dans un répertoire on trouve deux fichiers particuliers notés
"." (répertoire courant) et ".." (répertoire parent). ♥
Le système de fichier est représenté par un arbre dont la racine est le
répertoire root noté "/" ♥.**

Pratique personnelle de nommage pour les répertoires et fichiers  persos : la première lettre des noms de répertoires en majuscules, celle  des autres fichiers en minuscule : UnNomDeRepertoire ,  
unNomDeFichier .  

#### Exemple d’arborescence de fichiers
<p align='center'><img src='/images/95313653cfb2f0f0d23c71434dd16bd4.bmp'/></p>

_Figure – Le système de fichiers Linux (d’après malekal)_

        

#### Exemple personnel
<p align='center'><img src='/images/94856b0ae5a3d879c096d273826bb764.bmp'/></p>

_Figure – Une image de mon répertoire home donnée par la commande tree_

L’option -d pour ne lister que les répertoires, l’option -L pour indiquer  la profondeur de récursion.  

###   

#### Principaux répertoires


!!! quote "Définition"
    / C’est la racine de l’arborescence ♥


!!! quote "Définition"
    /bin stocke les exécutables et binaires essentiels lors du


!!! quote "Définition"
    /sbin stocke les exécutables et binaires essentiels lors du démarrage
mais réservées au superuser (administrateur système). ♥


!!! quote "Définition"
    /boot stocke les fichiers de démarrage Linux ♥


!!! quote "Définition"
    /dev (pour devices) les fichiers liés aux périphériques.


!!! quote "Définition"
    /etc Les fichiers de configuration de Linux et des applications. ♥


!!! quote "Définition"
    /home comptes des utilisateurs. ♥


!!! quote "Définition"
    /lib Les librairies et bibliothèques partagés pour le
fonctionnement de l’OS et des applications



**démarrage. Commandes utilisables ensuite par les utilisateurs
(ex : cat et ls ). ♥**


#### Les principaux répertoires


!!! quote "Définition"
    /usr répertoire des applications partagées par diﬀérentes machines
ou utilisateurs. Les programmes dans /usr ne sont pas
nécessaires au démarrage. ♥


!!! quote "Définition"
    /media points de montages des médias amovibles (clés USB...)♥


!!! quote "Définition"
    /proc Répertoire virtuel avec les informations système (l’état du
système, noyau Linux, etc) basé sur procfs (process file
system)


!!! quote "Définition"
    /root dossier personnel de l’utilisateur root


!!! quote "Définition"
    /var Contient des données mises à jour par diﬀérents programmes
durant le fonctionnement du système (fichiers journaux
(log), des fichiers de données (spool) ou des fichiers de
blocage (lock)) ♥


!!! quote "Définition"
    /tmp les fichiers temporaires. ♥

#### le répertoire /usr


!!! quote "Définition"
    /usr/bin Commandes utilisables par tous les utilisateurs, et non


!!! quote "Définition"
    nécessaires lors du démarrage du système.
/usr/sbin Commandes réservées au super-utilisateur , et non
nécessaires lors du démarrage du système.
/usr/lib le dossier des librairies utilisées par les applications
/usr/local applications installées localement par l’admnistrateur


!!! quote "Définition"
    /usr/src Les sources des applications que l’on peut compiler


!!! quote "Définition"
    /usr/share le dossier avec les fichiers qui peuvent être partagés avec


!!! quote "Définition"
    toutes les architectures (i386 -INTEL-, amd64 -AMD-, etc).
/usr/local/bin c’est ici que peuvent être placés les programmes persos à


!!! quote "Définition"
    ...
   


Données des applications des utilisateurs.  
système.  
partager avec d’autres utilisateurs (il faut quand même  demander les droits au superuser).  

#### le répertoire /var


!!! quote "Définition"
    /var Le répertoire /usr/ étant en lecture seule, tous les


!!! quote "Définition"
    /var/lock Fichiers de blocage, pour interdire par exemple deux
utilisations simultanées d’un périphérique.


!!! quote "Définition"
    /var/run Des fichiers liés aux applications en cours de fonctionnement.


!!! quote "Définition"
    /var/log les journaux et logs du système et des applications


!!! quote "Définition"
    /var/cache dossiers et fichiers de cache. Par exemple apt peut y stocker
les packages pour installer ou mettre à jour le système et les
applications.


!!! quote "Définition"
    /var/spool Pour stocker les fichiers de données des programmes.


programmes qui ont besoin d’écrire des fichiers journaux  (log), des fichiers de données (spool) ou des fichiers de  blocage (lock) devraient les écrire dans /var.  
Par exemple, on peut y trouver le PID de l’application.  

#### le répertoire /etc


!!! quote "Définition"
    /etc/init.d et /etc/default : les fichiers liés aux daemons Linux


!!! quote "Définition"
    /etc/password, /etc/group, /etc/shadow : les fichiers de configuration des


!!! quote "Définition"
    /etc/hosts : le fichier HOSTS de Linux (liens entre adresses IP et


!!! quote "Définition"
    /etc/sudoers et /etc/sudoers.d : la configuration de sudo.


!!! quote "Définition"
    /etc/sysctl.conf et /etc/sysctl.d les fichiers de configuration de démarrage


!!! quote "Définition"
    ...


stocke les fichiers de configurations du système ainsi que des applications.  Un sous-répertoire par application  
utilisateurs Linux.  
littérales)  
du noyau Linux.  

#### Des fichiers sensibles


!!! quote "Définition"
    /etc/passwd : fichier des utilisateurs et leurs mots de pass. Suppression de


!!! quote "Définition"
    /etc/fstab Liste des partitions utilisées par le système et selon quelle


!!! quote "Définition"
    /boot/vmlinuz Se situe généralement, soit sous la racine (/), soit sous


!!! quote "Définition"
    ...


Quelques fichiers particulièrement importants, sur lesquels repose une  grande partie de la stabilité du système, voire de son simple  fonctionnement  
ce répertoire =⇒ impossible d’utiliser le système.  
méthode il les utilise.  
/boot. En fait, il s’agit du système lui-même ! Ultra sensible !  

###   

#### Avertissement


On ne donne ici que quelques indications sur le shell bash :  des principes généraux d’écritures des commandes  
quelques commandes forcément incomplètes  
pas d’instruction de programmation pour les scripts bash : on a  assez à faire avec le langage C et OCAML.  

#### Terminal et console


!!! quote "Définition"
    Terminal


!!! quote "Définition"
    shell : interpréteur de commande ; programme lancé juste après la


!!! quote "Définition"
    Console : combinaison d’un terminal et d’un shell.
Ouverture d’un terminal sous Ubuntu : Ctr+Alt+T.


: environnement dans lequel on écrit et qui donne le retour  des commandes.  
Il peut être fourni par les serveur graphique et disposer  de fenêtres, menus, et autres boutons ;  ou sans fenêtre, comme lorsque on fait Ctrl+Alt+F3  (entrer Ctrl+Alt+F7 ou Ctrl+Alt+F2 pour retourner au  serveur graphique).  Peut très bien être constitué d’une imprimante avec un  clavier comme un téléscripteur.  
procédure de login et qui traite les commandes passées. Le  shell le plus répandu sous Linux est le bash (Bourne again  shell).  

#### Un terminal
<p align='center'><img src='/images/b69dbb71fa7e2b8d5ae766196beb3551.bmp'/></p>

_Figure – Un terminal dans lequel on n’a encore rien écrit_

Ouverture avec Ctrl + Alt + T  Avant le prompt $ : nom d’utilisateur @ nom de machine puis  répertoire courant  ˜ désigne le répertoire racine de mon compte personnel.  

#### Syntaxe générale des commandes


!!! quote "Définition"
    nom nom de la commande


!!! quote "Définition"
    options représente une ou plusieurs options


!!! quote "Définition"
    argument1 est le 1er argument


nom [-options] [argument1...]  
Explications :  
les options sont écrite le plus souvent sous la forme d’un caractère  accolé à un tiret ( ls -l ) .  si paramètre demandé, il est séparé par un espace :  gcc essai.c -o sortie (2 paramètres : essai.c et sortie )  les crochets indiquent un élément facultatif  
les points de suspension indiquent la possibilité de répéter un  argument, par exemple ls /etc /usr/bin  séparation par un espace ou une tabulation  

#### Commandes internes vs externes


Une commande externe est un fichier présent dans l’arborescence.  Exemple : Quand un utilisateur exécute la commande ls , le shell  demande au noyau Linux d’exécuter le fichier /bin/ls  
$file / bin / ls  / bin / ls : ELF 64 - bit LSB shared object , x86 -64 ,  version 1 ( SYSV ) , dynamically linked , [...]  
Une commande interne est intégrée au processus shell. Elle n’a  aucune correspondance avec un fichier sur le disque. L’accès à une  commande interne est plus rapide que pour une externe.  La commande type indique si une commande est interne ou externe.  
$ type ls  ls est un alias vers << ls -- color = auto >>  $ type cd  cd est une primitive du shell  
certaines commande ont une version externe et une interne.  

#### la commande man



**Pour connaître le mode d’emploi d’une commande, taper
man nomDeLaCommande (ex : man ls pour connaître le manuel de
ls ). Entrer q pour quitter. ♥
Consulter la documentation Ubuntu
Le manuel est décomposé en plusieurs sections**

 Programmes exécutables ou commandes de l’interpréteur de  
commandes (shell) ;  
 Appels système (Fonctions fournies par le noyau) ;   Appels de bibliothèque (fonctions fournies par des bibliothèques) ;   Fichiers spéciaux (situés généralement dans /dev) ;   Formats des fichiers et conventions (Par exemple /etc/passwd) ;...  


Parfois deux pages de manuel ont le même nom comme printf (en  
section 1 et 3). Entrer man 1 printf ou man 3 printf pour  spécifier.  
Entrer q pour quitter le manuel.  

#### Chemins absolus et relatifs ♥


    
Un élément de l’arborescence est repéré par son nom (par exemple  nom.jpg ) précédé de :  
son chemin absolu depuis la racine (ex :  /home/sue/Pictures/family/nom.jpg )  son chemin relatif depuis le répertoire courant. Par exemple, si je suis  dans pets : ../../family/nom.jpg  
/ : sépare les noms de fichiers  
˜ : répertoire racine du Home  

#### Commentaire


```linenums="1"
# kalin
```

Les commentaires ne sont pas interprétés :  
La commande kalin n’existe pas, elle soulève une erreur :  
$ kalin  La commande << kalin > > n ’ a pas é t é trouv ée , ...  
Le caractère # n’est pas interprété. On peut écrire kalin sans  erreur :  
$ $  

#### Contenu d’un répertoire ♥


La commande ls donne le contenu d’un répertoire :  
sans argument : les entrées du répertoire courant ls  avec argument : les entrées repérées par le (ou les) argument(s) :  ls myFile, ls myRep, ls /etc /usr/bin  
pour aﬃcher les fichiers cachés ls -a (indique notamment  .bashrc si je suis en ˜ )  pour pour tous les attributs (type, droits, liens physiques, propriétaire,  groupe, taille, date, nom) ls -l  ls -al au lieu de ls -a -l .  $   −            −−−−−       −−−−−       
                                  
          
          
    
Dans l’ordre : droits, nombres d’alias, username, groupname, taille  (par défaut en octet), date de dernière modif., nom de fichier.  ls -i aﬃche le numéro d’inode.  

#### Aﬃchage d’une chaîne de caractère


```linenums="1"
# afficher coucou
```

La commande echo aﬃche une ligne de texte  
(Le caractère # indique le début d’un commentaire  
$ echo ’ coucou ’ coucou  
le choix des guillements est important : "’" (touche 4),  "”" (touche 3) et "‘" (ALT GR + 7) n’ont pas le même sens.  
Pour aﬃcher le contenu d’une variable d’environnement :  
$ echo $LANG  fr_FR . UTF -8  

#### Les métacaractères


Les métacaractères du shell permettent :  
de construire des chaînes de caractères génériques  
de modifier l’interprétation d’une commande  

#### Métacaractères de construction


Prioritaires : *, ?  

*** désigne une chaîne de caractères quelconque ♥
? désigne un caractère quelconque ♥**

[...] désigne les caractères entre crochets, définis par énumération  ou par un intervalle :  
[Aa] désigne les caractères A ou a,  
[0-9a-zA-Z] désigne un caractère alphanumérique quelconque.  
[!0-9] désigne l’ensemble des caractères sauf les chiﬀres.  


Voici le contenu du répertoire courant :  
$ ls  alain  
Ali  
tata  
titi  
toto  
tutu  
zut  
ls t[ao]t[ao] retourne tata et toto ,  
ls ??? retourne les noms de 3 lettres donc zut et Ali  ls A* retourne les noms qui commencent par A donc Ali  ls t??o retourne les noms de 4 lettres qui terminent par o et  commencent par t donc toto  
ls [!b-z]* désigne les noms qui ne commencent pas par une lettre  entre b et z donc alain et Ali .  

#### Métacaractères de modification (PI)


; sépare deux commandes sur une même ligne  Les guillemets verticaux simples ’ (touche 4) délimitent une chaîne  de caractères contenant des espaces (à l’intérieur, tous les  métacaractères perdent leur signification) ;  Les guillemets verticaux doubles ” (touche 3) délimitent une chaîne  de caractères contenant des espaces (à l’intérieur, tous les  métacaractères perdent leur signification, à l’exception des  métacaractères ‘ et $) ;  Les guillemets obliques gauche-droite ‘ (ALT GR + 7)  "capturent" la sortie standard pour former un nouvel argument ou  une nouvelle commande ;  \ annule la signification du métacaractère qui suit : c’est un  caractère dit d’échappement.  le & à la fin d’une commande permet de lancer celle-ci en tâche de  fond, donc sans bloquer le terminal.  


```linenums="1"
# est - ce un dossier ?
# afficher la r é ponse du test pr é c é dent
```

Métacaractères de modification (PI)  Parenthèses  
(...) : les parenthèses encadrent une suite de commandes qui sont  exécutées par un shell secondaire. En particulier, les assignements  n’ont pas d’eﬀet en dehors des parenthèses.  
{...} : les accolades encadrent une suite de commandes qui sont  exécutées par le shell principal. En particulier, les assignements ont un  eﬀet en dehors des accolades.  [...] : les crochets sont utilisés pour les instructions  conditionnelles. Ils encadrent une expression à valeur bouléenne.  $ [ -d presentationLinux . tex ] $ echo $ ? 1  
On obtient 0 si le fichier existe et est un répertoire, 1 sinon.  
...  


Utiliser le résultat d’une commande comme argument  d’une autre (PI)  
Pour info. Les ‘ (ALT GR + 7) entourant une commande permettent  d’utiliser le résultat de cette commande comme argument(s) dans la ligne  de commande.  
$ echo " Nous (cid:32) sommes (cid:32) le " ‘ date +% d /% m /% y ‘  Nous sommes le 27/08/21  
Aﬃche la date du jour avec un format choisi.  
$ echo " 2 (cid:32) + (cid:32) 2 (cid:32) = " ‘ expr 2 + 2 ‘  2 + 2 = 4  

#### Positionnement/recherche dans l’arborescence ♥


!!! quote "Définition"
    pwd aﬃche le nom absolu du répertoire de travail


!!! quote "Définition"
    cd change le répertoire de travail.


!!! quote "Définition"
    ls liste les entrées d’un répertoire (déjà vu) ls .


!!! quote "Définition"
    find pour chercher récursivement un ou plusieurs fichiers dans une
arborescence. Beaucoup d’options (consulter le manuel).


Avec argument : se rend à la destination indiquée.  cd ../Rep1 ;  sans argument : retourne au répertoire de connexion du  user. cd  

#### Rechercher



**find cherche récursivement dans l’arborescence à partir du point
indiqué. find /usr -name "ls*" cherche les fichiers dont le nom
commence par ls dans le répertoire /usr et ses sous-répertoires. Il
y en a beaucoup ! ♥
L’option -type permet de ne chercher que les fichiers ( f ) ou les**

find /var/log/ -type d -name "*sm*" :  
répertoires ( d ).  chercher les répertoires dont le nom contient sm  recherche par taille :  find  fichiers dont la taille est comprise entre 20 Mo et 40Mo.  Recherche par utilisateur : find /tmp -user adrien cherche dans  
/Téléchargements -size +20M -size -40M cherche les  
/tmp les fichiers dont le propriétaire est adrien .  Beaucoup d’autres options : par date de création, date de dernière  modification, par type de permissions, recherche de fichiers vides etc...  

#### Consulter le contenu d’un fichier texte ♥


Commandes de base :  
cat monfichier , more monfichier : aﬃchage simple et page  par page ;  
head monfichier , head -n monfichier : aﬃchage des n  premières lignes ;  
tail monfichier , tail -n monfichier : aﬃchage des n  dernières lignes ;  
wc monfichier : aﬃchage du nombre de lignes, de mots, de  caractères. Options -l , -w et -c pour les nombres de lignes, de  mots et de caractères.  
cat toto titi : aﬃche le contenu de titi à la suite de celui de  toto ( cat pour concatène).  

#### Historique ♥


!!! quote "Définition"
    ! ! rappelle la dernière commande
!n rappelle la commande numéro n


!!! quote "Définition"
    !chaine rappelle la dernière commande commençant par chaine


Le shell bash enregistre toutes les commandes tapées et permet de  les rappeler pour les ré-exécuter soit telles quelles, soit modifiées.  
La commande history permet de lister le contenu de l’historique  
des commandes, de façon numérotée. Le caractère ! permet de  rappeler une commande.  
On peut aussi utiliser les ﬂèches haut et bas pour naviguer dans  l’historique des commandes.  

#### Taille du contenu d’un répertoire


du -h -d 1 monRepertoire : taille des fichiers et sous-répertoires.  (du pour Disk User))  
L’option -h force un aﬃchage "human readable" (par exemple, 1k,  236M, 2G).  L’option -d aﬃche la taille totale du répertoire exploré et pas  seulement la taille de ses constituants. Et le paramètre 1 indique la  profondeur de l’exploration (ici, on s’arrête aux fils, avec 2 comme  paramètre , ce serait aux petits-fils)  
La commande ncdu nomDuRepertoire , plus conviviale, permet de  connaître la place prise par les fichiers et dossiers en navigant dans  l’arborescence. Par exemple ncdu /home indique les tailles des  diﬀérents répertoires utilisateurs.  

#### Créer et supprimer


Créer un répertoire vide : mkdir Rep1  
Supprimer un répetoire vide : rmdir Rep1  
Créer un fichier vide : touch file1  Supprimer un fichier rm file1 , supprimer tous les fichiers du  répertoire rm *  
Créer un chemin complet mkdir -p R1/R2 crée dans la foulée R1  
puis son sous-répertoire R2 .  

#### Copier ( cp ) et déplacer ( mv ) ♥


Situation : un répertoire parent P possède deux sous-répertoires Rep1 et  Rep2. Dans Rep1 on trouve le fichier file1.  
Copier le fichier file1 du répertoire Rep1 dans le répertoire Rep2  sans changer le nom : cp Rep1/file1 Rep2/  Copier le fichier file1 du répertoire Rep1 dans le répertoire Rep2 en  changeant le nom : cp Rep1/file1 Rep2/file2  Je suis dans Rep1. Changer le nom du fichier file1 en restant dans  le même répertoire : mv file1 file2 .  Je suis dans Rep1. déplacer le fichier file1 sans changer son nom  mv file1 ../Rep2/ .  Je suis dans le répertoire parent de P. Je veux copier récursivement P  et tout ce qu’il contient (donc aussi les sous-répertoires) dans un  nouveau répertoire P2 : cp -r P1 P2  


```linenums="1"
# Creer r é pertoire Asup
# aller au r é pertoire Asup
# pas de contenu
# cr é er le fichier vide asup
# afficher fichiers cach é s
# supprimer asup
# plus de contenu
```

Créer, remplir, vider, supprimer un répertoire  Exemple  
$ mkdir Asup 
$ cd Asup $ ls 
$ touch asup $ ls - al total 8  drwxrwxr - x 2 ivan ivan 4096 ao ^u t  drwxrwxr - x 3 ivan ivan 4096 ao ^u t  0 ao ^u t  -rw - rw -r - - 1 ivan ivan  
19 15:28 .  19 15:28 ..  19 15:28 asup  
$ rm asup $ ls 


```linenums="1"
# revenir au p è re
# supprime rep Asup
# creer un r é pertoire et son fils
# supprimer un r é pertoire et son fils
```

Créer, remplir, vider, supprimer un répertoire  Exemple  
$ cd .. $ rmdir Asup 
Les répertoires Nouveau et son fils AutreNouveau sont créés  simultannément puis supprimés de même  
$ mkdir -p Nouveau / AutreNouveau $ rmdir -p Nouveau / AutreNouveau 

#### Explorer un fichier


grep aﬃche les lignes vérifiant un pattern.  Contenu d’un fichier myfile  
t o t o  t o t  t o t o o  a t o t o  t i t i  
grep "toto" myfile cherche le mot "toto" dans myfile  
grep -c "toto" myfile cherche le nombre d’occurences du mot  "toto" dans myfile.  Option grep -n "toto" pour aﬃcher les numéros de lignes.  Le joker * n’a pas ici la même signification que le métacaractère du  shell ! grep "toto*" myfile cherche les lignes contenant au moins  un tot suivi par 0, 1 ou plusieurs lettres "o" (renvoie toto, tot,  totoo et atoto)       

#### Explorer un fichier (suite)


grep aﬃche les lignes vérifiant un pattern.  Contenu d’un fichier myfile  
t o t o  t o t  t o t o o  a t o t o  t i t i  
grep "toto$" myfile : les lignes qui terminent par toto  
grep "^toto" : les lignes qui commencent par toto  
grep -v "toto" myfile :les lignes ne contenant pas toto  

#### Montage et démontage


L’arborescence de fichier de Linux inclut tous les périphériques  externes.  
Le système d’exploitation d’un ordinateur basique est installé sur le  disque dur. L’utilisateur peut décider d’insérer un périphérique externe  (ex : une clé USB).  
Les périphériques de stockage sont associés à un répertoire particulier.  Par exemple le disque dur principal sur lequel l’OS est installé est  associé au répertoire /  L’utilisateur peut choisir de monter temporairement un système de  fichier par ses propres moyens grâce à la commande mount . En  général le montage se fait dans le répertoire /mnt.  Les opérations de montage manuel nécessitent généralement les droits  du superuser.  
<p align='center'><img src='/images/2d315bde3c6e3e42e352fb96383fc0c6.bmp'/></p>

_Figure – Un bouton pour "éjecter" la clé_

Les médias amovibles (comme une clé USB) sont, sous Ubuntu,  montés automatiquement (i.e. appel automatique de mount ) dans  le répertoire /media.  Par exemple si l’utilisateur toto insère une clé USB, son arborescence  de fichier est visible dans un répertoire comme /media/toto/CLE.  Tous les fichiers de la clés sont disponibles dans ce répertoire.  Et les créations/modifications/suppressions de fichier dans ce  répertoire sont répercutées sur la clé.  Pour démonter un média amovible, il y a en général une interface  graphique.  


Pour démonter manuellement un système de fichier, on utilise la  commande umount .  Cette commande requiert en général les droits du superuser.  


```linenums="1"
|
```

Tube  L’opérateur 
On peut rediriger la sortie d’une commande vers l’entrée d’une autre.  
L’opérateur "|" réalise cette opération.  Récupérer la liste des sous-répertoires :  
on utilise ls -al qui donne toutes les indications sur les fichiers du  répertoire courant.  Les lignes qui commencent par un d correspondent à un répertoire.  Pour les récupérer, on envoie la sortie de ls sur l’entrée de grep ,  auquel on demande de filtrer les lignes qui commencent par d :  ivan @ fixe :~/.../ TD$ ls - al | grep " ^ d "  drwxrwxr - x 28 ivan ivan  drwxr - xr - x 17 ivan ivan  3 ivan ivan  drwxrwxr - x  3 ivan ivan  drwxrwxr - x  drwxrwxr - x  2 ivan ivan  ....  
4096 oct .  4096 d é c .  4096 sept . 21 10:45 ABR_tas  4096 sept . 21 10:45 Arbres  4096 sept . 21 10:45 BDD  
5 10:48 .  30 18:06 ..  
