
[[PL_SQL]]  quelques  rappel sur le pl/sql 

---

## 1. Introduction à Oracle
[]
### Points clés

- **Oracle Corporation** : Premier SGBDR au monde (1979)
- **Leader mondial** des systèmes de gestion de bases de données
- **SGBD réparti** orienté Web avec architecture client-serveur

### Éditions Oracle

|Édition|Caractéristiques|Usage|
|---|---|---|
|**Enterprise**|Ensemble complet des fonctionnalités|Grandes entreprises, volumes de données importants|
|**Standard**|Fonctionnalités de base|Maximum 4 processeurs|
|**Standard One**|Identique à Standard|Maximum 2 processeurs|
|**Personal**|Windows uniquement|Développeurs en mode mono-utilisateur|
|**Express (XE)**|Édition gratuite|Petites entreprises, usage académique|
|**Lite**|Applications mobiles|Déploiement sur terminaux mobiles|

### QCM 1

**Q1: Quelle est l'année de lancement de la première version commerciale d'Oracle ?**

- a) 1975
- b) 1977 (prototype)
- c) 1979 ✓
- d) 1980

**Q2: Quelle édition Oracle est gratuite ?**

- a) Personal Edition
- b) Express Edition ✓
- c) Lite Edition
- d) Standard Edition

---

## 2. Installation et produits Oracle

### Configuration système requise

```
RAM minimale     : 512 Mo
SWAP             : 1 Go (ou 2x la RAM)
Espace /tmp      : 400 Mo
Logiciel Oracle  : 1,5 Go
Base de données  : 1,5 Go
```

### Variables d'environnement essentielles

|Variable|Description|Exemple|
|---|---|---|
|`ORACLE_HOME`|Répertoire d'installation du noyau Oracle|`C:\oracle\product\10.2.0\db_1`|
|`ORACLE_BASE`|Répertoire racine des bases Oracle|`C:\oracle\product`|
|`ORACLE_SID`|Identifiant de l'instance active|`ORCL`|
|`NLS_LANG`|Configuration linguistique du système|`FRENCH_FRANCE.WE8MSWIN1252`|

### Produits Oracle principaux

- **Oracle Server** : Moteur de gestion de la base de données
- **SQL & PL/SQL** : Langages de requêtes et de programmation procédurale
- **Oracle Designer** : Outil de conception d'applications
- **Oracle Developer** : Suite de développement client/serveur
- **Oracle Discoverer** : Outil d'interrogation et de datawarehouse

### QCM 2

**Q1: Quelle variable d'environnement désigne l'instance Oracle active ?**

- a) ORACLE_HOME
- b) ORACLE_BASE
- c) ORACLE_SID ✓
- d) NLS_LANG

**Q2: Quelle est la RAM minimale requise pour installer Oracle ?**

- a) 256 Mo
- b) 512 Mo ✓
- c) 1 Go
- d) 2 Go

---

## 3. Gestion des instances Oracle

### Structure mémoire SGA (System Global Area)

La SGA est la zone mémoire partagée principale d'Oracle, composée de :

```
SGA
├── Database Buffer Cache (DB_CACHE_SIZE)
│   └── Cache des blocs de données
├── Redo Log Buffer (LOG_BUFFER)
│   └── Buffer circulaire pour journalisation
├── Shared Pool (SHARED_POOL_SIZE)
│   ├── Library Cache (requêtes SQL, plans d'exécution)
│   └── Dictionary Cache (métadonnées)
├── Large Pool (LARGE_POOL_SIZE)
│   └── Opérations volumineuses (RMAN, I/O parallèle)
├── Java Pool (JAVA_POOL_SIZE)
│   └── Code Java et environnement JVM
└── Streams Pool (STREAMS_POOL_SIZE)
    └── Réplication et streaming de données
```

### Processus d'arrière-plan essentiels

|Processus|Rôle|Criticité|
|---|---|---|
|**PMON**|Process Monitor - Nettoie les connexions anormalement terminées|Obligatoire ✓|
|**SMON**|System Monitor - Récupération d'instance au démarrage|Obligatoire ✓|
|**LGWR**|Log Writer - Écrit le contenu du Redo Log Buffer vers les fichiers Redo Log|Obligatoire ✓|
|**DBWR**|Database Writer - Écrit les blocs modifiés vers les fichiers de données|Obligatoire ✓|
|**CKPT**|Checkpoint - Synchronise les en-têtes de fichiers avec l'état actuel|Obligatoire ✓|
|**ARCn**|Archiver - Archive les Redo Logs pleins en mode ARCHIVELOG|Optionnel|

### Connexion à Oracle

#### Connexion standard

```sql
SQLPLUS /nolog
SQL> CONNECT nom_utilisateur/mot_de_passe@nom_service
```

#### Connexion avec privilèges administratifs

```sql
SET ORACLE_SID=Orcl
sqlplus /nolog
SQL> CONNECT / AS SYSDBA
```

### Cycle de vie de l'instance

```
SHUTDOWN → NOMOUNT → MOUNT → OPEN
```

**Détail des étapes :**

1. **NOMOUNT** : Instance démarrée, SGA allouée, processus lancés, fichier de paramètres lu
2. **MOUNT** : Fichiers de contrôle lus, structure physique identifiée
3. **OPEN** : Fichiers de données et Redo Logs ouverts, base accessible aux utilisateurs

#### Commandes de démarrage

```sql
STARTUP NOMOUNT    -- Démarre l'instance uniquement
STARTUP MOUNT      -- Démarre et monte la base
STARTUP OPEN       -- Démarre, monte et ouvre (équivalent à STARTUP)
STARTUP            -- Mode par défaut (OPEN)
```

### Modes d'arrêt

|Mode|Description|Rollback|Checkpoint|Attente|
|---|---|---|---|---|
|**NORMAL**|Attend la fin de toutes les sessions|Oui|Oui|Sessions actives|
|**TRANSACTIONAL**|Attend la fin des transactions en cours|Oui|Oui|Transactions|
|**IMMEDIATE**|Force la déconnexion immédiate|Oui|Oui|Aucune|
|**ABORT**|Arrêt brutal sans nettoyage|Non|Non|Aucune|

```sql
SHUTDOWN IMMEDIATE    -- Recommandé pour un arrêt propre et rapide
```

**Note :** ABORT nécessite une récupération d'instance au prochain démarrage.

### Vues système importantes

```sql
-- Vues statiques (requièrent base OPEN)
USER_*     -- Objets appartenant à l'utilisateur connecté
ALL_*      -- Objets accessibles par l'utilisateur
DBA_*      -- Tous les objets (privilèges administrateur requis)

-- Vues dynamiques (accessibles même en MOUNT)
V$INSTANCE    -- Informations sur l'instance
V$DATABASE    -- Informations sur la base de données
V$SGA         -- Composants de la SGA
V$PARAMETER   -- Paramètres d'initialisation
```

### QCM 3

**Q1: Quel processus écrit dans les fichiers Redo Log ?**

- a) DBWR
- b) LGWR ✓
- c) PMON
- d) SMON

**Q2: Quel mode d'arrêt est recommandé pour un arrêt propre et rapide ?**

- a) ABORT
- b) IMMEDIATE ✓
- c) NORMAL
- d) TRANSACTIONAL

**Q3: Quelle vue affiche les paramètres d'initialisation de l'instance ?**

- a) V$INSTANCE
- b) V$DATABASE
- c) V$PARAMETER ✓
- d) V$SGA

---

## 4. Architecture de la base de données

### Architecture physique - Fichiers composant la base

#### 1. Fichiers de contrôle (Control Files)

- Nombre minimum recommandé : **3 fichiers multiplexés**
- Contiennent la cartographie complète de la structure physique
- Lus au démarrage, mis à jour automatiquement lors de modifications structurelles
- Essentiels pour le démarrage et l'intégrité de la base

```sql
-- Sauvegarder la structure du fichier de contrôle
ALTER DATABASE BACKUP CONTROLFILE TO TRACE;
```

#### 2. Fichiers Redo Log

- Enregistrent toutes les modifications de données (mécanisme de journalisation)
- Fonctionnent en mode circulaire
- Minimum **2 groupes** requis
- **Multiplexage fortement recommandé** : 2 membres ou plus par groupe sur disques différents

```sql
-- Ajouter un groupe de Redo Log
ALTER DATABASE ADD LOGFILE GROUP 3
  ('disk5/log31.rdo', 'disk6/log32.rdo') SIZE 10M;

-- Ajouter un membre à un groupe existant
ALTER DATABASE ADD LOGFILE MEMBER
  'disk6/log12.rdo' TO GROUP 1;

-- Forcer un basculement de log
ALTER SYSTEM SWITCH LOGFILE;

-- Supprimer un groupe (doit être inactif)
ALTER DATABASE DROP LOGFILE GROUP 3;
```

