
# Les Systèmes d'Information d'Aide à la Décision (SIAD) et l'Informatique Décisionnelle

Ce document de synthèse analyse les concepts fondamentaux, les architectures techniques et les méthodologies de mise en œuvre des systèmes d'information dédiés au pilotage stratégique des organisations, tels que présentés dans le cours SIAD 2023-2024.

## Résumé Exécutif

Le passage de l'informatique transactionnelle (OLTP) à l'informatique décisionnelle (OLAP) répond à un besoin critique des organisations : transformer une masse de données brutes et éparpillées en informations pertinentes pour le pilotage stratégique. Le **Data Warehouse** (entrepôt de données) constitue le socle de cette architecture, offrant une base de données intégrée, historisée et non volatile. La technologie **OLAP** permet ensuite une analyse multidimensionnelle (navigation par "cubes"), facilitant l'exploration intuitive des indicateurs de performance. La mise en œuvre de tels systèmes repose sur des processus rigoureux d'**ETL** (Extraction, Transformation, Chargement) et une modélisation spécifique (en étoile ou en flocon).

--------------------------------------------------------------------------------

## 1. Fondements des Systèmes d'Information (SI)

### 1.1. Définition et Structure

Un système est un ensemble d'éléments en interaction dynamique organisés pour atteindre un objectif. Au sein d'une organisation, on distingue trois sous-systèmes principaux :

- **Système opérant (opérationnel) :** Exécute les tâches de production et de transformation.
- **Système de pilotage (décisionnel) :** Guide l'organisation vers ses objectifs par la planification et le contrôle.
- **Système d'information (SI) :** Sert d'interface entre les deux précédents. Selon Robert Reix, il regroupe les ressources (humaines, matérielles, logicielles) permettant d'acquérir, traiter, stocker et diffuser l'information.

### 1.2. Distinction Transactionnel vs Décisionnel

L'informatique moderne se sépare en deux branches aux finalités opposées :

|   |   |   |
|---|---|---|
|Caractéristique|Informatique Transactionnelle (OLTP)|Informatique Décisionnelle (OLAP)|
|**Objectif**|Gestion quotidienne des transactions (saisie, mise à jour).|Aide à la décision et mesure des résultats.|
|**Données**|Actuelles, très détaillées, volatiles.|Historisées, synthétiques, non volatiles.|
|**Utilisateurs**|Opérationnels (nombreux accès simultanés).|Décideurs et analystes.|
|**Modèle**|Relationnel normalisé (8 formes normales de Codd).|Multidimensionnel (Hypercubes).|

--------------------------------------------------------------------------------

## 2. Le Socle Décisionnel : Le Data Warehouse

### 2.1. Définition (Bill Inmon)

Le Data Warehouse (DW) est une collection de données **orientées sujet**, **intégrées**, **non volatiles** et **historisées**. Contrairement aux systèmes de production organisés par fonctions (paie, stock), le DW est structuré par thèmes transversaux (clients, ventes).

### 2.2. Architecture et Données

Un DW se structure en quatre classes de données selon leur niveau de synthèse :

1. **Données détaillées :** Événements récents issus du transactionnel.
2. **Données agrégées :** Résultats pré-calculés pour optimiser les performances.
3. **Métadonnées :** "L'annuaire" du DW décrivant la structure et les règles de transformation.
4. **Données historisées :** Archivage à long terme pour l'analyse de tendances.

**Les types d'architectures :**

- **Réelle :** SGBD séparé (recommandé pour la performance).
- **Virtuelle :** Accès direct aux données de production via middleware (peu utilisé).
- **Remote :** Combinaison hybride.

### 2.3. Data Marts (Magasins de données)

Le Data Mart est une version restreinte du DW, ciblée sur un département spécifique (ex: Marketing). Il permet de réduire la complexité et les coûts tout en offrant des résultats rapides (6 à 12 mois contre 9 à 18 pour un DW complet).

--------------------------------------------------------------------------------

## 3. Méthodologies de Construction et Alimentation

### 3.1. Approches de Conception

