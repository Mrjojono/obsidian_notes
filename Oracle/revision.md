# 📚 Résumé du Cours - Administration Oracle Database

---

## 📋 Table des matières

1. [Introduction à Oracle](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#1-introduction-%C3%A0-oracle)
2. [Installation et produits Oracle](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#2-installation-et-produits-oracle)
3. [Gestion des instances Oracle](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#3-gestion-des-instances-oracle)
4. [Architecture de la base de données](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#4-architecture-de-la-base-de-donn%C3%A9es)
5. [Gestion des utilisateurs et sécurité](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#5-gestion-des-utilisateurs-et-s%C3%A9curit%C3%A9)
6. [Gestion des objets (Tables, Vues, Index...)](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#6-gestion-des-objets)
7. [Sauvegarde et restauration](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#7-sauvegarde-et-restauration)
8. [Résolution de problèmes](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#8-r%C3%A9solution-de-probl%C3%A8mes)

---

## 1. Introduction à Oracle

### 🎯 Points clés

- **Oracle Corporation** : Premier SGBDR au monde (1979)
- **Leader mondial** des SGBD
- **SGBD réparti** orienté Web

### 📊 Éditions Oracle

|Édition|Caractéristiques|Usage|
|---|---|---|
|**Enterprise**|Toutes fonctionnalités|Grandes entreprises, données volumineuses|
|**Standard**|Fonctionnalités de base|Max 4 processeurs|
|**Standard One**|Identique Standard|Max 2 processeurs|
|**Personal**|Windows uniquement|Développeurs mono-utilisateur|
|**Express**|Gratuite|Petites entreprises, académique|
|**Lite**|Applications mobiles|Déploiement mobile|

### ✅ QCM #1

**Q1: Quelle est la première version d'Oracle ?**

- a) 1975
- b) 1977 (prototype)
- c) 1979 (version 1) ✓
- d) 1980

**Q2: Quelle édition est gratuite ?**

- a) Personal Edition
- b) Express Edition ✓
- c) Lite Edition
- d) Standard Edition

---

## 2. Installation et produits Oracle

### 🔧 Configuration système requise

```
RAM         : 512 Mo minimum
SWAP        : 1 Go (ou 2x RAM)
Espace /tmp : 400 Mo
Logiciel    : 1,5 Go
Base        : 1,5 Go
```

### 🗂️ Variables d'environnement essentielles

|Variable|Description|Exemple|
|---|---|---|
|`ORACLE_HOME`|Emplacement du noyau Oracle|`C:\oracle\product\10.2.0\db_1`|
|`ORACLE_BASE`|Emplacement des bases|`C:\oracle\product`|
|`ORACLE_SID`|Nom de l'instance|`ORCL`|
|`NLS_LANG`|Langage du système|`FRENCH_FRANCE.WE8MSWIN1252`|

### 📝 Produits Oracle clés

- **Oracle Server** : Gestionnaire de la BD
- **SQL & PL/SQL** : Langages de requêtes
- **Oracle Designer** : Conception d'applications
- **Oracle Developer** : Développement client/serveur
- **Oracle Discoverer** : Interrogation/Datawarehouse

### ✅ QCM #2

**Q1: Quelle variable désigne l'instance active ?**

- a) ORACLE_HOME
- b) ORACLE_BASE
- c) ORACLE_SID ✓
- d) NLS_LANG

**Q2: Quelle est la RAM minimum requise ?**

- a) 256 Mo
- b) 512 Mo ✓
- c) 1 Go
- d) 2 Go

---

## 3. Gestion des instances Oracle

### 🧠 Structure mémoire SGA (System Global Area)

```
SGA
├── Database Buffer Cache (DB_CACHE_SIZE)
├── Redo Log Buffer (LOG_BUFFER)
├── Shared Pool (SHARED_POOL_SIZE)
│   ├── Library Cache
│   └── Dictionary Cache
├── Large Pool (LARGE_POOL_SIZE)
├── Java Pool (JAVA_POOL_SIZE)
└── Streams Pool (STREAMS_POOL_SIZE)
```

### ⚙️ Processus essentiels

|Processus|Rôle|Criticité|
|---|---|---|
|**PMON**|Process Monitor - Nettoie connexions anormales|Obligatoire ✓|
|**SMON**|System Monitor - Restauration instance|Obligatoire ✓|
|**LGWR**|Log Writer - Écrit dans Redo Logs|Obligatoire ✓|
|**DBWR**|Database Writer - Écrit dans fichiers données|Obligatoire ✓|
|**CKPT**|Checkpoint - Met à jour état BD|Obligatoire ✓|
|**ARCn**|Archiver - Copie Redo Logs|Optionnel|

### 🔐 Connexion et démarrage

#### Connexion classique

```sql
SQLPLUS /nolog
SQL> CONNECT user_name/password@service_name
```

#### Connexion privilégiée

```sql
SET ORACLE_SID=Orcl
sqlplus /nolog
SQL> CONNECT / AS SYSDBA
```