#### 3. Fichiers de données (Datafiles)

- Contiennent les données réelles (tables, index, etc.)
- Organisés en tablespaces
- Peuvent être redimensionnés dynamiquement
- Supportent l'auto-extension

#### 4. Autres fichiers critiques

|Fichier|Description|Utilisation|
|---|---|---|
|**init.ora / spfile.ora**|Paramètres d'initialisation de l'instance|Lu au démarrage|
|**Alert.log**|Journal chronologique d'événements|Diagnostic, erreurs, opérations admin|
|**Fichiers traces**|Traces des processus et sessions|Débogage, analyse de problèmes|
|**Fichier mot de passe**|Authentification privilégiée|Connexion SYSDBA/SYSOPER|
|**Archives Redo Log**|Copies des Redo Logs remplis|Récupération point-in-time|

### Architecture logique

Hiérarchie d'organisation des données :

```
BASE DE DONNÉES
│
├── TABLESPACES (unités logiques de stockage)
│   ├── SYSTEM (dictionnaire de données, métadonnées système)
│   ├── SYSAUX (outils administratifs, AWR, STATSPACK)
│   ├── USERS (données utilisateurs par défaut)
│   ├── TEMP (opérations de tri et jointures temporaires)
│   └── UNDO (gestion de l'annulation et de la cohérence lecture)
│
├── SEGMENTS (objets de base de données)
│   ├── Table (données structurées)
│   ├── Index (accélération des recherches)
│   ├── Rollback (annulation de transactions)
│   └── Temporary (tri et opérations temporaires)
│
├── EXTENTS (allocations contiguës)
│   └── Ensemble de blocs Oracle adjacents
│
└── BLOCS ORACLE (plus petite unité d'I/O)
    └── Taille définie par DB_BLOCK_SIZE (typiquement 8K)
```

### Gestion des Tablespaces

#### Création de Tablespace permanent

```sql
CREATE TABLESPACE donnees_app
DATAFILE '/disk4/donnees_01.dbf' SIZE 500M,
         '/disk5/donnees_02.dbf' SIZE 500M
EXTENT MANAGEMENT LOCAL UNIFORM SIZE 128K
SEGMENT SPACE MANAGEMENT AUTO;
```

**Paramètres de stockage traditionnels (gestion manuelle) :**

```sql
CREATE TABLESPACE donnees_legacy
DATAFILE '/disk4/data.dbf' SIZE 500M
MINIMUM EXTENT 100K
DEFAULT STORAGE (
  INITIAL 64K        -- Taille du premier extent
  NEXT 64K           -- Taille des extents suivants
  MAXEXTENTS 500     -- Nombre maximum d'extents
  PCTINCREASE 50     -- Pourcentage d'augmentation
);
```

#### Création de Tablespace temporaire

```sql
CREATE TEMPORARY TABLESPACE temp_01
TEMPFILE '/disk3/temp_01.tmp' SIZE 200M
AUTOEXTEND ON NEXT 50M MAXSIZE 2G;
```

#### Création de Tablespace UNDO

```sql
CREATE UNDO TABLESPACE undo_tbs
DATAFILE '/disk2/undo_01.dbf' SIZE 100M
AUTOEXTEND ON NEXT 10M MAXSIZE 500M
RETENTION GUARANTEE;

-- Activer ce tablespace UNDO
ALTER SYSTEM SET UNDO_TABLESPACE = undo_tbs;
```

#### Opérations sur Tablespaces

```sql
-- Ajouter un fichier de données
ALTER TABLESPACE donnees_app 
ADD DATAFILE '/disk7/donnees_03.dbf' SIZE 1000M;

-- Redimensionner un fichier existant
ALTER DATABASE DATAFILE '/disk7/donnees_03.dbf' RESIZE 1500M;

-- Activer l'auto-extension
ALTER DATABASE DATAFILE '/disk7/donnees_03.dbf'
AUTOEXTEND ON NEXT 100M MAXSIZE 2000M;

-- Modifier le statut du tablespace
ALTER TABLESPACE donnees_app ONLINE;
ALTER TABLESPACE donnees_app OFFLINE NORMAL;
ALTER TABLESPACE donnees_app READ ONLY;
ALTER TABLESPACE donnees_app READ WRITE;

-- Supprimer un tablespace
DROP TABLESPACE donnees_app 
INCLUDING CONTENTS AND DATAFILES 
CASCADE CONSTRAINTS;
```

### Modes d'archivage

|Mode|Description|Usage recommandé|
|---|---|---|
|**NOARCHIVELOG**|Redo Logs écrasés cycliquement (mode par défaut)|Développement, test|
|**ARCHIVELOG**|Redo Logs archivés avant écrasement|Production ✓|

**Le mode ARCHIVELOG est essentiel pour :**

- Sauvegardes à chaud (base ouverte)
- Récupération point-in-time
- Récupération complète après panne

```sql
-- Vérifier le mode actuel
ARCHIVE LOG LIST;

-- Passer en mode ARCHIVELOG
SHUTDOWN IMMEDIATE;
STARTUP MOUNT;
ALTER DATABASE ARCHIVELOG;
ALTER DATABASE OPEN;

-- Vérifier à nouveau
ARCHIVE LOG LIST;
```

### QCM 4

**Q1: Combien de fichiers de contrôle minimum est-il recommandé d'avoir ?**

- a) 1
- b) 2
- c) 3 ✓
- d) 5

**Q2: Quel tablespace contient le dictionnaire de données Oracle ?**

- a) USERS
- b) SYSTEM ✓
- c) SYSAUX
- d) TEMP

**Q3: Quelle est la plus petite unité de stockage dans Oracle ?**

- a) Segment
- b) Extent
- c) Bloc Oracle ✓
- d) Tablespace

---

## 5. Gestion des utilisateurs et sécurité

### Création d'utilisateurs

```sql
CREATE USER dev_user
IDENTIFIED BY Mot2Passe123
DEFAULT TABLESPACE utilisateurs
QUOTA 10M ON utilisateurs
TEMPORARY TABLESPACE temp
QUOTA 5M ON temp
PROFILE profil_developpeur
PASSWORD EXPIRE;
```

**Explication des clauses :**

- `IDENTIFIED BY` : Définit le mot de passe
- `DEFAULT TABLESPACE` : Tablespace par défaut pour les objets de l'utilisateur
- `QUOTA` : Limite d'espace alloué sur un tablespace
- `TEMPORARY TABLESPACE` : Tablespace pour opérations temporaires
- `PROFILE` : Profil de ressources et sécurité à appliquer
- `PASSWORD EXPIRE` : Force le changement de mot de passe à la première connexion

### Méthodes d'authentification

|Méthode|Syntaxe|Description|
|---|---|---|
|**Par la base de données**|`IDENTIFIED BY mot_de_passe`|Mot de passe stocké dans Oracle|
|**Externe (OS)**|`IDENTIFIED EXTERNALLY`|Authentification déléguée au système d'exploitation|
|**Globale (LDAP)**|`IDENTIFIED GLOBALLY AS 'DN'`|Authentification via annuaire LDAP|

### Profils de ressources et sécurité

Les profils permettent de contrôler l'utilisation des ressources et d'appliquer des politiques de mots de passe.

```sql
CREATE PROFILE profil_application LIMIT
  -- Limites de sessions
  SESSIONS_PER_USER UNLIMITED
  -- Limites CPU
  CPU_PER_SESSION UNLIMITED
  CPU_PER_CALL 3000
  -- Limites de temps
  CONNECT_TIME 45
  IDLE_TIME 10
  -- Politique de mots de passe
  FAILED_LOGIN_ATTEMPTS 5
  PASSWORD_LIFE_TIME 90
  PASSWORD_REUSE_TIME 365
  PASSWORD_REUSE_MAX 10
  PASSWORD_LOCK_TIME 1
  PASSWORD_GRACE_TIME 7;
  
-- Assigner le profil à un utilisateur
ALTER USER utilisateur PROFILE profil_application;
```

#### Paramètres de profil importants

|Paramètre|Description|Unité|
|---|---|---|
|`FAILED_LOGIN_ATTEMPTS`|Nombre de tentatives de connexion échouées avant verrouillage|Tentatives|
|`PASSWORD_LIFE_TIME`|Durée de validité du mot de passe|Jours|
|`PASSWORD_LOCK_TIME`|Durée de verrouillage du compte après échecs|Jours|
|`PASSWORD_REUSE_TIME`|Délai avant réutilisation d'un ancien mot de passe|Jours|
|`PASSWORD_GRACE_TIME`|Période de grâce après expiration du mot de passe|Jours|
|`SESSIONS_PER_USER`|Nombre de sessions simultanées autorisées|Sessions|
|`CPU_PER_SESSION`|Temps CPU maximum par session|Centièmes de seconde|
|`IDLE_TIME`|Temps d'inactivité maximum avant déconnexion|Minutes|
|`CONNECT_TIME`|Durée maximale de connexion|Minutes|

