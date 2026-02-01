
ALTER TABLE MESEMPLOYEES READ WRITE;


En tant que DBA, afficher le propriétaire, le nom de la table et leur statut :

-- Description de la vue
DESC DBA_TABLES;

-- Affichage du propriétaire, du nom de la table et du statut

SELECT OWNER, TABLE_NAME, STATUS
FROM DBA_TABLES
WHERE TABLE_NAME = 'MESEMPLOYEES';

Afficher la liste de tous les tablespaces :
5.a. Interroger la vue DBA_TABLESPACES.
5.b. Description de la vue et commande pour afficher les noms des tablespaces, la taille maximale et s'ils sont en ligne :

-- Description de la vue
DESC DBA_TABLESPACES;

-- Affichage des noms des tablespaces, taille maximale et statut
SELECT TABLESPACE_NAME, MAX_SIZE, STATUS
FROM DBA_TABLESPACES;