### 🚀 Étapes de démarrage

```
SHUTDOWN → NOMOUNT → MOUNT → OPEN
```

1. **NOMOUNT** : Instance démarrée, SGA allouée
2. **MOUNT** : Fichiers de contrôle lus
3. **OPEN** : Fichiers de données et Redo Logs ouverts

#### Commandes

```sql
STARTUP NOMOUNT
STARTUP MOUNT
STARTUP OPEN  -- ou simplement STARTUP
```

### 🛑 Modes d'arrêt

|Mode|Description|Rollback|Checkpoint|
|---|---|---|---|
|**NORMAL**|Attend fin sessions|Oui|Oui|
|**IMMEDIATE**|Force déconnexion|Oui|Oui|
|**TRANSACTIONAL**|Attend fin transactions|Oui|Oui|
|**ABORT**|Arrêt brutal|Non|Non|

```sql
SHUTDOWN IMMEDIATE
```

### 📊 Vues importantes

```sql
-- Vues statiques (base ouverte)
USER_*     -- Objets de l'utilisateur
ALL_*      -- Objets accessibles
DBA_*      -- Tous objets (admin)

-- Vues dynamiques (même en MOUNT)
V$INSTANCE
V$DATABASE
V$SGA
V$PARAMETER
```

### ✅ QCM #3

**Q1: Quel processus écrit dans les Redo Logs ?**

- a) DBWR
- b) LGWR ✓
- c) PMON
- d) SMON

**Q2: Quel mode d'arrêt est le plus propre ?**

- a) ABORT
- b) IMMEDIATE ✓
- c) NORMAL
- d) TRANSACTIONAL

**Q3: Quelle vue montre les paramètres d'initialisation ?**

- a) V$INSTANCE
- b) V$DATABASE
- c) V$PARAMETER ✓
- d) V$SGA

---

## 4. Architecture de la base de données

### 📂 Architecture physique - Les fichiers

#### 1. **Fichiers de contrôle** (Control Files)

- Minimum 3 fichiers (multiplexés)
- Cartographie de la structure physique
- Lus au démarrage, mis à jour à chaque modification

```sql
-- Créer un fichier de contrôle
ALTER DATABASE BACKUP CONTROLFILE TO TRACE;
```

#### 2. **Fichiers Redo Log**

- Enregistrent toutes les modifications (transactions)
- Mode circulaire, minimum 2 groupes
- **Multiplexage recommandé** (2+ membres/groupe, disques différents)

```sql
-- Ajouter un groupe
ALTER DATABASE ADD LOGFILE 
  ('disk5/log31.rdo', 'disk6/log32.rdo') SIZE 10M;

-- Ajouter un membre
ALTER DATABASE ADD LOGFILE MEMBER
  'disk6/log12.rdo' TO GROUP 1;

-- Forcer un switch
ALTER SYSTEM SWITCH LOGFILE;
```

#### 3. **Fichiers de données** (Datafiles)

- Contiennent les données (tables, index...)
- Organisés en tablespaces
- Liés aux tablespaces (voir section suivante)

#### 4. **Autres fichiers importants**

|Fichier|Description|
|---|---|
|**init.ora / spfile.ora**|Paramètres d'initialisation|
|**Alert.log**|Journal chronologique (erreurs, opérations admin)|
|**Fichiers traces**|Dysfonctionnements processus|
|**Fichier mot de passe**|Authentification SYSDBA/SYSOPER|
|**Archives Redo Log**|Copies offline (mode ARCHIVELOG)|

### 🏗️ Architecture logique

```
BASE DE DONNÉES
│
├── TABLESPACES
│   ├── SYSTEM (dictionnaire)
│   ├── SYSAUX (outils admin)
│   ├── USERS (données utilisateurs)
│   ├── TEMP (tri temporaire)
│   └── UNDO (annulation)
│
├── SEGMENTS
│   ├── Table
│   ├── Index
│   ├── Rollback
│   └── Temporary
│
├── EXTENTS
│   └── Ensemble de blocs contigus
│
└── BLOCS ORACLE
    └── Plus petite unité (DB_BLOCK_SIZE)
```

### 📦 Gestion des Tablespaces

#### Création Tablespace permanent

```sql
CREATE TABLESPACE data_01
DATAFILE '/disk4/data_01.dbf' SIZE 500M,
         '/disk5/data_02.dbf' SIZE 500M
MINIMUM EXTENT 100K
DEFAULT STORAGE (
  INITIAL 64K 
  NEXT 64K 
  MAXEXTENTS 500 
  PCTINCREASE 50
);
```

#### Création Tablespace temporaire

```sql
CREATE TEMPORARY TABLESPACE temp_01
TEMPFILE 'filespec' SIZE 200M
AUTOEXTEND ON;
```

#### Création Tablespace UNDO

```sql
CREATE UNDO TABLESPACE undotbs1
DATAFILE 'chemin/fichier' SIZE 100M
AUTOEXTEND ON
RETENTION GUARANTEE;  -- Garantir conservation

-- Activer
ALTER SYSTEM SET UNDO_TABLESPACE = undotbs1;
```