### Gestion des privilèges

#### Types de privilèges

**1. Privilèges système** : Permettent d'effectuer des opérations au niveau de la base

```sql
-- Privilèges de connexion et création
GRANT CREATE SESSION TO utilisateur1;
GRANT CREATE TABLE TO utilisateur1;
GRANT CREATE VIEW TO utilisateur1;
GRANT CREATE PROCEDURE TO utilisateur1;
GRANT CREATE SEQUENCE TO utilisateur1;

-- Privilèges administratifs
GRANT ALTER SYSTEM TO admin_user;
GRANT DROP ANY TABLE TO admin_user;
```

**2. Privilèges objet** : Permettent d'effectuer des actions sur des objets spécifiques

```sql
-- Accorder des privilèges sur une table
GRANT SELECT, INSERT, UPDATE, DELETE 
ON schema_proprietaire.employes 
TO utilisateur1;

-- Avec possibilité de délégation (GRANT OPTION)
GRANT SELECT ON schema_proprietaire.employes 
TO utilisateur1 
WITH GRANT OPTION;

-- Privilèges sur d'autres objets
GRANT EXECUTE ON package_nom TO utilisateur1;
GRANT SELECT ON vue_nom TO utilisateur1;
```

### Gestion des rôles

Les rôles permettent de regrouper des privilèges pour simplifier leur administration.

```sql
-- Créer un rôle
CREATE ROLE role_comptabilite;

-- Assigner des privilèges au rôle
GRANT CREATE SESSION TO role_comptabilite;
GRANT SELECT, INSERT, UPDATE, DELETE 
ON comptabilite.factures 
TO role_comptabilite;
GRANT SELECT ON comptabilite.clients 
TO role_comptabilite;

-- Assigner le rôle à des utilisateurs
GRANT role_comptabilite TO user1, user2, user3;

-- Un rôle peut contenir d'autres rôles
CREATE ROLE role_chef_comptable;
GRANT role_comptabilite TO role_chef_comptable;
GRANT DELETE ON comptabilite.factures TO role_chef_comptable;
```

#### Rôles prédéfinis Oracle

|Rôle|Description|Privilèges principaux|
|---|---|---|
|**CONNECT**|Connexion basique|CREATE SESSION|
|**RESOURCE**|Création d'objets|CREATE TABLE, CREATE PROCEDURE, etc.|
|**DBA**|Administration complète|Tous privilèges système WITH ADMIN OPTION|

**Recommandation** : Créer des rôles personnalisés plutôt que d'utiliser CONNECT et RESOURCE qui sont obsolètes et accordent trop de privilèges.

### Révocation de privilèges

```sql
-- Révoquer un privilège système
REVOKE CREATE TABLE FROM utilisateur1;

-- Révoquer un privilège objet
REVOKE SELECT ON schema.employes FROM utilisateur1;

-- Révoquer un rôle
REVOKE role_comptabilite FROM utilisateur1;
```

**Effet en cascade** : Révoquer un privilège objet avec GRANT OPTION révoque aussi tous les privilèges délégués.

### Vues du dictionnaire pour la sécurité

```sql
-- Informations sur les utilisateurs
SELECT username, account_status, default_tablespace, profile
FROM DBA_USERS;

-- Quotas des utilisateurs
SELECT username, tablespace_name, max_bytes/1024/1024 AS quota_mb
FROM DBA_TS_QUOTAS;

-- Profils et leurs limites
SELECT profile, resource_name, limit
FROM DBA_PROFILES
ORDER BY profile, resource_name;

-- Rôles définis
SELECT role FROM DBA_ROLES;

-- Rôles attribués aux utilisateurs
SELECT grantee, granted_role
FROM DBA_ROLE_PRIVS
WHERE grantee = 'UTILISATEUR1';

-- Privilèges système
SELECT grantee, privilege, admin_option
FROM DBA_SYS_PRIVS
WHERE grantee = 'UTILISATEUR1';

-- Privilèges objet
SELECT grantee, owner, table_name, privilege
FROM DBA_TAB_PRIVS
WHERE grantee = 'UTILISATEUR1';
```

### QCM 5

**Q1: Quelle commande permet de créer un utilisateur ?**

- a) ADD USER
- b) CREATE USER ✓
- c) NEW USER
- d) INSERT USER

**Q2: Quel privilège système est nécessaire pour se connecter à Oracle ?**

- a) CREATE SESSION ✓
- b) CONNECT
- c) LOGIN
- d) ACCESS

**Q3: Un rôle Oracle peut contenir :**

- a) Uniquement des privilèges système
- b) Uniquement des privilèges objet
- c) Privilèges système, privilèges objet et autres rôles ✓
- d) Aucun privilège directement

---

## 6. Gestion des objets de base de données

### Tables

#### Types de données courants

|Type|Description|Exemple|Taille maximale|
|---|---|---|---|
|`CHAR(n)`|Chaîne de caractères fixe|`CHAR(10)`|2000 octets|
|`VARCHAR2(n)`|Chaîne de caractères variable|`VARCHAR2(50)`|4000 octets|
|`NUMBER(p,s)`|Nombre décimal|`NUMBER(10,2)`|Précision 38|
|`DATE`|Date et heure|`01-JAN-2025 14:30:00`|-|
|`TIMESTAMP`|Date et heure avec précision|Avec fractions de seconde|-|
|`CLOB`|Texte volumineux|Documents texte|Jusqu'à 4 Go|
|`BLOB`|Données binaires volumineuses|Images, fichiers|Jusqu'à 4 Go|
|`RAW(n)`|Données binaires brutes|`RAW(2000)`|2000 octets|
|`LONG`|Texte long (obsolète)|-|2 Go|

#### Création de table avec contraintes

```sql
CREATE TABLE employes (
  matricule NUMBER(5) CONSTRAINT pk_emp PRIMARY KEY,
  nom VARCHAR2(30) NOT NULL,
  prenom VARCHAR2(30),
  fonction VARCHAR2(20),
  responsable NUMBER(5),
  date_embauche DATE DEFAULT SYSDATE,
  salaire NUMBER(8,2) CHECK (salaire > 0),
  commission NUMBER(8,2),
  num_dept NUMBER(3) NOT NULL
    CONSTRAINT fk_dept REFERENCES departements(num_dept)
)
PCTFREE 10
PCTUSED 40
TABLESPACE donnees_utilisateurs
STORAGE (
  INITIAL 50K
  NEXT 50K
  MAXEXTENTS 10
  PCTINCREASE 25
);
```

**Explication des paramètres de stockage :**

- `PCTFREE` : Pourcentage d'espace réservé pour les mises à jour
- `PCTUSED` : Seuil de remplissage pour réinsertion dans la liste des blocs disponibles
- `INITIAL` : Taille du premier extent
- `NEXT` : Taille du prochain extent
- `PCTINCREASE` : Pourcentage d'augmentation des extents successifs

#### Création de table à partir d'une requête

```sql
-- Copie complète (structure + données)
CREATE TABLE employes_backup
AS SELECT * FROM employes;

-- Copie de structure uniquement
CREATE TABLE employes_template
AS SELECT * FROM employes WHERE 1=0;

-- Copie partielle avec transformation
CREATE TABLE employes_dept10
AS SELECT matricule, nom, prenom, salaire*12 AS salaire_annuel
FROM employes
WHERE num_dept = 10;
```

#### Modification de table

```sql
-- Ajouter une colonne
ALTER TABLE employes
ADD (email VARCHAR2(50), telephone VARCHAR2(15));

-- Ajouter avec valeur par défaut
ALTER TABLE employes
ADD (statut CHAR(1) DEFAULT 'A' NOT NULL);

-- Modifier une colonne
ALTER TABLE employes
MODIFY (salaire NUMBER(10,2), nom VARCHAR2(50) NOT NULL);

-- Renommer une colonne
ALTER TABLE employes
RENAME COLUMN fonction TO poste;

-- Supprimer une colonne
ALTER TABLE employes
DROP COLUMN commission;

-- Ajouter une contrainte
ALTER TABLE employes
ADD CONSTRAINT chk_salaire CHECK (salaire BETWEEN 1000 AND 100000);

-- Supprimer une contrainte
ALTER TABLE employes
DROP CONSTRAINT fk_dept;

-- Désactiver/Activer une contrainte
ALTER TABLE employes
DISABLE CONSTRAINT fk_dept;

ALTER TABLE employes
ENABLE CONSTRAINT fk_dept;
```

#### Suppression et renommage

```sql
-- Supprimer une table
DROP TABLE employes CASCADE CONSTRAINTS;

-- Renommer une table
RENAME employes TO salaries;

-- Tronquer une table (suppression rapide de toutes les lignes)
TRUNCATE TABLE employes;
```

### Index

