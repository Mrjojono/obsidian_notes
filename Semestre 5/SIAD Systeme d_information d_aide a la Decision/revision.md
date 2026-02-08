# SYSTEMES D'INFORMATION D'AIDE A LA DECISION (SIAD)

## Resume Complet du Cours


---

## FICHE DE REVISION SYNTHETIQUE

Cette section regroupe les definitions, concepts cles et syntaxes essentielles a connaitre sur les SIAD et les bases de donnees decisionnelles.

### 1. OLTP ET OLAP

#### OLTP (Online Transaction Processing)

Bases de donnees destinees au traitement des operations courantes.

**Caracteristiques:**

- Beaucoup de transactions courtes
- Donnees tres detaillees
- Operations: INSERT, UPDATE, DELETE
- Forte contrainte d'integrite
- Donnees actuelles (pas d'archives)
- Acces unitaires
- Mise a jour interactive possible
- Haute disponibilite en continu

**Exemples:**

- Gestion de comptes bancaires
- Systemes de facturation
- Applications de gestion (ERP)
- Inventaires de magasins
- Reservations d'hotel

#### OLAP (Online Analytical Processing)

Bases orientees analyse et aide a la decision.

**Caracteristiques:**

- Peu de mises a jour
- Donnees historiques
- Requetes complexes (SUM, AVG, GROUP BY)
- Donnees agregees
- Grande quantite de donnees par traitement
- Utilisation aleatoire
- Coherence globale
- Pas de mise a jour interactive

**Exemples:**

- Analyse des ventes
- Tableaux de bord
- Reporting strategique
- Analyse de tendances
- Previsions

**Comparaison OLTP vs OLAP:**

|Element|OLTP|OLAP|
|---|---|---|
|Objectif|Gestion|Decision|
|Donnees|Actuelles|Historiques|
|Requetes|Simples|Complexes|
|Utilisateurs|Operationnels|Strategiques|
|Orientation|Application|Activite (sujet)|
|Volume traite|Petit|Grand|
|Type d'acces|Unitaire|Ensemble|
|Mise a jour|Frequente|Rare|

### 2. ENTREPOT DE DONNEES (DATA WAREHOUSE)

#### Definition (William "Bill" Inmon)

"Le Data Warehouse est une collection de donnees orientees sujet, integrees, non volatiles et historisees, organisees pour le support d'un processus d'aide a la decision."

#### Quatre Proprietes Fondamentales

**1. Oriente Sujet:**

- Organisation autour des sujets majeurs de l'entreprise
- Exemple: clients, produits, ventes
- Structure par theme
- Ensemble des informations utiles sur un sujet

**2. Integre:**

- Donnees mises en forme et unifiees
- Description et codage uniques
- Consolidation pour vue homogene
- Phase d'integration: 60 a 90% de la charge totale

**3. Historise:**

- Donnee jamais mise a jour
- Referentiel temps associe
- Identification valeur particuliere dans le temps
- Conservation de l'historique

**4. Non Volatile:**

- Donnees non modifiables
- Meme requete = meme resultat (avec date reference)
- Consequence de l'historisation

#### Differences Systeme de Production vs Data Warehouse

|Critere|Systeme de Production|Data Warehouse|
|---|---|---|
|Niveau de detail|Tres detaille|Synthetique, parfois detaille|
|Utilisateurs|Une ou quelques fonctions|Plusieurs fonctions|
|Donnees figees|Non - evolution temps reel|Oui - archivage|
|Historique|Non|Oui|
|Operations|Ajout/MAJ/consultation|Consultation uniquement|
|Redondance|Uniques (theorie)|Peuvent etre redondantes|
|Structure|Statique, contenu variable|Flexible|

### 3. PROCESSUS ETL

#### ETL = Extract - Transform - Load

**1. EXTRACT (Extraction):**

- Collecte des donnees utiles
- Depuis systemes de production
- Identification donnees modifiees
- Extraction minimum de donnees
- Planification des extractions

**Technologies:**

- Passerelles (editeurs BD)
- Utilitaires de replication
- Outils specifiques d'extraction
- Developpement sur mesure

**2. TRANSFORM (Transformation):**

**Nettoyage:**

- Suppression des doublons
- Attribution valeurs coherentes
- Harmonisation formats (exemple: date jj/mm/aaaa)

**Operations:**

- Application de filtres predefinis
- Agregation ou calculs
- Association champs sources/cibles
- Analyses lexicales (Boulvd = Bd = Boulevard)

**3. LOAD (Chargement):**

- Derniere phase d'alimentation
- Phase delicate (volumes importants)
- Maitrise structures SGBD necessaire
- Optimisation tables et index
- Techniques de parallelisation

**Methodes de transfert:**

- Transfert de fichiers (creation puis transfert)
- Transfert base a base (extraction, nettoyage, transfert)

**Note Importante:** Phase d'alimentation = difficulte technique majeure et la plus couteuse

### 4. MODELISATION DECISIONNELLE

#### Schema en Etoile (Star Schema)

**Structure:**

- 1 table de faits centrale
- Plusieurs tables de dimensions
- Simple et performant

**Table de Faits:**

- Contient les mesures (montant, quantite)
- Cles etrangeres vers dimensions
- Volume tres important (lignes)
- Granularite definie

**Tables de Dimensions:**

- Temps
- Client
- Produit
- Localisation
- Contiennent attributs descriptifs

**Avantages:**

- Facilite de navigation
- Nombre de jointures limite
- Gestion des agregats

**Inconvenients:**

- Toutes dimensions ne concernent pas les mesures
- Redondances dans dimensions
- Alimentation complexe

#### Schema en Flocon (Snowflake)

**Structure:**

- Variante du schema en etoile
- Dimensions normalisees en sous-hierarchies
- Plus complexe

**Avantages:**

- Normalisation des dimensions
- Economie d'espace disque
- Reduction du volume

**Inconvenients:**

- Modele plus complexe (nombreuses jointures)
- Requetes moins performantes
- Navigation difficile

#### Schema en Constellation

**Structure:**

- Plusieurs tables de faits
- Partage de certaines dimensions
- Modele pour DW global ou d'entreprise

### 5. MESURES ET DIMENSIONS

#### Mesures (Metriques)

**Definition:** Valeurs numeriques agregables

**Operations:**

- SUM (somme)
- COUNT (comptage)
- AVG (moyenne)
- MIN (minimum)
- MAX (maximum)

**Exemples:**

- Chiffre d'affaires
- Quantite vendue
- Nombre de commandes
- Prix moyen

**Types selon axe temps:**

**Variables de flux:**

- Variations enregistrees
- Montant periode = cumul variations
- Exemple: CA mensuel = somme CA quotidiens

**Variables d'etat:**

- Contenu a instant t
- Cumul periode ≠ valeur periode
- Exemple: stock trimestriel = stock au 1er trimestre

#### Dimensions (Axes d'Analyse)

**Definition:** Axes d'analyse essentiels

**Exemples courants:**

- Temps (indispensable)
- Produit
- Client
- Localisation geographique
- Fournisseur

**Attributs:**

- Nom, description
- Hierarchies (pere/fils)
- Distinction: analyse vs information

**Hierarchies:**

- Permet d'affiner les dimensions
- Decomposition pour niveaux de detail
- Navigation du global au detail

**Exemple Temps:**

- Annee → Trimestre → Mois → Semaine → Jour

**Exemple Geographie:**

- Pays → Region → Ville

**Granularite:** Niveau de detail des donnees de la dimension

**Cles:**

**Cle fonctionnelle (operationnelle):**

- Commune avec systeme OLTP
- Identifie de maniere unique
- Utilisee pour mise a jour

**Cle technique (de substitution):**

- Propre a l'environnement decisionnel
- Generee par sequence
- Aucun sens fonctionnel
- Nombres entiers (jointures optimisees)
- Permet suivi evolution (exemple: client demenage)

**Variation des Dimensions:**

**Type 1 (ecrasement):**

- Nouvelle valeur remplace ancienne
- Faits comptabilises sur derniere valeur
- Simple mise a jour

**Type 2 (historisation):**

- Creation nouvel enregistrement
- Memorisation modification
- Nouveaux faits rattaches au nouvel enregistrement
- Colonnes necessaires: ValideDeb ut, ValideFin

### 6. DIMENSION TEMPS

**Importance:** Dimension indispensable en SIAD

**Attributs courants:**

- Jour
- Jour de la semaine
- Semaine
- Mois
- Trimestre
- Semestre
- Annee
- Jour ferie (oui/non)
- Jour ouvre (oui/non)

**Hierarchies possibles:**

- Annee → Semestre → Trimestre → Mois → Jour
- Annee → Semaine → Jour

### 7. CUBE OLAP

**Definition:** Schema en etoile dans lequel certains agregats ont ete precalcules, avec couche metier.

**Permet:** Analyser les donnees selon plusieurs dimensions simultanement

**Operations OLAP:**

**Roll-up (agregation):**

- Montee dans la hierarchie
- Du detail vers la synthese
- Exemple: jour → mois → annee

**Drill-down (detail):**

- Descente dans la hierarchie
- De la synthese vers le detail
- Exemple: annee → mois → jour

**Slice (tranche):**

- Extraction d'une vue
- Fixation d'une dimension
- Exemple: ventes de l'annee 2023

**Dice (sous-cube):**

- Selection sur plusieurs dimensions
- Extraction sous-ensemble
- Exemple: ventes 2023 de la region Nord

**Pivot (rotation):**

- Rotation des axes
- Changement de perspective
- Permutation lignes/colonnes

**Rotate:**

- Rotation hypercube
- Presentation face differente

**Data Surfing:**

- Navigation libre et intuitive
- Modification dynamique axes d'analyse
- Application nouveaux filtres

### 8. SQL SERVER - CONCEPTS UTILES

#### Nom Complet d'un Objet

**Syntaxe:**

```
Serveur.BaseDeDonnees.Schema.Objet
```

**Exemple:**

```
SERVEUR01.SOCIETE.dbo.Clients
```

**Par defaut:** Seul le nom de l'objet est precise

#### Fichiers d'une Base SQL Server

**.mdf (Master Data File):**

- Fichier primaire de donnees
- Contient le catalogue de base de donnees
- Obligatoire

**.ndf (Next Data File):**

- Fichiers secondaires de donnees
- Contiennent objets et donnees utilisateurs
- Facultatif

**.ldf (Log Data File):**

- Fichier journal des transactions
- Enregistre toutes modifications
- Obligatoire

#### Transactions SQL

**Debut explicite:**

```sql
BEGIN TRANSACTION
```

**Point de sauvegarde:**

```sql
SAVE TRANSACTION nom_point
```

**Validation:**

```sql
COMMIT TRANSACTION
```

**Annulation:**

```sql
ROLLBACK TRANSACTION [nom_point]
```

**Important:** Annulation partielle possible uniquement si SAVEPOINT existe

#### Journal des Transactions (Write Ahead Logging)

**Principe:** SQL Server ecrit d'abord dans le journal, puis dans les donnees

**Garantit:**

- Atomicite (tout ou rien)
- Durabilite (persistance)
- Recuperation apres crash
- Coherence des donnees

**Fonctionnement:**

1. Modification en memoire
2. Ecriture immediate vers journal (.ldf)
3. Ecriture differee vers fichiers donnees (.mdf/.ndf)

#### Agent SQL Server

**Service permettant:**

- Taches planifiees
- Sauvegardes automatiques
- Jobs ETL
- Alertes
- Surveillance SQL Server

**Nom du service:**

- Instance par defaut: SQL Server Agent (MSSQLSERVER)
- Instance nommee: SQL Server Agent (<NomInstance>)

### 9. DATA MART

**Definition:** Petite base de donnees decisionnelle moins couteuse que le Data Warehouse, plus legere, destinee a quelques utilisateurs d'un departement.

**Caracteristiques:**

- Structure tres ciblee
- Pilotee par besoins utilisateurs
- Meme vocation que DW
- Problematique precise
- Nombre restreint de sources

**Comparaison Data Warehouse vs Data Mart:**

|Aspect|Data Warehouse|Data Mart|
|---|---|---|
|Cible|Toute l'entreprise|Departement|
|Implication SI|Elevee|Faible/Moyenne|
|Base de donnees|SQL serveur|SQL milieu gamme, multidim.|
|Modeles|Echelle entreprise|Departement|
|Champ applicatif|Multi sujets, neutre|Quelques sujets, specifique|
|Sources|Multiples|Quelques-unes|
|Taille|Centaine de GO+|1 a 2 dizaines de GO|
|Temps mise en place|9 a 18 mois (3 etapes)|6 a 12 mois (etapes)|

### 10. ROLE DES SIAD

**Definition:** Systeme d'Information d'Aide a la Decision

**Permet:**

- Aider a la prise de decision
- Analyser les performances
- Anticiper les tendances
- Eclairer les choix strategiques
- Suivre evolution activite

**Utilisateurs concernes:**

- Managers
- Decideurs
- Directeurs
- Analystes
- Controleurs de gestion

**Trois categories de SIAD:**

**SIAD Operationnel:**

- Evite surcharge mentale operateur
- Solutions pour situations complexes
- Reponse a l'urgence
- Systemes experts

**SIAD de Gestion:**

- Pour responsables operationnels
- Indicateurs et alarmes quotidiens
- Pilotage travail operateurs
- Respect normes qualite

**SIAD Strategique:**

- Pour dirigeants
- Series chronologiques mensuelles
- Efficacite et positionnement
- Resultats, parts de marche, tendances

### 11. OUTILS DU SIAD

**Tableaux de bord:**

- Comportant alertes
- Indicateurs cles
- Suivi performance

**Tableaux preformates:**

- Statistiques activite
- Statistiques environnement
- Resultats essentiels

**Hypercubes:**

- Resultats synthetiques
- Technologie multidimensionnelle
- Navigation intuitive

**Datamining:**

- Analyses sophistiquees
- Analyses de correlation
- Simulations
- Extraction de connaissances

### 12. ARCHITECTURES DW

**Architecture Reelle (Recommandee):**

- Stockage SGBD separe production
- Alimente par extractions periodiques
- Donnees preparees pour decision
- Cout stockage supplementaire
- Manque acces temps reel

**Architecture Virtuelle (Rarement utilisee):**

- Donnees dans systeme production
- Visibles par middleware/passerelles
- Pas cout stockage supplementaire
- Acces temps reel
- Donnees non preparees

**Architecture Remote (Rarement utilisee):**

- Combinaison reelle et virtuelle
- Niveaux agregees implementes
- Niveau detail dans production
- Acces par middleware

### 13. APPROCHES DE CONSTRUCTION DW

**Top-Down (Bill Inmon):**

- DW consolide donnees detaillees entreprise
- DM construits ensuite selon demandes
- Architecture integree, complete
- Pas de redondances
- Methode lourde, contraignante, longue

**Bottom-Up (Ralph Kimball):**

- DW = ensemble des DM
- Chaque DM enrichit DW
- Simple, resultats rapides
- Efficace court terme
- Pas efficace long terme, risque redondances

**Middle-Out (Hybride):**

- Conception integrale entrepot
- Creation divisions gerable
- Meilleur des deux approches
- Developpement iteratif
- Compromis decoupage parfois necessaires

### 14. TECHNOLOGIES OLAP

**MOLAP (Multidimensional OLAP):**

- Stockage multidimensionnel
- Tout agregee par defaut
- Precalculs au chargement
- Limite: quelques giga octets
- Fonctionnalites avancees (prevision, simulation)
- Incompatible autres modes acces

**ROLAP (Relational OLAP):**

- Sur SGBD relationnel
- Pas d'agregation automatique
- Creation explicite agregats
- Adapte gros volumes
- Beneficie evolutions SGBD
- Administration plus lourde

**HOLAP (Hybrid OLAP):**

- Hybride MOLAP/ROLAP
- Donnees detaillees: tables relationnelles
- Donnees agregees: cubes
- Mixer avantages deux systemes
- Repartition selon moteur plus rapide

**OOLAP (Object OLAP):**

- Base sur paradigme objet
- Fait = classe de fait
- Dimension = classe de dimension
- Augmentation niveau abstraction

### 15. METADONNEES

**Definition:** Ensemble des informations concernant le Data Warehouse et les processus associes

**Fonction:** Veritable aide en ligne pour connaitre le contenu du DW

**Destinataires:**

**Utilisateur:**

- Semantique
- Localisation

**Equipes transformation:**

- Localisation dans systemes production
- Description regles
- Processus transformation

**Equipes agregation:**

- Processus creation donnees agregees
- A partir donnees detaillees

**Equipes administration BD:**

- Structure base implementant DW

**Equipes production:**

- Procedures chargement
- Historique mise a jour

**Integration:** Idealement dans un referentiel

### 16. REGLES OLAP (E.F. CODD - 12 REGLES)

**1. Vue Multidimensionnelle:**

- Analyse par produit, region, periode
- Manipulations: rotation, pivot, slice and dice

**2. Transparence Serveur OLAP:**

- Complement outils habituels
- Architecture ouverte
- Pas d'affectation systeme central

**3. Accessibilite Sources:**

- Acces donnees necessaires analyses
- Schema logique propre stockage
- Vue simple et coherente

**4. Performance Reporting:**

- Augmentation dimensions/volume
- Pas degradation visible utilisateur

**5. Architecture Client-Serveur:**

- Donnees sur gros systemes
- Accessibles via PC

**6. Dimensions Generiques:**

- Toutes dimensions equivalentes
- Structure logique unique
- Fonction applicable toute dimension

**7. Gestion Matrices Creuses:**

- Schema physique adapte
- Optimisation gestion matrices creuses
- Exemple: produits non vendus toutes regions

**8. Supports Multi-utilisateurs:**

- Acces concurrents
- Garantie integrite et securite
- Plusieurs utilisateurs meme modele

**9. Calculs Travers Dimensions:**

- Operations sur toutes dimensions
- Pas intervention utilisateur calculs hierarchiques

**10. Manipulation Intuitive:**

- Action directe sur cellules
- Sans menus ou chemins multiples

**11. Souplesse Rapports:**

- Presentation desiree
- Donnees synthetiques ou resultats
- Fonction orientation modele

**12. Nombre Illimite Niveaux:**

- Au moins 15 a 20 dimensions
- Niveaux agregation illimites

### 17. AGREGATS ET CUBES

**Agregat:** Valeur obtenue par combinaison de plusieurs valeurs selon operateur mathematique

**Operateurs d'agregation:**

- SUM (somme)
- AVG (moyenne)
- COUNT (comptage)
- MIN (minimum)
- MAX (maximum)

**Cube:** Schema en etoile dans lequel certains agregats ont ete precalcules

**Performance:** Particulierement performant pour:

- Extraction resultats agregees
- Pas faits detailles atomiques
- Mesures avec operateurs bien definis

**Dimensions:** Cube peut avoir plus de trois dimensions (hypercube)

### 18. CONDUITE PROJET DW

**Quatre principes:**

**1. Connaitre metiers utilisateurs:**

- Impliquer utilisateurs projets
- Participer definition metadonnees
- Former utilisateur logique DW
- Chef projet oriente utilisateur

**2. Vraie conduite projet:**

- Chef projet federe besoins
- Organise et planifie phases
- Source echec: humaine (non technique)
- Prototype necessaire

**3. Commencer petit, voir grand:**

- Depart: prototype ou Data Mart
- Vision globale

**4. Gerer evolutivite:**

- Maitriser alimentation
- Maitriser administration
- Points critiques construction

**Demarche iterative:**

- Decoupage iterations domaine fonctionnel
- Construction datamart
- Lots petite taille
- Satisfaction rapide utilisateurs
- Montee charge progressive
- Applications BI < 6 mois

**Etapes iteration:**

1. Choisir processus metier analyser
2. Definir grain DM (niveau detail)
3. Definir dimensions (axes analyse)
4. Definir mesures faits
5. Definir frequence alimentation
6. Definir profondeur historique

**Implementation:**

1. Creation DM relationnel SQL
2. Creation couche semantique
3. Creation applications BI
4. Validation modele et donnees
5. Creation alimentation DM
6. Recette

---

## GLOSSAIRE COMPLET

**ACID:** Proprietes transactions: Atomicite, Coherence, Isolation, Durabilite

**Agregat:** Valeur obtenue par combinaison valeurs selon operateur mathematique

**Architecture Client-Serveur:** Architecture ou postes clients acced ent serveur donnees

**Attribut:** Element constitutif dimension dans modele dimensionnel

**Back-End:** Partie cachee systeme decisionnel (preparation, stockage)

**Base donnees multidimensionnelle:** Base organisee selon plusieurs dimensions, synonyme hypercube

**BI (Business Intelligence):** Informatique decisionnelle, ensemble outils et processus aide decision

**BISM:** Business Intelligence Semantic Model, modele semantique Microsoft

**Bottom-Up:** Approche construction DW par DataMarts progressivement integres

**Cellule:** Position tableau multidimensionnel, intersection toutes dimensions

**CRM:** Customer Relationship Management, GRC Gestion Relation Client

**Cube:** Schema etoile avec agregats precalcules et couche metier

**Data Mart:** Magasin donnees, petite base decisionnelle ciblee departement

**Data Mining:** Forage donnees, extraction connaissances

**Data Surfing:** Navigation libre et intuitive dans modele dimensionnel

**Data Warehouse:** Entrepot donnees, collection orientee sujet, integree, non volatile, historisee

**Data Warehousing:** Processus construction et alimentation entrepot donnees

**DDL:** Data Definition Language, langage definition donnees

**Dimension:** Axe analyse dans modele multidimensionnel

**Dimension degradee:** Dimension proprietes integrees directement table faits

**Donnee:** Traduction codee information

**Drill-down:** Navigation niveau global au niveau detaille hierarchie

**Drill-up:** Navigation niveau detaille au niveau global hierarchie

**EDI:** Echange Donnees Informatise, echange electronique documents organisations

**EIS:** Executive Information System, systeme information dirigeants

**ERP:** Enterprise Resource Planning, PGI Progiciel Gestion Integre

**ETL:** Extract Transform Load, Extraction Transformation Chargement donnees

**Front-End:** Partie visible systeme decisionnel (distribution, applications BI)

**GED:** Gestion Electronique Documents, systeme archivage numerique

**Grain:** Niveau detail donnees dimension

**HOLAP:** Hybrid OLAP, stockage detaille relationnel, agregats multidimensionnel

**HRM:** Human Resource Management, SIRH Systeme Information RH

**Hypercube:** Format multidimensionnel stockage et analyse donnees

**Infocentre:** Collection donnees volatiles pour decision ponctuelle

**Information:** Element permettant completer connaissance personne, objet, evenement

**Matrice creuse:** Cube avec nombreuses cellules sans valeur

**Mesure:** Variable numerique agregable dans fait

**Metadonnees:** Donnees sur donnees, informations concernant DW

**Metrique:** Variable analyser tableau multidimensionnel, synonyme mesure

**Middleware:** Logiciel intermediaire entre applications

**MOLAP:** Multidimensional OLAP, stockage donnees agregats format multidimensionnel

**Modele dimensionnel:** Organisation donnees selon faits et dimensions

**Modele constellation:** Plusieurs tables faits partageant certaines dimensions

**Modele etoile:** Table faits centrale reliee tables dimensions

**Modele flocon:** Modele etoile avec dimensions normalisees sous-hierarchies

**OLAP:** OnLine Analytical Processing, processus analyse en ligne

**OLTP:** OnLine Transaction Processing, processus transaction en ligne

**OOLAP:** Object OLAP, OLAP base paradigme objet

**Paquet:** Fichier contenant flux ETL dans SSIS

**PDM:** Product Data Management, SGDT Systeme Gestion Donnees Techniques

**Power BI:** Suite outils Microsoft BI libre-service

**Power Pivot:** Module Excel modelisation donnees avancee

**Power Query:** ETL personnel Excel

**Power View:** Outil reporting dynamique Microsoft

**ROLAP:** Relational OLAP, OLAP sur base donnees relationnelle

**Rotate:** Rotation hypercube presenter face differente

**SCM:** Supply Chain Management, GCL Gestion Chaine Logistique

**SGBD:** Systeme Gestion Base Donnees

**SGBDR:** Systeme Gestion Base Donnees Relationnelles

**SID:** Systeme Information Decisionnel

**SIAD:** Systeme Information Aide Decision

**SIG:** Systeme Information Geographique

**SIS:** Systeme Information Strategique

**Slice and Dice:** Pivotement dynamique axes tableau analyse

**SSAS:** SQL Server Analysis Services, serveur modeles semantiques Microsoft

**SSDT:** SQL Server Data Tools, environnement developpement SQL Server

**SSIS:** SQL Server Integration Services, ETL Microsoft SQL Server

**SSMS:** SQL Server Management Studio, interface gestion SQL Server

**SSRS:** SQL Server Reporting Services, serveur rapports Microsoft

**Systeme:** Ensemble elements interaction transformant entrees sorties

**Systeme decision:** Systeme pilotage guidant organisation

**Systeme information:** Ensemble ressources acquérir, traiter, stocker, diffuser information

**Systeme operant:** Systeme effectuant transformation executant decisions

**Table faits:** Table centrale modele dimensionnel contenant mesures

**TFS:** Team Foundation Server, gestionnaire sources Microsoft

**Top-Down:** Approche construction DW global puis DataMarts

**Variable flux:** Variable variations enregistrees, cumul pour periode

**Variable etat:** Variable indiquant contenu instant t, pas cumul

**Workflow:** Systeme automatisation flux information

---

## POINTS CLES A RETENIR

### CONCEPTS FONDAMENTAUX

**Organisation:**

- Systeme pilotage (decision)
- Systeme operant (execution)
- Systeme information (interface)

**Information vs Donnee:**

- Information = donnee avec signification contexte
- Donnee = traduction codee information

**Classification SI:**

- SI transactionnels (OLTP)
- SI decisionnels (OLAP)
- ERP (transversalite)

### INFORMATIQUE DECISIONNELLE

**Naissance:**

- 1970: E.F. Codd invente SGBDR et OLTP
- 1993: E.F. Codd formalise OLAP

**Trois principes:**

- Modelisation OLAP
- Distribution information
- Entrepot donnees isole

**Incompatibilite OLTP/OLAP:**

- Objectifs performance opposes
- Necessite base separee

### SIAD

**Trois categories:**

- SIAD operationnel (urgence, systemes experts)
- SIAD gestion (pilotage quotidien)
- SIAD strategique (dirigeants, mensuel)

**Dimension technique:**

- Outil observation
- Pas explications
- Presentation synthetique

**Outils:**

- Tableaux bord
- Tableaux preformates
- Hypercubes
- Datamining

### DATA WAREHOUSE

**Definition (Bill Inmon):** Collection donnees orientees sujet, integrees, non volatiles, historisees, organisees pour aide decision

**Quatre caracteristiques:**

- Orientees sujet (organisation thematique)
- Integrees (coherence, 60-90% charge projet)
- Historisees (referentiel temps)
- Non volatiles (pas mise jour)

**Structure:**

- Donnees detaillees (recentes)
- Donnees agregees (synthese)
- Metadonnees (documentation)
- Donnees historisees

**Architectures:**

- Reelle (recommandee, stockage separe)
- Virtuelle (rarement utilisee, donnees production)
- Remote (combinaison, rarement utilisee)

### DATA MART

**Caracteristiques:**

- Moins couteux que DW
- Plus leger
- Cible departement
- Quelques utilisateurs

**Comparaison DW/DM:**

- DW: toute entreprise, multi-sujets, centaines GO, 9-18 mois
- DM: departement, quelques sujets, dizaines GO, 6-12 mois

### CONSTRUCTION DW

**Approches:**

- Top-Down (Inmon): DW global puis DM
- Bottom-Up (Kimball): DM puis DW
- Middle-Out: hybride

**Constituants:**

- Back-End: preparation, stockage, ETL
- Front-End: distribution, applications BI

**Composants fonctionnels:**

- Acquisition (extraction, preparation, chargement)
- Stockage (SGBD)
- Acces decisionnel (requeteurs, OLAP, datamining)

**Conduite projet:**

- Connaitre metiers utilisateurs
- Vraie conduite projet
- Commencer petit, voir grand
- Gerer evolutivite

**Demarche iterative:**

- Par domaine fonctionnel
- Applications BI < 6 mois
- Perimetre clairement defini

**Etapes iteration:**

1. Choisir processus metier
2. Definir grain
3. Definir dimensions
4. Definir mesures
5. Definir frequence alimentation
6. Definir profondeur historique

**Implementation:**

1. DM relationnel SQL
2. Couche semantique
3. Applications BI
4. Validation
5. Alimentation
6. Recette

**Alimentation:**

- Decouverte donnees
- Extraction
- Transformation (nettoyage, filtres, harmonisation)
- Chargement

### OLAP

**12 Regles E.F. Codd:**

1. Vue multidimensionnelle
2. Transparence serveur
3. Accessibilite sources
4. Performance reporting
5. Architecture client-serveur
6. Dimensions generiques
7. Matrices creuses
8. Multi-utilisateurs
9. Calculs travers dimensions
10. Manipulation intuitive
11. Souplesse rapports
12. Niveaux illimites

**Modele dimensionnel:**

- Dimensions (axes analyse hierarchies)
- Variables/Metriques (flux ou etat)

**Navigation:**

- Drill-down/up (detail/synthese)
- Slice and dice (pivotement)
- Rotate (rotation cube)
- Data surfing (navigation libre)

**Probleme matrices creuses:**

- Nombreuses cellules vides
- Solutions: choix dimensions, division cube, stockage relationnel

### MODELISATION DW

**Cinq axes qualification:**

- Lisibilite
- Performances chargement
- Performances execution
- Administration
- Evolutivite

**Modele relationnel:**

- Normalise (complet, performances mauvaises)
- Denormalise (simplifie, redondances)

**Modele dimensionnel:**

**En etoile:**

- Table faits centrale
- Tables dimensions reliees
- Avantages: navigation facile, jointures limitees
- Inconvenients: redondances, alimentation complexe

**En flocon:**

- Dimensions normalisees sous-hierarchies
- Avantages: espace disque, normalisation
- Inconvenients: complexite, performances

**En constellation:**

- Plusieurs tables faits
- Partage dimensions

**Dimensions:**

- Attributs (analyse vs information)
- Granularite (niveau detail)
- Cles (fonctionnelle vs technique/substitution)
- Variation (type 1: ecrasement, type 2: historisation)

**Faits:**

- Niveau atomique ou agrege
- Mesures avec operateurs agregation
- Cube = schema etoile + agregats precalcules

### TECHNOLOGIES OLAP

**MOLAP:**

- Stockage multidimensionnel
- Tout agrege par defaut
- Limite: quelques GO
- Fonctionnalites avancees

**ROLAP:**

- Sur SGBD relationnel
- Pas agregation automatique
- Adapte gros volumes
- Beneficie evolutions SGBD

**HOLAP:**

- Hybride
- Details relationnel
- Agregats multidimensionnel

**OOLAP:**

- Base objet
- Augmentation niveau abstraction

### BI MICROSOFT

**SQL Server:**

- MSSQL (relationnel)
- SSAS (modeles semantiques)
- SSRS (rapports)
- SSIS (ETL)

**SSAS:**

- Multidimensionnel (MOLAP/ROLAP/HOLAP)
- Tabulaire (Power Pivot)
- Power Pivot SharePoint

**Editions:**

- Standard (base, limite)
- Business Intelligence (BI complet)
- Enterprise (plus complet)

**Office:**

- Excel (consommation donnees, Power BI)
- Power Pivot (modele donnees)
- Power Query (ETL personnel)
- Power View (reporting dynamique)

**Outils:**

- SSDT (developpement)
- SSMS (gestion)
- Adventure Works (exemple)

### TABLEAU RECAPITULATIF FINAL

|Element|OLTP|OLAP|
|---|---|---|
|Objectif|Gestion operations|Aide decision|
|Donnees|Actuelles, detaillees|Historiques, agregees|
|Operations|INSERT, UPDATE, DELETE|SELECT, analyses|
|Requetes|Simples, rapides|Complexes, lourdes|
|Utilisateurs|Operationnels nombreux|Analystes, decideurs|
|Volume traite|Petit par transaction|Grand par analyse|
|Mise jour|Frequente, temps reel|Rare, periodique|
|Structure|Normalisee|Denormalisee (etoile/flocon)|
|Indexation|Peu index|Nombreux index|
|Orientation|Application, processus|Sujet, theme|

### FORMULES ET SYNTAXES ESSENTIELLES

**Nom complet objet:**

```
Serveur.BaseDeDonnees.Schema.Objet
```

**Transaction SQL:**

```sql
BEGIN TRANSACTION
  -- operations
  SAVE TRANSACTION point1
  -- autres operations
  ROLLBACK TRANSACTION point1
COMMIT TRANSACTION
```

**Schema etoile - Structure type:**

```
TableFaits (centre)
  - cle_dimension1 (FK)
  - cle_dimension2 (FK)
  - cle_dimension3 (FK)
  - mesure1
  - mesure2

TableDimension1
  - cle (PK)
  - attribut1
  - attribut2

TableDimension2
  - cle (PK)
  - attribut1
  - hierarchie_niveau1
  - hierarchie_niveau2
```

**Agregation type:**

```sql
SELECT 
  dim_temps.annee,
  dim_produit.categorie,
  SUM(faits.montant) as CA,
  COUNT(*) as nb_ventes,
  AVG(faits.quantite) as qte_moyenne
FROM faits
JOIN dim_temps ON faits.cle_temps = dim_temps.cle
JOIN dim_produit ON faits.cle_produit = dim_produit.cle
GROUP BY dim_temps.annee, dim_produit.categorie
```

---

## CONSEILS POUR EXAMEN

### Points Attention Particuliere

**Definitions a connaitre par coeur:**

- Data Warehouse (Bill Inmon)
- OLTP vs OLAP
- ETL
- SIAD
- Cube OLAP

**Differences a maitriser:**

- OLTP vs OLAP (tableau complet)
- Data Warehouse vs Data Mart
- Schema etoile vs flocon
- MOLAP vs ROLAP vs HOLAP
- Top-Down vs Bottom-Up
- Type 1 vs Type 2 (variations dimensions)

**Processus a comprendre:**

- ETL complet (3 phases)
- Construction iterative DW
- Navigation OLAP (drill-down, slice, dice, pivot)
- Alimentation DW
- Implementation DW

**Concepts techniques:**

- 4 caracteristiques DW (oriente sujet, integre, historise, non volatile)
- 12 regles OLAP (Codd)
- Structure 4 niveaux DW (detaille, agrege, metadata, historise)
- 3 architectures DW (reelle, virtuelle, remote)

**Pieges frequents:**

- Confondre donnee et information
- Confondre OLTP et OLAP
- Confondre DW et DM
- Oublier non-volatilite DW
- Confondre ETL avec simple replication
- Penser cube = 3 dimensions seulement

### Methode Revision

1. Lire ce document (vue ensemble)
2. Faire fiches par concept
3. Apprendre definitions par coeur
4. Comprendre differences (tableaux comparatifs)
5. Maitriser processus (ETL, construction, navigation)
6. Refaire exercices pratiques
7. Questions types examen blanc

### Questions Types Preparation

**QCM frequents:**

- Quelle caracteristique pour Data Warehouse?
- Difference OLTP vs OLAP?
- Quelle phase ETL pour nettoyage?
- Quel schema pour modele decisionnel?
- Quelle operation OLAP pour detail?

**Questions ouvertes:**

- Expliquer processus ETL complet
- Decrire architecture Data Warehouse
- Difference schema etoile vs flocon
- Expliquer 4 caracteristiques DW
- Role SIAD dans organisation

**Pratique:**

- Modeliser schema etoile pour cas donne
- Identifier dimensions et mesures
- Proposer hierarchies
- Definir processus ETL
- Choisir approche construction (Top-Down vs Bottom-Up)

---

**FIN DU RESUME COMPLET**

Document pret pour revision approfondie, preparation examen ou certification SIAD.