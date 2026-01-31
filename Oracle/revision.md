
Devoir 2023
Exercice 1:
1. Les 2 principaux composants d'un systeme Oracle Database sont :
	- Instance
	- Bases de données associées 
2. Une instance se compose de structure mémoire et de processus en arriere plan
3. Une session est une connexion entre le processus utilisateur et le processus serveur

4. Composants de la mémoire SGA
	- Cache de tampon de la base de données (buffer cache)
	- Tampon de journalisation (redo log buffer)
	- Zone de mémoire partagée : Cache "library" et le cache de dictionnaire de données
	- Zone de mémoire LARGE POOL
	- Zone de mémoire JAVA
	- Zone de mémoire Streams
5. Six processus en arriere plan
	- DBWn Database Writer
	- LGWR Log Writer
	- CKPT Processus de point de reprise
	- SMON System monitor
	- PMON Process monitor
	- RECO Recoverer
	- ARCn Processus d'archivage
	- QMNn Processus de surveillance de file d'attente
	Pour voir les processus en arriere plan : desc V$BGPROCESS
6. Principaux fichiers associés a une base de données oracle:
	- Fichiers de données
	- Fichiers de controles
	- Fichiers de journalisation
Autres fichiers : fichier de parametres, de mots de passe, de sauvegarde, de journalisation archivés, traces, alerte

7. Structure de stockage logiques d'une base de données
	- "Base de données"
	- Tablespace   Schéma
	- Segment
	- Extent
	- Bloc de données Oracle
8. SHUTDOWN IMMEDIATE

Exercice 2:

Probleme :
1.	Un schéma : l’ensemble des objets qui appartiennent à un user spécifique ; chaque utilisateur a son schéma oracle
Ordre de création : 
	TYPE_OPERATION
	TYPE_COMPTE
	CLIENT
	COMPTE
	OPERATION
2.	La commande : 
Conn oracle/trusty as sysdba;
Create user c##AZIGLOSSOU identified by Judith;
3.	Conn c##AZIGLOSSOU/Judith;
	a. L'utilisateur n'a initialement aucun privilège.
	b. L'utilisateur aura besoin au minimum des privilèges CREATE SESSION pour se connecter à la base de données et CREATE TABLE pour créer des tables.
4.	Pour accorder les privilèges nécessaires à l'utilisateur Gaïa, vous émettriez les commandes suivantes : 
GRANT CREATE SESSION, CREATE TABLE TO c##AZIGLOSSOU;
5.	Les commandes SQL
6.	Dans SQLPLUS @fichier.sql
	Dans SQLDEVELOPER Ctrl C + Ctrl V
7.	Procédure d'impression : 
	Fichier > Data Modeler > Importer > Dictionnaire de données
	Boite de dialogue Assistant Import de dictionnaire de données
		Selectionner la connexion
		Selectionner le schéma
		Selectionner les objets a importer (les tables)
		Valider
	Clic droit sur le nouvel onglet apparu: Imprimer un diagramme > Vers un fichier PDF 
	Enregistrer 


Cours oracle Page 29,230,302, 309

Chap 1,7,8

ALTER USER HR IDENTIFIED BY HR 
ACCOUNT UNLOCK

Révision

Chap 7 : Administrer la sécurité utilisateur
 - Créer des users
 - Authentifier les users
 - Affecter des tablespaces
 - Accorder/revoquer des privileges
 - Créer et gérer les roles/profils

Définitions : 
	Un compte user : moyen d'organiser l'appartenance des objets de la base de données et l'acces a ces objets
	Un mot de passe : 
	Privilege : droit d'executer un type particulier d'instruction SQL ou d'acceder a l'objet d'un autre user
	Role : groupe de privileges qui sont accordés a des users ou a d'autres roles
	Profil : ensembles de limites concernant l'utilisation de la base de données et les ressources des instances
	Quota : une allocation d'espace dans un tablespace donné, uun des moyens de controler l'utilisation des ressources par les users


Pas d'entrée/sortie disponible avec PL/SQL
Donc il faut utiliser un package pour afficher les valeurs des variables