#### Opérations sur Tablespaces

```sql
-- Ajouter un datafile
ALTER TABLESPACE data_01 
ADD DATAFILE '/disk7/data_03.dbf' SIZE 1000M;

-- Redimensionner
ALTER DATABASE DATAFILE '/disk7/data_03.dbf' RESIZE 1500M;

-- Autoextend
ALTER DATABASE DATAFILE '/disk7/data_03.dbf'
AUTOEXTEND ON MAXSIZE 1500M;

-- Statuts
ALTER TABLESPACE data_01 ONLINE;
ALTER TABLESPACE data_01 OFFLINE;
ALTER TABLESPACE data_01 READ ONLY;
ALTER TABLESPACE data_01 READ WRITE;

-- Supprimer
DROP TABLESPACE data_01 INCLUDING CONTENTS CASCADE CONSTRAINTS;
```

### 🔄 Modes d'archivage

|Mode|Description|Usage|
|---|---|---|
|**NOARCHIVELOG**|Redo Logs écrasés (défaut)|Développement|
|**ARCHIVELOG**|Redo Logs archivés|Production ✓|

```sql
-- Passer en mode ARCHIVELOG
STARTUP MOUNT
ALTER DATABASE ARCHIVELOG;
ALTER DATABASE OPEN;

-- Vérifier
ARCHIVE LOG LIST;
```

### ✅ QCM #4

**Q1: Nombre minimum de fichiers de contrôle recommandé ?**

- a) 1
- b) 2
- c) 3 ✓
- d) 5

**Q2: Quel tablespace contient le dictionnaire de données ?**

- a) USERS
- b) SYSTEM ✓
- c) SYSAUX
- d) TEMP

**Q3: Quelle est la plus petite unité de stockage ?**

- a) Segment
- b) Extent
- c) Bloc Oracle ✓
- d) Tablespace

---

## 5. Gestion des utilisateurs et sécurité

### 👤 Création d'utilisateurs

```sql
CREATE USER helyos
IDENTIFIED BY mypass1
DEFAULT TABLESPACE tbs_users
QUOTA 10M ON tbs_users
TEMPORARY TABLESPACE tmp_users
QUOTA 5M ON tmp_users
PROFILE app_user
PASSWORD EXPIRE;
```

### 🔑 Authentification

|Méthode|Syntaxe|
|---|---|
|**Par la BD**|`IDENTIFIED BY password`|
|**Externe (OS)**|`IDENTIFIED EXTERNALLY`|
|**Globale (LDAP)**|`IDENTIFIED GLOBALLY AS 'nom'`|

### 🛡️ Profils (Limitations ressources)

```sql
CREATE PROFILE app_user LIMIT
  SESSIONS_PER_USER UNLIMITED
  CPU_PER_SESSION UNLIMITED
  CPU_PER_CALL 3000
  CONNECT_TIME 45
  IDLE_TIME 10
  FAILED_LOGIN_ATTEMPTS 5
  PASSWORD_LIFE_TIME 90
  PASSWORD_LOCK_TIME 1;
  
-- Assigner
ALTER USER bob PROFILE app_user;
```

#### Paramètres de profil importants

|Paramètre|Description|
|---|---|
|`FAILED_LOGIN_ATTEMPTS`|Tentatives avant verrouillage|
|`PASSWORD_LIFE_TIME`|Durée validité mot de passe (jours)|
|`PASSWORD_LOCK_TIME`|Durée verrouillage compte|
|`PASSWORD_REUSE_TIME`|Délai réutilisation même mot de passe|
|`SESSIONS_PER_USER`|Sessions max simultanées|
|`CPU_PER_SESSION`|CPU max par session (centièmes sec)|
|`IDLE_TIME`|Temps inactivité max (minutes)|

### 🔐 Privilèges

#### Deux types

1. **Privilèges système** : Opérations sur la BD
2. **Privilèges objet** : Actions sur objets spécifiques

```sql
-- Privilèges système
GRANT CREATE SESSION TO user1;
GRANT CREATE TABLE TO user1;
GRANT CREATE VIEW TO user1;

-- Privilèges objet
GRANT SELECT, INSERT, UPDATE, DELETE 
ON scott.emp 
TO user1;

-- Avec possibilité de délégation
GRANT SELECT ON scott.emp 
TO user1 
WITH GRANT OPTION;
```

### 👥 Rôles

```sql
-- Créer un rôle
CREATE ROLE comptabilite;

-- Assigner privilèges au rôle
GRANT SELECT, INSERT, UPDATE, DELETE 
ON cpt.facture 
TO comptabilite;

-- Assigner rôle à utilisateur
GRANT comptabilite TO user1;
```

#### Rôles standards Oracle

|Rôle|Usage|
|---|---|
|**CONNECT**|Connexion basique|
|**RESOURCE**|Création objets|
|**DBA**|Administration complète|

