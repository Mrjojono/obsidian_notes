# MICROSOFT SQL SERVER - ADMINISTRATION

## Résumé des Essentiels pour l'Examen

**Version:** SQL Server 2016  
**Niveau:** Administration des Bases de Données

---



## CHAPITRE 1: Présentation

### 1.1 Définition

**SQL Server** est un SGBDR (Système de Gestion de Base de Données Relationnelles) développé par Microsoft.

**Responsabilités principales:**

- Stocker les données
- Vérifier les contraintes d'intégrité
- Garantir la cohérence des données
- Assurer les relations entre les données

### 1.2 Intégration avec Windows

SQL Server exploite les services Windows :

|Composant Windows|Utilisation par SQL Server|
|---|---|
|**Observateur d'événements**|Journal des erreurs SQL Server|
|**Traitements parallèles**|Exploitation multiprocesseurs|
|**Analyseur de Performance**|Compteurs de performance|
|**Sécurité Windows**|Authentification unique (SSO)|
|**Services Windows**|Exécution des composants|
|**Active Directory**|Enregistrement des instances|

### 1.3 Types de Bases de Données

#### OLTP (Online Transactional Processing)

**Caractéristiques:**

- Stockage et réutilisation directe des données
- Serveur transactionnel
- Opérations quotidiennes (INSERT, UPDATE, DELETE)
- Gestion courante (commandes, clients, stocks)

#### OLAP (Online Analytical Processing)

**Caractéristiques:**

- Informations statistiques et agrégées
- Cubes multidimensionnels
- Serveur décisionnel (Data Warehouse)
- Aide à la décision
- S'appuie sur données OLTP

**Comparaison OLTP vs OLAP:**

|Critère|OLTP|OLAP|
|---|---|---|
|**Objectif**|Opérations quotidiennes|Analyse et décision|
|**Type d'opération**|Lectures/écritures fréquentes|Lectures massives|
|**Volume de données**|Modéré|Très important|
|**Historisation**|Courte|Longue|
|**Normalisation**|Forte (3NF)|Faible (schéma en étoile)|
|**Utilisateurs**|Nombreux|Analystes/Décideurs|

### 1.4 Composants SQL Server

#### Services Windows (Composants Serveur)

**1. SQL Server (Moteur de BD)**

- Service principal
- Nom instance par défaut: `SQLSERVER (MSSQLSERVER)`
- Nom instance nommée: `SQLSERVER (<nomInstance>)`

**2. SQL Server Agent**

- Gestion des tâches planifiées
- Surveillance et alertes

**3. Microsoft Full Text Search**

- Indexation de documents texte
- Recherches par mots-clés

#### Outils de Gestion

|Outil|Rôle|
|---|---|
|**SSMS** (SQL Server Management Studio)|Principal outil d'administration graphique|
|**Gestionnaire de Configuration**|Gestion des services SQL Server|
|**SQL Server Profiler**|Analyse de la charge de travail|

#### Autres Composants

- **SSIS** (Integration Services): Import/Export de données
- **Réplication**: Distribution des données
- **Langage R**: Statistiques pour bases décisionnelles
- **CLR**: Développement en VB.NET et C#

### 1.5 Nom Complet des Objets

```
serveur.nomBase.propriétaire.objet
```

### 1.6 Bases de Données Système

#### Bases Système Obligatoires

**1. MASTER** (La plus importante)

- Base de données principale
- Contient : comptes de connexion, options de configuration, liste des bases utilisateurs
- **À sauvegarder impérativement**

**2. MODEL**

- Template pour nouvelles bases
- Tout objet ajouté dans MODEL apparaît dans les nouvelles bases
- Contient les tables système par défaut

**3. TEMPDB**

