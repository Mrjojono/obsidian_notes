# Exemples Pratiques pour l'Architecture des Systèmes IoT

## 1. Architecture en Couches - Exemple Concret

### Exemple : Station Météo Personnelle Complète

**Couche 1 - Perception (Matériel physique)**

```
Capteurs installés sur le toit :
- DHT22 : température et humidité (-40°C à +80°C)
- BMP280 : pression atmosphérique (300-1100 hPa)
- Pluviomètre à augets : précipitations (0.2mm par basculement)
- Anémomètre : vitesse du vent (0-50 m/s)
```

**Couche 2 - Réseau (Communication)**

```
ESP32 comme microcontrôleur principal :
- WiFi 2.4GHz pour connexion internet domestique
- Protocole MQTT vers broker local (Mosquitto)
- Transmission toutes les 30 secondes
- Buffer local de 100 mesures en cas de déconnexion
```

**Couche 3 - Traitement (Logique)**

```
Serveur Raspberry Pi 4 à la maison :
- Node-RED pour orchestration des flux
- InfluxDB pour stockage des séries temporelles
- Calculs : moyennes horaires, min/max quotidiens
- Alertes : température < 0°C → notification gel
```

**Couche 4 - Application (Interface)**

```
Dashboard accessible via navigateur :
- Grafana pour visualisation temps réel
- Application mobile React Native
- API REST pour intégration domotique
- Export CSV pour analyses Excel
```

---

## 2. Architecture Edge Computing - Exemple Détaillé

### Exemple : Station Météo Agricole Autonome

**Scénario** : Ferme viticole avec 5 parcelles, connexion internet instable

**Architecture Edge :**

```
┌─────────────────────────────────────┐
│  CAPTEURS (chaque parcelle)         │
│  - Température/Humidité DHT22       │
│  - Humidité sol capacitif           │
│  - Pluviomètre                      │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│  PASSERELLE EDGE (Raspberry Pi)     │
│                                      │
│  Traitement Local :                 │
│  ✓ Moyenne glissante 10 minutes     │
│  ✓ Détection gel immédiate          │
│  ✓ Calcul évapotranspiration        │
│  ✓ Déclenchement alarme locale      │
│                                      │
│  Stockage Local :                   │
│  ✓ SQLite : 7 jours de données      │
│  ✓ Buffer : 1000 mesures non envoyées│
│                                      │
│  Décisions Autonomes :              │
│  ✓ Si T < 2°C → SMS agriculteur     │
│  ✓ Si pluie > 50mm → arrêt irrigation│
└──────────────┬──────────────────────┘
               │ (4G/LoRaWAN)
               ▼
┌─────────────────────────────────────┐
│  CLOUD (uniquement agrégation)      │
│  - Données horaires (pas brutes)    │
│  - Analyses long terme              │
│  - Comparaison entre parcelles      │
└─────────────────────────────────────┘
```

**Avantages dans ce cas :**

- Alarme gel fonctionne même sans internet (critique pour vignes)
- Économie de bande passante 4G (coûteuse en zone rurale)
- Historique local disponible pour diagnostic maintenance

---

## 3. Architecture Cloud-Centric - Exemple AWS IoT

### Exemple : Réseau Urbain de 100 Stations Météo

**Architecture complète AWS :**

```
┌──────────────────────────────────────────┐
│  STATIONS MÉTÉO (ESP8266 simple)         │
│  Code minimal : lire capteurs + envoyer  │
│  Pas de logique métier                   │
│  Consommation : 80mA en transmission     │
└────────────┬─────────────────────────────┘
             │ MQTT over TLS
             ▼
┌──────────────────────────────────────────┐
│  AWS IoT Core (Broker MQTT managé)       │
│  - Authentification par certificat       │
│  - 100 connexions simultanées            │
│  - Routage automatique des messages      │
└────────────┬─────────────────────────────┘
             │
        ┌────┴────┬──────────────┐
        ▼         ▼              ▼
┌─────────┐ ┌──────────┐ ┌────────────┐
│ Lambda  │ │Timestream│ │  Kinesis   │
│Functions│ │ Database │ │   Stream   │
└────┬────┘ └─────┬────┘ └──────┬─────┘
     │            │             │
     │            │             ▼
     │            │      ┌──────────────┐
     │            │      │  Analytics   │
     │            │      │  (tendances) │
     │            │      └──────────────┘
     ▼            ▼
┌──────────────────────────────────────────┐
│  QuickSight Dashboard Public             │
│  - Carte interactive 100 stations        │
│  - Température moyenne par quartier      │
│  - Îlots de chaleur urbains              │
└──────────────────────────────────────────┘
```