⚠️ **Recommandation** : Créer des rôles personnalisés plutôt qu'utiliser les standards.

### 🗑️ Révoquer privilèges

```sql
-- Révoquer privilège système
REVOKE CREATE TABLE FROM user1;

-- Révoquer privilège objet
REVOKE SELECT ON scott.emp FROM user1;
```

### 📊 Vues de sécurité

```sql
-- Utilisateurs
SELECT * FROM DBA_USERS;
SELECT * FROM DBA_TS_QUOTAS;

-- Profils
SELECT * FROM DBA_PROFILES;

-- Rôles
SELECT * FROM DBA_ROLES;
SELECT * FROM DBA_ROLE_PRIVS;

-- Privilèges
SELECT * FROM DBA_SYS_PRIVS;    -- Système
SELECT * FROM DBA_TAB_PRIVS;    -- Objets
SELECT * FROM DBA_COL_PRIVS;    -- Colonnes
```

### ✅ QCM #5

**Q1: Quelle commande crée un utilisateur ?**

- a) ADD USER
- b) CREATE USER ✓
- c) NEW USER
- d) INSERT USER

**Q2: Quel privilège permet de se connecter ?**

- a) CREATE SESSION ✓
- b) CONNECT
- c) LOGIN
- d) ACCESS

**Q3: Un rôle peut contenir :**

- a) Uniquement privilèges système
- b) Uniquement privilèges objet
- c) Les deux ✓
- d) Aucun des deux

---

## 6. Gestion des objets

### 📊 Les Tables

#### Types de données courants

|Type|Description|Exemple|
|---|---|---|
|`CHAR(n)`|Chaîne fixe (max 2000)|`CHAR(10)`|
|`VARCHAR2(n)`|Chaîne variable (max 4000)|`VARCHAR2(50)`|
|`NUMBER(p,s)`|Numérique|`NUMBER(10,2)`|
|`DATE`|Date+heure|`01-JAN-2025 14:30:00`|
|`TIMESTAMP`|Date+heure précise|Avec microsecondes|
|`CLOB`|Texte long|Jusqu'à 4 Go|
|`BLOB`|Binaire long|Jusqu'à 4 Go|

#### Création table

```sql
CREATE TABLE emp1 (
  empno NUMBER(5) PRIMARY KEY,
  ename VARCHAR2(15) NOT NULL,
  job VARCHAR2(10),
  mgr NUMBER(5),
  hiredate DATE DEFAULT SYSDATE,
  sal NUMBER(7,2),
  comm NUMBER(7,2),
  deptno NUMBER(3) NOT NULL
    CONSTRAINT dept_fkey REFERENCES dept
)
PCTFREE 10
PCTUSED 40
TABLESPACE users
STORAGE (
  INITIAL 50K
  NEXT 50K
  MAXEXTENTS 10
  PCTINCREASE 25
);
```

#### Création avec données existantes

```sql
CREATE TABLE emp_backup
AS SELECT * FROM emp;
```

#### Modification table

```sql
-- Ajouter colonne
ALTER TABLE emp1
ADD (sexe CHAR(1) DEFAULT 'M' NOT NULL);

-- Modifier colonne
ALTER TABLE emp1
MODIFY (sal NUMBER(8,2), ename NOT NULL);

-- Supprimer contrainte
ALTER TABLE emp1
DROP CONSTRAINT dept_fkey;
```

#### Suppression/Renommage

```sql
DROP TABLE emp1;
RENAME emp TO salarie;
```

### 🔍 Les Index

```sql
-- Créer index
CREATE INDEX ideptno ON emp(deptno);

-- Index composé
CREATE INDEX emp_idx ON emp(deptno, job)
PCTFREE 10
TABLESPACE idx
STORAGE (INITIAL 64K NEXT 64K);

-- Reconstruire index
ALTER INDEX emp_idx REBUILD TABLESPACE idx_02;

-- Supprimer index
DROP INDEX ideptno;
```

**💡 Quand indexer ?**

- ✅ Clés primaires
- ✅ Clés étrangères
- ✅ Colonnes souvent dans WHERE
- ❌ Colonnes souvent modifiées
- ❌ Tables très petites

### 👁️ Les Vues

```sql
-- Créer vue
CREATE VIEW dept_10
AS SELECT empno, ename, job
FROM emp
WHERE deptno = 10;

-- Avec vérification
CREATE VIEW dept_10
AS SELECT empno, ename, job
FROM emp
WHERE deptno = 10
WITH CHECK OPTION;

-- Modifier vue
CREATE OR REPLACE VIEW dept_10
AS SELECT empno, ename, job, sal
FROM emp
WHERE deptno = 10;

-- Supprimer vue
DROP VIEW dept_10;
```

**Limitations mises à jour vues :**

- ❌ Avec jointure
- ❌ Avec GROUP BY / DISTINCT
- ❌ Avec ROWNUM
- ✅ Vue simple (1 table, sans fonctions groupées)

### 🔢 Les Séquences