Les index accélèrent les recherches mais ralentissent les insertions/mises à jour.

```sql
-- Créer un index simple
CREATE INDEX idx_dept ON employes(num_dept);

-- Index composé
CREATE INDEX idx_nom_prenom ON employes(nom, prenom)
PCTFREE 10
TABLESPACE index_tbs
STORAGE (INITIAL 64K NEXT 64K);

-- Index unique
CREATE UNIQUE INDEX idx_email ON employes(email);

-- Index bitmap (pour faible cardinalité)
CREATE BITMAP INDEX idx_statut ON employes(statut);

-- Reconstruire un index
ALTER INDEX idx_dept REBUILD TABLESPACE nouveau_tbs;

-- Rendre un index inutilisable puis le reconstruire
ALTER INDEX idx_dept UNUSABLE;
ALTER INDEX idx_dept REBUILD;

-- Supprimer un index
DROP INDEX idx_dept;
```

**Quand créer des index :**

- ✓ Sur les clés primaires (automatique)
- ✓ Sur les clés étrangères
- ✓ Sur les colonnes fréquemment utilisées dans les clauses WHERE
- ✓ Sur les colonnes utilisées dans les jointures
- ✗ Sur les petites tables (< 1000 lignes)
- ✗ Sur les colonnes fréquemment modifiées
- ✗ Sur les colonnes à faible sélectivité (sauf bitmap)

### Vues

Les vues sont des requêtes stockées qui apparaissent comme des tables virtuelles.

```sql
-- Créer une vue simple
CREATE VIEW vue_dept_10
AS SELECT matricule, nom, prenom, fonction
FROM employes
WHERE num_dept = 10;

-- Vue avec vérification des contraintes
CREATE VIEW vue_dept_10_check
AS SELECT matricule, nom, prenom, fonction, num_dept
FROM employes
WHERE num_dept = 10
WITH CHECK OPTION;

-- Vue en lecture seule
CREATE VIEW vue_salaires
AS SELECT nom, prenom, salaire
FROM employes
WITH READ ONLY;

-- Vue avec jointure
CREATE VIEW vue_employes_complet
AS SELECT e.matricule, e.nom, e.prenom, d.nom_dept, d.localisation
FROM employes e
JOIN departements d ON e.num_dept = d.num_dept;

-- Modifier une vue
CREATE OR REPLACE VIEW vue_dept_10
AS SELECT matricule, nom, prenom, fonction, salaire
FROM employes
WHERE num_dept = 10;

-- Supprimer une vue
DROP VIEW vue_dept_10;
```

**Limitations pour les vues modifiables (INSERT/UPDATE/DELETE) :**

- ✗ Vue avec jointures
- ✗ Vue avec GROUP BY, DISTINCT, fonctions d'agrégation
- ✗ Vue avec ROWNUM ou expressions
- ✗ Vue avec WITH READ ONLY
- ✓ Vue simple basée sur une seule table sans transformation complexe

### Séquences

Les séquences génèrent des valeurs numériques uniques, souvent utilisées pour les clés primaires.

```sql
-- Créer une séquence
CREATE SEQUENCE seq_employe
INCREMENT BY 1
START WITH 1000
MAXVALUE 999999
MINVALUE 1
NOCYCLE
CACHE 20
ORDER;

-- Utiliser la séquence
INSERT INTO employes (matricule, nom, prenom, num_dept)
VALUES (seq_employe.NEXTVAL, 'DUPONT', 'Jean', 30);

-- Obtenir la valeur courante
SELECT seq_employe.CURRVAL FROM dual;

-- Modifier une séquence
ALTER SEQUENCE seq_employe
INCREMENT BY 5
MAXVALUE 9999999
CACHE 50;

-- Réinitialiser une séquence (méthode indirecte)
-- Impossible directement, il faut la supprimer et la recréer
DROP SEQUENCE seq_employe;
CREATE SEQUENCE seq_employe START WITH 1;

-- Supprimer une séquence
DROP SEQUENCE seq_employe;
```

**Pseudo-colonnes de séquence :**

- `NEXTVAL` : Génère et retourne la prochaine valeur
- `CURRVAL` : Retourne la dernière valeur générée dans la session courante

### Synonymes

Les synonymes créent des alias pour des objets, simplifiant l'accès.

```sql
-- Synonyme privé (accessible uniquement par le créateur)
CREATE SYNONYM emp FOR ressources_humaines.employes;

-- Synonyme public (accessible par tous)
CREATE PUBLIC SYNONYM employes
FOR ressources_humaines.employes;

-- Synonyme pour objet distant
CREATE SYNONYM emp_distant
FOR employes@serveur_rh.domaine.com;

-- Utilisation
SELECT * FROM emp;  -- Au lieu de SELECT * FROM ressources_humaines.employes

-- Supprimer un synonyme
DROP SYNONYM emp;
DROP PUBLIC SYNONYM employes;
```

### Database Links

Les database links permettent d'accéder à des objets sur une base de données distante.

```sql
-- Créer un database link
CREATE DATABASE LINK lien_production
CONNECT TO utilisateur_distant IDENTIFIED BY mot_de_passe
USING 'service_tns_distant';

-- Utiliser le database link
SELECT * FROM employes@lien_production;

INSERT INTO commandes@lien_production
(client_id, commande_id, montant)
VALUES (1001, 5234, 15000);

-- Database link public
CREATE PUBLIC DATABASE LINK lien_entrepot
CONNECT TO utilisateur IDENTIFIED BY mot_de_passe
USING 'entrepot_donnees';

-- Supprimer un database link
DROP DATABASE LINK lien_production;
```

### QCM 6

**Q1: Quel type de données pour une chaîne de caractères variable ?**

- a) CHAR
- b) VARCHAR
- c) VARCHAR2 ✓
- d) STRING

**Q2: Quelles sont les pseudo-colonnes d'une séquence ?**

- a) CURRENT et NEXT
- b) CURRVAL et NEXTVAL ✓
- c) NOW et NEXT
- d) VALUE et NEXTVALUE

**Q3: Une vue peut-elle toujours être mise à jour ?**

- a) Jamais
- b) Toujours
- c) Selon certaines conditions ✓
- d) Uniquement par un administrateur DBA

---

## 7. Sauvegarde et restauration

### Stratégies de sauvegarde

```
SAUVEGARDES
├── PHYSIQUES (copie de fichiers du système d'exploitation)
│   ├── À FROID (base fermée - cohérente)
│   └── À CHAUD (base ouverte - nécessite ARCHIVELOG)
│
└── LOGIQUES (export/import de données et métadonnées)
    ├── FULL (toute la base)
    ├── SCHEMA (objets d'un utilisateur)
    └── TABLE (objets spécifiques)
```

### Sauvegarde physique à froid

Sauvegarde la plus simple et la plus sûre, mais nécessite l'arrêt de la base.

```sql
-- 1. Arrêter la base proprement
SHUTDOWN IMMEDIATE;

-- 2. Copier TOUS les fichiers critiques (commandes système)
--    - Tous les fichiers de données (.dbf)
--    - Tous les fichiers de contrôle
--    - Tous les fichiers Redo Log
--    - Fichier de paramètres (init.ora ou spfile.ora)

-- 3. Redémarrer la base
STARTUP;
```

**Avantages :** Simplicité, cohérence garantie **Inconvénients :** Indisponibilité pendant la sauvegarde

### Sauvegarde physique à chaud

Permet de sauvegarder sans arrêter la base, mais nécessite le mode ARCHIVELOG.

#### Prérequis : Mode ARCHIVELOG activé

```sql
-- Vérifier le mode actuel
ARCHIVE LOG LIST;

-- Activer le mode ARCHIVELOG
SHUTDOWN IMMEDIATE;
STARTUP MOUNT;
ALTER DATABASE ARCHIVELOG;
ALTER DATABASE OPEN;
```

#### Sauvegarde tablespace par tablespace

**Méthode 1 : Tablespace OFFLINE**

```sql
-- Mettre le tablespace hors ligne
ALTER TABLESPACE utilisateurs OFFLINE NORMAL;

-- Copier les fichiers de données du tablespace (commande OS)
-- cp /u01/oradata/utilisateurs01.dbf /sauvegarde/

-- Remettre le tablespace en ligne
ALTER TABLESPACE utilisateurs ONLINE;
```

**Méthode 2 : Tablespace ONLINE (mode backup)**

```sql
-- Démarrer le mode backup
ALTER TABLESPACE utilisateurs BEGIN BACKUP;

-- Copier les fichiers de données (commande OS)
-- cp /u01/oradata/utilisateurs01.dbf /sauvegarde/

-- Terminer le mode backup
ALTER TABLESPACE utilisateurs END BACKUP;
```

**Important :** Toujours sauvegarder les fichiers de contrôle et les archives

