

### Gestion des rôles 

il existe des roles serveur par défaut  mais nous pouvons nous même crée  des rôle serveurs  

```
CREATE SERVER ROLE <nomRoleServer> ;
```


## Accorder les rôles  

Pour ajouter ou supprimer une connexion à un rôle de serveur, il faut utiliser l’instruction ALTER SERVER ROLE 


## roles au niveau de base de données 

db_owner : p


connaitre la base de données  par défaut  de chaque connexion dans sql server 

```
use master;
GO
	SELECT name as 'Nom' ,loginName as 'Connexion', dbname as 'base par defaut'
	DATABASEPROPERTYEX (dbname, 'Status') as 'Etat de la base' FROM sys.syslogins
```
