

---

# Cahier des charges

## Projet : Station météo IoT connectée (Big Data)

---

## 1. Contexte du projet

Dans le cadre du projet Big Data, le groupe doit concevoir une **station météo IoT connectée** permettant de collecter, traiter et visualiser des données météorologiques en temps réel ou quasi temps réel.

Le système repose sur une **architecture IoT complète**, intégrant :

- une source de données météo
- une chaîne de transmission vers le cloud
- une couche de stockage et de traitement Big Data
- une interface de visualisation (dashboard)  
    

Les données peuvent provenir :

- de capteurs physiques ( si un debout dans le groupe peut en trouver)
- ou de données satellitaires ouvertes (NASA Earthdata / NASA POWER)
    

---

## 2. Objectifs spécifiques

Le projet doit permettre de :

1. Acquérir des données météo (température, humidité, pression, pluie)    
2. Transmettre les données vers une plateforme cloud
3. Stocker les données de manière durable
4. Traiter et analyser les données météo
5. Afficher les données en temps réel et sous forme historique    
6. Illustrer des cas d’usage concrets (agriculture, écoles, énergie)
    
---

## 3. Périmètre fonctionnel (ce que le système DOIT faire)

### 3.1 Acquisition des données

Le système doit :

- Collecter les paramètres météorologiques requis  
- Associer chaque mesure à :
    - une date  
    - une heure   
    - une localisation
Dans le cas des données NASA :

- Les satellites sont considérés comme des capteurs globaux   
- Les données sont filtrées pour une zone géographique donnée
    

---

### 3.2 Transmission des données

Le système doit :

- Envoyer les données vers le cloud 
- Supporter un mode de transmission en flux continu
- Garantir l’intégrité des données envoyées
    

Les protocoles utilisés doivent être adaptés à l’IoT (ex. MQTT ou HTTP).

---

### 3.3 Stockage des données (Big Data)

Le système doit :

- Stocker un grand volume de données météo    
- Conserver l’historique sur une longue période 
- Permettre une consultation rapide des données
    
Les données doivent être structurées de façon à faciliter :

- l’analyse
- l’agrégation
- la visualisation
    

---

### 3.4 Traitement et analyse des données

Le système doit :

- Nettoyer les données (valeurs aberrantes, données manquantes)
- Calculer des indicateurs :
    
    - moyennes    
    - minimums / maximums  
    - tendances
        
- Détecter des événements météo (pluie, chaleur élevée, etc.)
    

Cette partie constitue le **cœur Big Data du projet**.

---

### 3.5 Visualisation : dashboard en temps réel

Le dashboard doit :

- Afficher les données météo actuelles 
- Mettre à jour automatiquement les informations
- Proposer des graphiques temporels
- Être accessible depuis un navigateur web

Le dashboard doit être compréhensible par un utilisateur non technique.

---

## 4. Cas d’utilisation (applications)

### 4.1 Agriculture

- Suivi des conditions climatiques locales
- Aide à la planification de l’irrigation
- Anticipation des pluies et périodes sèches
    

### 4.2 Écoles

- Support pédagogique pour l’apprentissage de la météo
- Sensibilisation à l’IoT et au Big Data
- Analyse de données réelles
### 4.3 Énergie

- Estimation du potentiel solaire    
- Aide à l’optimisation des installations énergétiques
- Analyse des conditions climatiques favorables ou défavorables

---

## 5. Livrables attendus

Le groupe devra fournir :

1. **Un prototype fonctionnel** 
    - Chaîne complète de données (source → cloud → stockage → dashboard)
2. **Un dashboard opérationnel**
    - Données temps réel et historiques
3. **Un rapport de projet**
    
    - Architecture du système
    - Choix techniques justifiés   
    - Cas d’usage
        
4. **Une présentation orale**
    
    - Démonstration 
    - Explication du rôle Big Data
        

---

## 6. Critères de réussite

Le projet sera considéré comme réussi si :

- L’architecture IoT est cohérente
- Les données circulent correctement
- Le dashboard affiche des informations exploitables
- Le lien avec le Big Data est clairement démontré
- Les applications concrètes sont pertinentes

---


