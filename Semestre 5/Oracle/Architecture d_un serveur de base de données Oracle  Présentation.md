
recherche a faire : 
- architecture 3 tier  explication 
- architecture client- serveur  

	-une instance oracle est composé  de structure mémoire (SGA : Sytem global Area)  et de de processus  d'arrière  plan

	-les données  se trouve dans la structure de stockage  

	-il y a des processus  au niveau du serveur  


[[Architecture  d'un serveur de base de données Oracle.png]] 

les clients sont des des software  qu'on installe  pour accéder  a la base de données  

| seveur                                    | client                   |
| ----------------------------------------- | ------------------------ |
| une instance et une structure  de memoire | software  avec processus |

on dispose de 3 types de [[Structure de base de données]] 

Un cluster est un ensemble de plusieurs instances réparties sur plusieurs nœuds afin d’assurer la haute disponibilité.

Chaque instance est connecté  a une base de données (structure de données)

-  **Connexion** : Communication  entre un processus utilisateur et une instance 
		c'est une voie de communication  entre un processus utilisateur et une instance oracle Database  

-  **Session**: Connexion spécifique  d'un utilisateur a une instance via un processus utilisateur  
	  il représente l'activités  d'un utilisateur spécifique  connecté à l'instance de la base de données 



SGA:  système  global area 
PGA: program Global area 


Structure  memoire de la base de données oracle 


la modification d'une base de données  signifie injection d'instrucution LMD ou LDD 


Tampon de journalisation:  (Redo log buffer)
- memoire tampon reutilisable  situe dans la memoire SGA 
- le tampon de journalisation contient des informations sur les modifications apportées  a la base de données 
- 

le tampon de journnalisation est une memoire tampon (buffer) reutilisable située dans la memoire SGA qui contient des informations sur les modifications effectuées dans la base de données 

Zone de mémoire partagée :


la zone de mémoire partagée  est une portion de la mémoire SGA qui contient le cache "library", le cache du dictionnaire de données, le cache des resustats d'interrogations SQL, le cahce des resultats de fonctions PL/SQL , des memoires tampons( buffers) pour les messages d'execution en parallèle   et des structures  de contrôle  


le processus serveur est créer au lancement de l'instance  

le code associé aux utilisateurs  connectés peut être  configuré  en tant que  serveur dédié  ou partagé: 
- serveur dédié : 
	Pour chaque utilisateur, l'application de base de données est exécuter par un processus utilisateur 
- serveur partagé 
	 Chaque connexion ne nécessite pas un processus serveur dédié. Un processus répartiteur dirige plusieurs demandes de session réseau entrantes vers un pool de processus serveur partagé 
les processus serveur  sont créer  pour le compte de chaue applications  utilisateurs pour effecturer des taches spécifiques: 

Les processus D'arriere plan : 

DBWR:  Database Writer 
-  il ecris sur le disque les tampon qui ont été modifier ("dirty")  ( de manière asynchrone , de manière periodique ) il copie les insertions chaque 3s  depuis le database buffer cache ou au niveau de la sga 
[[LGWR]](Log writer):
[[CKPT( checkpoint)]](processus de point de reprise)
[[SMON( System Monitor)]](System Monitor)
[[PMON ( Process Monitor )]](Process Monitor)
[[Processus RECO ( Recoverer)]](Recoverer)
[[ARcn ( Archiver)]](Processus d'archivage)

les autres processus: 
MMON : Tache  en arrière plan   visant a simplifier la  gestion du système  
MMNL :  Tache fréquentes  et légères visant a simplifier  la gestion  du système 
MMAN : Tâches  de gestion automatique  de la mémoire 
CJQ0 :  Tâche   utilisateur pour les traitements par lots 
QMNx : surveillance  des files d'attente de messages Oracle Stream Advanced Queuing  

les vues qui sont dans le dictionnaire sont appelés  des vues dynamiques 

==les fichiers de données on pour extension  .dbf==  
==les fichiers contrôle on pour extension   .ctr==  

Question sur 8 processus 
Qu'es ce qu'une instance ? 
Différences entre connexion et session 
Quelle rôle jouer par PMON 
C'est quoi le lo.. balancing 

lister les fichiers de la base de données   
 quel role joue le fichier de contrôle 
quels sont les quatres autres fichiers de base de données
question sur ceci [[structure  de  base données logiques et physique.png]]  

énumérer   les principaux composants de l'architecture  de la base de données oracle 
décrire  