**Fonction Lambda - Exemple de traitement :**

```python
def lambda_handler(event, context):
    # Réception données station
    station_id = event['station_id']
    temperature = event['temperature']
    
    # Validation
    if temperature < -50 or temperature > 60:
        send_alert_maintenance(station_id)
        return
    
    # Enrichissement
    location = get_station_location(station_id)
    quartier = get_quartier(location)
    
    # Stockage
    save_to_timestream({
        'station': station_id,
        'quartier': quartier,
        'temperature': temperature,
        'timestamp': now()
    })
    
    # Alerte canicule
    if temperature > 35:
        publish_heatwave_alert(quartier)
```

**Coûts typiques (100 stations, lecture/minute) :**

- AWS IoT Core : ~50€/mois
- Timestream : ~30€/mois
- Lambda : ~10€/mois
- Total : ~90€/mois (scalable automatiquement)

---

## 4. Architecture Hybride - Exemple Réel

### Exemple : Campus Universitaire Intelligent

**Problème** : 20 bâtiments, besoin d'autonomie locale + analyses centralisées

```
┌─────────────────────────────────────────────┐
│  BÂTIMENT A (Edge Gateway)                  │
│                                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  │
│  │ Station  │  │ Station  │  │ Station  │  │
│  │ Toit     │  │ Façade N │  │ Façade S │  │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  │
│       └─────────────┼─────────────┘         │
│                     ▼                        │
│       ┌──────────────────────────┐          │
│       │  Raspberry Pi 4          │          │
│       │  TRAITEMENT LOCAL :      │          │
│       │  ✓ Moyenne 3 stations    │          │
│       │  ✓ Contrôle CVC si T>25°C│          │
│       │  ✓ Ouverture stores      │          │
│       │  ✓ Décision en 100ms     │          │
│       └──────────┬───────────────┘          │
└──────────────────┼──────────────────────────┘
                   │ Ethernet
                   ▼
┌──────────────────────────────────────────────┐
│  SERVEUR CAMPUS (On-Premise)                 │
│                                               │
│  PostgreSQL + TimescaleDB :                  │
│  - Agrégation 20 bâtiments                   │
│  - Calculs économies énergie                 │
│  - Prévisions consommation                   │
│  - Dashboard administrateurs                 │
│                                               │
│  SYNCHRONISATION CLOUD :                     │
│  - Export quotidien données anonymisées      │
│  - Analyses machine learning (Azure)         │
│  - Rapports mensuels                         │
└───────────────────────────────────────────────┘
```

**Répartition Intelligence :**

|Fonction|Localisation|Latence|Raison|
|---|---|---|---|
|Contrôle CVC|Edge (bâtiment)|<1s|Confort immédiat|
|Détection fenêtre ouverte|Edge|<5s|Économie énergie|
|Agrégation campus|Serveur local|1min|Cohérence données|
|Prévisions ML|Cloud|1h|Puissance calcul|
|Rapports annuels|Cloud|24h|Stockage long terme|

---

## 5. Modèle Publication-Abonnement - Exemple MQTT

### Exemple : Réseau Communautaire de Stations

**Structure des Topics MQTT :**

```
meteo/
├── france/
│   ├── paris/
│   │   ├── 75001/
│   │   │   ├── station_123/temperature
│   │   │   ├── station_123/humidite
│   │   │   ├── station_123/pression
│   │   │   └── station_456/temperature
│   │   └── 75002/
│   │       └── station_789/temperature
│   └── lyon/
│       └── 69001/
│           └── station_321/temperature
└── belgique/
    └── bruxelles/
        └── 1000/
            └── station_654/temperature
```

