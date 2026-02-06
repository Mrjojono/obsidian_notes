
# FICHE DE RÉVISION - SÉCURITÉ DES BASES DE DONNÉES

**Cours :** Sécurité des Bases de Données  
**Chargés de cours :** MM. TETTEH Amen & AZOTI Pierre  
**Niveau :** IAI Semestre 5

---

## CHAPITRE 1 : GÉNÉRALITÉS SUR LA SÉCURITÉ DES BASES DE DONNÉES

### 1.1 Définition de la sécurité des bases de données

La sécurité des bases de données fait référence à l'ensemble des outils, contrôles et mesures destinés à établir et protéger la confidentialité, l'intégrité et la disponibilité des bases de données.

**Important :** La confidentialité est l'élément qui est compromis dans la plupart des violations de données.

### 1.2 Éléments à protéger

La sécurité des bases de données doit traiter et protéger les éléments suivants :

1. **Les données de la base de données** - Les informations stockées elles-mêmes
2. **Le système de gestion de la base de données (SGBD)** - Le logiciel qui gère la BD
3. **Toutes les applications associées** - Applications qui interagissent avec la BD
4. **Le serveur de base de données** - Physique et/ou virtuel ainsi que le matériel sous-jacent
5. **L'infrastructure informatique et/ou réseau** - Utilisée pour accéder à la base de données

### 1.3 Paradoxe de la sécurité des bases de données

La sécurité des bases de données est une activité complexe et difficile qui implique tous les aspects des technologies et des pratiques de sécurité de l'information.

**La règle d'Anderson :** Il existe une contradiction naturelle entre la convivialité et la sécurité d'une base de données :

- Plus une base de données est accessible et utilisable, plus elle est vulnérable aux menaces de sécurité
- Inversement, plus une base de données est invulnérable aux menaces, plus elle est difficile d'accès et d'utilisation

Ce paradoxe représente un défi constant dans la gestion de la sécurité des BD.

---

## CHAPITRE 2 : IMPORTANCE DE LA SÉCURITÉ DES BASES DE DONNÉES

### 2.1 Définition d'une atteinte à la protection des données

Une atteinte à la protection des données est un manquement à la confidentialité des données d'une base de données.

### 2.2 Conséquences d'une violation de données

Le préjudice qu'une atteinte à la protection des données inflige à une entreprise dépend de plusieurs facteurs :

**1. Propriété intellectuelle compromise**

- Perte de secrets commerciaux
- Vol de brevets ou d'innovations
- Exposition de stratégies commerciales

**2. Atteinte à la réputation de la marque**

- Perte de confiance des clients
- Impact négatif sur l'image de l'entreprise
- Difficultés à attirer de nouveaux clients

**3. Continuité des opérations (ou absence de continuité)**

- Interruption des services
- Perturbation des processus métier
- Perte de productivité

**4. Amendes ou pénalités en cas de non-conformité**

- Sanctions réglementaires
- Violations de lois sur la protection des données
- Poursuites judiciaires

**5. Coûts de réparation des violations et de notification aux clients**

- Frais de réparation technique
- Coûts de communication avec les clients affectés
- Frais juridiques et de consultation

---

## CHAPITRE 3 : MENACES LIÉES À LA SÉCURITÉ DES BASES DE DONNÉES

### 3.1 Classification générale des attaques

#### 3.1.1 Attaques non frauduleuses

Ces attaques ne sont pas intentionnelles mais peuvent causer des dommages importants :

- **Catastrophes naturelles** - Incendies, inondations, tremblements de terre, etc.
- **Pannes de logiciel ou de matériel** - Défaillances techniques imprévues
- **Erreurs humaines** - Suppressions accidentelles, mauvaises configurations, erreurs de manipulation

#### 3.1.2 Attaques frauduleuses

Ces attaques sont intentionnelles et malveillantes :

- **Utilisation abusive de leurs droits par les utilisateurs** - Employés qui exploitent leurs accès légitimes
- **Agents hostiles** - Personnes exécutant des actions de destruction du logiciel ou du matériel, ou lisant/mettant à jour des données protégées
- **Chevaux de Troie** - Agents malveillants cachés dans des actions légales

### 3.2 Menaces spécifiques détaillées

#### A. Menaces internes

Les menaces provenant de l'intérieur de l'organisation, souvent les plus dangereuses car les utilisateurs internes ont déjà des accès légitimes.

**Caractéristiques :**

- Accès légitimes aux systèmes
- Connaissance de l'infrastructure
- Difficiles à détecter

#### B. Erreur humaine

Les erreurs involontaires commises par les utilisateurs ou administrateurs qui peuvent compromettre la sécurité.

**Exemples :**

- Suppression accidentelle de données
- Mauvaise configuration de permissions
- Partage involontaire d'informations sensibles
- Utilisation de mots de passe faibles

#### C. Exploitation des vulnérabilités des logiciels de base de données

Les attaquants exploitent les failles de sécurité dans les SGBD ou les applications associées.

**Types de vulnérabilités :**

- Failles de sécurité non corrigées
- Bugs dans le code
- Versions obsolètes de logiciels

#### D. Attaques par injection SQL/NoSQL

**Injection SQL :** Technique d'attaque où du code SQL malveillant est inséré dans les requêtes pour manipuler la base de données.

**Exemples d'impacts :**

- Lecture de données sensibles
- Modification ou suppression de données
- Exécution de commandes administratives
- Contournement de l'authentification

**Injection NoSQL :** Variante pour les bases de données NoSQL utilisant des techniques similaires adaptées à ces systèmes.

#### E. Exploitation des dépassements de mémoire tampon