```sql
-- Créer séquence
CREATE SEQUENCE eseq
INCREMENT BY 10
START WITH 5000
MAXVALUE 999999
MINVALUE 1
NOCYCLE
CACHE 20;

-- Utiliser séquence
INSERT INTO emp (empno, ename, deptno)
VALUES (eseq.NEXTVAL, 'MARTIN', 40);

-- Valeur courante
SELECT eseq.CURRVAL FROM dual;

-- Modifier séquence
ALTER SEQUENCE eseq INCREMENT BY 5;

-- Supprimer séquence
DROP SEQUENCE eseq;
```

### 🏷️ Les Synonymes

```sql
-- Synonyme privé
CREATE SYNONYM emp FOR scott.emp;

-- Synonyme public
CREATE PUBLIC SYNONYM employees
FOR hr.employees@sales;

-- Supprimer synonyme
DROP SYNONYM emp;
```

### 🔗 Database Links

```sql
-- Créer DB Link
CREATE DATABASE LINK sales.hq.acme.com
CONNECT TO hr IDENTIFIED BY hr
USING 'sales';

-- Utiliser DB Link
SELECT * FROM employees@sales.hq.acme.com;

INSERT INTO orders@sales.hq.acme.com
(customer_id, order_id, order_total)
VALUES (5001, 1235, 2000);
```

### ✅ QCM #6

**Q1: Quel type pour une chaîne variable ?**

- a) CHAR
- b) VARCHAR ✓
- c) VARCHAR2 ✓
- d) STRING

**Q2: Pseudo-colonnes de séquence ?**

- a) CURRENT et NEXT
- b) CURRVAL et NEXTVAL ✓
- c) NOW et NEXT
- d) VALUE et NEXTVALUE

**Q3: Une vue peut-elle être mise à jour ?**

- a) Jamais
- b) Toujours
- c) Selon conditions ✓
- d) Uniquement par DBA

---

## 7. Sauvegarde et restauration

### 💾 Types de sauvegarde

```
SAUVEGARDES
├── PHYSIQUES (fichiers OS)
│   ├── À FROID (base fermée)
│   └── À CHAUD (base ouverte)
│
└── LOGIQUES (export/import)
    ├── FULL (toute la base)
    ├── SCHEMA (utilisateur)
    └── TABLE (objets spécifiques)
```

### ❄️ Sauvegarde physique à FROID

```sql
-- 1. Arrêter la base
SHUTDOWN IMMEDIATE;

-- 2. Copier TOUS les fichiers
-- - Fichiers de données (.dbf)
-- - Fichiers de contrôle
-- - Redo logs
-- - init.ora / spfile.ora

-- 3. Redémarrer
STARTUP;
```

### 🔥 Sauvegarde physique à CHAUD

#### Prérequis : Mode ARCHIVELOG

```sql
-- Vérifier mode
ARCHIVE LOG LIST;

-- Tablespace offline
ALTER TABLESPACE users OFFLINE NORMAL;
-- Copier fichiers
ALTER TABLESPACE users ONLINE;

-- Tablespace online
ALTER TABLESPACE users BEGIN BACKUP;
-- Copier fichiers (commandes OS)
ALTER TABLESPACE users END BACKUP;
```

### 📤 Export / Import

#### Modes export

|Mode|Paramètre|Description|
|---|---|---|
|**Full**|`FULL=Y`|Toute la base|
|**Schema**|`OWNER=user`|Objets d'un utilisateur|
|**Table**|`TABLES=table`|Tables spécifiques|
|**Tablespace**|`TABLESPACES=tbs`|Objets d'un tablespace|

#### Exemples Export

```bash
# Export FULL
exp system/manager FILE=full.dmp LOG=full.log FULL=Y

# Export schéma
exp system/manager FILE=scott.dmp LOG=scott.log OWNER=scott

# Export table
exp system/manager FILE=emp.dmp LOG=emp.log TABLES=scott.emp

# Avec fichier paramètres
exp PARFILE=params.txt
```

**Fichier params.txt :**

```
userid=system/manager
file=export.dmp
log=export.log
owner=scott
rows=y
```

#### Exemples Import

```bash
# Import FULL
imp system/manager FILE=full.dmp FULL=Y LOG=import.log

# Import schéma
imp scott/tiger FILE=scott.dmp LOG=import.log

# Import schéma dans autre schéma
imp system/manager FILE=scott.dmp LOG=import.log 
  FROMUSER=scott TOUSER=test

# Import table
imp system/manager FILE=emp.dmp LOG=import.log 
  FROMUSER=scott TOUSER=test TABLES=emp
```

#### Paramètres importants

|Paramètre|Description|Valeur défaut|
|---|---|---|
|`USERID`|Connexion|-|
|`FILE`|Fichier dump|expdat.dmp|
|`LOG`|Fichier log|-|
|`FULL`|Export complet|N|
|`OWNER`|Utilisateur(s)|-|
|`TABLES`|Table(s)|-|
|`ROWS`|Données|Y|
|`CONSISTENT`|Read-only session|N|
|`IGNORE`|Ignorer erreurs|N|
|`COMMIT`|Commits fréquents|N|

