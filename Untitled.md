
# PLAN D'ACTION - FLOODWATCH AFRICA

## Hackathon Rust - Deadline: 18 Janvier 2026

## Équipe de 4 personnes

---

## RÉSUMÉ EXÉCUTIF

**Objectif**: Système d'identification des zones inondables en Afrique **Stack**: Rust (Axum backend) + Frontend simple **Livrable**: Prototype fonctionnel avec visualisation sur carte

---

## 👥 RÉPARTITION DES RÔLES (4 PERSONNES)

### 🔧 PERSONNE 1 - BACKEND RUST 

**Focus**: Architecture MVC avec Axum + API
#### Responsabilités:
1. **Setup & Architecture**
    - Initialiser le projet Cargo
    - Structurer en MVC (Models, Controllers, Services)
    - Configurer Axum avec routes et middleware
2. **Models (Structures de données)**
    - Zone géographique (id, nom, coordonnées, données)
    - Score de risque (valeur, niveau, détails)
    - Données climatiques (pluie, altitude, rivières)
3. **Controllers (Routes API)**
    - GET /api/zones - Liste des zones
    - GET /api/zones/:id/risk - Risque d'une zone
    - GET /api/geojson - Export pour la carte
    - Configuration CORS pour frontend
#### Livrables:
- API REST fonctionnelle
- Documentation des endpoints
- Configuration serveur Axum

---

###  PERSONNE 2 - LOGIQUE MÉTIER RUST 

**Focus**: Services & Algorithmes

#### Responsabilités:

1. **Services (Logique métier)**
    - Parser les fichiers CSV/JSON
    - Algorithme de calcul du risque d'inondation
    - Agrégation et analyse des données
    - Validation des données
2. **Traitement des données**
    - Lecture et parsing des CSV/JSON
    - Nettoyage des données côté Rust
    - Calculs statistiques (moyenne, max, min)
    - Génération du GeoJSON
3. **Tests & Performance**
    - Tests unitaires des algorithmes
    - Optimisation des calculs
    - Gestion des erreurs

#### Livrables:

- Module de calcul du risque
- Module de parsing de données
- Tests unitaires

---

###  PERSONNE 3 - DONNÉES & INTÉGRATION 

**Focus**: Collecte, nettoyage, formatage des données

#### Responsabilités:
1. **Recherche de données** 
    - Trouver datasets gratuits et ouverts
    - Vérifier la qualité et disponibilité
    - Documenter les sources
2. **Préparation des datasets**
    - Télécharger les données brutes
    - Nettoyer (enlever doublons, erreurs)
    - Formater en CSV/JSON exploitables
    - Créer des fichiers de test
3. **Zones d'étude**
    - Sélectionner 5-10 villes africaines
    - Extraire leurs coordonnées GPS
    - Rassembler les données par zone
    - Vérifier cohérence des données
4. **Documentation**
    - Créer un fichier DATA.md expliquant chaque dataset
    - Noter les limites et hypothèses
    - Lister les sources avec liens
#### Livrables:
- Fichiers data/rainfall.csv
- Fichiers data/elevation.json
- Fichiers data/rivers.geojson
- Fichiers data/zones.csv
- Documentation DATA.md

---

###  PERSONNE 4 - FRONTEND & VISUALISATION (Kemh)

**Focus**: Interface web + Carte interactive

#### Responsabilités:
1. **Interface HTML/CSS**
    - Page d'accueil simple et claire
    - Dashboard avec statistiques
    - Design responsive
2. **Carte interactive**
    - Intégration Leaflet.js ou Mapbox
    - Affichage des zones par couleur de risque
    - Popup avec détails au clic
    - Zoom et navigation
3. **Intégration API**
    - Appels fetch vers l'API Rust
    - Affichage dynamique des données
    - Gestion des erreurs et loading
    - Refresh des données
4. **Expérience utilisateur**
    - Légende claire (Faible/Modéré/Élevé)
    - Filtres par niveau de risque
    - Loading states et animations
    - Messages d'erreur clairs
#### Livrables:
- index.html fonctionnel
- Carte avec zones colorées
- Interface utilisateur claire
- Design responsive

---

##  STRUCTURE DU PROJET

