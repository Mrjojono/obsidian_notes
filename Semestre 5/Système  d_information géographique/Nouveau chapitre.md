
quel est le systeme coordonnées  géographique  pour le togo 
WS84 
EPSV44



### **Dans QGIS, quel outil permet de choisir les données à charger en fonction du type de données ?**

Dans QGIS, l’outil **Gestionnaire de sources de données** permet de choisir les données à charger selon leur type.

### **Quel élément permet au lecteur de carte de comprendre la signification des caractéristiques affichées ?**

L’élément qui permet de comprendre la signification des caractéristiques est la **légende**.



### **Quel outil permet de configurer le fonctionnement d’une couche selon l’échelle ?**

L’outil utilisé est la **visibilité dépendante de l’échelle** (ou plage de visibilité).  
Il se configure dans les **propriétés de la couche**, onglet _Général_ ou _Rendu_, et permet d’afficher ou masquer une couche selon une échelle minimale et maximale.


# Synthèse Fondamentale sur les Systèmes d'Information Géographique (SIG)

## Résumé Exécutif

Un Système d'Information Géographique (SIG) dépasse la simple cartographie assistée par ordinateur pour s'affirmer comme un outil complexe de gestion et d'aide à la décision. Il repose sur la fusion de données spatiales (localisation, forme) et de données attributaires (caractéristiques descriptives). Le SIG permet de répondre à cinq questions fondamentales : **Où**, **Quoi**, **Comment**, **Quand** et **Et si** (scénarios prospectifs). Son efficacité dépend de la qualité de l'acquisition des données (numérisation, télédétection) et de la précision du référencement géographique (projections). L'usage de logiciels spécialisés comme MapInfo ou QGIS permet d'exploiter ces informations via des analyses thématiques, des requêtes SQL et des superpositions de couches pour une gestion optimisée de l'espace urbain, environnemental ou technique.

--------------------------------------------------------------------------------

## 1. Concepts et Architecture du SIG

### 1.1 Définitions et Finalités

Un SIG est défini comme un système informatique capable de rassembler, organiser, gérer, analyser, combiner et présenter des informations localisées géographiquement. Selon les sources, il est perçu sous trois angles complémentaires :

- **Un Système de Gestion de Base de Données (SGBD) :** Pour la saisie, le stockage et l'interrogation de données localisées.
- **Un outil de synthèse :** Structuré pour extraire des informations utiles à la décision.
- **Un environnement d'analyse :** Pour simuler des phénomènes naturels ou gérer des infrastructures.

### 1.2 Les "5 A" : Les fonctionnalités clés

Tout système complet doit remplir cinq fonctions essentielles :

1. **Abstraction :** Modélisation du monde réel.
2. **Acquisition :** Récupération et alimentation du système en données.
3. **Archivage :** Stockage et structuration pour une interrogation facile.
4. **Analyse :** Cœur du système, permettant de répondre aux requêtes spatiales.
5. **Affichage :** Restitution graphique et cartographique.

### 1.3 Composantes du système

Le succès d'un projet SIG repose sur cinq piliers :

- **Matériels :** Serveurs, PC, GPS, satellites, drones.
- **Logiciels :** Outils d'exploitation (MapInfo, ArcGIS, QGIS).
- **Données :** Données "métiers" (activités spécifiques) et données de base (réseaux, topographie).
- **Utilisateurs :** Experts techniques ou décideurs.
- **Processus/Méthodes :** Cadre méthodologique définissant l'activité.

--------------------------------------------------------------------------------

## 2. La Donnée Géographique : Structure et Typologie

Pour qu'un objet spatial soit exploitable, il doit posséder une **position géographique**, une **relation spatiale (topologie)** et des **attributs**.

### 2.1 Types de données

|   |   |   |
|---|---|---|
|Catégorie|Description|Exemples|
|**Spatiales**|Déterminent la localisation, la forme et la taille.|Point (punaise), Ligne (route), Polygone (parcelle).|
|**Associées**|Complètent la représentation géométrique.|Nom d'une rue, diamètre d'une conduite, propriétaire.|