### 📥 SQL*Loader

Chargement de fichiers texte dans Oracle.

```sql
-- Créer table cible
CREATE TABLE matable (
  numero NUMBER,
  nom VARCHAR2(35),
  prenom VARCHAR2(15),
  date_naissance DATE,
  sexe CHAR(1)
);
```

**Fichier de contrôle (param.ctl) :**

```
LOAD DATA INFILE 'monfic.dat'
REPLACE
INTO TABLE matable
FIELDS TERMINATED BY ','
OPTIONALLY ENCLOSED BY '"'
(NUMERO, NOM, PRENOM, 
 DATE_NAISSANCE DATE "DDMMYY", 
 SEXE)
```

**Exécution :**

```bash
sqlldr user/password CONTROL=param.ctl LOG=load.log
```

### 🔄 Duplication de base

#### Méthode 1 : Export/Import

```bash
# 1. Export base source
exp system/manager@source FULL=Y FILE=backup.dmp

# 2. Créer base clone (même nom, paramètres)

# 3. Import
imp system/manager@clone FULL=Y IGNORE=Y FILE=backup.dmp
```

#### Méthode 2 : Sauvegarde physique

```bash
# 1. Arrêter base source
SHUTDOWN IMMEDIATE

# 2. Copier tous fichiers vers nouvelle destination

# 3. Créer instance pointant vers fichiers

# 4. Démarrer base clone
STARTUP
```

### ✅ QCM #7

**Q1: Quel mode requis pour sauvegarde à chaud ?**

- a) NOARCHIVELOG
- b) ARCHIVELOG ✓
- c) READ ONLY
- d) RESTRICTED

**Q2: Export FULL nécessite quel privilège ?**

- a) DBA
- b) EXP_FULL_DATABASE ✓
- c) SYSDBA
- d) RESOURCE

**Q3: SQL*Loader charge des données depuis :**

- a) Fichiers binaires
- b) Fichiers texte ✓
- c) Autre base Oracle
- d) Fichiers XML

---

## 8. Résolution de problèmes

### 🔴 Erreurs courantes et solutions

#### ORA-01034: Oracle not Available

**Cause :** Base non démarrée

```sql
SET ORACLE_SID=ma_base
sqlplus / as sysdba
SHUTDOWN ABORT
STARTUP
```

#### ORA-12154: TNS could not resolve service name

**Cause :** Problème résolution alias SQL*Net

✅ **Solutions :**

1. Vérifier `TNSNAMES.ORA`
2. Vérifier nom serveur DNS
3. Vérifier port listener (1521)
4. Vérifier SID

```bash
# Tester listener
lsnrctl status
```

#### ORA-01653: unable to extend table

**Cause :** Tablespace saturé

```sql
-- Solution 1: Autoextend
ALTER DATABASE DATAFILE 'fichier.dbf'
AUTOEXTEND ON MAXSIZE 1500M;

-- Solution 2: Resize
ALTER DATABASE DATAFILE 'fichier.dbf' 
RESIZE 1500M;

-- Solution 3: Ajouter fichier
ALTER TABLESPACE data_01
ADD DATAFILE 'nouveau.dbf' SIZE 500M;
```

#### ORA-01652: unable to extend temp segment

**Cause :** Segment temporaire trop petit

```sql
-- Solution 1: Augmenter SORT_AREA_SIZE
-- Dans init.ora, puis redémarrer

-- Solution 2: Agrandir TEMP
ALTER TABLESPACE temp
ADD TEMPFILE 'temp02.dbf' SIZE 200M;
```

#### ORA-01650: unable to extend rollback segment

**Cause :** Rollback segment saturé

```sql
-- Solution 1: COMMITs fréquents
COMMIT;

-- Solution 2: Utiliser gros RBS
SET TRANSACTION USE ROLLBACK SEGMENT rbs_large;

-- Pour IMPORT
imp ... COMMIT=Y
```

#### ORA-00942: table or view does not exist

**Causes possibles :**

1. Table n'existe pas
2. Table existe mais pas de droits
3. Table d'un autre schéma

```sql
-- Vérifier existence
SELECT table_name FROM USER_TABLES;

-- Préfixer par schéma
SELECT * FROM proprietaire.table;

-- Créer synonyme
CREATE SYNONYM table FOR proprietaire.table;

-- Demander droits
GRANT SELECT ON proprietaire.table TO user;
```

#### ORA-01031: insufficient privileges

**Cause :** Droits insuffisants

```sql
-- Donner droit + délégation
GRANT SELECT ON table TO user 
WITH GRANT OPTION;
```

### 🔍 Vérifications utiles