```sql
-- Sauvegarder le fichier de contrôle
ALTER DATABASE BACKUP CONTROLFILE TO '/sauvegarde/control.ctl';

-- Ou générer un script de recréation
ALTER DATABASE BACKUP CONTROLFILE TO TRACE;
```

### Export / Import (sauvegarde logique)

#### Modes d'export

|Mode|Paramètre|Description|Usage|
|---|---|---|---|
|**Full**|`FULL=Y`|Toute la base (sauf SYS)|Sauvegarde complète|
|**Schema**|`OWNER=utilisateur`|Objets d'un ou plusieurs utilisateurs|Migration utilisateur|
|**Table**|`TABLES=table1,table2`|Tables spécifiques|Export sélectif|
|**Tablespace**|`TABLESPACES=tbs`|Tous objets d'un tablespace|Migration partielle|

#### Exemples d'export

```bash
# Export complet de la base
exp system/manager FILE=sauvegarde_complete.dmp LOG=export_full.log FULL=Y

# Export d'un schéma
exp system/manager FILE=schema_rh.dmp LOG=export_rh.log OWNER=ressources_humaines

# Export de plusieurs schémas
exp system/manager FILE=schemas.dmp LOG=export.log OWNER=(rh,compta,ventes)

# Export de tables spécifiques
exp scott/tiger FILE=tables_emp.dmp LOG=export_tables.log TABLES=(emp,dept)

# Export avec compression
exp system/manager FILE=backup.dmp LOG=export.log FULL=Y COMPRESS=Y

# Export cohérent (snapshot au début)
exp system/manager FILE=backup.dmp LOG=export.log FULL=Y CONSISTENT=Y

# Export avec fichier de paramètres
exp PARFILE=parametres_export.txt
```

**Fichier de paramètres (parametres_export.txt) :**

```
USERID=system/manager
FILE=export_schema.dmp
LOG=export.log
OWNER=ressources_humaines
ROWS=Y
CONSTRAINTS=Y
GRANTS=Y
INDEXES=Y
TRIGGERS=Y
STATISTICS=NONE
```

#### Exemples d'import

```bash
# Import complet
imp system/manager FILE=sauvegarde_complete.dmp FULL=Y LOG=import.log

# Import d'un schéma dans le même schéma
imp scott/tiger FILE=schema_scott.dmp LOG=import.log

# Import d'un schéma vers un autre schéma
imp system/manager FILE=scott.dmp LOG=import.log 
  FROMUSER=scott TOUSER=utilisateur_test

# Import de tables spécifiques
imp system/manager FILE=tables.dmp LOG=import.log 
  FROMUSER=scott TOUSER=nouveau_schema TABLES=(emp,dept)

# Import en ignorant les erreurs de création (objets existants)
imp system/manager FILE=backup.dmp FULL=Y LOG=import.log IGNORE=Y

# Import avec commits fréquents (grosses tables)
imp system/manager FILE=backup.dmp FULL=Y LOG=import.log COMMIT=Y BUFFER=1000000
```

#### Paramètres Import/Export importants

|Paramètre|Description|Valeur par défaut|Usage|
|---|---|---|---|
|`USERID`|Identifiants de connexion|-|Obligatoire|
|`FILE`|Fichier dump|expdat.dmp|Chemin fichier|
|`LOG`|Fichier log|Écran|Traçabilité|
|`FULL`|Export/Import complet|N|Sauvegarde totale|
|`OWNER`|Schéma(s) à exporter|-|Export sélectif|
|`TABLES`|Tables à exporter|-|Export partiel|
|`ROWS`|Inclure les données|Y|Structure seule si N|
|`CONSTRAINTS`|Inclure contraintes|Y|Intégrité|
|`INDEXES`|Inclure index|Y|Performance|
|`GRANTS`|Inclure privilèges|Y|Sécurité|
|`TRIGGERS`|Inclure déclencheurs|Y|Logique métier|
|`CONSISTENT`|Snapshot cohérent|N|Cohérence lecture|
|`COMPRESS`|Compresser extents|Y|Optimisation|
|`IGNORE`|Ignorer erreurs création|N|Import répétitif|
|`COMMIT`|Commits fréquents|N|Grosses tables|
|`BUFFER`|Taille buffer|OS-dépendant|Performance|
|`FROMUSER`|Schéma source|-|Migration schéma|
|`TOUSER`|Schéma cible|-|Migration schéma|

### SQL*Loader

Outil de chargement en masse de données depuis des fichiers texte.

#### Préparation

```sql
-- Créer la table cible
CREATE TABLE employes_import (
  numero NUMBER(5),
  nom VARCHAR2(35),
  prenom VARCHAR2(15),
  date_naissance DATE,
  sexe CHAR(1)
);
```

#### Fichier de données (donnees.dat)

```
1001,DUPONT,Jean,150685,M
1002,MARTIN,Marie,230590,F
1003,BERNARD,Pierre,101278,M
```

#### Fichier de contrôle (chargement.ctl)

```
LOAD DATA
INFILE 'donnees.dat'
BADFILE 'rejets.bad'
DISCARDFILE 'exclus.dsc'
APPEND
INTO TABLE employes_import
FIELDS TERMINATED BY ','
OPTIONALLY ENCLOSED BY '"'
TRAILING NULLCOLS
(
  NUMERO,
  NOM,
  PRENOM,
  DATE_NAISSANCE DATE "DDMMYY",
  SEXE
)
```

**Options de chargement :**

- `INSERT` : Table vide obligatoire
- `APPEND` : Ajoute aux données existantes
- `REPLACE` : Supprime et recharge
- `TRUNCATE` : Tronque et recharge

#### Exécution

```bash
sqlldr userid=scott/tiger CONTROL=chargement.ctl LOG=chargement.log
```

**Fichiers générés :**

- `.log` : Journal d'exécution
- `.bad` : Enregistrements rejetés (erreurs)
- `.dsc` : Enregistrements exclus (conditions WHERE)

### Duplication de base de données

#### Méthode 1 : Export/Import complet

```bash
# 1. Exporter la base source
exp system/manager@source FULL=Y FILE=base_complete.dmp LOG=export.log CONSISTENT=Y

# 2. Créer la base cible (même structure, paramètres similaires)
# Utiliser DBCA ou CREATE DATABASE

# 3. Importer dans la base cible
imp system/manager@cible FULL=Y FILE=base_complete.dmp LOG=import.log IGNORE=Y
```

#### Méthode 2 : Sauvegarde physique

```bash
# 1. Arrêter la base source
sqlplus / as sysdba
SHUTDOWN IMMEDIATE;

# 2. Copier tous les fichiers vers la destination
# - Fichiers de données
# - Fichiers de contrôle
# - Fichiers Redo Log
# - Fichiers de paramètres

# 3. Créer l'instance sur le serveur cible
# Modifier le fichier de paramètres si nécessaire

# 4. Démarrer la base clone
STARTUP;
```

#### Méthode 3 : RMAN Duplicate (recommandée)

```bash
# Configuration RMAN pour duplication automatisée
rman TARGET sys/password@source AUXILIARY sys/password@cible

DUPLICATE TARGET DATABASE TO cible
FROM ACTIVE DATABASE
NOFILENAMECHECK;
```

### QCM 7

**Q1: Quel mode d'archivage est requis pour une sauvegarde à chaud ?**

- a) NOARCHIVELOG
- b) ARCHIVELOG ✓
- c) READ ONLY
- d) RESTRICTED

**Q2: Quel privilège est nécessaire pour un export FULL ?**

- a) DBA
- b) EXP_FULL_DATABASE ✓
- c) SYSDBA
- d) RESOURCE

**Q3: SQL*Loader charge des données depuis :**

- a) Fichiers binaires Oracle
- b) Fichiers texte (CSV, délimités) ✓
- c) Autre base de données Oracle
- d) Fichiers XML uniquement

---

## 8. Résolution de problèmes

### Erreurs courantes et solutions

#### ORA-01034: ORACLE not available

**Cause :** L'instance Oracle n'est pas démarrée

**Diagnostic :**

```sql
-- Vérifier l'état de l'instance
SELECT status FROM V$INSTANCE;
```

**Solution :**

```sql
SET ORACLE_SID=nom_base
sqlplus / as sysdba
SHUTDOWN ABORT;
STARTUP;
```

---

#### ORA-12154: TNS:could not resolve the connect identifier

**Cause :** Le nom de service réseau n'est pas trouvé ou mal configuré

**Points de vérification :**

1. Fichier `tnsnames.ora` correct
2. Variable `TNS_ADMIN` pointant vers le bon répertoire
3. Nom de service correctement orthographié
4. Listener actif

**Solutions :**

```bash
# Vérifier le listener
lsnrctl status

# Tester la connectivité
tnsping nom_service

# Vérifier le fichier tnsnames.ora
cat $ORACLE_HOME/network/admin/tnsnames.ora
```

---

#### ORA-01653: unable to extend table ... by ... in tablespace ...