Attaque qui consiste à écrire plus de données dans un tampon mémoire que ce qu'il peut contenir, permettant d'exécuter du code malveillant.

**Conséquences :**

- Exécution de code arbitraire
- Crash du système
- Élévation de privilèges

#### F. Logiciels malveillants (Malware)

Programmes malveillants conçus pour endommager, perturber ou obtenir un accès non autorisé aux systèmes.

**Types :**

- Virus
- Vers
- Ransomware
- Spyware
- Rootkits

#### G. Attaques des sauvegardes (Exposition de données de sauvegarde)

Les sauvegardes peuvent être ciblées car elles contiennent souvent des données complètes et peuvent être moins protégées.

**Risques :**

- Vol de supports de sauvegarde
- Accès non autorisé aux fichiers de sauvegarde
- Sauvegardes non chiffrées

#### H. Attaques par déni de service (DoS/DDoS)

**DoS (Denial of Service) :** Attaque visant à rendre un service indisponible en le saturant de requêtes.

**DDoS (Distributed Denial of Service) :** Version distribuée utilisant plusieurs sources pour amplifier l'attaque.

**Méthodes :**

- Saturation de la bande passante
- Épuisement des ressources système
- Exploitation de failles pour crasher le service

#### I. Abus de privilèges excessifs et légitimes

Situation où des utilisateurs possèdent plus de droits d'accès que nécessaire pour leurs fonctions.

**Problèmes :**

- Accès à des données non nécessaires au travail
- Risque d'utilisation malveillante
- Difficulté à tracer les responsabilités

**Principe de solution :** Appliquer le principe du moindre privilège

#### J. Élévation de privilège

Technique permettant à un attaquant d'obtenir des droits d'accès supérieurs à ceux qui lui sont normalement accordés.

**Types :**