```sql
-- Fichiers de données
SELECT file_name, tablespace_name, bytes/1024/1024 MB
FROM DBA_DATA_FILES;

-- Espace libre
SELECT tablespace_name, 
       SUM(bytes)/1024/1024 MB_FREE
FROM DBA_FREE_SPACE
GROUP BY tablespace_name;

-- Sessions actives
SELECT username, status, program
FROM V$SESSION
WHERE username IS NOT NULL;

-- Processus
SELECT program, pga_used_mem/1024/1024 MB_PGA
FROM V$PROCESS;

-- Redo Logs
SELECT group#, sequence#, bytes/1024/1024 MB, status
FROM V$LOG;

-- Paramètres instance
SELECT name, value
FROM V$PARAMETER
WHERE name LIKE '%archive%';
```

### ✅ QCM #8

**Q1: ORA-01034 signifie ?**

- a) Base verrouillée
- b) Base non disponible ✓
- c) Tablespace plein
- d) Mot de passe incorrect

**Q2: Pour agrandir un tablespace saturé ?**

- a) DROP puis CREATE
- b) ALTER TABLESPACE ADD DATAFILE ✓
- c) Impossible
- d) Redémarrer base

**Q3: Comment voir l'espace libre ?**

- a) V$TABLESPACE
- b) DBA_FREE_SPACE ✓
- c) USER_SPACE
- d) V$DATAFILE

---

## 📚 Points supplémentaires à retenir

### 🛠️ Outils d'administration

|Outil|Usage|
|---|---|
|**DBCA**|Database Configuration Assistant - Créer/modifier/supprimer BD|
|**OEM**|Oracle Enterprise Manager - Administration graphique|
|**SQL*Plus**|Interface ligne commande SQL|
|**Net Configuration Assistant**|Configurer listener et services réseau|
|**Net Manager**|Gérer configuration réseau Oracle|

### 🌐 Oracle Net (SQL*Net)

#### Listener

```bash
# Démarrer
lsnrctl start

# Arrêter
lsnrctl stop

# Statut
lsnrctl status
```

#### Service réseau (tnsnames.ora)

```
TEST =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521))
    (CONNECT_DATA =
      (SERVICE_NAME = test)
    )
  )
```

### 🔒 Sécurité - Piliers

|Pilier|Description|
|---|---|
|**Confidentialité**|Contrôle d'accès granulaire|
|**Disponibilité**|Haute disponibilité (HA)|
|**Intégrité**|Contraintes, transactions ACID|
|**Fiabilité**|Sauvegardes, journalisation|
|**Traçabilité**|Audit, logs|
|**Maintenabilité**|Facilité maintenance/reprise|

### 🎯 Bonnes pratiques

✅ **À FAIRE :**

- Multiplexer fichiers critiques (control, redo)
- Utiliser mode ARCHIVELOG en production
- Sauvegardes régulières (physiques + logiques)
- Créer rôles personnalisés
- Surveiller fichiers alert.log
- Définir quotas utilisateurs
- Utiliser profils de ressources
- Documenter architecture

❌ **À ÉVITER :**

- SHUTDOWN ABORT (sauf urgence)
- Rôles DBA/RESOURCE à utilisateurs
- Mot de passe par défaut (change_on_install)
- Tablespace SYSTEM pour données utilisateur
- Mélanger données et index même tablespace
- PCTINCREASE élevé (fragmentation)

### 📊 Formules utiles

```
Taille SGA = DB_CACHE_SIZE + SHARED_POOL_SIZE + 
             LARGE_POOL_SIZE + JAVA_POOL_SIZE + 
             STREAMS_POOL_SIZE + LOG_BUFFER

Taille segment = INITIAL + NEXT * (1 + PCTINCREASE/100)^(n-2)

Espace tablespace = Σ (taille datafiles)

Taux hit cache = (1 - physical reads / logical reads) * 100
```

---

## 🎓 Mini-exercices pratiques

### Exercice 1: Création utilisateur complet

```sql
-- 1. Créer profil
CREATE PROFILE dev_profile LIMIT
  SESSIONS_PER_USER 3
  IDLE_TIME 30
  FAILED_LOGIN_ATTEMPTS 3
  PASSWORD_LIFE_TIME 60;

-- 2. Créer utilisateur
CREATE USER dev_user
IDENTIFIED BY Dev123!
DEFAULT TABLESPACE users
TEMPORARY TABLESPACE temp
QUOTA 50M ON users
PROFILE dev_profile;

-- 3. Créer rôle
CREATE ROLE dev_role;
GRANT CREATE SESSION, CREATE TABLE, CREATE VIEW TO dev_role;

-- 4. Assigner rôle
GRANT dev_role TO dev_user;
```

### Exercice 2: Gestion tablespace

```sql
-- 1. Créer tablespace
CREATE TABLESPACE app_data
DATAFILE '/u01/app_data01.dbf' SIZE 100M
AUTOEXTEND ON NEXT 10M MAXSIZE 500M;

-- 2. Créer table dedans
CREATE TABLE test_table (
  id NUMBER PRIMARY KEY,
  data VARCHAR2(100)
) TABLESPACE app_data;

-- 3. Vérifier espace
SELECT tablespace_name, SUM(bytes)/1024/1024 MB
FROM DBA_FREE_SPACE
WHERE tablespace_name = 'APP_DATA'
GROUP BY tablespace_name;
```