**Cause :** Espace insuffisant dans le tablespace

**Diagnostic :**

```sql
-- Vérifier l'espace utilisé
SELECT tablespace_name, 
       SUM(bytes)/1024/1024 AS total_mb,
       SUM(maxbytes)/1024/1024 AS max_mb
FROM dba_data_files
GROUP BY tablespace_name;

-- Vérifier l'espace libre
SELECT tablespace_name, 
       SUM(bytes)/1024/1024 AS free_mb
FROM dba_free_space
GROUP BY tablespace_name;
```

**Solutions :**

```sql
-- Solution 1: Activer l'auto-extension
ALTER DATABASE DATAFILE '/chemin/fichier.dbf'
AUTOEXTEND ON NEXT 100M MAXSIZE 2000M;

-- Solution 2: Redimensionner le fichier
ALTER DATABASE DATAFILE '/chemin/fichier.dbf' 
RESIZE 1500M;

-- Solution 3: Ajouter un nouveau fichier
ALTER TABLESPACE nom_tablespace
ADD DATAFILE '/chemin/nouveau_fichier.dbf' SIZE 500M
AUTOEXTEND ON NEXT 100M MAXSIZE UNLIMITED;
```

---

#### ORA-01652: unable to extend temp segment by ... in tablespace TEMP

**Cause :** Segment temporaire saturé (tri, jointures, index)

**Diagnostic :**

```sql
-- Vérifier l'utilisation du tablespace temporaire
SELECT tablespace_name, 
       SUM(bytes_used)/1024/1024 AS used_mb,
       SUM(bytes_free)/1024/1024 AS free_mb
FROM V$TEMP_SPACE_HEADER
GROUP BY tablespace_name;
```

**Solutions :**

```sql
-- Solution 1: Augmenter le paramètre SORT_AREA_SIZE
-- Modifier init.ora ou spfile, puis redémarrer

-- Solution 2: Agrandir le tablespace TEMP
ALTER TABLESPACE temp
ADD TEMPFILE '/chemin/temp02.dbf' SIZE 500M
AUTOEXTEND ON NEXT 100M;

-- Solution 3: Optimiser la requête
-- Analyser le plan d'exécution et créer des index appropriés
```

---

#### ORA-01650: unable to extend rollback segment ... by ... in tablespace ...

**Cause :** Segment UNDO/Rollback saturé (transaction volumineuse)

**Solutions :**

```sql
-- Solution 1: COMMITs plus fréquents dans les traitements batch
BEGIN
  FOR rec IN (SELECT * FROM grosse_table) LOOP
    -- Traitement
    IF MOD(SQL%ROWCOUNT, 1000) = 0 THEN
      COMMIT;
    END IF;
  END LOOP;
  COMMIT;
END;
/

-- Solution 2: Utiliser un segment rollback plus grand
SET TRANSACTION USE ROLLBACK SEGMENT gros_rbs;

-- Solution 3: Pour Import
imp userid=user/pass FILE=fichier.dmp COMMIT=Y BUFFER=10000000

-- Solution 4: Agrandir le tablespace UNDO
ALTER TABLESPACE undotbs1
ADD DATAFILE '/chemin/undo02.dbf' SIZE 500M;
```

---

#### ORA-00942: table or view does not exist

**Causes possibles :**

1. La table n'existe vraiment pas
2. Problème de droits d'accès
3. Table appartient à un autre schéma
4. Erreur de syntaxe (nom mal orthographié)

**Diagnostic :**

```sql
-- Vérifier l'existence dans votre schéma
SELECT table_name FROM user_tables WHERE table_name = 'NOM_TABLE';

-- Vérifier dans tous les schémas accessibles
SELECT owner, table_name FROM all_tables WHERE table_name = 'NOM_TABLE';

-- Vérifier les privilèges
SELECT * FROM user_tab_privs WHERE table_name = 'NOM_TABLE';
```

**Solutions :**

```sql
-- Si la table existe dans un autre schéma
SELECT * FROM schema_proprietaire.nom_table;

-- Créer un synonyme
CREATE SYNONYM nom_table FOR schema_proprietaire.nom_table;

-- Demander les droits nécessaires
-- L'administrateur doit exécuter :
GRANT SELECT ON schema_proprietaire.nom_table TO votre_user;
```

---

#### ORA-01031: insufficient privileges

**Cause :** Droits insuffisants pour l'opération demandée

**Solutions :**

```sql
-- Pour privilèges système
GRANT CREATE TABLE TO utilisateur;
GRANT DROP ANY TABLE TO utilisateur;

-- Pour privilèges objet
GRANT SELECT, INSERT, UPDATE, DELETE ON table TO utilisateur;

-- Avec possibilité de délégation
GRANT SELECT ON table TO utilisateur WITH GRANT OPTION;

-- Vérifier les privilèges actuels
SELECT * FROM user_sys_privs;
SELECT * FROM user_tab_privs;
```

---

#### ORA-00054: resource busy and acquire with NOWAIT specified

**Cause :** Ressource verrouillée par une autre session

**Diagnostic :**

```sql
-- Identifier les verrous actifs
SELECT s.sid, s.serial#, s.username, s.program,
       o.object_name, l.locked_mode
FROM v$locked_object l
JOIN dba_objects o ON l.object_id = o.object_id
JOIN v$session s ON l.session_id = s.sid;
```

**Solution :**

```sql
-- Tuer la session bloquante (avec précaution)
ALTER SYSTEM KILL SESSION 'sid,serial#';
```

---

### Requêtes de diagnostic utiles

#### Vérification des fichiers et de l'espace

```sql
-- Fichiers de données et espace utilisé
SELECT file_name, tablespace_name, 
       bytes/1024/1024 AS size_mb,
       maxbytes/1024/1024 AS max_mb,
       autoextensible
FROM dba_data_files
ORDER BY tablespace_name;

-- Espace libre par tablespace
SELECT tablespace_name, 
       SUM(bytes)/1024/1024 AS free_mb,
       COUNT(*) AS fragments
FROM dba_free_space
GROUP BY tablespace_name
ORDER BY 1;

-- Utilisation par tablespace (pourcentage)
SELECT a.tablespace_name,
       ROUND(((c.bytes - NVL(b.bytes,0)) / c.bytes) * 100, 2) AS pct_used
FROM dba_tablespaces a,
     (SELECT tablespace_name, SUM(bytes) bytes
      FROM dba_free_space GROUP BY tablespace_name) b,
     (SELECT tablespace_name, SUM(bytes) bytes
      FROM dba_data_files GROUP BY tablespace_name) c
WHERE a.tablespace_name = b.tablespace_name(+)
  AND a.tablespace_name = c.tablespace_name
ORDER BY pct_used DESC;
```

#### Sessions et processus

```sql
-- Sessions actives
SELECT username, sid, serial#, status, program, machine,
       logon_time, last_call_et/60 AS idle_minutes
FROM v$session
WHERE username IS NOT NULL
ORDER BY logon_time;

-- Processus et mémoire PGA
SELECT program, 
       pga_used_mem/1024/1024 AS pga_used_mb,
       pga_max_mem/1024/1024 AS pga_max_mb
FROM v$process
ORDER BY pga_used_mem DESC;

-- Requêtes actives
SELECT s.sid, s.username, s.status, q.sql_text
FROM v$session s
JOIN v$sql q ON s.sql_id = q.sql_id
WHERE s.username IS NOT NULL
  AND s.status = 'ACTIVE';
```

#### Fichiers Redo Log et archivage

```sql
-- État des Redo Logs
SELECT group#, sequence#, bytes/1024/1024 AS size_mb, 
       members, status, archived
FROM v$log
ORDER BY group#;

-- Membres des groupes de Redo Log
SELECT group#, member, status
FROM v$logfile
ORDER BY group#, member;

-- Vérifier le mode d'archivage
SELECT log_mode, force_logging FROM v$database;

-- Destination des archives
SELECT name, value FROM v$parameter 
WHERE name LIKE 'log_archive%';
```

#### Paramètres d'instance

```sql
-- Tous les paramètres
SELECT name, value, isdefault, description
FROM v$parameter
ORDER BY name;

-- Paramètres modifiés (non par défaut)
SELECT name, value, description
FROM v$parameter
WHERE isdefault = 'FALSE'
ORDER BY name;

-- Paramètres spécifiques
SELECT name, value FROM v$parameter
WHERE name IN ('db_block_size', 'sga_target', 'pga_aggregate_target',
               'processes', 'sessions', 'open_cursors');
```

### QCM 8

**Q1: Que signifie l'erreur ORA-01034 ?**

- a) Base de données verrouillée
- b) Instance Oracle non disponible ✓
- c) Tablespace saturé
- d) Mot de passe incorrect

**Q2: Comment agrandir un tablespace saturé ?**