**Exemple Code Station (Publication) :**

```python
import paho.mqtt.client as mqtt
import time

# Connexion broker
client = mqtt.Client("station_123")
client.connect("broker.meteo-community.org", 1883)

# Boucle de mesure
while True:
    temp = read_temperature()
    humidity = read_humidity()
    pressure = read_pressure()
    
    # Publication sur topics appropriés
    client.publish("meteo/france/paris/75001/station_123/temperature", 
                   f"{temp}", qos=1)
    client.publish("meteo/france/paris/75001/station_123/humidite", 
                   f"{humidity}", qos=1)
    client.publish("meteo/france/paris/75001/station_123/pression", 
                   f"{pressure}", qos=1)
    
    time.sleep(60)  # Mesure par minute
```

**Exemple Application Utilisateur (Abonnement) :**

```python
import paho.mqtt.client as mqtt

def on_message(client, userdata, message):
    print(f"Topic: {message.topic}")
    print(f"Valeur: {message.payload.decode()}")

# Application dashboard quartier
client = mqtt.Client("dashboard_75001")
client.on_message = on_message
client.connect("broker.meteo-community.org", 1883)

# Abonnement toutes stations du quartier
client.subscribe("meteo/france/paris/75001/+/temperature")
client.subscribe("meteo/france/paris/75001/+/humidite")

client.loop_forever()
```

**Exemple Service Alerte (Abonnement sélectif) :**

```python
import paho.mqtt.client as mqtt

def on_message(client, userdata, message):
    temp = float(message.payload.decode())
    
    # Extraction station depuis topic
    station_id = message.topic.split('/')[4]
    
    # Alerte canicule
    if temp > 35:
        send_push_notification(
            f"🌡️ Alerte canicule station {station_id}: {temp}°C"
        )

client = mqtt.Client("alerte_canicule")
client.on_message = on_message
client.connect("broker.meteo-community.org", 1883)

# Surveillance toute la France
client.subscribe("meteo/france/+/+/+/temperature")

client.loop_forever()
```

**Avantages Publication-Abonnement :**

1. **Découplage** : La station ne connaît pas les applications qui l'utilisent
2. **Scalabilité** : Ajouter 1000 nouvelles applications sans toucher aux stations
3. **Flexibilité** : Chaque utilisateur choisit ses abonnements (son quartier uniquement)
4. **Hiérarchie** : Abonnements wildcards (`+` = un niveau, `#` = tous sous-niveaux)

**Exemple abonnement hiérarchique :**

```python
# Toutes températures Paris
client.subscribe("meteo/france/paris/#/temperature")

# Toutes données station 123
client.subscribe("meteo/france/paris/75001/station_123/#")

# Toutes pressions de France
client.subscribe("meteo/france/+/+/+/pression")
```

---

## 6. Comparaison Architectures - Cas Pratique

**Scénario** : Vous devez choisir l'architecture pour votre projet

|Critère|Edge|Cloud|Hybride|
|---|---|---|---|
|**Coût initial**|Moyen (RPi)|Faible (ESP)|Élevé (les deux)|
|**Coût mensuel**|Nul|50-200€|20-100€|
|**Latence décisions**|<100ms|1-5s|<100ms (local)|
|**Résilience panne**|⭐⭐⭐⭐⭐|⭐⭐|⭐⭐⭐⭐|
|**Scalabilité**|⭐⭐|⭐⭐⭐⭐⭐|⭐⭐⭐⭐|
|**Analyses complexes**|⭐⭐|⭐⭐⭐⭐⭐|⭐⭐⭐⭐|
|**Maintenance**|Difficile|Facile|Moyenne|

**Recommandation par cas d'usage :**

- **Hobby/Éducation** → Edge (Raspberry Pi + InfluxDB)
- **Startup/PME** → Cloud (AWS/Azure IoT)
- **Industrie critique** → Hybride (décisions locales + analytics cloud)
- **Recherche** → Cloud (puissance calcul ML)

---

Ces exemples concrets montrent comment les architectures théoriques se traduisent en implémentations réelles avec du code, des composants précis et des cas d'usage authentiques.