- Espace de stockage temporaire partagé
- **Recréée à chaque démarrage**
- Contient : tables temporaires (#, ##), tables de travail, résultats de tris
- Non sauvegardable

**4. MSDB**

- Utilisée par SQL Server Agent
- Contient : tâches planifiées, alertes, opérateurs, historique

**5. RESOURCE**

- Base en lecture seule
- Contient la définition des objets système
- Objets dans le schéma `sys`
- Sauvegarde manuelle du fichier .mdf nécessaire

### 1.7 Vues Système Importantes

|Vue Système|Description|
|---|---|
|`sys.server_principals`|Liste des connexions serveur|
|`sys.messages`|Messages et avertissements|
|`sys.sysdatabases`|Liste des bases de données|
|`sys.configurations`|Options de configuration|
|`sys.columns`|Colonnes de tables et vues|
|`sys.objects`|Objets définis dans la BD courante|
|`sys.database_principals`|Utilisateurs de BD|
|`sys.database_files`|Fichiers composant la BD|

### 1.8 Procédures Stockées Système

|Procédure|Description|
|---|---|
|`sp_help [objet]`|Informations sur un objet|
|`sp_helpdb [base]`|Informations sur une base|
|`sp_helpindex [table]`|Index d'une table|
|`sp_helplogins [connexion]`|Informations sur une connexion|
|`sp_who` / `sp_who2`|Utilisateurs connectés|

### 1.9 Fonctions Système

|Fonction|Utilité|
|---|---|
|`DB_ID(nom)`|Identifiant de la base|
|`USER_NAME(ID)`|Nom d'utilisateur depuis son ID|
|`COL_LENGTH(table, colonne)`|Longueur d'une colonne|
|`STATS_DATE(table_id, stats_id)`|Date de MAJ des statistiques|
|`DATALENGTH(expression)`|Longueur d'une expression|

### 1.10 Tâches de l'Administrateur

1. Gérer les services SQL Server
2. Gérer les instances SQL Server
3. Mettre en place sauvegardes et restaurations
4. Configurer la disponibilité des données
5. Gérer les configurations réseau
6. Import/Export de données
7. Automatiser les tâches avec SQL Agent
8. Mesurer et optimiser les performances

---

## CHAPITRE 2: Installation et Configuration

### 2.1 Éditions de SQL Server

#### Comparaison des Éditions

|Édition|Caractéristiques|Usage|
|---|---|---|
|**Enterprise**|Édition complète, tous composants, 32 et 64 bits, partitionnement, compression|Production grandes entreprises, volumes très importants|
|**Standard**|Moteur performant, fonctionnalités essentielles, limitations sur volumes|PME, charges moyennes|
|**Express**|**GRATUITE**, Limite 10 Go de données, Limite 1 Go de mémoire, Utilisateurs illimités|Applications monoposte, développement, petites applications|
|**Developer**|Toutes fonctionnalités Enterprise, **Interdite en production**, Licence développement|Développement, tests, formation|
|**Web**|Orientée web, coût réduit, admin réduite, pas de BI|Hébergement web, sites à faible coût|

### 2.2 Notion d'Instance

**Définition:**

- Plusieurs instances SQL Server sur un même serveur physique
- Optimisation des licences et cloisonnement
- Chaque instance = environnement indépendant

**Types d'instances:**

1. **Instance par défaut**: `MSSQLSERVER`
2. **Instances nommées**: `MSSQLSERVER\NomInstance`

### 2.3 SQL Server Management Studio (SSMS)

**Outil principal d'administration:**

**Fonctionnalités:**

- Interface graphique complète
- Requêteur avancé avec coloration syntaxique, débogage, complétion automatique
- Gestion centralisée d'instances distantes
- Pour administrateurs ET développeurs
- Gestion graphique des instances et objets

### 2.4 Configuration des Services

#### États des Services

|État|Description|
|---|---|
|**Démarré**|Prêt à accepter connexions et traiter requêtes|
|**Suspendu**|Connexions en cours non impactées|
|**Arrêté**|Service indisponible|

#### Méthodes de Gestion

1. Gestionnaire de Configuration SQL Server (recommandé)
2. Gestionnaire des Services Windows
3. SQL Server Management Studio

### 2.5 Modes de Licence

#### Tableau Récapitulatif

|Type de Licence|Principe|Quand l'utiliser|
|---|---|---|
|**Par Cœur**|Licence pour chaque cœur de processeur, utilisateurs illimités|Nombreux utilisateurs, accès Internet, utilisateurs inconnus|
|**Par Utilisateur (CAL)**|Licence pour chaque utilisateur + licence serveur|Nombre d'utilisateurs connu et limité, utilisateurs multi-serveurs|
|**Par Poste**|Licence pour chaque périphérique + licence serveur|Postes partagés, multiples utilisateurs par poste|

**Disponibilité par Édition:**

|Édition|Licence par Cœur|Licence par Serveur + CAL|
|---|---|---|
|Enterprise|Oui|Non|
|Standard|Oui|Oui|
|Business Intelligence|Oui|Non|

---

## CHAPITRE 3: Gestion des Bases de Données

### 3.1 Organisation Physique

#### Séparation Logique/Physique

```
Utilisateur
    ↓ (Transact-SQL)
Base de Données LOGIQUE (tables, vues, procédures)
    ↓
Base de Données PHYSIQUE (fichiers .mdf, .ndf, .ldf)
    ↓
Disques durs
```

**Principe:**

- Chaque base = minimum 2 fichiers (données + journal)
- Fichiers propres à chaque base (pas de partage)

### 3.2 Transactions

#### Définition ACID

Transaction = ensemble **indivisible** d'ordres T-SQL

**Propriétés ACID:**

- **Atomicité**: Tout ou rien
- **Cohérence**: État cohérent avant et après
- **Isolation**: Pas d'interférence entre transactions
- **Durabilité**: Modifications permanentes après COMMIT

#### Commandes de Transaction

|Commande|Rôle|
|---|---|
|`BEGIN TRAN`|Démarrer une transaction explicite|
|`SAVE TRAN nom`|Créer un point de sauvegarde|
|`ROLLBACK TRAN [nom]`|Annuler tout ou partie de la transaction|
|`COMMIT TRAN`|Valider la transaction (persistance)|

**Exemple de Flux:**

```sql
BEGIN TRAN
    -- Opération 1
    SAVE TRAN point1
    -- Opération 2
    SAVE TRAN point2
    -- Opération 3
    ROLLBACK TRAN point1  -- Annule opérations 2 et 3
COMMIT TRAN  -- Valide opération 1
```

### 3.3 Structure des Fichiers

#### Types de Fichiers SQL Server

**Fichiers de Données**

|Type|Extension|Obligatoire|Contenu|
|---|---|---|---|
|**Primaire**|`.mdf`|OUI|Catalogue de base de données + données|
|**Secondaire**|`.ndf`|Non|Objets et données utilisateurs|

**Fichiers Journaux**

|Type|Extension|Obligatoire|Contenu|
|---|---|---|---|
|**Journal**|`.ldf`|OUI|Images avant/après modifications|

#### Rôle des Fichiers Journaux

**Objectif:** Garantir la cohérence des données

**Utilisation:**

- Restauration automatique après arrêt brutal
- Opérations de sauvegarde
- Réplication de données

**Opérations journalisées:**

- INSERT, UPDATE, DELETE
- Création d'index
- Modifications de structure

#### Fonctionnement de la Journalisation (WAL - Write Ahead Log)

```
1. Modification en mémoire
2. Écriture immédiate vers journal (.ldf)
3. Écriture différée vers fichiers données (.mdf, .ndf)
```

### 3.4 Structure Interne des Fichiers

#### Les Pages (8 Ko)

**Unité de travail de SQL Server:**

- Taille fixe: **8 Ko** (8 192 octets)
- Lecture/Écriture toujours par page complète
- Une ligne = maximum **8 060 octets** (sauf BLOB)

**Types de pages:**

- Pages de données
- Pages d'index
- Pages d'allocation
- Pages de texte/image

#### Les Extensions (64 Ko)

**Regroupement logique:**

- 1 extension = **8 pages contiguës** = 64 Ko
- Évite la dispersion des données

**Types d'extensions:**

|Type|Description|
|---|---|
|**Extensions mixtes**|Partagées entre plusieurs objets (petites tables)|
|**Extensions uniformes**|Dédiées à un seul objet (grandes tables)|

### 3.5 Création de Base de Données

#### Paramètres des Fichiers

|Paramètre|Obligatoire|Description|
|---|---|---|
|**NAME**|OUI|Nom logique du fichier (utilisé en T-SQL)|
|**FILENAME**|OUI|Chemin et nom physique sur disque|
|**SIZE**|Non|Taille initiale (défaut: 8 MB pour .mdf, 1 MB minimum)|
|**MAXSIZE**|Non|Taille maximale (défaut: croissance jusqu'à saturation), Limites: 16 TB (données), 2 TB (journal)|
|**FILEGROWTH**|Non|Pas d'accroissement (défaut: 64 MB ou %), 0 = pas de croissance automatique|

**Syntaxe minimale:**

```sql
CREATE DATABASE MaBase;
```

**Exemple complet:**

```sql
CREATE DATABASE MaBase
ON PRIMARY
(
    NAME = MaBase_Data,
    FILENAME = 'C:\Data\MaBase.mdf',
    SIZE = 100MB,
    MAXSIZE = 1GB,
    FILEGROWTH = 10MB
)
LOG ON
(
    NAME = MaBase_Log,
    FILENAME = 'C:\Logs\MaBase.ldf',
    SIZE = 50MB,
    MAXSIZE = 500MB,
    FILEGROWTH = 10MB
);
```

### 3.6 Gestion de l'Espace Disque

#### Options d'Accroissement

**AUTOEXTEND ON (Accroissement dynamique)**

- Croissance automatique selon FILEGROWTH
- Pas d'intervention manuelle
- Risque de saturation disque

**AUTOEXTEND OFF (Accroissement manuel)**

- Nécessite intervention administrateur
- Contrôle total sur la taille

#### Ajout de Fichiers

```sql
ALTER DATABASE NomBase
ADD FILE
(
    NAME = NomLogique,
    FILENAME = 'chemin\fichier.ndf',
    SIZE = 100MB
);
```

#### Compactage de Base de Données

**DBCC SHRINKDATABASE:**

- Compacte tous les fichiers de la base
- Ne peut pas descendre sous la taille initiale

```sql
DBCC SHRINKDATABASE (NomBase, 10);  -- 10% d'espace libre souhaité
```

**DBCC SHRINKFILE:**

- Compacte un fichier spécifique

```sql
DBCC SHRINKFILE (NomLogiqueFichier, 500);  -- Taille cible en MB
```

### 3.7 Options de Configuration

#### Options Principales

|Option|Valeurs|Description|
|---|---|---|
|**AUTO_SHRINK**|ON / OFF|Compactage auto si >25% espace libre (Déconseillé en production)|
|**READ_ONLY**|-|Mode lecture seule|
|**READ_WRITE**|-|Mode lecture/écriture (défaut)|
|**SINGLE_USER**|-|Un seul utilisateur autorisé|
|**RESTRICTED_USER**|-|Accès limité aux rôles: db_owner, db_creator, sysadmin|
|**MULTI_USER**|-|Mode normal (défaut)|
|**AUTO_CREATE_STATISTICS**|ON / OFF|Création auto des statistiques manquantes|
|**AUTO_UPDATE_STATISTICS**|ON / OFF|MAJ auto des statistiques obsolètes|

**Modification d'option:**

```sql
ALTER DATABASE NomBase SET SINGLE_USER;
```

#### Consultation des Options

```sql
-- Fonction DATABASEPROPERTYEX
SELECT DATABASEPROPERTYEX('NomBase', 'NomOption');

-- Vue sys.databases
SELECT name, collation_name, user_access_desc, state_desc
FROM sys.databases;

-- Procédures stockées
EXEC sp_helpdb [NomBase];
EXEC sp_spaceused;
```

### 3.8 Groupes de Fichiers (Filegroups)

#### Concept

**Objectif:** Dissocier les objets de leur stockage physique

**Définition:** Ensemble logique de fichiers dans une base de données

**Avantages:**

- Meilleure lisibilité
- Sécurité simplifiée
- Performances optimisées
- Sauvegardes sélectives

#### Groupe PRIMARY

- Créé automatiquement
- Contient obligatoirement les tables système
- Fichier primaire (.mdf) toujours dans PRIMARY

#### Création et Utilisation

```sql
-- Créer le groupe
ALTER DATABASE MaBase ADD FILEGROUP GroupeIndex;

-- Ajouter des fichiers au groupe
ALTER DATABASE MaBase
ADD FILE
(
    NAME = Index_Data,
    FILENAME = 'D:\Index\index.ndf',
    SIZE = 500MB
)
TO FILEGROUP GroupeIndex;

-- Créer objet dans le groupe
CREATE TABLE MaTable
(
    col1 INT,
    col2 VARCHAR(50)
)
ON GroupeIndex;
```

### 3.9 Partitionnement de Tables

#### Concept

**Objectif:** Diviser les grandes tables en partitions plus petites

**Bénéfices:**

- Meilleures performances sur gros volumes
- Maintenance facilitée
- Sauvegardes sélectives
- Requêtes parallélisées

#### Étape 1: Fonction de Partitionnement

```sql
CREATE PARTITION FUNCTION pfClients (int)
AS RANGE LEFT FOR VALUES (10000, 20000, 30000);
```

**Résultat:**

- Partition 1: valeurs ≤ 10 000
- Partition 2: valeurs > 10 000 et ≤ 20 000
- Partition 3: valeurs > 20 000 et ≤ 30 000
- Partition 4: valeurs > 30 000

**LEFT vs RIGHT:**

- **LEFT**: Valeur frontière incluse dans partition inférieure
- **RIGHT**: Valeur frontière incluse dans partition supérieure

#### Étape 2: Schéma de Partitionnement

```sql
CREATE PARTITION SCHEME schemaClients
AS PARTITION pfClients
TO (groupe1, groupe2, groupe3, groupe4);
```

#### Étape 3: Création de Table Partitionnée

```sql
CREATE TABLE TCLIENTS
(
    numero INT IDENTITY(1,1) PRIMARY KEY,
    nom NVARCHAR(80),
    prenom NVARCHAR(80),
    telephone CHAR(14)
)
ON schemaClients (numero);  -- Colonne de partitionnement
```

**Important:**

- Type de la colonne = type du paramètre de la fonction
- Transparent pour les utilisateurs
- Répartition automatique des données

### 3.10 Suppression de Base de Données

```sql
DROP DATABASE NomBase;
```

**Limitations:**

- Base en cours d'utilisation
- Base ouverte (même en lecture)
- Base participant à la réplication

**Conséquences:**

- Suppression de tous les fichiers
- Suppression de toutes les données
- **IRRÉVERSIBLE** (sauf restauration sauvegarde)

**Bonne pratique:** Sauvegarder la base MASTER après suppression de bases utilisateurs

---

## CHAPITRE 4: Sécurité des Accès

### 4.1 Concepts Fondamentaux

#### Terminologie de Sécurité

**1. Entités de Sécurité (Security Principals)**

- Comptes disposant d'un accès à SQL Server
- Exemples: connexions (logins), utilisateurs, rôles

**2. Sécurisables (Securables)**

- Objets gérés par le serveur
- Hiérarchie: Serveur → Base de données → Schéma → Objet

**3. Autorisations (Permissions)**

- Accordées aux entités pour travailler avec les sécurisables
- Héritage hiérarchique

**Exemple d'héritage:** Autorisation SELECT sur schéma → applicable à TOUTES les tables du schéma

#### Types de Rôles

|Type de Rôle|Niveau|Description|
|---|---|---|
|**Rôles de Serveur**|Serveur|Autorisations valides pour toutes les bases|
|**Rôles de Base de Données**|Base|Droits au niveau de la BD spécifique|
|**Rôles d'Application**|Base|Droits pour exécution d'une application|

### 4.2 Architecture de Sécurité

```
NIVEAU SERVEUR
  Connexion (Login) → Rôle Serveur (sysadmin)
      ↓ (Mapping)
      
NIVEAU BASE DE DONNÉES
  Utilisateur (User) → Rôle de BD (db_owner)
      ↓
  Autorisations sur Objets (Tables, Vues)
```

### 4.3 Modes d'Authentification

#### 1. Mode Sécurité Windows (Recommandé)

**Caractéristiques:**

- Authentification par Windows
- Connexion mappée avec compte Windows
- Mode par défaut
- Single Sign-On (SSO)
- Politique de mots de passe Windows

**Avantages:**

- Gestion centralisée des comptes
- Mots de passe robustes
- Groupes Windows exploitables

#### 2. Mode Sécurité Mixte (SQL Server + Windows)

**Caractéristiques:**

- Authentification Windows OU SQL Server
- Connexions SQL avec login/password
- Nécessaire pour clients non-Microsoft

**Connexions SQL Server:**

- Gérées dans base MASTER
- Mots de passe cryptés
- Politiques de mots de passe configurables

#### Comparaison

|Critère|Windows|Mixte|
|---|---|---|
|**Sécurité**|Excellente|Bonne|
|**Gestion**|Centralisée|Double gestion|
|**Compatibilité**|Windows uniquement|Tous clients|
|**SSO**|Oui|Non (pour SQL logins)|

### 4.4 Gestion des Connexions (Logins)

#### Création de Connexion

**Authentification Windows:**

```sql
CREATE LOGIN [DOMAINE\Utilisateur]
FROM WINDOWS
WITH DEFAULT_DATABASE = MaBase;
```

**Authentification SQL Server:**

```sql
CREATE LOGIN Pierre
WITH PASSWORD = 'Mot2P@sse!',
     DEFAULT_DATABASE = GESCOM,
     CHECK_EXPIRATION = ON,
     CHECK_POLICY = ON;
```

#### Paramètres Importants

|Paramètre|Description|
|---|---|
|**PASSWORD**|Mot de passe (obligatoire pour SQL)|
|**MUST_CHANGE**|Forcer changement à la 1ère connexion|
|**DEFAULT_DATABASE**|Base par défaut (ne donne PAS de droits)|
|**CHECK_EXPIRATION**|Appliquer l'expiration des mots de passe|
|**CHECK_POLICY**|Appliquer les politiques Windows|
|**CREDENTIAL**|Lier à un credential pour ressources externes|

#### Credentials

**Objectif:** Permettre aux connexions SQL d'accéder à des ressources Windows externes

**Définition:** Compte Windows rattaché à un compte SQL Server

**Création:**

```sql
CREATE CREDENTIAL MonCredential
WITH IDENTITY = 'DOMAINE\CompteWindows',
     SECRET = 'MotDePasse';

CREATE LOGIN CompteSQL
WITH PASSWORD = 'Pass123',
     CREDENTIAL = MonCredential;
```

#### Gestion des Connexions

**Activer/Désactiver:**

```sql
ALTER LOGIN Pierre DISABLE;
ALTER LOGIN Pierre ENABLE;
```

**Consultation:**

```sql
-- Liste des connexions
SELECT name, loginName, dbname
FROM sys.syslogins;

-- Connexions actives
EXEC sp_who;
```

#### Instruction DENY

**Objectif:** Refus explicite (prioritaire sur autorisations)

**Cas d'usage:**

- Utilisateur membre de plusieurs groupes
- Un groupe a accès, mais pas cet utilisateur spécifique
- DENY prend le dessus sur GRANT

### 4.5 Utilisateurs de Base de Données

#### Caractéristiques

**Principes:**

- Créés au niveau base de données
- Rattachés à une connexion (login)
- Reçoivent les droits sur les objets
- Stockés dans `sys.database_principals`

**Sans utilisateur de BD:** Accès limité aux tables système et bases avec compte `guest` actif

#### Utilisateurs Particuliers

**1. dbo (Database Owner)**

- Présent dans toutes les bases
- Membres de `sysadmin` mappés à dbo
- Compte `sa` mappé à dbo
- Ne peut être supprimé
- Objets créés par sysadmin → propriétaire = dbo

**2. guest**

- Présent dans toutes les bases
- Désactivé par défaut
- Permet accès aux connexions sans utilisateur de BD

**Activation/Désactivation de guest:**

```sql
GRANT CONNECT TO guest;   -- Activer
REVOKE CONNECT FROM guest; -- Désactiver
```

**3. sys et INFORMATION_SCHEMA**

- Utilisateurs système
- Non rattachés à une connexion

#### Création d'Utilisateur

```sql
CREATE USER PierreUser
FOR LOGIN Pierre
WITH DEFAULT_SCHEMA = dbo;

ALTER USER PierreUser
WITH NAME = NouveauNom,
     DEFAULT_SCHEMA = VentesSchema;

DROP USER PierreUser;
```

#### Consultation

```sql
-- Liste des utilisateurs
SELECT * FROM sys.database_principals;

-- Connexions et utilisateurs associés
SELECT s.name AS 'Connexion',
       p.name AS 'Utilisateur'
FROM sys.database_principals p
INNER JOIN sys.server_principals s ON s.sid = p.sid;

-- Utilisateurs connectés
EXEC sp_who;
```

### 4.6 Gestion des Schémas

#### Concept

**Objectif:** Dissocier utilisateurs des objets qu'ils créent

**Définition:** Ensemble logique d'objets dans une base de données

**Avantages:**

- Meilleure lisibilité
- Sécurité simplifiée
- Partage facilité entre utilisateurs
- Gestion des privilèges par schéma

#### Utilisation

**Accès aux objets:**

```sql
-- Objet dans schéma par défaut
SELECT * FROM MaTable;

-- Objet hors schéma par défaut
SELECT * FROM AutreSchema.MaTable;
```

#### Gestion des Schémas

```sql
-- Création
CREATE SCHEMA Ventes AUTHORIZATION Pierre;

-- Création avec objets
CREATE SCHEMA Ventes AUTHORIZATION Pierre
    CREATE TABLE Clients (...)
    CREATE VIEW VueClients AS ...;

-- Transfert d'objet
ALTER SCHEMA NouveauSchema TRANSFER Ancien.ObjetTable;

-- Suppression (doit être vide)
DROP SCHEMA NomSchema;

-- Consultation
SELECT * FROM sys.schemas;
```

### 4.7 Gestion des Droits

#### Niveaux d'Attribution

1. **Serveur** → Connexions
2. **Base de données** → Utilisateurs
3. **Schéma** → Collections d'objets
4. **Objet** → Tables, vues, procédures

#### Types de Privilèges

**Privilèges d'Instructions (Statement Permissions):**

- CREATE DATABASE
- CREATE TABLE
- CREATE VIEW
- CREATE PROCEDURE
- CREATE FUNCTION
- BACKUP DATABASE
- BACKUP LOG

**Privilèges d'Objets (Object Permissions):**

- SELECT
- INSERT
- UPDATE
- DELETE
- EXECUTE (procédures/fonctions)

#### Syntaxe de Gestion

**GRANT (Accorder):**

```sql
-- Privilège d'instruction
GRANT CREATE TABLE TO Pierre WITH GRANT OPTION;

-- Privilège sur objet
GRANT SELECT, INSERT ON schema.Table TO Pierre WITH GRANT OPTION;
```

**REVOKE (Retirer):**

```sql
-- Retirer privilège simple
REVOKE CREATE TABLE FROM Pierre;

-- Retirer WITH GRANT OPTION uniquement
REVOKE GRANT OPTION FOR CREATE TABLE FROM Pierre;

-- Retirer en cascade
REVOKE SELECT ON schema.Table FROM Pierre CASCADE;
```

**DENY (Interdire):**

```sql
DENY DELETE ON schema.Table TO Pierre CASCADE;
```

#### Hiérarchie des Autorisations

```
DENY (le plus fort)
  ↓
GRANT
  ↓
Pas d'autorisation (le plus faible)
```

**Règle:** DENY est **toujours prioritaire** sur GRANT

#### WITH GRANT OPTION

**Permet à l'utilisateur:**

- Recevoir le privilège
- Transmettre le privilège à d'autres

**Exemple:**

```sql
GRANT CREATE TABLE TO Jean WITH GRANT OPTION;
-- Jean peut ensuite :
GRANT CREATE TABLE TO Marie;
```

**Retrait en cascade:**

```sql
REVOKE CREATE TABLE FROM Jean CASCADE;
-- Retire aussi le privilège à Marie
```

#### Consultation des Privilèges

```sql
SELECT p.name AS 'Utilisateur',
       p.type_desc AS 'Type',
       perm.class_desc AS 'Classe',
       perm.permission_name AS 'Permission'
FROM sys.database_principals p
INNER JOIN sys.database_permissions perm
    ON perm.grantee_principal_id = p.principal_id;
```

### 4.8 Gestion des Rôles

#### Définition

**Rôle** = Regroupement nommé de privilèges

**Types:**

1. Rôles fixes (prédéfinis)
2. Rôles utilisateur (personnalisés)

**Niveaux:**

- Serveur
- Base de données
- Application

#### Cumul de Privilèges

Un utilisateur dispose de tous les privilèges :

- Accordés directement à la connexion
- Via rôles de serveur
- Via rôles de base de données
- Accordés directement à l'utilisateur de BD

### 4.9 Rôles de Serveur

#### Rôles Fixes Prédéfinis

|Rôle|Description|
|---|---|
|**sysadmin**|Super administrateur - Tous pouvoirs sur l'instance|
|**serveradmin**|Configuration paramètres serveur|
|**setupadmin**|Gestion serveurs liés, procédures stockées système|
|**securityadmin**|Gestion connexions (peut élever ses privilèges)|
|**processadmin**|Gestion des processus SQL Server, terminer processus|
|**dbcreator**|Créer, modifier, supprimer bases de données|
|**diskadmin**|Gestion fichiers sur disque|
|**bulkadmin**|Exécution BULK INSERT|
|**public**|Rôle par défaut de toute connexion, lister les bases|

#### Gestion des Rôles Serveur

```sql
-- Création (rôle personnalisé)
CREATE SERVER ROLE MonRoleServeur;

-- Attribution
ALTER SERVER ROLE sysadmin ADD MEMBER Pierre;

-- Retrait
ALTER SERVER ROLE sysadmin DROP MEMBER Pierre;

-- Consultation
EXEC sp_helpsrvrole;
EXEC sp_helpsrvrolemember;
```

### 4.10 Rôles de Base de Données

#### Rôles Fixes Prédéfinis

|Rôle|Droits|
|---|---|
|**db_owner**|Propriétaire - Tous pouvoirs sur la base|
|**db_accessadmin**|Ajouter/supprimer utilisateurs|
|**db_datareader**|SELECT sur toutes les tables|
|**db_datawriter**|INSERT, UPDATE, DELETE sur toutes les tables|
|**db_ddladmin**|Ordres DDL (CREATE, ALTER, DROP)|
|**db_securityadmin**|Gérer rôles et autorisations|
|**db_backupoperator**|Sauvegardes de la base|
|**db_denydatareader**|Interdire SELECT sur toute la base|
|**db_denydatawriter**|Interdire INSERT, UPDATE, DELETE|

#### Gestion des Rôles de BD

```sql
-- Création
CREATE ROLE RoleVentes AUTHORIZATION Pierre;

-- Attribution
ALTER ROLE db_datareader ADD MEMBER Jean;

-- Retrait
ALTER ROLE db_datareader DROP MEMBER Jean;

-- Suppression
DROP ROLE RoleVentes;
```

### 4.11 Rôles d'Application

#### Caractéristiques

**Spécificités:**

- Définis au niveau base de données
- Pas d'utilisateurs membres
- Protégés par mot de passe
- Droits nécessaires pour une application
- Doivent être **activés** par un utilisateur
- Prennent le dessus sur les privilèges utilisateur

#### Gestion

```sql
-- Création
CREATE APPLICATION ROLE RoleApp
WITH PASSWORD = 'M0tDeP@sse!',
     DEFAULT_SCHEMA = dbo;

-- Modification
ALTER APPLICATION ROLE RoleApp
WITH PASSWORD = 'Nouv3@uP@ss',
     DEFAULT_SCHEMA = Ventes;

-- Suppression
DROP APPLICATION ROLE RoleApp;

-- Activation
EXEC sp_setapprole 'RoleApp', 'M0tDeP@sse!';
```

**Important:** Une fois activé, le rôle d'application remplace tous les privilèges de l'utilisateur pour la session en cours.

---

## POINTS CRITIQUES POUR L'EXAMEN

### Concepts Fondamentaux

#### À Maîtriser Absolument

**Éditions SQL Server:**

- Enterprise: Édition complète, tous composants
- Standard: PME, fonctionnalités essentielles
- Express: GRATUITE, 10 Go max, 1 Go RAM
- Developer: Comme Enterprise, DÉVELOPPEMENT uniquement

**Bases Système:**

- **MASTER**: Configuration serveur (CRITIQUE - à sauvegarder)
- **MODEL**: Template nouvelles bases
- **TEMPDB**: Temporaire (recréée au démarrage)
- **MSDB**: SQL Agent (tâches planifiées)
- **RESOURCE**: Objets système

**Types de Bases:**

- **OLTP**: Transactionnel, opérations quotidiennes
- **OLAP**: Décisionnel, analyse, Data Warehouse

**Structure Physique:**

- **Page**: 8 Ko (unité de base)
- **Extension**: 64 Ko (8 pages)
- **.mdf**: Fichier primaire données
- **.ndf**: Fichiers secondaires données
- **.ldf**: Fichier journal

**Transactions ACID:**

- **Atomicité**: Tout ou rien
- **Cohérence**: État cohérent
- **Isolation**: Pas d'interférence
- **Durabilité**: Permanent après COMMIT

**Commandes:**

- `BEGIN TRAN` → `SAVE TRAN` → `ROLLBACK` / `COMMIT`

**Modes de Licence:**

- **Par Cœur**: Utilisateurs illimités
- **Par CAL**: Nombre fixe d'utilisateurs
- Enterprise: Cœur uniquement
- Standard: Cœur OU CAL

### Sécurité

#### Architecture

```
SERVEUR
  Connexion (Login) → Rôle Serveur
      ↓
BASE DE DONNÉES
  Utilisateur (User) → Rôle BD
      ↓
  Autorisations Objets
```

#### Authentification

**Windows** (Recommandé):

- SSO, gestion centralisée
- Politique Windows

**Mixte**:

- Windows OU SQL Server
- Nécessaire pour non-Microsoft

#### Hiérarchie Autorisations

```
DENY (prioritaire)
  ↓
GRANT
  ↓
Rien
```

**Règle:** DENY > GRANT toujours

#### Rôles Serveur

- **sysadmin**: Super admin
- **dbcreator**: Créer bases
- **securityadmin**: Gérer connexions

#### Rôles BD

- **db_owner**: Propriétaire
- **db_datareader**: SELECT toutes tables
- **db_datawriter**: INSERT/UPDATE/DELETE toutes tables

#### Commandes

```sql
GRANT privilege TO user [WITH GRANT OPTION];
REVOKE privilege FROM user [CASCADE];
DENY privilege TO user [CASCADE];
```

### Gestion Bases de Données

#### Groupes de Fichiers

**PRIMARY**: Obligatoire, contient tables système

**Personnalisés**: Organisation logique, performances, sauvegardes sélectives

#### Partitionnement

**3 Étapes:**

1. Fonction de partitionnement (plages)
2. Schéma de partitionnement (groupes fichiers)
3. Table partitionnée (colonne)

**LEFT vs RIGHT:**

- LEFT: Frontière dans partition inférieure
- RIGHT: Frontière dans partition supérieure

#### Schémas

**Définition**: Ensemble logique d'objets

**Avantages**:

- Dissociation utilisateurs/objets
- Sécurité simplifiée
- Gestion privilèges par schéma

### Pièges Fréquents

**Confusions:**

- Connexion ≠ Utilisateur
- Base par défaut ≠ Droits
- REVOKE ≠ DENY
- Extension (64 Ko) ≠ Page (8 Ko)
- OLTP ≠ OLAP

**Erreurs:**

- Oublier CASCADE avec REVOKE
- Confondre rôles serveur et BD
- .mdf vs .ldf
- Fonction vs Schéma partitionnement

### Mémo Rapide

**Fichiers:**

- .mdf: Primaire données
- .ndf: Secondaire données
- .ldf: Journal (WAL)

**Transactions:**

- BEGIN → SAVE → ROLLBACK/COMMIT
- ACID: Atomicité, Cohérence, Isolation, Durabilité

**Sécurité:**

- Login (serveur) → User (BD)
- Windows > Mixte
- DENY > GRANT > Rien

**Partitionnement:**

1. CREATE PARTITION FUNCTION
2. CREATE PARTITION SCHEME
3. CREATE TABLE ON scheme(column)

**Bases Système:**

- MASTER: CRITIQUE
- MODEL: Template
- TEMPDB: Temporaire
- MSDB: Agent
- RESOURCE: Système

---

**Document préparé pour révision examen SQL Server**  
**Basé sur supports de cours N'SOUGAN Folly W.**