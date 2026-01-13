

# DONNÉES NÉCESSAIRES – Station Météo IoT Connectée (Big Data)

---

##  1. DONNÉES D'IDENTIFICATION

_Ces données permettent d'identifier l'origine et le contexte de chaque mesure._

| **Donnée**              | **Description**             | **Exemple**                                  |
| ----------------------- | --------------------------- | -------------------------------------------- |
| **Identifiant station** | Code unique de la station   | `STN_LOM_001`                                |
| **Nom de la station**   | Nom descriptif              | `Station Lomé-Centre`                        |
| **Localisation**        | Coordonnées géographiques   |                                              |
| └ Latitude              | Position Nord/Sud           | `6.1256° N`                                  |
| └ Longitude             | Position Est/Ouest          | `1.2226° E`                                  |
| └ Ville/Région          | Localisation administrative | `Lomé, Maritime`                             |
| **Horodatage**          | Date et heure de la mesure  |                                              |
| └ Date                  | Jour de collecte            | `2026-01-02`                                 |
| └ Heure                 | Heure précise               | `14:30:00`                                   |
| └ Fuseau horaire        | Référence temporelle        | `GMT+0`                                      |
| **Source**              | Origine des données         | `Capteur IoT` / `NASA POWER` / `API externe` |

---

##  2. DONNÉES MÉTÉOROLOGIQUES PRINCIPALES

_Ces données constituent le cœur fonctionnel de la station météo._

### **Mesures essentielles**

|**Variable**|**Unité**|**Plage typique**|**Fréquence**|
|---|---|---|---|
|**Température de l'air**|°C|-10 à +50°C|Temps réel (5 min)|
|**Humidité relative**|%|0 à 100%|Temps réel (5 min)|
|**Pression atmosphérique**|hPa|950 à 1050 hPa|Temps réel (15 min)|
|**Précipitations**|mm|0 à 200 mm/h|Temps réel (1 min)|
|└ Type de précipitation|Catégorie|Pluie / Bruine / Orage|Optionnel|

---

##  3. DONNÉES MÉTÉOROLOGIQUES COMPLÉMENTAIRES

_Ces données enrichissent l'analyse et renforcent l'aspect Big Data._

| **Variable**              | **Unité**  | **Utilité**                           |
| ------------------------- | ---------- | ------------------------------------- |
| **Vitesse du vent**       | km/h       | Prévisions tempêtes, énergie éolienne |
| **Direction du vent**     | ° (0-360)  | Analyse circulation atmosphérique     |
| **Couverture nuageuse**   | % ou Oktas | Prévision météo, énergie solaire      |
| **Rayonnement solaire**   | W/m²       | Production photovoltaïque             |
| **Indice UV**             | 0-11+      | Santé publique                        |
| **Température ressentie** | °C         | Confort thermique                     |

---

##  4. DONNÉES TEMPORELLES ET STATISTIQUES

_Ces données sont utilisées pour l'analyse et la visualisation._

| **Agrégation**          | **Description**             | **Usage**               |
| ----------------------- | --------------------------- | ----------------------- |
| **Moyenne horaire**     | Moyenne sur 1 heure         | Tendances court terme   |
| **Moyenne journalière** | Moyenne sur 24 heures       | Analyse quotidienne     |
| **Minimum**             | Valeur la plus basse        | Identification extrêmes |
| **Maximum**             | Valeur la plus élevée       | Détection pics          |
| **Tendance**            | Hausse / Baisse / Stabilité | Prévisions simples      |

**Exemples de calculs** :

- Température minimale journalière
- Précipitations cumulées sur 7 jours
- Moyenne mobile sur 30 jours

---

##  5. DONNÉES DE QUALITÉ ET CONTRÔLE

_Elles garantissent la fiabilité et la traçabilité des données._