Les données associées se divisent en données de **classification** (catégorie d'objet), d'**identification** (nom ou numéro unique) et **attributaires** (informations supplémentaires).

### 2.2 Modes de représentation (Vecteur vs Raster)

- **Mode Vecteur :** Représente les objets par des points, lignes et polygones.
    - _Avantages :_ Précision, facilité de transformation des coordonnées, extraction aisée des détails.
    - _Inconvénients :_ Croisement de couches complexe nécessitant une topologie parfaite.
- **Mode Raster (Matriciel) :** Décrit la surface point par point (pixels). Utilisé pour l'imagerie et les Modèles Numériques d'Élévation (MNE).
    - _Avantages :_ Idéal pour les données surfaciques et les traitements numériques (filtres, pentes).
    - _Inconvénients :_ Lourd en stockage, inadapté à la recherche d'itinéraires (réseaux).

--------------------------------------------------------------------------------

## 3. Acquisition et Référencement Géographique

### 3.1 Méthodes d'acquisition

Les données sont dites "multisources" et proviennent de :

- **Numérisation (Digitalisation) :** Récupération de la géométrie depuis des plans papier via une table à digitaliser ou un curseur.
- **Balayage électronique (Scanning) :** Transformation d'une carte papier en fichier numérique.
- **Photogrammétrie et Télédétection :** Utilisation de photos aériennes ou d'images satellitaires.
- **GPS :** Mesures directes sur le terrain.
- **Import de fichiers :** Conversion de formats externes (DXF/DWG, Excel, etc.).

### 3.2 Projections et Coordonnées

Le passage de la terre (3D) à la carte (2D) nécessite un système de projection pour minimiser les distorsions.

- **Systèmes courants :** Lambert (conique), Mercator (cylindrique), UTM (Universal Transverse Mercator).
- **En Algérie :** Utilisation du système Lambert (Nord/Sud) et de 4 fuseaux UTM (29 à 32).
- **Emplacement :** Il peut être **absolu** (coordonnées longitude/latitude) ou **relatif** (par rapport à un repère proche).

--------------------------------------------------------------------------------

## 4. Exploitation Technique et Logicielle

### 4.1 Logiciels de référence

- **Propriétaires :** MapInfo (langage MapBasic, environnement Windows), ArcGIS (ESRI, langages C++/Python).
- **Open Source :** QGIS (langage C++, supporte Python pour les extensions), compatible avec les bases de données PostGIS via le modèle SFS (Simple Feature for SQL).

### 4.2 Focus sur MapInfo Professional

MapInfo utilise des **tables** constituées de plusieurs fichiers liés :

- `.TAB` : Description de la table.
- `.MAP` : Informations géométriques.
- `.DAT` : Données attributaires.
- `.WOR` : Document (Workspace) enregistrant l'ensemble de la session de travail.

### 4.3 Analyse et Requêtes

L'interrogation des données se fait principalement par le langage **SQL (Structured Query Language)**.

- **Requêtes attributaires :** Sélection selon des critères textuels ou numériques (ex: `Salaire > 20000`).
- **Requêtes spatiales :** Sélection selon la proximité ou l'inclusion (ex: objets à moins de 300m d'une autoroute).
- **Analyse thématique :** Permet de varier la sémiologie (couleurs, symboles) pour mettre en évidence des tendances (densité de population, longueurs de réseaux).

--------------------------------------------------------------------------------

## 5. Domaines d'Application

Les SIG interviennent à différentes échelles pour soutenir la gestion technique et la décision politique :

- **Grandes échelles (Urbain/Technique) :** Gestion foncière et cadastrale, planification urbaine, gestion des réseaux (eau, gaz, électricité), voirie et signalisation.
- **Moyennes et petites échelles (Territoire/Environnement) :** Études d'impact environnemental, ingénierie routière, prévention des risques naturels, gestion des ressources naturelles (hydrographie, géologie).

--------------------------------------------------------------------------------

## 6. Synthèse des Concepts Académiques Connexes

Les sources mentionnent également des principes de développement qui encadrent la création de ces outils :

- **Génie Logiciel :** Importance des cycles de vie (cascade, itératif, agile) et de l'analyse des besoins (cahier des charges, diagrammes UML comme les cas d'utilisation ou de classes).
- **Administration de Bases de Données :** Utilisation de schémas multidimensionnels (en étoile, en flocon) pour les entrepôts de données (Data Warehouse) et outils décisionnels (ETL, OLAP).
- **Sécurité :** Protection contre les injections SQL et gestion des privilèges (modèles discrétionnaires vs obligatoires) pour garantir l'intégrité des données géographiques et personnelles.