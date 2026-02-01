

#  MICROSOFT SQL SERVER - ADMINISTRATION

## Résumé des Essentiels Théoriques

**Version:** SQL Server 2016  
**Enseignant:** N'SOUGAN Folly W.  
**Niveau:** Cours d'Administration des Bases de Données

---

## 📑 Table des Matières

1. [Présentation de SQL Server](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#chapitre-1-pr%C3%A9sentation)
2. [Installation et Configuration](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#chapitre-2-installation-et-configuration)
3. [Gestion des Bases de Données](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#chapitre-3-gestion-des-bases-de-donn%C3%A9es)
4. [Sécurité des Accès](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#chapitre-4-s%C3%A9curit%C3%A9-des-acc%C3%A8s)
5. [Glossaire](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#glossaire)
6. [Bonnes Pratiques](https://claude.ai/chat/83b8872c-12c7-406a-abdf-e2d4d4b94c06#bonnes-pratiques)

---

## 🎯 CHAPITRE 1: Présentation

### 1.1 Qu'est-ce que SQL Server ?

**SQL Server est un SGBDR (Système de Gestion de Base de Données Relationnelles)**

**Responsabilités principales:**

- ✅ Stocker les données
- ✅ Vérifier les contraintes d'intégrité
- ✅ Garantir la cohérence des données (même en cas de panne)
- ✅ Assurer les relations entre les données

### 1.2 Intégration avec Windows

SQL Server est **complètement intégré à Windows**, ce qui lui permet de bénéficier de:

|Composant Windows|Utilisation par SQL Server|
|---|---|
|**Observateur d'événements**|Journal des applications pour consigner les erreurs SQL Server|
|**Traitements parallèles**|Exploitation des architectures multiprocesseurs via threads Windows|
|**Analyseur de Performance**|Compteurs SQL Server pour détecter les goulots d'étranglement|
|**Sécurité Windows**|Authentification unique (single sign-on) pour les utilisateurs|
|**Services Windows**|Exécution des composants SQL Server|
|**Active Directory**|Enregistrement automatique des instances SQL Server|

### 1.3 Types de Bases de Données

#### 📊 OLTP (Online Transactional Processing)

**Caractéristiques:**

- Stockage direct des informations
- Réutilisation ultérieure des données telles qu'enregistrées
- **Serveur transactionnel**
- Focus: opérations quotidiennes (INSERT, UPDATE, DELETE)

**Utilisation:** Gestion courante des données (commandes, clients, stocks, etc.)

#### 📈 OLAP (Online Analytical Processing)

**Caractéristiques:**

- Informations statistiques et agrégées
- Extraction sous forme de cubes multidimensionnels
- **Serveur décisionnel (Data Warehouse)**
- Focus: aide à la décision
- S'appuie sur des données OLTP

**Utilisation:** Analyses, rapports, business intelligence

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

#### Composants Serveur (Services Windows)

**1. SQL Server (Moteur de BD)**

- Service principal
- Nom: `SQLSERVER (MSSQLSERVER)` pour instance par défaut
- Nom: `SQLSERVER (<nomInstance>)` pour instance nommée
- Gère le stockage et le traitement des données

**2. SQL Server Agent**

- Gestion des tâches planifiées
- Surveillance et alertes
- Nom: `SQL Server Agent (MSSQLSERVER)` ou `SQL Server Agent (<nomInstance>)`

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

### 1.5 Architecture Globale

**Connectivité Client:**

- Bibliothèques OLE-DB et ODBC
- SMO (SQL Server Management Objects) pour outils graphiques
- Gestion du réseau automatique

**Nom Complet des Objets:**

```
serveur.nomBase.propriétaire.objet
```

### 1.6 Bases de Données Système

#### 🔑 Bases Système Obligatoires

**1. MASTER** ⭐ (La plus importante)

- Base de données principale
- Contient:
    - Comptes de connexion
    - Options de configuration
    - Liste des bases de données utilisateurs
    - Références vers les fichiers de bases utilisateurs
- ⚠️ **À sauvegarder impérativement**

**2. MODEL**

- Template pour nouvelles bases
- Tout objet ajouté dans MODEL apparaît dans les nouvelles bases créées
- Contient les tables système par défaut

**3. TEMPDB**

- Espace de stockage temporaire **partagé**
- Recréée à chaque démarrage
- Contient:
    - Tables temporaires locales (#) et globales (##)
    - Tables de travail intermédiaires
    - Résultats de tris
- Non sauvegardable (recréée automatiquement)

**4. MSDB**

- Utilisée par SQL Server Agent
- Contient:
    - Définition des tâches planifiées
    - Alertes et opérateurs
    - Historique d'exécution des tâches

**5. RESOURCE**

- Base de données en lecture seule
- Contient la définition des objets système
- Objets apparaissent dans le schéma `sys`
- ⚠️ Sauvegarde manuelle du fichier .mdf nécessaire

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

**Principales procédures (préfixe sp_):**

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

**Responsabilités principales:**

1. 🔧 Gérer les services SQL Server
2. 🖥️ Gérer les instances SQL Server
3. 💾 Mettre en place sauvegardes et restaurations
4. ⚡ Configurer la disponibilité des données
5. 🌐 Gérer les configurations réseau
6. 📥 Import/Export de données
7. 🤖 Automatiser les tâches avec SQL Agent
8. 📊 Mesurer et optimiser les performances

---

## 🔧 CHAPITRE 2: Installation et Configuration

### 2.1 Éditions de SQL Server

#### Comparaison des Éditions

|Édition|Caractéristiques|Usage|
|---|---|---|
|**Enterprise**|- Édition la plus complète<br>- Tous les composants disponibles<br>- 32 et 64 bits<br>- Partitionnement de tables<br>- Compression de données<br>- BI avancée|Production grandes entreprises<br>Volumes très importants|
|**Standard**|- Moteur performant<br>- Fonctionnalités essentielles<br>- Limitations sur volumes|PME<br>Charges moyennes|
|**Express**|- **GRATUITE** (aucune licence)<br>- Limite: 10 Go de données<br>- Limite: 1 Go de mémoire<br>- Utilisateurs illimités|Applications monoposte<br>Développement<br>Petites applications|
|**Developer**|- Toutes fonctionnalités Enterprise<br>- ⚠️ **Interdite en production**<br>- Licence développement|Développement<br>Tests<br>Formation|
|**Web**|- Orientée web<br>- Coût réduit<br>- Admin réduite<br>- Pas de BI|Hébergement web<br>Sites à faible coût|

### 2.2 Notion d'Instance

**Qu'est-ce qu'une instance ?**

- Plusieurs instances SQL Server sur un même serveur physique
- Optimisation des licences
- Cloisonnement de la gestion
- Chaque instance = environnement SQL Server indépendant

**Types d'instances:**

1. **Instance par défaut**: `MSSQLSERVER`
2. **Instances nommées**: `MSSQLSERVER\NomInstance`

### 2.3 Préparation de l'Installation

**Prérequis système:**

- Système d'exploitation Windows Server (ou poste de travail pour dev)
- Espace disque suffisant
- RAM adéquate selon l'édition
- Processeur compatible

**Outil d'Analyse de Configuration Système:**

- Vérifie les prérequis avant installation
- Détecte les problèmes potentiels
- Recommandations d'installation

### 2.4 SQL Server Management Studio (SSMS)

**Outil principal d'administration:**

**Fonctionnalités:**

- 🎨 Interface graphique complète
- 💻 Requêteur avancé avec:
    - Coloration syntaxique
    - Débogage
    - Complétion automatique (depuis 2012)
- 🌐 Gestion centralisée d'instances distantes
- 👥 Pour administrateurs ET développeurs
- 📝 Gestion graphique des instances et objets

### 2.5 Configuration des Services

#### États des Services

|État|Description|
|---|---|
|**Démarré**|Prêt à accepter connexions et traiter requêtes|
|**Suspendu**|Connexions en cours non impactées|
|**Arrêté**|Service indisponible|

#### Méthodes de Gestion

1. **Gestionnaire de Configuration SQL Server** (recommandé)
2. **Gestionnaire des Services Windows**
3. **SQL Server Management Studio**

### 2.6 Modes de Licence

#### Tableau Récapitulatif

|Type de Licence|Principe|Quand l'utiliser|
|---|---|---|
|**Par Cœur**|Licence pour chaque cœur de processeur<br>Utilisateurs illimités|Environnements avec nombreux utilisateurs<br>Accès Internet<br>Utilisateurs inconnus|
|**Par Utilisateur (CAL)**|Licence pour chaque utilisateur<br>+ Licence serveur|Nombre d'utilisateurs connu et limité<br>Utilisateurs multi-serveurs|
|**Par Poste**|Licence pour chaque périphérique<br>+ Licence serveur|Postes partagés<br>Multiples utilisateurs par poste|

**Disponibilité par Édition:**

|Édition|Licence par Cœur|Licence par Serveur + CAL|
|---|---|---|
|Enterprise|✅|❌|
|Standard|✅|✅|
|Business Intelligence|✅|❌|

#### Exemple Pratique (Licence par Cœur)

**Contexte:**

- 1 serveur SQL
- 10 stations de travail
- 2 groupes de 5 postes (jour/nuit)
- Pas de chevauchement

**Réponse:** Nombre de licences = nombre de cœurs du serveur (pas de CAL nécessaires)

#### Licence par Utilisateur vs Poste

**Serveur d'Application Intermédiaire:**

Lorsqu'un serveur d'application se place entre les utilisateurs et SQL Server:

- Les utilisateurs se connectent au serveur d'application
- Le serveur d'application se connecte à SQL Server
- **Solution:** Licence serveur d'application + CAL pour chaque utilisateur

---

## 💾 CHAPITRE 3: Gestion des Bases de Données

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

- Chaque base = au minimum 2 fichiers (données + journal)
- Fichiers propres à chaque base (pas de partage entre bases)

### 3.2 Notion de Transaction

#### Définition ACID

Une transaction = ensemble **indivisible** d'ordres T-SQL

**Propriétés ACID:**

- **A**tomicité: Tout ou rien
- **C**ohérence: État cohérent avant et après
- **I**solation: Pas d'interférence entre transactions
- **D**urabilité: Modifications permanentes après COMMIT

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

**Cas d'Usage Classique:** Virement bancaire

- Débit compte A + Crédit compte B dans une même transaction
- Si une opération échoue → les deux sont annulées

### 3.3 Structure des Fichiers

#### Types de Fichiers SQL Server

**1. Fichiers de Données**

|Type|Extension|Obligatoire|Contenu|
|---|---|---|---|
|**Primaire**|`.mdf`|✅ OUI|Catalogue de base de données + données|
|**Secondaire**|`.ndf`|❌ Non|Objets et données utilisateurs|

**Chemin par défaut (SQL 2019):**

```
C:\RepertoireInstance\MSSQL15.NomInstance\MSSQL\Data
```

**2. Fichiers Journaux**

|Type|Extension|Obligatoire|Contenu|
|---|---|---|---|
|**Journal**|`.ldf`|✅ OUI|Images avant/après modifications|

#### Rôle des Fichiers Journaux

**Objectif:** Garantir la cohérence des données

**Utilisation:**

- ✅ Restauration automatique après arrêt brutal
- ✅ Opérations de sauvegarde
- ✅ Réplication de données

**Opérations journalisées:**

- INSERT, UPDATE, DELETE
- Création d'index
- Modifications de structure

#### Fonctionnement de la Journalisation

```
┌─────────────────────────────────────┐
│     MÉMOIRE VIVE (SQL SERVER)       │
│  ┌──────────┐      ┌──────────┐    │
│  │  Données │      │  Journal │    │
│  │  modif.  │──1──→│   WAL    │    │
│  └──────────┘      └──────────┘    │
│       │                   │         │
└───────│───────────────────│─────────┘
        │                   │
        2                   3
        │                   │
        ↓                   ↓
   ┌─────────┐         ┌─────────┐
   │ .mdf    │         │  .ldf   │
   │ .ndf    │         │         │
   └─────────┘         └─────────┘

1. Modification en mémoire
2. Écriture différée vers fichiers données
3. Écriture immédiate vers journal (WAL - Write Ahead Log)
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

**Schéma:**

```
┌───────────────────────────────────────────┐
│         FICHIER DE DONNÉES (.mdf)         │
├───────────────────────────────────────────┤
│  Extension 1 (64 Ko)                      │
│  ┌──────┬──────┬──────┬──────┬─────┐     │
│  │ Page │ Page │ Page │ Page │ ... │     │
│  │ 8Ko  │ 8Ko  │ 8Ko  │ 8Ko  │     │     │
│  └──────┴──────┴──────┴──────┴─────┘     │
├───────────────────────────────────────────┤
│  Extension 2 (64 Ko)                      │
│  ┌──────┬──────┬──────┬──────┬─────┐     │
│  │ Page │ Page │ Page │ Page │ ... │     │
│  └──────┴──────┴──────┴──────┴─────┘     │
└───────────────────────────────────────────┘
```

### 3.5 Création de Base de Données

#### Paramètres de Création

**Informations minimales:**

1. Nom de la base de données
2. Classement (COLLATION)
3. Taille initiale
4. Emplacement de stockage

#### Paramètres des Fichiers

|Paramètre|Obligatoire|Description|
|---|---|---|
|**NAME**|✅|Nom logique du fichier (utilisé en T-SQL)|
|**FILENAME**|✅|Chemin et nom physique sur disque|
|**SIZE**|❌|Taille initiale (défaut: 8 MB pour .mdf, 1 MB minimum)|
|**MAXSIZE**|❌|Taille maximale (défaut: croissance jusqu'à saturation)<br>Limites: 16 TB (données), 2 TB (journal)|
|**FILEGROWTH**|❌|Pas d'accroissement (défaut: 64 MB ou %)<br>0 = pas de croissance automatique|

**Exemple de syntaxe minimale:**

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

**1. AUTOEXTEND ON (Accroissement dynamique)**

- Croissance automatique selon FILEGROWTH
- Pas d'intervention manuelle
- ⚠️ Risque de saturation disque

**2. AUTOEXTEND OFF (Accroissement manuel)**

- Nécessite intervention administrateur
- Contrôle total sur la taille
- Planification de la croissance

#### Ajout de Fichiers

**Syntaxe:**

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
- Extensions regroupées en début de fichier
- Ne peut pas descendre sous la taille initiale
- Compactage différé pour les journaux

**Syntaxe:**

```sql
DBCC SHRINKDATABASE (NomBase, 10);  -- 10% d'espace libre souhaité
```

**DBCC SHRINKFILE:**

- Compacte un fichier spécifique
- Option EMPTYFILE: vide le fichier (préparation suppression)

**Syntaxe:**

```sql
DBCC SHRINKFILE (NomLogiqueFichier, 500);  -- Taille cible en MB
```

### 3.7 Options de Configuration

#### Options Principales

|Option|Valeurs|Description|
|---|---|---|
|**AUTO_SHRINK**|ON / OFF|Compactage auto si >25% espace libre<br>⚠️ Déconseillé en production|
|**READ_ONLY**|-|Mode lecture seule|
|**READ_WRITE**|-|Mode lecture/écriture (défaut)|
|**SINGLE_USER**|-|Un seul utilisateur autorisé|
|**RESTRICTED_USER**|-|Accès limité aux rôles: db_owner, db_creator, sysadmin|
|**MULTI_USER**|-|Mode normal (défaut)|
|**AUTO_CREATE_STATISTICS**|ON / OFF|Création auto des statistiques manquantes|
|**AUTO_UPDATE_STATISTICS**|ON / OFF|MAJ auto des statistiques obsolètes|

**Modification d'option:**

```sql
ALTER DATABASE NomBase
SET SINGLE_USER;
```

#### Consultation des Options

**Fonction DATABASEPROPERTYEX:**

```sql
SELECT DATABASEPROPERTYEX('NomBase', 'NomOption');
```

**Vue sys.databases:**

```sql
SELECT name, collation_name, user_access_desc, state_desc
FROM sys.databases;
```

**Procédures stockées:**

- `sp_helpdb [NomBase]`
- `sp_spaceused`

### 3.8 Groupes de Fichiers (Filegroups)

#### Concept

**Objectif:** Dissocier les objets de leur stockage physique

**Définition:** Ensemble logique de fichiers dans une base de données

**Avantages:**

- 📖 Meilleure lisibilité
- 🔒 Sécurité simplifiée
- 🚀 Performances optimisées
- 💾 Sauvegardes sélectives

#### Groupe PRIMARY

- Créé automatiquement
- Contient obligatoirement les tables système
- Fichier primaire (.mdf) toujours dans PRIMARY

#### Création de Groupe de Fichiers

```sql
-- Créer le groupe
ALTER DATABASE MaBase
ADD FILEGROUP GroupeIndex;

-- Ajouter des fichiers au groupe
ALTER DATABASE MaBase
ADD FILE
(
    NAME = Index_Data,
    FILENAME = 'D:\Index\index.ndf',
    SIZE = 500MB
)
TO FILEGROUP GroupeIndex;
```

#### Utilisation

**Lors de la création d'objet:**

```sql
CREATE TABLE MaTable
(
    col1 INT,
    col2 VARCHAR(50)
)
ON GroupeIndex;  -- Spécifier le groupe de fichiers
```

**Accès aux objets hors schéma par défaut:**

```
nomSchema.nomObjet
```

### 3.9 Partitionnement de Tables

#### Concept

**Objectif:** Diviser les grandes tables en partitions plus petites

**Bénéfices:**

- ⚡ Meilleures performances sur gros volumes
- 🔧 Maintenance facilitée
- 💾 Sauvegardes sélectives
- 🚀 Requêtes parallélisées

**Principe:**

```
Grande Table (millions de lignes)
        ↓
Partition 1  |  Partition 2  |  Partition 3
(Fichier 1)  |  (Fichier 2)  |  (Fichier 3)
```

#### Étape 1: Fonction de Partitionnement

**Définit les plages de valeurs:**

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

**Associe les partitions aux groupes de fichiers:**

```sql
CREATE PARTITION SCHEME schemaClients
AS PARTITION pfClients
TO (groupe1, groupe2, groupe3, groupe4);
```

**Option NEXT USED:**

- Groupe de fichiers supplémentaire pour futures partitions
- Marqué comme prochain à utiliser

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

**Syntaxe:**

```sql
DROP DATABASE NomBase;
```

**Limitations:**

- ❌ Base en cours d'utilisation
- ❌ Base ouverte (même en lecture)
- ❌ Base participant à la réplication

**Conséquences:**

- Suppression de tous les fichiers
- Suppression de toutes les données
- ⚠️ **IRRÉVERSIBLE** (sauf restauration sauvegarde)

**Bonne pratique:**

- 💾 Sauvegarder la base MASTER après suppression de bases utilisateurs

---

## 🔐 CHAPITRE 4: Sécurité des Accès

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

**Exemple d'héritage:**

```
Autorisation SELECT sur schéma
    ↓
Applicable à TOUTES les tables du schéma
```

#### Types de Rôles

|Type de Rôle|Niveau|Description|
|---|---|---|
|**Rôles de Serveur**|Serveur|Autorisations valides pour toutes les bases|
|**Rôles de Base de Données**|Base|Droits au niveau de la BD spécifique|
|**Rôles d'Application**|Base|Droits pour exécution d'une application|

### 4.2 Architecture de Sécurité

```
┌───────────────────────────────────────────────┐
│              NIVEAU SERVEUR                    │
│  ┌──────────────┐    ┌──────────────┐        │
│  │  Connexion   │───→│ Rôle Serveur │        │
│  │   (Login)    │    │   (sysadmin) │        │
│  └──────────────┘    └──────────────┘        │
│         │                                      │
│         ↓ (Mapping)                           │
├───────────────────────────────────────────────┤
│           NIVEAU BASE DE DONNÉES              │
│  ┌──────────────┐    ┌──────────────┐        │
│  │ Utilisateur  │───→│  Rôle de BD  │        │
│  │     (User)   │    │  (db_owner)  │        │
│  └──────────────┘    └──────────────┘        │
│         │                 │                   │
│         ↓                 ↓                   │
│    ┌─────────────────────────┐               │
│    │   Autorisations sur     │               │
│    │  Objets (Tables, Vues)  │               │
│    └─────────────────────────┘               │
└───────────────────────────────────────────────┘
```

### 4.3 Modes d'Authentification

#### 1. Mode Sécurité Windows (Recommandé)

**Caractéristiques:**

- ✅ Authentification par Windows
- ✅ Connexion mappée avec compte Windows
- ✅ Mode par défaut
- ✅ Single Sign-On (SSO)
- ✅ Politique de mots de passe Windows

**Avantages:**

- Gestion centralisée des comptes
- Mots de passe robustes
- Groupes Windows exploitables

#### 2. Mode Sécurité Mixte (SQL Server + Windows)

**Caractéristiques:**

- Authentification Windows **OU** SQL Server
- Connexions SQL avec login/password
- Nécessaire pour clients non-Microsoft

**Connexions SQL Server:**

- Gérées dans base MASTER
- Mots de passe cryptés
- Politiques de mots de passe configurables

#### Comparaison

|Critère|Windows|Mixte|
|---|---|---|
|**Sécurité**|⭐⭐⭐ Excellente|⭐⭐ Bonne|
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

-- Puis lier à la connexion
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
-- Liste des connexions et BD par défaut
SELECT name AS 'Nom',
       loginName AS 'Connexion',
       dbname AS 'Base par défaut'
FROM sys.syslogins;

-- Connexions actives
EXEC sp_who;
```

#### Instruction DENY

**Objectif:** Refus explicite (prioritaire sur autorisations)

**Cas d'usage:**

- Utilisateur membre de plusieurs groupes
- Un groupe a accès, mais pas cet utilisateur spécifique
- DENY prend le dessus

**Exemple:**

```
Groupe_Dev → Connexion accordée
Utilisateur Jean (membre Groupe_Dev)
DENY connexion à Jean → Jean ne peut pas se connecter
```

### 4.5 Utilisateurs de Base de Données

#### Caractéristiques

**Principes:**

- Créés au niveau base de données
- Rattachés à une connexion (login)
- Reçoivent les droits sur les objets
- Stockés dans `sys.database_principals`

**Sans utilisateur de BD:** Un utilisateur avec connexion mais sans utilisateur BD ne peut que:

- Consulter tables système
- Accéder aux bases avec compte `guest` actif
- Exécuter instructions sans autorisation (PRINT)

#### Utilisateurs Particuliers

**1. dbo (Database Owner)**

- ✅ Présent dans toutes les bases
- ✅ Membres de `sysadmin` mappés à dbo
- ✅ Compte `sa` mappé à dbo
- ❌ Ne peut être supprimé
- Objets créés par sysadmin → propriétaire = dbo

**2. guest**

- ✅ Présent dans toutes les bases
- ❌ Désactivé par défaut
- Permet accès aux connexions sans utilisateur de BD

**Activation/Désactivation de guest:**

```sql
-- Activer
GRANT CONNECT TO guest;

-- Désactiver
REVOKE CONNECT FROM guest;
```

**3. sys** et **INFORMATION_SCHEMA**

- Utilisateurs système
- Non rattachés à une connexion

#### Création d'Utilisateur

**Syntaxe:**

```sql
CREATE USER PierreUser
FOR LOGIN Pierre
WITH DEFAULT_SCHEMA = dbo;
```

**Modification:**

```sql
ALTER USER PierreUser
WITH NAME = NouveauNom,
     DEFAULT_SCHEMA = VentesSchema;
```

**Suppression:**

```sql
DROP USER PierreUser;
```

#### Consultation

**Liste des utilisateurs:**

```sql
SELECT * FROM sys.database_principals;
```

**Connexions et utilisateurs associés:**

```sql
SELECT s.name AS 'Connexion',
       p.name AS 'Utilisateur'
FROM sys.database_principals p
INNER JOIN sys.server_principals s
    ON s.sid = p.sid;
```

**Utilisateurs connectés:**

```sql
EXEC sp_who;
-- Affiche: loginname, hostname, dbname
```

### 4.6 Gestion des Schémas

#### Concept

**Objectif:** Dissocier utilisateurs des objets qu'ils créent

**Définition:** Ensemble logique d'objets dans une base de données

**Avantages:**

- 📖 Meilleure lisibilité
- 🔒 Sécurité simplifiée
- 👥 Partage facilité entre utilisateurs
- 🎯 Gestion des privilèges par schéma

#### Utilisation

**Propriétaire du schéma:**

- Gère les droits du schéma
- Gère les droits des objets
- Peut transférer la gestion

**Accès aux objets:**

```
-- Objet dans schéma par défaut
SELECT * FROM MaTable;

-- Objet hors schéma par défaut
SELECT * FROM AutreSchema.MaTable;
```

#### Gestion des Schémas

**Création:**

```sql
CREATE SCHEMA Ventes
AUTHORIZATION Pierre;
```

**Création avec objets:**

```sql
CREATE SCHEMA Ventes
AUTHORIZATION Pierre
    CREATE TABLE Clients (...)
    CREATE VIEW VueClients AS ...;
```

**Transfert d'objet:**

```sql
ALTER SCHEMA NouveauSchema
TRANSFER Ancien.ObjetTable;
```

**Suppression:**

```sql
DROP SCHEMA NomSchema;
-- ⚠️ Le schéma doit être vide
```

**Consultation:**

```sql
SELECT *
FROM sys.schemas;
-- Colonnes: name, schema_id, principal_id
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
GRANT CREATE TABLE
TO Pierre
WITH GRANT OPTION;

-- Privilège sur objet
GRANT SELECT, INSERT
ON schema.Table
TO Pierre
WITH GRANT OPTION;
```

**REVOKE (Retirer):**

```sql
-- Retirer privilège simple
REVOKE CREATE TABLE
FROM Pierre;

-- Retirer WITH GRANT OPTION uniquement
REVOKE GRANT OPTION FOR CREATE TABLE
FROM Pierre;

-- Retirer en cascade
REVOKE SELECT
ON schema.Table
FROM Pierre
CASCADE;
```

**DENY (Interdire):**

```sql
DENY DELETE
ON schema.Table
TO Pierre
CASCADE;
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
- **Transmettre** le privilège à d'autres

**Exemple:**

```sql
-- Jean peut créer des tables ET donner ce droit à d'autres
GRANT CREATE TABLE
TO Jean
WITH GRANT OPTION;

-- Jean accorde ensuite
GRANT CREATE TABLE TO Marie;
```

**Retrait en cascade:**

```sql
REVOKE CREATE TABLE
FROM Jean
CASCADE;
-- Retire aussi le privilège à Marie
```

#### Autorisations au Niveau Base de Données

**Portée:** Actions sur l'ensemble de la base

**Exemples:**

```sql
-- Accorder
GRANT CREATE PROCEDURE
TO Pierre
WITH GRANT OPTION;

-- Interdire
DENY CREATE TABLE
TO Marie;
```

#### Autorisations au Niveau Serveur

**Prérequis:** Être sur la base MASTER

**Exemples:**

```sql
USE master;
GRANT ALTER ANY LOGIN
TO AdminSecurite;
```

#### Consultation des Privilèges

**Vues système:**

- `sys.database_permissions`: Autorisations
- `sys.database_principals`: Comptes de sécurité

**Requête complète:**

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

1. **Rôles fixes** (prédéfinis)
2. **Rôles utilisateur** (personnalisés)

**Niveaux:**

- Serveur
- Base de données
- Application

#### Cumul de Privilèges

**Un utilisateur dispose de tous les privilèges:**

- ✅ Accordés directement à la connexion
- ✅ Via rôles de serveur
- ✅ Via rôles de base de données
- ✅ Accordés directement à l'utilisateur de BD

```
┌─────────────────────────────────────┐
│         NIVEAU SERVEUR              │
│  Connexion  ←───  Rôle Serveur      │
│      │          (Privilèges)         │
└──────│──────────────────────────────┘
       │
       ↓ (mapping)
┌─────────────────────────────────────┐
│      NIVEAU BASE DE DONNÉES         │
│  Utilisateur  ←───  Rôle BD         │
│      │          (Privilèges)         │
│      │                               │
│      └────→  Privilèges directs      │
└─────────────────────────────────────┘
```

### 4.9 Rôles de Serveur

#### Rôles Fixes Prédéfinis

|Rôle|Description|
|---|---|
|**sysadmin**|⭐ Super administrateur<br>Tous pouvoirs sur l'instance|
|**serveradmin**|Configuration paramètres serveur|
|**setupadmin**|Gestion serveurs liés<br>Procédures stockées système|
|**securityadmin**|Gestion connexions<br>⚠️ Peut élever ses privilèges|
|**processadmin**|Gestion des processus SQL Server<br>Terminer processus|
|**dbcreator**|Créer, modifier, supprimer bases de données|
|**diskadmin**|Gestion fichiers sur disque|
|**bulkadmin**|Exécution BULK INSERT|
|**public**|Rôle par défaut de toute connexion<br>Lister les bases|

#### Gestion des Rôles Serveur

**Création (rôle personnalisé):**

```sql
CREATE SERVER ROLE MonRoleServeur;
```

**Attribution:**

```sql
ALTER SERVER ROLE sysadmin
ADD MEMBER Pierre;
```

**Retrait:**

```sql
ALTER SERVER ROLE sysadmin
DROP MEMBER Pierre;
```

**Consultation:**

```sql
-- Liste des rôles
EXEC sp_helpsrvrole;

-- Membres d'un rôle
EXEC sp_helpsrvrolemember;
```

### 4.10 Rôles de Base de Données

#### Rôles Fixes Prédéfinis

|Rôle|Droits|
|---|---|
|**db_owner**|⭐ Propriétaire<br>Tous pouvoirs sur la base|
|**db_accessadmin**|Ajouter/supprimer utilisateurs|
|**db_datareader**|SELECT sur toutes les tables|
|**db_datawriter**|INSERT, UPDATE, DELETE sur toutes les tables|
|**db_ddladmin**|Ordres DDL (CREATE, ALTER, DROP)|
|**db_securityadmin**|Gérer rôles et autorisations|
|**db_backupoperator**|Sauvegardes de la base|
|**db_denydatareader**|❌ Interdire SELECT sur toute la base|
|**db_denydatawriter**|❌ Interdire INSERT, UPDATE, DELETE|

#### Gestion des Rôles de BD

**Création:**

```sql
CREATE ROLE RoleVentes
AUTHORIZATION Pierre;
```

**Attribution:**

```sql
ALTER ROLE db_datareader
ADD MEMBER Jean;
```

**Retrait:**

```sql
ALTER ROLE db_datareader
DROP MEMBER Jean;
```

**Suppression:**

```sql
DROP ROLE RoleVentes;
```

### 4.11 Rôles d'Application

#### Caractéristiques

**Spécificités:**

- ✅ Définis au niveau base de données
- ❌ Pas d'utilisateurs membres
- 🔐 Protégés par mot de passe
- 🎯 Droits nécessaires pour une application
- ⚡ Doivent être **activés** par un utilisateur
- 🔄 Prennent le dessus sur les privilèges utilisateur

**Cas d'usage:** Application client-serveur nécessitant des droits spécifiques, indépendamment de l'utilisateur connecté.

#### Gestion

**Création:**

```sql
CREATE APPLICATION ROLE RoleApp
WITH PASSWORD = 'M0tDeP@sse!',
     DEFAULT_SCHEMA = dbo;
```

**Modification:**

```sql
ALTER APPLICATION ROLE RoleApp
WITH PASSWORD = 'Nouv3@uP@ss',
     DEFAULT_SCHEMA = Ventes;
```

**Suppression:**

```sql
DROP APPLICATION ROLE RoleApp;
```

**Activation:**

```sql
EXEC sp_setapprole 'RoleApp',
     'M0tDeP@sse!';
```

**Important:** Une fois activé, le rôle d'application remplace tous les privilèges de l'utilisateur pour la session en cours.

---

## 📖 GLOSSAIRE

|Terme|Définition|
|---|---|
|**ACID**|Atomicité, Cohérence, Isolation, Durabilité - Propriétés des transactions|
|**CAL**|Client Access License - Licence d'accès client|
|**Connexion (Login)**|Compte d'accès au niveau serveur|
|**Credential**|Compte Windows lié à une connexion SQL pour accès ressources externes|
|**DBCC**|Database Console Commands - Commandes maintenance et diagnostic|
|**Extension**|Ensemble de 8 pages contiguës (64 Ko)|
|**Filegroup**|Groupe de fichiers logiques pour organisation du stockage|
|**Instance**|Installation indépendante de SQL Server sur un serveur|
|**Journal (Log)**|Fichier .ldf enregistrant toutes les modifications|
|**OLAP**|Online Analytical Processing - Base décisionnelle|
|**OLTP**|Online Transactional Processing - Base transactionnelle|
|**Page**|Unité de stockage de 8 Ko|
|**Partition**|Division d'une grande table en plusieurs sous-ensembles|
|**Rôle**|Regroupement de privilèges|
|**Schéma**|Ensemble logique d'objets dans une base|
|**Sécurisable**|Objet sur lequel des autorisations peuvent être accordées|
|**SGBDR**|Système de Gestion de Base de Données Relationnelles|
|**SMO**|SQL Server Management Objects - API d'administration|
|**SSMS**|SQL Server Management Studio - Principal outil graphique|
|**Transaction**|Ensemble indivisible d'opérations (tout ou rien)|
|**T-SQL**|Transact-SQL - Extension Microsoft du langage SQL|
|**Utilisateur (User)**|Compte d'accès au niveau base de données|
|**WAL**|Write-Ahead Logging - Écriture journal avant données|

---

## ✅ BONNES PRATIQUES

### Gestion des Bases de Données

#### Fichiers et Stockage

✅ **Séparer les fichiers:**

- Fichiers de données (.mdf, .ndf) sur disque rapide
- Fichiers journaux (.ldf) sur disque séparé
- TempDB sur disque dédié

✅ **Dimensionnement initial:**

- Définir SIZE appropriée (éviter extensions fréquentes)
- FILEGROWTH: Valeur fixe (MB) plutôt que % pour gros fichiers
- MAXSIZE: Limiter pour éviter saturation disque

✅ **Groupes de fichiers:**

- PRIMARY: Tables système uniquement
- Groupe séparé pour données utilisateur
- Groupe séparé pour index
- Groupe séparé pour objets volumineux (BLOB)

❌ **À éviter:**

- AUTO_SHRINK = ON (impact performances)
- FILEGROWTH trop petit (fragmentations multiples)
- Fichiers sur même disque que système d'exploitation

#### Transactions

✅ **Utiliser BEGIN TRAN explicitement** pour opérations critiques ✅ **SAVE TRAN** pour points de retour partiels ✅ **Transactions courtes** (minimiser verrouillage) ✅ **Gérer les erreurs** avec TRY-CATCH

❌ **À éviter:**

- Transactions longues (blocages utilisateurs)
- Oublier COMMIT (transaction ouverte)
- Nested transactions non contrôlées

### Sécurité

#### Authentification

✅ **Privilégier l'authentification Windows:**

- Gestion centralisée
- Politique de mots de passe robuste
- Audit Windows

✅ **Mode mixte uniquement si nécessaire:**

- Clients non-Microsoft
- Applications spécifiques

✅ **Politique de mots de passe:**

```sql
CHECK_EXPIRATION = ON
CHECK_POLICY = ON
```

#### Gestion des Privilèges

✅ **Principe du moindre privilège:**

- Accorder uniquement les droits nécessaires
- Utiliser des rôles personnalisés
- Éviter sysadmin/db_owner sauf nécessaire

✅ **Utiliser les rôles:**

- Créer des rôles métier
- Affecter utilisateurs aux rôles
- Gérer privilèges au niveau rôle

✅ **Schémas pour organisation:**

- Regrouper objets par fonctionnalité
- Gérer droits au niveau schéma
- Séparation logique des données

❌ **À éviter:**

- Compte `sa` pour applications
- WITH GRANT OPTION à outrance
- Utilisateur `guest` activé
- Privilèges directs sur objets (préférer rôles)

#### Connexions et Utilisateurs

✅ **Gestion rigoureuse:**

- Désactiver (DISABLE) plutôt que supprimer
- Révision périodique des connexions
- Documentation des rôles et privilèges

✅ **Base de données par défaut:**

- Définir pour chaque connexion
- Mais ne donne PAS de droits automatiques

✅ **DENY pour exceptions:**

- Utilisateur dans groupe autorisé mais doit être bloqué
- DENY prioritaire sur GRANT

### Maintenance

#### Sauvegardes

✅ **Stratégie de sauvegarde:**

- Sauvegarder MASTER après modifications serveur
- Sauvegardes complètes régulières
- Sauvegardes différentielles entre complètes
- Sauvegardes journaux (mode FULL recovery)

✅ **Tester les restaurations:**

- Vérifier régulièrement
- Documenter procédures

#### Monitoring

✅ **Surveillance quotidienne:**

- Espace disque disponible
- Taille des fichiers journaux
- Sessions actives (sp_who2)
- Journal des événements
- Tâches planifiées

✅ **Statistiques:**

- AUTO_CREATE_STATISTICS = ON
- AUTO_UPDATE_STATISTICS = ON
- MAJ manuelle si nécessaire

### Performance

#### Optimisation Tables

✅ **Partitionnement pour grandes tables:**

- Tables > plusieurs millions de lignes
- Requêtes ciblant périodes spécifiques
- Maintenance par partition

✅ **Index appropriés:**

- Groupe de fichiers séparé pour index
- Reconstruction régulière (fragmentation)

✅ **Typage approprié:**

- Pas de VARCHAR(MAX) si taille connue
- Types appropriés (éviter conversions)

#### Configuration

✅ **Options de BD adaptées:**

- READ_WRITE pour bases production
- READ_ONLY pour bases archives/reporting
- SINGLE_USER pour maintenance uniquement

✅ **Ressources serveur:**

- Mémoire suffisante (éviter swapping)
- Processeurs exploités (parallélisme)
- Disques rapides (SSD pour TempDB)

### Administration

#### Documentation

✅ **Documenter:**

- Architecture des bases
- Schémas de partitionnement
- Rôles et privilèges
- Procédures de sauvegarde/restauration
- Procédures d'urgence

✅ **Utiliser les outils:**

- SSMS pour gestion graphique
- T-SQL pour scripts reproductibles
- Versionner les scripts (Git)

#### Automatisation

✅ **SQL Server Agent:**

- Tâches de maintenance planifiées
- Alertes sur événements critiques
- Opérateurs notifiés

✅ **Scripts T-SQL:**

- Automatisation déploiements
- Vérifications régulières
- Génération rapports

---

## 🎓 CONSEILS POUR L'EXAMEN / CERTIFICATION

### Points Clés à Maîtriser

#### Chapitre 1: Présentation

- [ ] Différence OLTP vs OLAP
- [ ] Composants SQL Server (services)
- [ ] Bases système et leurs rôles
- [ ] Vues et procédures système importantes

#### Chapitre 2: Installation

- [ ] Caractéristiques de chaque édition
- [ ] Notion d'instance
- [ ] Modes de licence (par cœur vs CAL)
- [ ] SSMS et ses fonctionnalités

#### Chapitre 3: Gestion BD

- [ ] Structure physique (pages, extensions)
- [ ] Transactions (BEGIN, COMMIT, ROLLBACK, SAVE)
- [ ] Fichiers de données vs journaux
- [ ] Groupes de fichiers
- [ ] Partitionnement (fonction, schéma, table)
- [ ] Options de configuration

#### Chapitre 4: Sécurité

- [ ] Modes d'authentification
- [ ] Connexions vs Utilisateurs
- [ ] Schémas
- [ ] GRANT, REVOKE, DENY
- [ ] Rôles serveur vs rôles BD
- [ ] Hiérarchie autorisations
- [ ] Rôles d'application

### Pièges Fréquents

❌ **Confusions courantes:**

- Connexion ≠ Utilisateur
- Base par défaut ≠ Droits sur la base
- REVOKE ≠ DENY (DENY est plus fort)
- Extension (64 Ko) ≠ Page (8 Ko)
- OLTP ≠ OLAP

❌ **Erreurs typiques:**

- Oublier CASCADE avec REVOKE
- Confondre rôles serveur et rôles BD
- Ne pas distinguer .mdf et .ldf
- Mélanger fonction et schéma de partitionnement

### Méthode de Révision

1. **Lire ce document** (vue d'ensemble)
2. **Faire des fiches** par concept
3. **Pratiquer en T-SQL** (syntaxes)
4. **Créer des schémas** (architecture, sécurité)
5. **Refaire les TDs/Labs** du cours
6. **Questions types** examen blanc

### Questions Types à Préparer

**QCM fréquents:**

- Quelle édition pour quel besoin ?
- Combien de licences dans tel scénario ?
- Quelle commande pour telle opération ?
- Quel rôle pour tel privilège ?
- Quelle base système contient quoi ?

**Questions ouvertes:**

- Expliquer le fonctionnement des transactions
- Décrire l'architecture de sécurité
- Procédure de partitionnement d'une table
- Différence OLTP/OLAP avec exemples

**Pratique:**

- Créer une base avec groupes de fichiers
- Mettre en place un partitionnement
- Créer connexions, utilisateurs, rôles
- Accorder/retirer privilèges

---

## 📌 RÉSUMÉ ULTRA-CONDENSÉ (Mémo Rapide)

### Éditions

- **Enterprise**: Tout, gros volumes
- **Standard**: PME, fonctionnalités essentielles
- **Express**: GRATUIT, 10 Go max, 1 Go RAM
- **Developer**: Comme Enterprise, DEV uniquement

### Bases Système

- **MASTER**: Configuration serveur ⚠️ CRITIQUE
- **MODEL**: Template nouvelles bases
- **TEMPDB**: Stockage temporaire (recréée)
- **MSDB**: SQL Agent (tâches)
- **RESOURCE**: Objets système

### Fichiers

- **.mdf**: Fichier primaire données
- **.ndf**: Fichiers secondaires données
- **.ldf**: Fichier journal (WAL)
- Page: 8 Ko | Extension: 64 Ko (8 pages)

### Transactions

- BEGIN TRAN → SAVE TRAN → ROLLBACK/COMMIT
- ACID: Atomicité, Cohérence, Isolation, Durabilité

### Sécurité

- **Connexion** (Login) → Serveur
- **Utilisateur** (User) → Base de données
- **Windows** > Mixte (sécurité)
- DENY > GRANT > Rien

### Rôles Serveur

- **sysadmin**: Super admin
- **dbcreator**: Créer bases
- **securityadmin**: Gérer connexions

### Rôles BD

- **db_owner**: Propriétaire
- **db_datareader**: SELECT toutes tables
- **db_datawriter**: INSERT/UPDATE/DELETE toutes tables

### Partitionnement

1. Fonction de partitionnement (plages)
2. Schéma de partitionnement (groupes fichiers)
3. Table partitionnée (colonne partitionnement)

---

**📅 Document généré pour révision examen/certification SQL Server**  
**✍️ Basé sur les supports de cours N'SOUGAN Folly W.**  
**📖 Version: Résumé Essentiels Théoriques**

---

_Bon courage pour vos révisions ! 🚀_