Deux théories majeures s'affrontent :

- **Top-Down (Inmon) :** Création d'un DW global détaillé pour toute l'entreprise, puis déduction des Data Marts. Plus cohérent mais lourd.
- **Bottom-Up (Kimball) :** Le DW est l'union de tous les Data Marts. Plus rapide et flexible, mais risque de redondances.
- **Middle-Out (Hybride) :** Modélisation globale itérative.

### 3.2. Le processus ETL (Extract, Transform, Load)

C'est la phase la plus critique et coûteuse (60 à 90 % de la charge d'un projet) :

1. **Extraction :** Collecte des données utiles dans les sources hétérogènes.
2. **Transformation :** Nettoyage (suppression des doublons), harmonisation des formats (ex: dates), et agrégations.
3. **Chargement :** Insertion finale dans le DW ou le cube, nécessitant une optimisation des index pour gérer les gros volumes.

--------------------------------------------------------------------------------

## 4. Concept OLAP et Analyse Multidimensionnelle

### 4.1. Principes de l'OLAP

L'OnLine Analytical Processing (OLAP) repose sur le concept d'**Hypercube**. Les données sont visualisées selon des axes d'analyse (Dimensions) et des indicateurs chiffrés (Mesures/Faits).

**Navigation dans les données :**

- **Drill-Down / Drill-Up :** Passer d'une vue globale à une vue détaillée (ex: Année -> Mois -> Jour) et inversement.
- **Slice and Dice :** Extraire une tranche de données ou faire pivoter les axes du tableau.
- **Rotate :** Changer la dimension de lecture de l'hypercube.

### 4.2. Modélisation Dimensionnelle

Elle vise la performance et la lisibilité pour l'utilisateur final :

- **Schéma en Étoile :** Une table de faits centrale (mesures) reliée à des tables de dimensions (axes). Simple et performant.
- **Schéma en Flocon :** Les tables de dimensions sont normalisées (éclatées en sous-hiérarchies). Économise l'espace mais complexifie les requêtes.
- **Schéma en Constellation :** Plusieurs tables de faits partageant les mêmes dimensions.

### 4.3. Technologies OLAP

- **MOLAP (Multidimensional) :** Stockage dans un format propriétaire optimisé. Très rapide mais limité en volume (quelques Go).
- **ROLAP (Relational) :** Simulation du multidimensionnel sur une base relationnelle. Adapté aux très gros volumes.
- **HOLAP (Hybrid) :** Données détaillées en relationnel et agrégats en multidimensionnel.

--------------------------------------------------------------------------------

## 5. L'Écosystème BI Microsoft

Microsoft propose une suite intégrée couvrant l'ensemble de la chaîne décisionnelle via **SQL Server** et **Office** :

### 5.1. Composants SQL Server

- **SSIS (Integration Services) :** L'outil ETL pour l'alimentation et l'automatisation.
- **SSAS (Analysis Services) :** Le moteur de modèles sémantiques (Multidimensionnel ou Tabulaire).
- **SSRS (Reporting Services) :** Serveur de rapports statiques et dynamiques.
- **MSSQL Server :** Le moteur relationnel optimisé pour le stockage des schémas en étoile.

### 5.2. Outils de Front-End et Office

Excel est l'outil privilégié de consommation des données :

- **Power Pivot :** Création de modèles sémantiques en mémoire.
- **Power Query :** ETL personnel pour connecter et nettoyer diverses sources.
- **Power View :** Reporting dynamique et intuitif.

### 5.3. Outils d'Administration et Développement

- **SSDT (SQL Server Data Tools) :** Environnement basé sur Visual Studio pour le développement de projets (ETL, Cubes, Rapports).
- **SSMS (SQL Server Management Studio) :** Interface de gestion des instances de serveurs et des bases de données.

--------------------------------------------------------------------------------

## Bibliographie de Référence

- **R. Kimball :** Guide pratique de modélisation dimensionnelle.
- **B. Inmon :** Concepts du Data Warehouse.
- **E.F. Codd :** Inventeur du SGBDR et théoricien de l'OLAP.