- Élévation verticale (utilisateur vers administrateur)
- Élévation horizontale (accès aux données d'autres utilisateurs)

#### K. Exploitation de failles des bases de données vulnérables ou mal configurées

**Bases de données vulnérables :**

- Non mise à jour avec les correctifs de sécurité
- Utilisation de versions obsolètes
- Présence de vulnérabilités connues

**Bases de données mal configurées :**

- Paramètres de sécurité par défaut non modifiés
- Comptes administrateurs avec mots de passe faibles
- Services inutiles activés
- Ports ouverts inutilement

#### L. Faiblesse de l'audit natif

Insuffisance des capacités d'audit intégrées aux SGBD.

**Conséquences :**

- Difficulté à détecter les activités suspectes
- Manque de traçabilité
- Impossibilité d'analyser les incidents de sécurité
- Non-conformité aux exigences réglementaires

#### M. Vulnérabilités des protocoles de communication des bases de données

Failles dans les protocoles utilisés pour communiquer avec la base de données.

**Risques :**

- Interception des communications (man-in-the-middle)
- Écoute du trafic réseau
- Modification des données en transit
- Absence de chiffrement

#### N. Copies non autorisées de données sensibles

Duplication illégitime de données sensibles en dehors des systèmes contrôlés.

**Méthodes :**

- Export massif de données
- Copie sur supports amovibles
- Envoi par email
- Téléchargement vers des services cloud personnels

---

## CHAPITRE 4 : DÉFIS POUR LA SÉCURITÉ DES DONNÉES

### 4.1 Défi n°1 : Des équipes informatiques en sous-effectif

**Problématique :**

- Manque de personnel qualifié en sécurité
- Charge de travail importante sur un nombre limité de personnes
- Difficulté à maintenir une surveillance continue
- Temps insuffisant pour la formation et la veille technologique

**Conséquences :**

- Réponse lente aux incidents
- Mise à jour retardée des systèmes
- Surveillance inadéquate
- Burnout des équipes

### 4.2 Défi n°2 : Des budgets limités

**Problématique :**

- Ressources financières insuffisantes pour la sécurité
- Difficulté à justifier les investissements préventifs
- Compromis entre sécurité et autres priorités métier

**Conséquences :**

- Impossibilité d'acquérir les meilleurs outils
- Formation limitée du personnel
- Infrastructure de sécurité incomplète
- Dépendance à des solutions gratuites ou limitées

### 4.3 Défi n°3 : Des dépenses consacrées à des outils inefficaces

**Problématique :**

- Investissements dans des solutions inadaptées
- Outils mal configurés ou sous-utilisés
- Multiplication d'outils qui ne communiquent pas entre eux
- Complexité excessive des solutions

**Conséquences :**

- Gaspillage de ressources financières
- Faux sentiment de sécurité
- Complexification de l'infrastructure
- Difficultés d'intégration et de maintenance

---

## CHAPITRE 5 : OBJECTIFS DE LA SÉCURISATION DES BASES DE DONNÉES

Assurer la sécurité d'une base de données, c'est maintenir trois objectifs fondamentaux formant la triade CIA :

### 5.1 LA CONFIDENTIALITÉ

**Définition :** La confidentialité consiste à détecter et/ou empêcher les accès non autorisés aux données sensibles.

**Principe de base :** Le principe du moindre privilège

- Chaque utilisateur ne doit avoir accès qu'aux données strictement nécessaires à ses fonctions
- Limitation des droits au minimum requis

**Importance cruciale dans :**

- Les environnements critiques ou stratégiques :
    - Secteur de la sécurité nationale
    - Secteur bancaire et financier
    - Opérateurs de télécommunications
    - Secteur de la santé

**Protection des droits individuels :**

- Respect du droit des individus concernant leurs données à caractère personnel
- Droit de décider comment et dans quel but les informations les concernant peuvent être :
    - Extraites
    - Mémorisées
    - Transmises à d'autres individus

**Mécanismes de protection :**

- Contrôle d'accès
- Chiffrement des données
- Anonymisation et pseudonymisation
- Classification des données selon leur sensibilité

### 5.2 L'INTÉGRITÉ

**Définition :** L'intégrité consiste à protéger les données contre toute suppression ou modification abusive.

**Objectifs :** Détecter ou empêcher des modifications illicites des données qu'elles soient dues à :

- Des pannes de système
- Des manipulations erronées
- Des sabotages intentionnels

**Moyens de garantir l'intégrité :**

1. **Signature numérique**
    
    - Vérification de l'authenticité des contenus
    - Garantie que les données n'ont pas été altérées
    - Utilisation largement répandue par les pouvoirs publics et organismes de santé
2. **Contrôles d'intégrité**
    
    - Checksums et hash
    - Validation des transactions
    - Journalisation des modifications
3. **Transactions sécurisées**
    
    - Propriétés ACID (Atomicité, Cohérence, Isolation, Durabilité)
    - Mécanismes de rollback

**Applications pratiques :**

- Dossiers médicaux électroniques
- Transactions financières
- Documents juridiques
- Enregistrements gouvernementaux

### 5.3 LA DISPONIBILITÉ

**Définition :** Il s'agit de détecter ou d'empêcher des dénis de service.

**Concept de déni de service :** Il y a déni de service lorsqu'un utilisateur ne parvient pas à accéder dans un délai raisonnable à une information ou à une ressource pour laquelle il a une autorisation d'accès.

**Exemple typique d'attaque :** Une attaque consistant à saturer un serveur de fausses requêtes empêchant les requêtes valides d'être exécutées.

**Mécanismes de protection :**

- Redondance des systèmes
- Plans de continuité d'activité (PCA)
- Plans de reprise d'activité (PRA)
- Systèmes de backup
- Distribution de charge (load balancing)
- Protection contre les attaques DDoS

**Objectifs de disponibilité :**

- Temps de disponibilité (uptime) maximal
- Temps de réponse acceptable
- Capacité de récupération rapide
- Maintien des services critiques

---

## CHAPITRE 6 : CONCEPTS DE BASE DE LA SÉCURITÉ DES DONNÉES

### 6.1 Évaluez et atténuez vos risques informatiques

**Identifier les comptes d'utilisateur périmés dans vos répertoires**

Problématique :

- Comptes d'anciens employés toujours actifs
- Comptes de test ou temporaires oubliés
- Comptes dormants non désactivés

Actions à mener :

- Audit régulier des comptes utilisateurs
- Désactivation automatique des comptes inactifs
- Revue périodique des accès
- Processus de déprovisionnement lors des départs

**Trouver les utilisateurs dotés de privilèges d'administrateur superflus**

Problématique :

- Trop d'utilisateurs avec des droits administratifs
- Privilèges accordés temporairement et jamais révoqués
- Comptes de service avec privilèges excessifs

Actions à mener :

- Inventaire des comptes administrateurs
- Justification de chaque privilège élevé
- Révocation des privilèges non nécessaires
- Mise en place d'un processus d'approbation pour les privilèges élevés

**Analyser votre environnement à la recherche de fichiers potentiellement nuisibles**

Actions à mener :

- Scan antivirus et antimalware réguliers
- Recherche de fichiers suspects (extensions inhabituelles, tailles anormales)
- Analyse des fichiers exécutables
- Vérification de l'intégrité des fichiers système
- Surveillance des modifications de fichiers critiques

### 6.2 Effectuez un inventaire des actifs

**Dresser une liste de tous vos serveurs et le but de chacun d'entre eux**

Documentation nécessaire :

- Nom et identification du serveur
- Fonction et rôle du serveur
- Applications hébergées
- Données stockées
- Propriétaire/responsable
- Criticité pour l'entreprise
- Dépendances avec d'autres systèmes

**Vérifier vos systèmes d'exploitation**

Points de contrôle :

- Version et édition des OS
- Niveau de patch de sécurité
- Date de dernière mise à jour
- Planification des mises à jour
- Systèmes obsolètes à remplacer
- Licences et support

**Assurer d'avoir un antivirus installé et à jour**

Exigences :

- Installation sur tous les systèmes
- Mises à jour automatiques des définitions de virus
- Scans réguliers programmés
- Protection en temps réel activée
- Rapports de scan consultés
- Incidents traités rapidement

**Examiner les autres programmes et services**

À vérifier :

- Liste complète des logiciels installés
- Versions et mises à jour
- Licences valides
- Services en cours d'exécution
- Ports ouverts
- Services inutiles à désactiver
- Applications non autorisées

### 6.3 Connaissez vos données

**Faire une examination exhaustive de chaque recoin de votre environnement**

Objectif : Savoir exactement où se trouvent les données sensibles

**Défis à relever :**

1. **Les données peuvent être disséminées dans plusieurs systèmes**
    
    - Serveurs de fichiers
    - Bases de données multiples
    - Postes de travail
    - Appareils mobiles
    - Services cloud
    - Emails et systèmes de messagerie
    - Sauvegardes
2. **Les données peuvent être structurées et non structurées**
    
    Données structurées :
    
    - Bases de données relationnelles
    - Feuilles de calcul
    - Fichiers CSV
    
    Données non structurées :
    
    - Documents Word, PDF
    - Emails
    - Images et vidéos
    - Fichiers audio
    - Documents scannés
3. **Les données sont sujettes à des modifications constantes**
    
    - Création continue de nouvelles données
    - Modification des données existantes
    - Déplacement des données
    - Copie et duplication
    - Suppression et archivage

**Actions recommandées :**

- Cartographie complète des données
- Classification par niveau de sensibilité
- Identification des données personnelles (RGPD)
- Traçabilité des flux de données
- Inventaire régulièrement mis à jour

### 6.4 Sachez qui peut accéder à quoi

**Vous devez vous pencher sur les autorisations d'accès**

**Déterminer le niveau d'accès de chaque utilisateur**

Questions à se poser :

- Qui a accès à quelles données ?
- Quels sont les niveaux de permissions (lecture, écriture, modification, suppression) ?
- Les accès sont-ils justifiés par les fonctions de l'utilisateur ?
- Y a-t-il des accès excessifs ?

Méthodes :

- Matrice d'accès (utilisateurs × ressources)
- Groupes et rôles définis
- Documentation des besoins d'accès par fonction
- Processus de demande d'accès formalisé

**Réexaminer régulièrement les droits d'accès**

Fréquence recommandée :

- Revue trimestrielle pour les accès sensibles
- Revue annuelle pour tous les accès
- Revue immédiate lors de changements de poste
- Revue lors des départs

**Établir et maintenir un modèle d'accès basé sur le principe du moindre privilège**

Principe du moindre privilège :

- Accorder uniquement les permissions strictement nécessaires
- Commencer avec un accès minimal
- Ajouter des permissions selon les besoins justifiés
- Révocation automatique après une période
- Séparation des tâches (separation of duties)

Mise en œuvre :

- Définition de rôles standards
- Attribution par rôle plutôt que par utilisateur
- Élévation temporaire de privilèges si nécessaire
- Traçabilité de toute élévation de privilèges

### 6.5 Voyez ce qui se passe

**Rechercher les pics suspects d'activité**

Indicateurs d'anomalies :

- Volume inhabituel de requêtes
- Accès à un grand nombre de fichiers en peu de temps
- Téléchargements massifs de données
- Modifications en masse
- Tentatives d'accès échouées répétées
- Changements de configuration système

Outils nécessaires :

- Systèmes de monitoring en temps réel
- Alertes automatiques
- Tableaux de bord de surveillance
- Analyse de comportement (baseline vs anomalies)

**Rechercher les activités en dehors des heures de travail**

Surveillance spécifique :

- Connexions nocturnes ou week-end non justifiées
- Accès depuis des fuseaux horaires inhabituels
- Activités pendant les périodes de congés
- Processus automatisés suspects

Questions à se poser :

- L'utilisateur est-il censé travailler à cette heure ?
- L'activité correspond-elle à son rôle ?
- Y a-t-il une raison métier légitime ?

**Contrôler les accès VPN anormaux**

Points de vigilance :

- Connexions depuis des pays ou régions inhabituels
- Connexions multiples simultanées du même compte
- Utilisation de VPN depuis des IP suspectes
- Connexions juste après un voyage à l'étranger
- Patterns de connexion inhabituels

Mesures de sécurité :

- Authentification multi-facteurs (MFA) obligatoire
- Restriction géographique si possible
- Limitation des heures de connexion
- Surveillance des sessions actives
- Déconnexion automatique après inactivité

---

## CHAPITRE 7 : TYPES D'ATTAQUES - CLASSIFICATION DÉTAILLÉE

### 7.1 Attaques non frauduleuses

**Catastrophes naturelles**

- Tremblements de terre
- Inondations
- Incendies
- Ouragans et tempêtes
- Foudre

Mesures de protection :

- Localisation géographique réfléchie des datacenters
- Redondance géographique
- Plans de continuité d'activité
- Systèmes de protection physique (anti-incendie, etc.)

**Pannes de logiciel ou de matériel**

- Défaillance des disques durs
- Problèmes de mémoire
- Bugs logiciels
- Corruptions de données
- Incompatibilités de versions

Mesures de protection :

- Maintenance préventive
- Redondance matérielle (RAID, etc.)
- Mises à jour régulières
- Tests de récupération

**Erreurs humaines**

- Suppressions accidentelles
- Mauvaises configurations
- Erreurs de saisie
- Commandes erronées
- Procédures mal suivies

Mesures de protection :

- Formation continue
- Procédures claires et documentées
- Contrôles de validation
- Sauvegardes fréquentes
- Possibilité de rollback

### 7.2 Attaques frauduleuses

**Utilisation abusive de leurs droits par les utilisateurs**

Scénarios typiques :

- Employé vendant des informations confidentielles
- Consultation de données par curiosité
- Modification de données pour avantage personnel
- Sabotage par un employé mécontent
- Accès à des données de collègues

Prévention :

- Monitoring des activités
- Séparation des tâches
- Principe du moindre privilège
- Audit régulier des accès
- Culture de sécurité

**Agents hostiles**

Types d'agents :

- Hackers externes
- Organisations criminelles
- Concurrents
- Acteurs étatiques (cyber-espionnage)
- Hacktivistes

Actions malveillantes :

- Destruction de logiciel ou matériel
- Vol de données
- Modification de données
- Espionnage industriel
- Ransomware

**Chevaux de Troie**

Caractéristiques :

- Logiciel apparemment légitime
- Contient une fonctionnalité cachée malveillante
- Peut être caché dans des actions légales
- Difficile à détecter

Exemples :

- Application de productivité avec backdoor
- Mise à jour de logiciel compromise
- Plugin ou extension malveillante
- Utilitaire gratuit avec spyware intégré

Protection :

- Vérification de l'origine des logiciels
- Analyse antivirus
- Principe de moindre privilège
- Surveillance des comportements anormaux
- Sandboxing pour les applications suspectes

---

## CHAPITRE 8 : TYPES DE SÉCURITÉ DES DONNÉES

### 8.1 Contrôle des accès

**Définition :** Type de sécurité comprenant la limitation des accès physiques et numériques aux systèmes et données stratégiques.

**Contrôles d'accès physiques :**

Protection des espaces :

- Accès limité aux salles serveurs
- Badges et systèmes d'identification
- Vidéosurveillance
- Agents de sécurité
- Sas d'entrée
- Registres de visiteurs

Protection des équipements :

- Verrouillage des racks
- Câbles antivol
- Alarmes
- Contrôle environnemental (température, humidité)

**Contrôles d'accès numériques :**

Protection des systèmes :

- Tous les ordinateurs et appareils protégés
- Saisie obligatoire d'informations d'identification
- Verrouillage automatique après inactivité
- Chiffrement des disques
- Gestion centralisée des accès

Niveaux de contrôle :

- Contrôle d'accès au réseau (NAC)
- Contrôle d'accès aux applications
- Contrôle d'accès aux données
- Segmentation réseau
- VLANs et firewalls

**Modèles de contrôle d'accès :**

1. **DAC (Discretionary Access Control)**
    
    - Le propriétaire décide des accès
    - Flexible mais risqué
2. **MAC (Mandatory Access Control)**
    
    - Contrôle basé sur des classifications
    - Très rigide mais très sécurisé
3. **RBAC (Role-Based Access Control)**
    
    - Accès basés sur les rôles
    - Équilibre entre flexibilité et sécurité
4. **ABAC (Attribute-Based Access Control)**
    
    - Accès basés sur des attributs multiples
    - Très granulaire

### 8.2 Authentification

**Définition :** L'authentification permet d'identifier avec précision les utilisateurs avant qu'ils aient accès aux données.

**Les trois facteurs d'authentification :**

1. **Ce que vous savez** (Knowledge)
    
    - Mots de passe
    - Codes PIN
    - Phrases de passe
    - Questions de sécurité
    - Patterns
2. **Ce que vous avez** (Possession)
    
    - Jetons de sécurité (tokens)
    - Cartes magnétiques
    - Cartes à puce
    - Téléphone mobile (pour codes OTP)
    - Clés USB de sécurité
3. **Ce que vous êtes** (Inherence)
    
    - Identifiants biométriques :
        - Empreintes digitales
        - Reconnaissance faciale
        - Scan de l'iris
        - Reconnaissance vocale
        - Reconnaissance des veines
        - Analyse de la démarche

**Authentification multi-facteurs (MFA) :**

Principe :

- Combinaison de deux facteurs ou plus
- Augmente considérablement la sécurité
- Recommandée pour tous les accès sensibles

Exemples :

- Mot de passe + code SMS
- Carte à puce + PIN
- Biométrie + mot de passe
- Application d'authentification + empreinte digitale

**Bonnes pratiques :**

- MFA obligatoire pour les accès administrateurs
- MFA pour les accès distants (VPN)
- Tokens FIDO2 pour maximum de sécurité
- Pas de SMS pour les systèmes critiques (vulnérable au SIM swapping)
- Utilisation d'applications d'authentification (Google Authenticator, Authy)

**Single Sign-On (SSO) :**

- Authentification unique pour plusieurs applications
- Améliore l'expérience utilisateur
- Centralise le contrôle d'accès
- Nécessite une sécurisation renforcée du point d'authentification

### 8.3 Sauvegarde et récupération

**Définition :** Mettre en place une bonne sécurité des données, c'est aussi prévoir un plan permettant d'accéder aux données en toute sécurité en cas de défaillance du système, de catastrophe, ou de corruption ou fuite des données.

**Principe de base :** Disposer d'une copie des données sauvegardées dans un format distinct du format d'origine pour pouvoir les récupérer si nécessaire.

**Supports de sauvegarde :**

1. **Disque physique local**
    
    - Disques durs externes
    - NAS (Network Attached Storage)
    - Avantages : Rapide, contrôle total
    - Inconvénients : Risque de destruction simultanée
2. **Réseau local**
    
    - Serveurs de sauvegarde dédiés
    - SAN (Storage Area Network)
    - Avantages : Centralisé, géré
    - Inconvénients : Même site physique
3. **Cloud**
    
    - Services de sauvegarde cloud
    - Stockage objet (S3, Azure Blob)
    - Avantages : Hors site, scalable, géographiquement distant
    - Inconvénients : Dépendance à Internet, coûts variables

**Stratégie 3-2-1 :**

- 3 copies des données
- 2 types de supports différents
- 1 copie hors site

**Stratégie 3-2-1-1-0 :**

- Ajout de : 1 copie offline (air-gapped)
- 0 erreur de vérification

**Types de sauvegardes :**

1. **Sauvegarde complète (Full backup)**
    
    - Copie de toutes les données
    - Récupération la plus simple
    - Prend le plus de temps et d'espace
2. **Sauvegarde incrémentielle**
    
    - Uniquement les modifications depuis la dernière sauvegarde
    - Rapide et économe en espace
    - Récupération plus complexe
3. **Sauvegarde différentielle**
    
    - Modifications depuis la dernière sauvegarde complète
    - Compromis entre complète et incrémentielle

**Fréquence de sauvegarde :**

- Données critiques : Continu ou plusieurs fois par jour
- Données importantes : Quotidien
- Données standards : Hebdomadaire
- Archives : Mensuel

**Plan de récupération (DRP - Disaster Recovery Plan) :**

Éléments essentiels :

- RTO (Recovery Time Objective) : Temps maximum acceptable de panne
- RPO (Recovery Point Objective) : Perte de données maximale acceptable
- Procédures documentées de récupération
- Tests réguliers de restauration
- Équipe de récupération identifiée
- Communication de crise

**Tests de restauration :**

- Vérifier régulièrement que les sauvegardes fonctionnent
- Tester différents scénarios de panne
- Mesurer les temps de récupération réels
- Identifier les problèmes avant une vraie catastrophe
- Former le personnel aux procédures

**Sécurité des sauvegardes :**

- Chiffrement des sauvegardes
- Contrôle d'accès strict
- Protection physique des supports
- Sauvegarde immuable (protection ransomware)
- Journalisation des accès aux sauvegardes

### 8.4 Effacement des données

**Définition :** L'effacement des données repose sur le remplacement total par un logiciel des données figurant sur un appareil de stockage.

**Pourquoi effacer correctement ?**

- Éviter la récupération de données sensibles
- Conformité réglementaire (RGPD, etc.)
- Protection lors de la mise au rebut d'équipements
- Réutilisation sécurisée des supports
- Fin de contrat avec employés ou prestataires

**Différence avec la suppression classique :**

Suppression classique :

- Supprime uniquement le pointeur vers le fichier
- Les données restent physiquement sur le disque
- Récupérables avec des outils appropriés
- Rapide mais non sécurisée

Effacement sécurisé :

- Écrase les données plusieurs fois
- Rend la récupération impossible ou extrêmement difficile
- Plus long mais sécurisé
- Garantit la confidentialité

**Méthodes d'effacement :**

1. **Écrasement logiciel**
    
    - Écriture de zéros
    - Écriture de données aléatoires
    - Multiples passes (DoD 5220.22-M : 7 passes)
    - Méthode Gutmann (35 passes)
2. **Destruction physique**
    
    - Broyage (shredding)
    - Incinération
    - Démagnétisation (degaussing)
    - Destruction mécanique
3. **Chiffrement avec destruction de clé**
    
    - Données chiffrées dès le départ
    - Destruction de la clé de chiffrement
    - Rend les données irrécupérables

**Normes et standards :**

- NIST SP 800-88 : Directives pour l'assainissement des médias
- DoD 5220.22-M : Standard du Département de la Défense américain
- HMG Infosec Standard 5 : Standard britannique

**Quand effacer ?**

- Mise au rebut d'équipements
- Réaffectation d'équipements
- Retour d'équipements loués
- Fin de projet ou de contrat
- Départ d'employés
- Régulièrement pour les données temporaires

**Outils d'effacement :**

- DBAN (Darik's Boot and Nuke)
- Eraser
- Blancco
- CCleaner (fonction d'effacement sécurisé)
- Commandes système (shred sous Linux, cipher sous Windows)

**Certification et documentation :**

- Certificat d'effacement
- Traçabilité des supports effacés
- Journal d'effacement
- Preuve de conformité pour audits

### 8.5 Masquage des données

**Définition :** Les logiciels de masquage des données cachent les informations en remplaçant des lettres et chiffres par d'autres caractères.

**Principe :** Si une personne non autorisée parvient à accéder aux données, elle ne pourra pas consulter les informations clés. Les données retrouvent leur forme d'origine uniquement lorsqu'un utilisateur autorisé les reçoit.

**Techniques de masquage :**

1. **Substitution**
    
    - Remplacement par des valeurs fictives mais réalistes
    - Exemple : "Jean Dupont" devient "Pierre Martin"
    - Maintient le format et la validité
2. **Brouillage (Shuffling)**
    
    - Mélange de valeurs réelles entre enregistrements
    - Exemple : Attribution aléatoire de dates de naissance à des clients
    - Données réalistes mais relations brisées
3. **Nullification**
    
    - Remplacement par des valeurs NULL ou vides
    - Simple mais perte totale d'information
4. **Variation**
    
    - Modification légère des valeurs
    - Exemple : Salaire de 50 000€ devient 51 234€
    - Préserve les statistiques générales
5. **Masquage de caractères**
    
    - Affichage partiel
    - Exemple : Carte bancale **** **** **** 1234
    - Numéro de téléphone : 06 ** ** ** 78

**Cas d'usage :**

**Environnements de développement et test :**

- Utilisation de données de production masquées
- Les développeurs ne voient pas de données réelles
- Respect de la confidentialité
- Tests réalistes possibles

**Conformité réglementaire :**

- RGPD : Anonymisation des données personnelles
- PCI DSS : Masquage des numéros de cartes bancaires
- HIPAA : Protection des données de santé

**Partage de données :**

- Avec des tiers ou partenaires
- Pour analyses statistiques
- Pour formation

**Différence avec le chiffrement :**

Masquage :

- Données transformées de façon irréversible (dans la plupart des cas)
- Pas de clé pour "démasquer"
- Utilisable directement
- Pour environnements non-production

Chiffrement :

- Transformation réversible
- Nécessite une clé pour déchiffrer
- Données inutilisables sans déchiffrement
- Pour protéger les données en production

**Pseudonymisation vs Anonymisation :**

Pseudonymisation :

- Possibilité de ré-identification avec informations supplémentaires
- Données toujours considérées comme personnelles (RGPD)
- Réversible

Anonymisation :

- Impossible de ré-identifier
- Données ne sont plus personnelles
- Irréversible

**Outils de masquage :**

- Oracle Data Masking and Subsetting
- IBM InfoSphere Optim Data Privacy
- Delphix
- IRI FieldShield
- Solutions open-source (Faker, etc.)

### 8.6 Résilience des données

**Définition :** Une sécurité des données complète signifie que vos systèmes peuvent surmonter des défaillances.

**Principe :** L'intégration de la notion de résilience à votre matériel et vos logiciels signifie que les événements comme des pannes de courant ou des catastrophes naturelles n'auront pas de conséquences sur la sécurité.

**Composantes de la résilience :**

**1. Redondance matérielle**

Stockage :

- RAID (Redundant Array of Independent Disks)
    - RAID 1 : Mirroring
    - RAID 5 : Parité distribuée
    - RAID 6 : Double parité
    - RAID 10 : Combinaison mirroring + striping
- Systèmes de stockage distribués

Serveurs :

- Clustering
- Serveurs en haute disponibilité (HA)
- Failover automatique
- Load balancing

Alimentation :

- UPS (Onduleurs)
- Générateurs de secours
- Alimentations redondantes
- Batteries de secours

Réseau :

- Liens redondants
- Routes multiples
- Fournisseurs d'accès multiples
- Équipements réseau redondants

**2. Réplication des données**

Synchrone :

- Écriture simultanée sur sites primaire et secondaire
- Aucune perte de données
- Impact sur les performances
- Distance limitée

Asynchrone :

- Écriture d'abord sur site primaire
- Réplication différée vers secondaire
- Risque de perte minime
- Distances illimitées

Géo-réplication :

- Réplication vers des sites géographiquement distants
- Protection contre catastrophes régionales
- Temps de latence à considérer

**3. Haute disponibilité (HA)**

Objectifs :

- Uptime de 99.9% à 99.999% (five nines)
- Réduction des temps d'arrêt planifiés et non planifiés

Techniques :

- Clustering actif-actif
- Clustering actif-passif
- Virtual IP failover
- Mises à jour sans interruption (rolling updates)

**4. Architectures résilientes**

Microservices :

- Isolation des défaillances
- Scalabilité indépendante
- Récupération partielle possible

Architecture distribuée :

- Pas de point unique de défaillance (SPOF)
- Distribution géographique
- Équilibrage de charge

**5. Préparation aux catastrophes**

Plan de continuité d'activité (BCP) :

- Identification des fonctions critiques
- Stratégies de maintien en opération
- Ressources de secours
- Procédures de basculement

Plan de reprise d'activité (DRP) :

- Procédures de récupération détaillées
- Sites de secours
- RTO et RPO définis
- Tests réguliers

**6. Capacité de récupération**

Auto-guérison (Self-healing) :

- Détection automatique des défaillances
- Redémarrage automatique des services
- Réaffectation automatique des ressources
- Orchestration automatisée (Kubernetes)

**7. Protection contre les pannes de courant**

Niveaux de protection :

- Onduleurs (UPS) : Protection immédiate
- Générateurs : Protection prolongée
- Alimentation redondante : Pas de point unique de défaillance
- Batterie de secours pour équipements critiques

**8. Protection environnementale**

Contrôles :

- Climatisation redondante
- Détection d'incendie et extinction automatique
- Contrôle d'humidité
- Détection de fuite d'eau
- Surveillance environnementale 24/7

**Métriques de résilience :**

- **RTO (Recovery Time Objective)** : Temps maximum acceptable de panne
- **RPO (Recovery Point Objective)** : Perte de données maximale acceptable
- **MTBF (Mean Time Between Failures)** : Temps moyen entre pannes
- **MTTR (Mean Time To Repair)** : Temps moyen de réparation
- **Uptime** : Pourcentage de disponibilité

**Tests de résilience :**

- Tests de failover réguliers
- Simulations de catastrophes
- Chaos engineering (Netflix Chaos Monkey)
- Exercices de gestion de crise
- Validation des procédures de récupération

### 8.7 Chiffrement

**Définition :** Un algorithme informatique convertit des caractères en un format illisible à l'aide de clés de chiffrement. Seuls les utilisateurs autorisés disposant des clés appropriées pourront déverrouiller ces informations et les consulter.

**Scope d'application :** Toutes les ressources, des fichiers aux e-mails en passant par les bases de données, peuvent et doivent être chiffrées.

**Types de chiffrement :**

**1. Chiffrement symétrique**

Principe :

- Une seule clé pour chiffrer et déchiffrer
- Rapide et efficace
- Problème : Distribution sécurisée de la clé

Algorithmes courants :

- AES (Advanced Encryption Standard) : Standard actuel, 128/192/256 bits
- DES (Data Encryption Standard) : Obsolète, 56 bits
- 3DES (Triple DES) : En phase de retrait
- Blowfish
- Twofish

Utilisation :

- Chiffrement de grandes quantités de données
- Chiffrement de disques
- Chiffrement de bases de données
- VPN

**2. Chiffrement asymétrique**

Principe :

- Paire de clés : publique et privée
- Clé publique : Chiffrer (peut être partagée)
- Clé privée : Déchiffrer (doit rester secrète)
- Plus lent que le symétrique

Algorithmes courants :

- RSA (Rivest-Shamir-Adleman) : 2048/4096 bits
- ECC (Elliptic Curve Cryptography) : Plus efficace, clés plus courtes
- DSA (Digital Signature Algorithm)
- Diffie-Hellman : Échange de clés

Utilisation :

- Échange sécurisé de clés symétriques
- Signatures numériques
- SSL/TLS
- Email chiffré (PGP, S/MIME)

**3. Chiffrement hybride**

Principe :

- Combinaison du symétrique et asymétrique
- Asymétrique pour échanger une clé symétrique
- Symétrique pour chiffrer les données
- Combine vitesse et sécurité

Utilisation :

- HTTPS/TLS
- La plupart des systèmes modernes

**Applications du chiffrement :**

**Chiffrement en transit (Data in transit) :**

- Protection pendant le transfert
- Protocoles :
    - TLS/SSL : Pour le web (HTTPS)
    - SSH : Pour l'administration à distance
    - VPN : IPSec, OpenVPN
    - SFTP : Transfert de fichiers sécurisé
    - S/MIME, PGP : Email chiffré

**Chiffrement au repos (Data at rest) :**

- Protection des données stockées
- Applications :
    - Chiffrement de disque complet (BitLocker, FileVault, LUKS)
    - Chiffrement de bases de données (TDE - Transparent Data Encryption)
    - Chiffrement de fichiers individuels
    - Chiffrement de sauvegardes
    - Chiffrement de supports amovibles

**Chiffrement en cours d'utilisation (Data in use) :**

- Technologies émergentes
- Homomorphic encryption : Calculs sur données chiffrées
- Secure enclaves : Zones mémoire sécurisées
- Trusted Execution Environments (TEE)

**Gestion des clés de chiffrement :**

**KMS (Key Management System) :**

- Génération de clés sécurisée
- Stockage sécurisé des clés
- Rotation régulière des clés
- Contrôle d'accès aux clés
- Audit de l'utilisation des clés
- Archivage et récupération

**HSM (Hardware Security Module) :**

- Module matériel dédié
- Génération et stockage ultra-sécurisés
- Protection physique contre l'extraction
- Conformité réglementaire
- Performance cryptographique

**Bonnes pratiques :**

1. **Sélection d'algorithmes**
    
    - Utiliser des algorithmes standards et éprouvés
    - Éviter les algorithmes propriétaires ou faits maison
    - AES-256 pour le symétrique
    - RSA-2048 minimum ou ECC pour l'asymétrique
2. **Longueur de clés**
    
    - Clés symétriques : 256 bits minimum
    - Clés RSA : 2048 bits minimum (4096 recommandé)
    - Suivre les recommandations NIST/ANSSI
3. **Rotation des clés**
    
    - Changer régulièrement les clés
    - Planification basée sur le risque
    - Processus automatisé
    - Archivage des anciennes clés pour déchiffrement historique
4. **Protection des clés**
    
    - Ne jamais coder en dur les clés
    - Stockage dans des systèmes dédiés (KMS, HSM)
    - Séparation des clés et des données
    - Contrôle d'accès strict
5. **Chiffrement de bout en bout**
    
    - Données chiffrées du point A au point B
    - Pas de déchiffrement intermédiaire
    - Applications : Messagerie (Signal), Transferts de fichiers

**Conformité et réglementations :**

- RGPD : Recommande fortement le chiffrement
- PCI DSS : Exige le chiffrement des données de cartes
- HIPAA : Recommande le chiffrement des données de santé
- FIPS 140-2/140-3 : Standards de modules cryptographiques

**Limitations et considérations :**

- Impact sur les performances
- Complexité de gestion
- Perte de clé = perte de données
- Ne protège pas contre tous les types d'attaques
- Ne remplace pas les autres mesures de sécurité

---

## POINTS CLÉS À RETENIR POUR L'EXAMEN

### Triade CIA

- **Confidentialité** : Principe du moindre privilège, empêcher accès non autorisés
- **Intégrité** : Protéger contre modifications abusives, utiliser signatures numériques
- **Disponibilité** : Empêcher dénis de service, garantir accès dans délai raisonnable

### Règle d'Anderson

Plus une BD est accessible et utilisable, plus elle est vulnérable aux menaces de sécurité, et inversement.

### Principales menaces

1. Menaces internes et erreur humaine
2. Injection SQL/NoSQL
3. Dépassements de mémoire tampon
4. Attaques DoS/DDoS
5. Abus et élévation de privilèges
6. Bases de données vulnérables ou mal configurées
7. Faiblesse de l'audit natif
8. Vulnérabilités des protocoles de communication

### Les 5 concepts de base

1. Évaluer et atténuer les risques
2. Effectuer un inventaire des actifs
3. Connaître ses données
4. Savoir qui peut accéder à quoi
5. Voir ce qui se passe (surveillance)

### Les 7 types de sécurité

1. **Contrôle des accès** : Limitation accès physiques et numériques
2. **Authentification** : Identification utilisateurs (mot de passe, biométrie, tokens)
3. **Sauvegarde et récupération** : Plan d'accès en cas de défaillance, stratégie 3-2-1
4. **Effacement des données** : Remplacement total, plus sûr que suppression
5. **Masquage des données** : Remplacement de caractères, protection en environnement non-prod
6. **Résilience des données** : Systèmes capables de surmonter défaillances
7. **Chiffrement** : Conversion en format illisible, symétrique/asymétrique

### Différences importantes

- **Suppression vs Effacement** : Suppression efface le pointeur, effacement écrase les données
- **Masquage vs Chiffrement** : Masquage irréversible pour non-prod, chiffrement réversible avec clé pour prod
- **Chiffrement symétrique vs asymétrique** : Symétrique = 1 clé (rapide), Asymétrique = 2 clés (échange sécurisé)
- **Attaques frauduleuses vs non frauduleuses** : Frauduleuses intentionnelles, non frauduleuses accidentelles

### Principes fondamentaux

- **Principe du moindre privilège** : Accorder uniquement les permissions strictement nécessaires
- **Défense en profondeur** : Plusieurs couches de sécurité
- **Séparation des tâches** : Division des responsabilités sensibles
- **Stratégie 3-2-1** : 3 copies, 2 supports, 1 hors site

### Questions d'examen probables

1. Expliquer la triade CIA avec exemples
2. Décrire la règle d'Anderson
3. Lister et expliquer les principales menaces sur les BD
4. Différencier attaques frauduleuses et non frauduleuses
5. Expliquer le principe du moindre privilège
6. Décrire les 7 types de sécurité des données
7. Expliquer la différence entre masquage et chiffrement
8. Décrire la stratégie de sauvegarde 3-2-1
9. Expliquer l'authentification multi-facteurs
10. Décrire les méthodes de contrôle d'accès (DAC, MAC, RBAC, ABAC)