### Exercice 3: Sauvegarde schéma

```bash
# Export
exp system/manager OWNER=scott FILE=scott_backup.dmp LOG=scott_exp.log

# Import dans autre schéma
imp system/manager FILE=scott_backup.dmp FROMUSER=scott TOUSER=scott_copy LOG=scott_imp.log
```

---

## 🎯 Checklist DBA quotidienne

- [ ] Vérifier alert.log
- [ ] Vérifier espace tablespaces
- [ ] Vérifier sessions actives
- [ ] Vérifier archives redo logs
- [ ] Vérifier sauvegardes
- [ ] Vérifier listener
- [ ] Analyser performances (SGA, PGA)

---

## 📖 Ressources complémentaires

### Documentation officielle

- Oracle Database Administrator's Guide
- Oracle Database SQL Reference
- Oracle Database Backup and Recovery Guide

### Vues système essentielles

```sql
-- Dictionnaire complet
SELECT * FROM DICTIONARY;
SELECT * FROM DICT_COLUMNS WHERE table_name = 'DBA_TABLES';

-- Vues courantes
DBA_USERS, DBA_TABLES, DBA_INDEXES, DBA_CONSTRAINTS
DBA_TABLESPACES, DBA_DATA_FILES, DBA_FREE_SPACE
DBA_ROLES, DBA_SYS_PRIVS, DBA_TAB_PRIVS
V$DATABASE, V$INSTANCE, V$SESSION, V$PARAMETER
```

---

## ✅ QCM Final - Récapitulatif

**Q1: Processus obligatoires Oracle (5) ?**

- a) PMON, SMON, LGWR, DBWR, CKPT ✓
- b) PMON, SMON, ARCH, RECO, LCK
- c) LGWR, DBWR, ARCn, SNPn, Pnn
- d) Tous les processus sont optionnels

**Q2: Mode requis pour restauration point-in-time ?**

- a) NOARCHIVELOG
- b) ARCHIVELOG ✓
- c) READ ONLY
- d) MOUNT

**Q3: Tablespace système contenant dictionnaire ?**

- a) USERS
- b) SYSTEM ✓
- c) SYSAUX
- d) TEMP

**Q4: Privilège minimum pour connexion ?**

- a) CONNECT
- b) CREATE SESSION ✓
- c) DBA
- d) RESOURCE

**Q5: Commande arrêt propre et rapide ?**

- a) SHUTDOWN NORMAL
- b) SHUTDOWN IMMEDIATE ✓
- c) SHUTDOWN ABORT
- d) STOP DATABASE

**Q6: Fichiers minimum pour base Oracle ?**

- a) 1 datafile, 1 redo, 1 control
- b) 1 datafile, 2 redo, 3 control ✓
- c) 1 datafile, 3 redo, 1 control
- d) Multiple datafiles, 1 redo, 1 control

**Q7: Export FULL exporte ?**

- a) Toutes tables
- b) Tous objets sauf SYS ✓
- c) Tous objets incluant SYS
- d) Seulement données

**Q8: Augmenter tablespace saturé ?**

- a) Impossible sans redémarrage
- b) ALTER TABLESPACE ADD DATAFILE ✓
- c) DROP puis CREATE
- d) RESIZE uniquement

**Q9: Vue pour voir paramètres instance ?**

- a) DBA_PARAMETERS
- b) V$PARAMETER ✓
- c) INIT_PARAMETERS
- d) SYS.PARAMETERS

**Q10: Un rôle peut contenir ?**

- a) Seulement privilèges système
- b) Seulement privilèges objet
- c) Privilèges système + objet + autres rôles ✓
- d) Aucun privilège directement

---

## 🎊 Félicitations !

Vous avez terminé ce résumé complet du cours Oracle Database Administration !

### 📌 Points clés à retenir absolument :

1. **Architecture** : Instance (SGA + Processus) + Base (Fichiers)
2. **5 processus obligatoires** : PMON, SMON, LGWR, DBWR, CKPT
3. **Mode ARCHIVELOG** : Essentiel en production
4. **Multiplexage** : Control files, Redo Logs
5. **Sauvegardes** : Physiques (chaud/froid) + Logiques (exp/imp)
6. **Sécurité** : Utilisateurs → Profils → Rôles → Privilèges
7. **Surveillance** : alert.log, vues V$, DBA_*

### 🚀 Prochaines étapes suggérées :

- Installer Oracle en environnement test
- Créer une base de données avec DBCA
- Pratiquer commandes SQL*Plus
- Tester scénarios sauvegarde/restauration
- Explorer Oracle Enterprise Manager
- Approfondir tuning performance

**Bonne chance pour vos examens et votre carrière Oracle DBA ! 💪**

---

_Document généré pour l'étude - Oracle Database Administration - Version 10g/11g_