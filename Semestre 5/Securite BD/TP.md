
	1. Créer un utilisateur superadmin qui  a tous les droits  administratif  
	2. Superadmin pour donner  les droits de lecture des données  dans la table matières  a l'etudiant 
	3. superadmin donne les droits d'écriture a l'etudiant  sur la table etudiant 

pour Mysql server voici les commandes pour  faire le TP : 

1. CREATE USER 'etudiant' IDENTIFIED BY 'etudiant' 

	CREATE LOGIN etudiant WITH PASSWORD= 'enseignant';
	use Securite_BD;
	CREATE USER etudiant for LOGIN etudiant;
	GRANT SELECT ON  dbo.matiere TO etudiant;
	
2. CREATE USER 'enseignant ' IDENTIFIED BY 'ensRoot'
3. GRANT SELECT,INSERT,UPDATE,DELETE  TO 'enseignant'

		GRANT INSERT on dbo.etudiant To etudiant;
	GRANT SELECT,INSERT,UPDATE,DELETE on dbo.etudiant To etudiant; 
	
GRANT ALL PRIVILÈGES ON  gestion_utilisateur.* TO 'super_admin'@'localhost' WITH GRANT OPTION;



Create Table Client ( 
	idCLi int primary key,
	nom VARCHAR(255), 
	ville VARCHAR(255));

CREATE TABLE Produit( 
idPro  int primary key ,
	nom VARCHAR (255), 
	prix float,
	qstock int );

CREATE TABLE Commande( 
	idCli int, 
	idPro int ,
	date DATE ,
	qte int ,

	FOREIGN KEY (idCli) REFERENCES Client,
	FOREIGN KEY (idPro) REFERENCES Produit
	
	);

CREATE  LOGIN gestionnaire WITH PASSWORD= 'samuel';
CREATE LOGIN superviseur WITH PASSWORD=  'komi';
CREATE LOGIN invite WITH PASSWORD = 'issifou'; 

USE Gestion_consommables;  

CREATE USER lawson_samuel FOR LOGIN gestionnaire; 
CREATE USER tchadjobo_komi FOR LOGIN superviseur;
CREATE USER degbe_issifou FOR LOGIN invite;

GRANT SELECT,UPDATE ON dbo.Client  TO lawson_samuel;
GRANT SELECT ON dbo.Commande  TO lawson_samuel;
GRANT SELECT,UPDATE ON dbo.Produit  TO lawson_samuel;

GRANT SELECT ON dbo.Client TO tchadjobo_komi WITH GRANT OPTION;
GRANT SELECT ON dbo.Commande TO tchadjobo_komi WITH GRANT OPTION;
GRANT SELECT ON dbo.Produit TO tchadjobo_komi WITH GRANT OPTION;