```
floodwatch-africa/
│
├── backend/                    # Projet Rust (Joan)
│   ├── Cargo.toml
│   ├── src/
│   │   ├── main.rs            # Point d'entrée Axum
│   │   ├── models/            # Structures de données
│   │   │   ├── mod.rs
│   │   │   ├── zone.rs
│   │   │   └── risk.rs
│   │   ├── services/          # Logique métier
│   │   │   ├── mod.rs
│   │   │   ├── data_parser.rs
│   │   │   └── risk_calculator.rs
│   │   ├── controllers/       # Routes API
│   │   │   ├── mod.rs
│   │   │   └── zones.rs
│   │   └── utils/
│   │       └── mod.rs
│   └── data/                  # Données (Marina)
│       ├── rainfall.csv
│       ├── elevation.json
│       ├── rivers.geojson
│       └── zones.csv
│
├── frontend/                   # Interface web (Kemh)
│   ├── index.html
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   └── app.js
│   └── assets/
│
├── docs/                       # Documentation
│   ├── README.md
│   ├── DATA.md               # Doc des données
│   └── API.md                # Doc de l'API
│
└── presentation/               # Pour le pitch final
    └── slides.md
```

---

##  PLANNING DÉTAILLÉ (10 JOURS)

###  JOUR 1-2 (Aujourd'hui 10 Jan - 11 Jan) - SETUP

**Objectif**: Tout le monde démarre

 (Backend - Architecture):

- [ ] Créer le projet Cargo
- [ ] Installer dépendances (axum, serde, tokio, tower-http)
- [ ] Créer structure MVC de base
- [ ] Hello World API qui fonctionne sur port 3000
- [ ] Setup CORS pour le frontend

####  (Backend - Services):

- [ ] Créer module services/
- [ ] Structure de base pour data_parser.rs
- [ ] Structure de base pour risk_calculator.rs
- [ ] Tests unitaires vides (à remplir plus tard)

#### (Données):

- [ ] Lister 5 sources de données potentielles
- [ ] Télécharger 1 dataset de pluie de test
- [ ] Créer template CSV pour zones
- [ ] Lister 8-10 villes africaines cibles avec coordonnées

#### (Frontend):

- [ ] Créer index.html de base
- [ ] Intégrer Leaflet.js via CDN
- [ ] Afficher une carte simple centrée sur Afrique
- [ ] Tester appel API dummy (localhost:3000)

**Checkpoint Jour 2**:

- API "Hello World" accessible
- Structure MVC créée
- 1 dataset de test disponible
- Carte s'affiche dans le navigateur

---

###  JOUR 3-4 (12-13 Jan) - CORE FEATURES

**Objectif**: Premières fonctionnalités qui marchent

- [ ] Créer les Models (Zone, Risk, ClimateData)
- [ ] Route GET /api/zones qui retourne données mockées
- [ ] Route GET /api/health pour vérifier l'API
- [ ] Gérer les erreurs HTTP proprement

- [ ] Service pour lire fichiers CSV
- [ ] Parser zones.csv et retourner Vec<Zone>
- [  ]  Fonction de base pour calculer score (simple somme)
- [  ] Tester le parsing avec données 


- [ ] Nettoyer et formater données pluie (rainfall.csv)
- [ ] Nettoyer et formater données altitude (elevation.json)
- [ ] Créer fichier zones.csv complet avec 8 villes
- [ ] Documenter format exact de chaque fichier


- [ ] Afficher des points sur la carte (markers Leaflet)
- [ ] Créer la légende de base (Faible/Modéré/Élevé)
- [ ] Styler les zones avec 3 couleurs différentes
- [ ] Fetch GET /api/zones et afficher résultats

**Checkpoint Jour 4**:

- API retourne vraies données des CSV
- Carte affiche au moins 5 zones
- Données nettoyées et formatées
- Parsing fonctionne

JOUR 5-6 (14-15 Jan) - ALGORITHME DE RISQUE

**Objectif**: Calcul du risque fonctionne correctement

- [ ] Route GET /api/zones/:id/risk
- [ ] Route GET /api/geojson (export complet)
- [ ] Intégrer les calculs de Gedeon dans l'API
- [ ] Gérer les cas d'erreur (zone inexistante, etc.)


- [ ] Implémenter l'algorithme complet de calcul
- [ ] Fonction calculate_rainfall_score()
- [ ] Fonction calculate_elevation_score()
- [ ] Fonction calculate_river_proximity_score()
- [ ] Fonction finale calculate_total_risk()
- [ ] Tests unitaires pour chaque fonction

- [ ] Finaliser données rivières (rivers.geojson)
- [ ] Vérifier cohérence entre tous les datasets
- [ ] Créer 3 cas de test : risque faible, modéré, élevé
- [ ] Ajouter données historiques d'inondations si possible

- [ ] Colorer les zones selon niveau de risque (vert/orange/rouge)
- [ ] Popup au clic avec détails de la zone
- [ ] Affichage du score et du niveau de risque
- [ ] Statistiques globales (combien de zones par niveau)

**Checkpoint Jour 6**:

- Calcul de risque fonctionnel et testé
- Carte affiche zones colorées correctement
- Toutes données intégrées
- API complète et documentée

 JOUR 7-8 (16-17 Jan) - FINALISATION

**Objectif**: Polish, tests et documentation


- [ ] Optimiser performances (caching si besoin)
- [ ] Gérer toutes les erreurs proprement
- [ ] Créer fichier API.md avec documentation
- [ ] Ajouter logging (println! ou tracing)


- [ ] Finir tous les tests unitaires
- [ ] Vérifier edge cases (données manquantes, etc.)
- [ ] Optimiser l'algorithme si trop lent
- [ ] Commenter le code important


- [ ] Vérifier que toutes sources sont citées
- [ ] Créer DATA.md complet et détaillé
- [ ] Préparer slides avec graphiques des données
- [ ] Créer un fichier SOURCES.md avec tous les liens


- [ ] Améliorer UX (transitions, animations)
- [ ] Ajouter loading states partout
- [ ] Messages d'erreur clairs si API down
- [ ] Tester sur mobile et tablette
- [ ] README.md pour lancer le frontend

**Checkpoint Jour 8**:

- Prototype complet et testé de bout en bout
- Documentation prête (README, API.md, DATA.md)
- Démo répétée entre vous
- Slides de présentation prêtes

 JOUR 9 (18 Jan) - HACKATHON DAY! 

**Matin**:

- [ ] Derniers tests rapides
- [ ] Vérifier que tout build correctement
- [ ] Répétition du pitch (5-10 min)

**Pendant le hackathon**:

- [ ] Soumission du projet
- [ ] Présentation finale
- [ ] Démo live
- [ ] Q&A avec les juges

ALGORITHME DE RISQUE (Logique Simple)

### Indicateurs à calculer:

1. **Score Pluie (0-40 points)**
    
    - Pluie > 200mm/mois → 40 pts
    - Pluie 100-200mm → 20 pts
    - Pluie < 100mm → 0 pts
2. **Score Altitude (0-30 points)**
    
    - Altitude < 10m → 30 pts
    - Altitude 10-50m → 15 pts
    - Altitude > 50m → 0 pts
3. **Score Proximité Rivière (0-30 points)**
    
    - Distance < 100m → 30 pts
    - Distance 100-500m → 15 pts
    - Distance > 500m → 0 pts

### Classification finale:

- **Score ≥ 70**: Risque ÉLEVÉ (rouge)
- **Score 40-69**: Risque MODÉRÉ (orange)
- **Score < 40**: Risque FAIBLE (vert)

---

## SOURCES DE DONNÉES RECOMMANDÉES

### 🌧️ Données de Pluie:

1. **Meteostat** (https://meteostat.net/)
    
    - API gratuite
    - Historique précipitations
    - Facile à utiliser
2. **OpenWeatherMap** (https://openweathermap.org/)
    
    - API gratuite (limite 1000 appels/jour)
    - Données actuelles + historiques

### 🗻 Données d'Altitude:

1. **OpenTopoData** (https://www.opentopodata.org/)
    
    - API gratuite
    - Altitude précise par coordonnées GPS
2. **SRTM via Google Earth Engine**
    
    - Données satellites
    - Résolution 30m

### 🌊 Données Rivières:

1. **OpenStreetMap Overpass API**
    
    - Gratuit
    - Extraction cours d'eau par zone
    - Format GeoJSON
2. **Natural Earth Data** (https://www.naturalearthdata.com/)
    
    - Datasets préfabriqués
    - Rivières majeures d'Afrique

---

## 📝 VILLES AFRICAINES SUGGÉRÉES

Choisir 5-10 villes parmi:

1. **Lomé, Togo** (6.13°N, 1.22°E)
2. **Lagos, Nigeria** (6.52°N, 3.38°E)
3. **Accra, Ghana** (5.56°N, -0.20°E)
4. **Dakar, Sénégal** (14.69°N, -17.45°W)
5. **Abidjan, Côte d'Ivoire** (5.36°N, -4.01°W)
6. **Kinshasa, RDC** (4.32°S, 15.31°E)
7. **Dar es Salaam, Tanzanie** (6.79°S, 39.21°E)
8. **Nairobi, Kenya** (1.29°S, 36.82°E)

**Critère**: Villes avec historique d'inondations

---

## 🎯 LIVRABLES FINAUX

### 1. Code Source

- Repo GitHub propre
- README complet
- Code commenté

### 2. Application Fonctionnelle

- API Rust qui tourne
- Frontend accessible
- Démo live

### 3. Documentation

- README.md (comment lancer)
- DATA.md (sources de données)
- API.md (endpoints)

### 4. Présentation

- Slides (5-10 minutes)
- Démo vidéo backup
- Impact africain clair

---