- a) Supprimer et recréer le tablespace
- b) Ajouter un datafile ou redimensionner ✓
- c) Impossible sans redémarrage
- d) Redémarrer uniquement la base

**Q3: Quelle vue permet de voir l'espace libre dans les tablespaces ?**

- a) V$TABLESPACE
- b) DBA_FREE_SPACE ✓
- c) USER_FREE_SPACE
- d) V$DATAFILE

---

## 9. Outils et administration réseau

### Outils d'administration Oracle

|Outil|Description|Usage|
|---|---|---|
|**DBCA**|Database Configuration Assistant|Créer, configurer, supprimer des bases|
|**OEM**|Oracle Enterprise Manager|Interface graphique complète d'administration|
|**SQL*Plus**|Interface ligne de commande|Exécution de requêtes et scripts SQL|
|**Net Configuration Assistant**|Assistant de configuration réseau|Configurer listener et services|
|**Net Manager**|Gestionnaire réseau|Configurer finement Oracle Net|
|**RMAN**|Recovery Manager|Sauvegardes et restaurations avancées|

### Oracle Net (SQL*Net)

#### Gestion du Listener

Le listener est le processus qui écoute les demandes de connexion.

```bash
# Démarrer le listener
lsnrctl start

# Arrêter le listener
lsnrctl stop

# Recharger la configuration sans arrêt
lsnrctl reload

# Vérifier le statut
lsnrctl status

# Afficher les services
lsnrctl services

# Voir l'aide
lsnrctl help
```

#### Configuration du fichier tnsnames.ora

Fichier de configuration côté client définissant les alias de connexion.

**Emplacement :** `$ORACLE_HOME/network/admin/tnsnames.ora`

```
# Connexion locale
ORCL =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl)
    )
  )

# Connexion distante
PROD =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = serveur1.domaine.com)(PORT = 1521))
      (ADDRESS = (PROTOCOL = TCP)(HOST = serveur2.domaine.com)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SERVICE_NAME = production)
      (FAILOVER_MODE =
        (TYPE = SELECT)
        (METHOD = BASIC)
      )
    )
  )
```

#### Configuration du fichier listener.ora

Fichier de configuration côté serveur.

**Emplacement :** `$ORACLE_HOME/network/admin/listener.ora`

```
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = serveur.domaine.com)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = orcl)
      (ORACLE_HOME = /u01/app/oracle/product/11.2.0/dbhome_1)
      (SID_NAME = orcl)
    )
  )
```

### Sécurité - Les piliers

|Pilier|Description|Mécanismes Oracle|
|---|---|---|
|**Confidentialité**|Protection contre accès non autorisés|Privilèges, rôles, VPD|
|**Disponibilité**|Système accessible quand nécessaire|RAC, Data Guard, backups|
|**Intégrité**|Données exactes et complètes|Contraintes, triggers, transactions ACID|
|**Fiabilité**|Fonctionnement stable et prévisible|Redo logs, UNDO, checkpoints|
|**Traçabilité**|Suivi des actions et modifications|Audit, Flashback, LogMiner|
|**Maintenabilité**|Facilité de maintenance et récupération|RMAN, Data Pump, OEM|

---

## 10. Bonnes pratiques DBA

### À FAIRE

- ✓ **Multiplexer les fichiers critiques** : Au moins 3 fichiers de contrôle et 2 membres par groupe de Redo Log sur disques différents
- ✓ **Mode ARCHIVELOG en production** : Indispensable pour récupération point-in-time
- ✓ **Sauvegardes régulières** : Physiques (quotidiennes) + logiques (hebdomadaires)
- ✓ **Créer des rôles personnalisés** : Au lieu d'utiliser CONNECT/RESOURCE
- ✓ **Surveiller alert.log quotidiennement** : Détecter erreurs et problèmes rapidement
- ✓ **Définir des quotas utilisateurs** : Éviter saturation des tablespaces
- ✓ **Utiliser des profils de ressources** : Limiter consommation CPU, sessions
- ✓ **Documenter l'architecture** : Schémas, procédures, configurations
- ✓ **Séparer données et index** : Utiliser des tablespaces différents
- ✓ **Monitorer les performances** : Vues V$, AWR, ADDM
- ✓ **Planifier la croissance** : Prévoir évolution volumes de données
- ✓ **Tester les restaurations** : Vérifier régulièrement les sauvegardes

### À ÉVITER

- ✗ **SHUTDOWN ABORT en routine** : Utiliser uniquement en urgence
- ✗ **Donner DBA/RESOURCE aux utilisateurs** : Accorde trop de privilèges
- ✗ **Garder mots de passe par défaut** : Changer immédiatement (change_on_install, oracle, etc.)
- ✗ **Stocker données utilisateurs dans SYSTEM** : Réserver pour dictionnaire uniquement
- ✗ **Mélanger données et index** : Séparer pour optimiser I/O
- ✗ **PCTINCREASE élevé** : Cause fragmentation rapide
- ✗ **Ignorer les alertes** : Traiter rapidement les messages d'erreur
- ✗ **Négliger la sécurité** : Auditer, changer mots de passe, limiter accès
- ✗ **Oublier les statistiques** : Collecter régulièrement pour optimiseur
- ✗ **Sur-indexer** : Chaque index ralentit INSERT/UPDATE/DELETE

---

## 11. Formules et calculs utiles

### Calcul de la taille SGA

```
Taille SGA = DB_CACHE_SIZE + SHARED_POOL_SIZE + 
             LARGE_POOL_SIZE + JAVA_POOL_SIZE + 
             STREAMS_POOL_SIZE + LOG_BUFFER
```

Ou utiliser le paramètre automatique :

```sql
ALTER SYSTEM SET SGA_TARGET = 2G SCOPE=SPFILE;
```

### Calcul de la taille d'un segment

Avec gestion manuelle des extents :

```
Taille = INITIAL + NEXT + NEXT*(1 + PCTINCREASE/100) + 
         NEXT*(1 + PCTINCREASE/100)² + ... jusqu'à MAXEXTENTS
```

### Espace total d'un tablespace

```
Espace total = Σ (taille de tous les datafiles du tablespace)
```

### Taux de succès du cache (hit ratio)

```
Hit Ratio = (1 - Physical Reads / Logical Reads) × 100
```

Un bon taux est généralement > 90%

---

## 12. Checklist DBA quotidienne

**Chaque matin :**

- [ ] Vérifier l'état des instances (`V$INSTANCE`)
- [ ] Consulter le fichier `alert.log` pour erreurs
- [ ] Vérifier l'espace des tablespaces (`DBA_FREE_SPACE`)
- [ ] Contrôler l'état des sauvegardes (RMAN ou scripts)
- [ ] Vérifier l'archivage des Redo Logs (`V$LOG`, `V$ARCHIVED_LOG`)
- [ ] Contrôler le statut du listener (`lsnrctl status`)
- [ ] Examiner les sessions actives et longues (`V$SESSION`)
- [ ] Vérifier les jobs et tâches planifiées (`DBA_JOBS`, `DBA_SCHEDULER_JOBS`)
- [ ] Analyser les performances (`V$SGA`, `V$SYSSTAT`, AWR)
- [ ] Vérifier les verrous bloquants (`V$LOCKED_OBJECT`)

**Chaque semaine :**

- [ ] Sauvegarde logique complète (Export)
- [ ] Vérification intégrité fichiers (RMAN VALIDATE)
- [ ] Analyse des performances tendances (AWR Reports)
- [ ] Révision des alertes et incidents (OEM)
- [ ] Nettoyage des fichiers de traces anciens
- [ ] Collecte des statistiques (`DBMS_STATS`)

**Chaque mois :**

- [ ] Test de restauration complète
- [ ] Revue des comptes utilisateurs inactifs
- [ ] Audit des privilèges et rôles
- [ ] Vérification des patches de sécurité
- [ ] Archivage/purge des données obsolètes
- [ ] Documentation des changements

---

## 13. Mini-exercices pratiques

### Exercice 1 : Création utilisateur complète

```sql
-- 1. Créer un profil avec limitations
CREATE PROFILE profil_dev LIMIT
  SESSIONS_PER_USER 3
  IDLE_TIME 30
  FAILED_LOGIN_ATTEMPTS 3
  PASSWORD_LIFE_TIME 60
  PASSWORD_REUSE_TIME 365;

-- 2. Créer l'utilisateur
CREATE USER developpeur
IDENTIFIED BY Dev2025!
DEFAULT TABLESPACE utilisateurs
TEMPORARY TABLESPACE temp
QUOTA 50M ON utilisateurs
PROFILE profil_dev
ACCOUNT UNLOCK
PASSWORD EXPIRE;

-- 3. Créer un rôle applicatif
CREATE ROLE role_dev;
GRANT CREATE SESSION TO role_dev;
GRANT CREATE TABLE TO role_dev;
GRANT CREATE VIEW TO role_dev;
GRANT CREATE PROCEDURE TO role_dev;
GRANT CREATE SEQUENCE TO role_dev;

-- 4. Assigner le rôle à l'utilisateur
GRANT role_dev TO developpeur;

-- 5. Vérifier la configuration
SELECT username, account_status, default_tablespace, profile
FROM dba_users WHERE username = 'DEVELOPPEUR';

SELECT grantee, granted_role FROM dba_role_privs
WHERE grantee = 'DEVELOPPEUR';
```