|**Indicateur**|**Valeurs possibles**|**Objectif**|
|---|---|---|
|**Date de réception**|Timestamp|Traçabilité complète|
|**Statut de la donnée**|`Valide` / `Manquante` / `Estimée` / `Erronée`|Filtrage qualité|
|**Indicateur de fiabilité**|0-100%|Score de confiance|
|**Méthode de collecte**|`Capteur physique` / `Satellite` / `Interpolation`|Transparence|
|**Fréquence de mise à jour**|1 min / 5 min / 1 heure|Fraîcheur données|

---

##  6. DONNÉES POUR LE DASHBOARD

_Ces données sont utilisées pour l'affichage en temps réel._

### **Affichage temps réel**

|**Élément**|**Description**|
|---|---|
|**Valeur actuelle**|Mesure en temps réel (ex: 28°C)|
|**Valeur précédente**|Mesure à t-1 (ex: 27°C il y a 5 min)|
|**Évolution**|↗ Augmentation / ↘ Diminution / → Stabilité|

### **Seuils d'alerte météorologique**

|**Type d'alerte**|**Seuil**|**Action**|
|---|---|---|
|**Température élevée**|> 40°C|⚠️ Alerte canicule|
|**Forte pluie**|> 50 mm/h|⚠️ Risque inondation|
|**Pression anormale**|< 980 hPa|⚠️ Alerte tempête|
|**Vent violent**|> 80 km/h|⚠️ Alerte vent fort|

---

##  7. DONNÉES SPÉCIFIQUES AUX APPLICATIONS

###  **Agriculture**

| **Donnée**                | **Utilité**                |
| ------------------------- | -------------------------- |
| Pluviométrie cumulée      | Planification irrigation   |
| Humidité du sol (estimée) | Détection stress hydrique  |
| Conditions favorables     | Optimisation semis/récolte |
| Risque de gel             | Protection des cultures    |

**Exemple** : "Précipitations insuffisantes (< 20 mm sur 30 jours) → Irrigation recommandée"

---

###  **Écoles & Recherche**

|**Donnée**|**Utilité**|
|---|---|
|Historique météo|Études climatologiques|
|Comparaison périodes|Analyse changement climatique|
|Données simplifiées|Support pédagogique|
|Graphiques interactifs|Apprentissage visuel|

**Exemple** : "Comparaison température moyenne 2020 vs 2026"

---

###  **Énergie Renouvelable**

| **Donnée**               | **Utilité**                  |
| ------------------------ | ---------------------------- |
| Ensoleillement quotidien | Production photovoltaïque    |
| Vitesse du vent          | Production éolienne          |
| Couverture nuageuse      | Prévision production solaire |
| Température panneaux     | Efficacité photovoltaïque    |

**Exemple** : "Ensoleillement > 6 kWh/m²/jour → Production optimale"

---

## SYNTHÈSE : VOLUME DE DONNÉES

|**Catégorie**|**Nombre de variables**|**Fréquence**|**Volume/jour**|
|---|---|---|---|
|Identification|8 variables|Par mesure|~200 Ko|
|Données principales|4 variables|5 min (288/jour)|~5 Mo|
|Données complémentaires|6 variables|15 min (96/jour)|~3 Mo|
|Statistiques|5 calculs|1 heure (24/jour)|~500 Ko|
|Qualité|5 indicateurs|Par mesure|~1 Mo|
|**TOTAL par station**|**28+ variables**|**Variable**|**~10 Mo/jour**|

**Pour 10 stations sur 1 an** :

- 10 stations × 10 Mo/jour × 365 jours = **~36,5 Go/an**

---



## CHECKLIST COMPLÉTUDE DES DONNÉES

- [ ] Identification station complète
- [ ] 4 mesures principales activées
- [ ] Horodatage précis (date + heure)
- [ ] Géolocalisation GPS
- [ ] Indicateurs de qualité
- [ ] Seuils d'alerte configurés
- [ ] Source de données tracée
- [ ] Fréquence de collecte définie
