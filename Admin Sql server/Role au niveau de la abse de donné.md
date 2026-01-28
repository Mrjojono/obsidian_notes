

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


## creation d'une connexion a sql server  

- Mode de sécurité  Windows  
```
CREATE LOGIN <nomde conncexion> FROM WINDOWS WIH DEFAULT_DATABASES= ..
```

- Mode de sécurité  Mixte 
```
CREATE LOGIN Joan WITH PASSWORD= joankekeli MUST_CHANGE, CHECK_EXPIRATION = on , CHECK_POLICY = on , DEFAULT_DATABASE = bdVille
```

Le MUST_CHANGE ne functionne que si check_expiration et check_policy sont en mode on 


## Activer, désactiver une connexion 

```
ALTER LOGIN {ENABLE | DISABLE} ;
```


## Modifier la base de données par défaut, le mot de passe 

```
ALTER LOGIN WITH DEFAULT_DATABASE = [;] 
ALTER LOGIN WITH PASSWORD= ‘mot_de_passe’[;]
```

## Activation du compte guest

```
GRANT CONNECT TO guest
```

## Désactivation du compte guest 
```
REVOKE CONNECT FROM guest
```

## Création, modification et suppression des utilsateurs 

```
CREATE USER 'nomutilisateur' FOR LOGIN 'nomconnexion' [WITH DEFAULT_SCHEMA = ]
```

	*modification  d'un utilisateur *
```
ALTER USER < nomUtilisateur > WITH NAME =< nouveauNomUtilisateur >, DEFAULT_SCHEMA 
```


## schema  
**ensemble  logique d'objet a l'intérieur  d'une base de données 