### Exercice 2 : Gestion complète d'un tablespace

```sql
-- 1. Créer le tablespace
CREATE TABLESPACE donnees_app
DATAFILE '/u01/oradata/app_data01.dbf' SIZE 100M
AUTOEXTEND ON NEXT 10M MAXSIZE 500M
EXTENT MANAGEMENT LOCAL UNIFORM SIZE 128K
SEGMENT SPACE MANAGEMENT AUTO;

-- 2. Créer une table dans ce tablespace
CREATE TABLE transactions (
  id NUMBER PRIMARY KEY,
  date_transaction DATE,
  montant NUMBER(10,2),
  description VARCHAR2(200)
) TABLESPACE donnees_app;

-- 3. Vérifier l'espace utilisé et disponible
SELECT tablespace_name, 
       SUM(bytes)/1024/1024 AS total_mb
FROM dba_data_files
WHERE tablespace_name = 'DONNEES_APP'
GROUP BY tablespace_name;

SELECT tablespace_name,
       SUM(bytes)/1024/1024 AS free_mb
FROM dba_free_space
WHERE tablespace_name = 'DONNEES_APP'
GROUP BY tablespace_name;

-- 4. Ajouter un datafile
ALTER TABLESPACE donnees_app
ADD DATAFILE '/u01/oradata/app_data02.dbf' SIZE 100M
AUTOEXTEND ON NEXT 10M MAXSIZE 500M;

-- 5. Mettre en lecture seule (avant sauvegarde par exemple)
ALTER TABLESPACE donnees_app READ ONLY;

-- 6. Remettre en lecture/écriture
ALTER TABLESPACE donnees_app READ WRITE;
```

### Exercice 3 : Sauvegarde et restauration d'un schéma

```bash
# 1. Export complet du schéma
exp system/manager OWNER=application FILE=app_backup.dmp LOG=app_export.log STATISTICS=NONE

# 2. Créer un nouveau schéma cible
sqlplus system/manager
CREATE USER application_test IDENTIFIED BY Test123 
DEFAULT TABLESPACE users QUOTA UNLIMITED ON users;
GRANT CONNECT, RESOURCE TO application_test;

# 3. Importer dans le nouveau schéma
imp system/manager FILE=app_backup.dmp FROMUSER=application TOUSER=application_test LOG=app_import.log

# 4. Vérifier les objets importés
sqlplus system/manager
SELECT object_type, COUNT(*) 
FROM dba_objects 
WHERE owner = 'APPLICATION_TEST'
GROUP BY object_type
ORDER BY object_type;
```

---

## 14. QCM Final - Récapitulatif

**Q1: Quels sont les 5 processus d'arrière-plan obligatoires d'Oracle ?**

- a) PMON, SMON, LGWR, DBWR, CKPT ✓
- b) PMON, SMON, ARCn, RECO, LCKn
- c) LGWR, DBWR, ARCn, SNPn, Pnnn
- d) Tous les processus sont optionnels

**Q2: Quel mode d'archivage est requis pour une restauration point-in-time ?**

- a) NOARCHIVELOG
- b) ARCHIVELOG ✓
- c) READ ONLY
- d) MOUNT

**Q3: Quel tablespace système contient le dictionnaire de données Oracle ?**

- a) USERS
- b) SYSTEM ✓
- c) SYSAUX
- d) TEMP

**Q4: Quel est le privilège système minimum requis pour se connecter à Oracle ?**

- a) CONNECT (rôle obsolète)
- b) CREATE SESSION ✓
- c) DBA
- d) RESOURCE

**Q5: Quelle commande permet un arrêt propre et rapide de la base ?**

- a) SHUTDOWN NORMAL
- b) SHUTDOWN IMMEDIATE ✓
- c) SHUTDOWN ABORT
- d) STOP DATABASE

**Q6: Configuration minimale de fichiers recommandée pour une base Oracle ?**

- a) 1 datafile, 1 redo log, 1 control file
- b) 1+ datafiles, 2+ redo log groups, 3+ control files ✓
- c) 1 datafile, 3 redo logs, 1 control file
- d) Multiples datafiles, 1 redo log, 1 control file

**Q7: Que sauvegarde un export FULL ?**

- a) Toutes les tables uniquement
- b) Tous les objets sauf le schéma SYS ✓
- c) Tous les objets incluant SYS
- d) Seulement les données, pas les structures

**Q8: Comment agrandir un tablespace saturé ?**

- a) Impossible sans redémarrage
- b) ALTER TABLESPACE ADD DATAFILE ou RESIZE ✓
- c) DROP puis CREATE uniquement
- d) RESIZE du tablespace directement

**Q9: Quelle vue dynamique affiche les paramètres d'initialisation ?**

- a) DBA_PARAMETERS
- b) V$PARAMETER ✓
- c) INIT_PARAMETERS
- d) SYS.PARAMETERS

**Q10: Un rôle Oracle peut contenir :**

- a) Seulement des privilèges système
- b) Seulement des privilèges objet
- c) Privilèges système, privilèges objet et autres rôles ✓
- d) Aucun privilège directement

---

## 15. Conclusion et ressources

### Points clés à retenir

**Architecture :**

- Une **instance** Oracle = SGA (mémoire) + processus d'arrière-plan
- Une **base de données** Oracle = fichiers physiques (contrôle, données, redo logs)
- 5 processus obligatoires : **PMON, SMON, LGWR, DBWR, CKPT**

**Sécurité :**

- Hiérarchie : **Utilisateurs → Profils → Rôles → Privilèges**
- Toujours créer des rôles personnalisés
- Appliquer le principe du moindre privilège

**Sauvegarde :**

- **Mode ARCHIVELOG** essentiel en production
- **Multiplexer** fichiers de contrôle et Redo Logs
- Tester régulièrement les restaurations
- Combiner sauvegardes physiques et logiques

**Performance :**

- Surveiller quotidiennement alert.log et vues V$
- Séparer données et index (tablespaces différents)
- Collecter régulièrement les statistiques
- Utiliser AWR et ADDM pour analyses

**Administration :**

- SHUTDOWN IMMEDIATE pour arrêt propre
- Documenter toute modification
- Planifier la croissance des tablespaces
- Automatiser les tâches répétitives

### Prochaines étapes

1. **Pratiquer sur un environnement de test**
    
    - Installer Oracle Database Express Edition (gratuit)
    - Créer et configurer une base avec DBCA
    - Tester toutes les commandes de ce guide
2. **Approfondir vos connaissances**
    
    - Tuning et optimisation de performances
    - Oracle Real Application Clusters (RAC)
    - Oracle Data Guard pour haute disponibilité
    - RMAN avancé (sauvegardes incrémentales, block change tracking)
3. **Certifications Oracle**
    
    - Oracle Certified Associate (OCA)
    - Oracle Certified Professional (OCP)
    - Oracle Certified Master (OCM)

### Ressources officielles

**Documentation Oracle :**

- Oracle Database Administrator's Guide
- Oracle Database SQL Language Reference
- Oracle Database Backup and Recovery User's Guide
- Oracle Database Performance Tuning Guide

**Vues système essentielles à connaître :**

```sql
-- Dictionnaire de données complet
SELECT * FROM DICTIONARY;
SELECT * FROM DICT_COLUMNS WHERE table_name = 'DBA_TABLES';

-- Vues statiques les plus utilisées
DBA_USERS, DBA_TABLES, DBA_INDEXES, DBA_CONSTRAINTS
DBA_TABLESPACES, DBA_DATA_FILES, DBA_FREE_SPACE, DBA_TEMP_FILES
DBA_ROLES, DBA_SYS_PRIVS, DBA_TAB_PRIVS, DBA_ROLE_PRIVS
DBA_SEGMENTS, DBA_EXTENTS, DBA_OBJECTS

-- Vues dynamiques les plus utilisées
V$DATABASE, V$INSTANCE, V$SESSION, V$PROCESS
V$PARAMETER, V$SGA, V$PGASTAT, V$SYSSTAT
V$LOG, V$LOGFILE, V$ARCHIVED_LOG
V$DATAFILE, V$TEMPFILE, V$CONTROLFILE
V$SQL, V$SQLAREA, V$SQL_PLAN
```

---

**Document préparé pour l'étude - Administration Oracle Database**

**Versions couvertes :** Oracle Database 10g / 11g / 12c

**Bonne chance pour vos examens et votre carrière d'administrateur Oracle DBA !**