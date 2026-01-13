
fichier de contrôle : ils contiennent les données  relatives a la nase de données . ces fichiers sont essentielles   pour la base de données . sans les fichiers contrôle   vous ne pouvez pas ouvrir les fichiers de données

fichier données :  ils contiennent  les données utilisateur ou les données des applications, ainsi que les métadonnées  et le dictionnaire de données  

fichiers de journalisation en ligne (online redo log) : ils permettent   la récupération   d'une instance de la base de données 
on récupère l'instance avec ces fichiers lors d'une défaillance du serveur de base de données  

les fichiers si dessous sont essentielles au bon fonctionnement de la base de données  

Fichier de paramètres   : il est utilisé  pour definir la facon dont l'instance est configuré  
au démarrage  

Fichier de mots de passe  : il permet aux utilisateur  sysdba , sysoper et sysasm de se connecter a distance a la base de données  d'effectuer des tache d'administration 

fichier de sauvegarde  : ils est utilisé pour la récupération  de la base de données  
en règles générale   

**fichier de journalisation archivés** :  ils contiennent  des modifications  de données générées par l'instance  

**fichier trace** ! chaque processus serveur  et processus d'arrière plan peut écrire dans un fichier trace associé 
lorsque  un processus détecte une erreur interne , il réalise dans sont fichier trace un dump des informations relatives  a cette erreur 

==dump : c'est un package de fichiers que le système  créer==   

**fichier d'alertes** :  il contient des entrées  de trace spéciales . Le fichier d'alertes d'une base de données est un journal chronologique  

structure  de base de données  logiques et physiques  

[[structure  de  base données logiques et physique.png]] 

- TableSpace  : une base de données  est divisé  en unité  de stockage logique qu'on appel  tablespaces lesquelles regroupent des structure  logiques liées . 
-  un schema c'est l'assemble des objets qui appartiennent  a un utilisateur  

lvm : logical volume manager 


