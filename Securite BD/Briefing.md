
# Briefing Doc : Administration Sécurisée et Protection des Systèmes de Bases de Données

## Résumé Exécutif

La sécurité des bases de données n'est plus une option, mais une nécessité vitale pour toute organisation. Les données constituent le cœur des systèmes d'information, et leur compromission — qu'il s'agisse de vol, de modification frauduleuse ou de destruction — peut entraîner des conséquences financières, légales et réputationnelles dévastatrices. Ce document synthétise les stratégies critiques pour l'administration sécurisée des SGBD (Systèmes de Gestion de Bases de Données), couvrant les architectures relationnelles (MySQL, PostgreSQL) et non relationnelles (NoSQL/MongoDB), ainsi que les environnements Cloud.

Les points clés de cette synthèse incluent :

- **La primauté du Contrôle d'Accès :** L'adoption de modèles robustes (RBAC, ABAC, MAC) et l'application stricte du principe du moindre privilège.
- **La lutte contre l'Injection SQL :** Une menace persistante qui nécessite une validation rigoureuse des entrées et l'utilisation systématique de requêtes paramétrées.
- **La Modélisation des Menaces :** L'utilisation de méthodologies comme STRIDE pour anticiper les attaques dès la phase de conception (_Security by Design_).
- **La Gestion des Clés et du Chiffrement :** L'importance des systèmes KMS pour centraliser le cycle de vie des clés cryptographiques.
- **L'Audit et la Surveillance :** Le déploiement d'outils de _Database Activity Monitoring_ (DAM) pour assurer la traçabilité et la détection d'intrusions en temps réel.

--------------------------------------------------------------------------------

## I. Architecture et Fondamentaux des SGBD

### 1. SGBD Relationnels (SGBDR)

Le modèle relationnel, inventé par Edgar F. Codd en 1970, organise les données en tables structurées.

- **PostgreSQL :** Privilégié pour les applications complexes grâce à son extensibilité, sa conformité SQL stricte et son support des types avancés (JSON).
- **MySQL :** Reconnu pour sa rapidité et sa simplicité, utilisant principalement le moteur InnoDB pour assurer la robustesse des transactions (conformité ACID).

### 2. SGBD NoSQL (Cas de MongoDB)

Contrairement au SQL, le NoSQL (Not Only SQL) mise sur la scalabilité horizontale et la flexibilité du schéma.

- **Modèle Documentaire :** MongoDB stocke les données sous forme de documents BSON (JSON binaire).
- **Défis de Sécurité :** L'absence de schéma fixe rend la validation des données complexe et expose les systèmes à des injections NoSQL spécifiques.

--------------------------------------------------------------------------------

## II. Analyse des Menaces et Vulnérabilités

La sécurité doit répondre à trois piliers : **Confidentialité**, **Intégrité** et **Disponibilité**.

### 1. Menaces Internes vs Externes

- **Menaces Internes :** Proviennent d'utilisateurs légitimes (employés, prestataires). Elles peuvent être malveillantes (vengeance, profit), négligentes (erreurs de configuration) ou résulter de comptes compromis par des tiers.
- **Menaces Externes :** Cybercriminels exploitant des ports ouverts (ex: 27017 pour MongoDB), des vulnérabilités logicielles, des attaques par force brute ou des dénis de service (DDoS).

### 2. Le Risque Majeur : L'Injection SQL (SQLi)

L'injection SQL consiste à insérer des commandes malveillantes dans des champs de saisie pour manipuler la base de données.

- **Causes :** Validation insuffisante des entrées et concaténation directe de chaînes de caractères dans les requêtes.
- **Types d'injections :**
    - _In-band (en bande) :_ L'attaquant reçoit les résultats directement (via des erreurs ou l'opérateur UNION).
    - _Inférentielle (Blind SQLi) :_ L'attaquant déduit des informations en observant le comportement du système (réponses booléennes ou temps de réponse).
    - _Out-of-band (hors bande) :_ Exfiltration via des canaux DNS ou HTTP.

### 3. Matrice de Risques des Menaces Courantes

|   |   |   |   |
|---|---|---|---|
|Menace|Probabilité|Impact|Niveau de Risque|
|Injection SQL (SQLi)|Élevée|Élevé|Fort|
|Ransomware|Moyenne|Très élevé|Fort|
|Vol / Fuite de données|Moyenne|Très élevé|Fort|
|Force Brute|Élevée|Moyen|Moyen|
|Faille de configuration|Élevée|Moyen|Moyen|

--------------------------------------------------------------------------------

## III. Modèles de Contrôle d'Accès

Le contrôle d'accès est la première ligne de défense, transformant les exigences organisationnelles en règles techniques.

- **DAC (Discretionary Access Control) :** Le propriétaire de la ressource définit les permissions. Flexible mais difficile à auditer à grande échelle.
- **MAC (Mandatory Access Control) :** Basé sur des labels de sécurité (Secret, Top Secret). Imposé par une autorité centrale, idéal pour les secteurs militaires.
- **RBAC (Role-Based Access Control) :** Les droits sont liés à des rôles (ex: comptable, RH). Simplifie la gestion des mouvements de personnel.
- **ABAC (Attribute-Based Access Control) :** Modèle dynamique utilisant des attributs (utilisateur, heure, localisation, type d'appareil). Offre la granularité la plus fine.

### Sécurité Multiniveau (MLS)

- **Bell-LaPadula (Confidentialité) :** Interdit la lecture vers le haut ("no read up") et l'écriture vers le bas ("no write down").
- **Biba (Intégrité) :** Interdit la lecture vers le bas ("no read down") et l'écriture vers le haut ("no write up").

--------------------------------------------------------------------------------

## IV. Stratégies de Sécurisation Technique

### 1. Gestion des Identifiants et Mots de Passe

- **Interdiction du stockage en clair :** Utiliser des algorithmes de hachage robustes (famille SHA-2) combinés à un "sel" (salt) unique.
- **Complexité :** Privilégier les "passphrases" (ex: "LeCh@tBleuDanseSousLaPluie2025!") plutôt que des mots de passe courts et complexes.
- **Authentification Forte :** Déployer l'authentification multi-facteurs (MFA) et les certificats x.509 pour les accès sensibles.

### 2. Sécurité Réseau

- **Chiffrement en transit :** Activer systématiquement SSL/TLS pour protéger les échanges contre les attaques de type "Man-in-the-Middle".
- **Isolation :** Placer les serveurs de base de données derrière des pare-feu ou dans une DMZ, en restreignant l'accès aux seules adresses IP autorisées.

### 3. Protection des Objets Stockés (Procédures et Triggers)

Les triggers et procédures stockées sont puissants mais dangereux :

- **Risques :** Escalade de privilèges si le code s'exécute avec les droits du créateur (_Security Definer_) plutôt que de l'appelant (_Invoker_).
- **Bonnes pratiques :** Revue de code obligatoire, interdiction du SQL dynamique non paramétré et journalisation des opérations DDL (CREATE/ALTER/DROP).

--------------------------------------------------------------------------------

## V. Modélisation et Planification de la Sécurité

### 1. Méthodologie STRIDE

Pour chaque composant, il convient d'analyser six catégories de menaces :

1. **Spoofing (Usurpation) :** Usurpation d'identité d'un utilisateur.
2. **Tampering (Altération) :** Modification non autorisée des données.
3. **Repudiation (Répudiation) :** Impossibilité de prouver une action.
4. **Information Disclosure (Fuite) :** Divulgation de données sensibles.
5. **Denial of Service (Déni de service) :** Saturation des ressources.
6. **Elevation of Privilege (Escalade) :** Obtention de droits administrateur.

### 2. Le Modèle de Responsabilité Partagée (Cloud)

Dans le Cloud, la sécurité est répartie :

- **Fournisseur (ex: AWS, Azure) :** Responsable de l'infrastructure physique et de la virtualisation.
- **Client :** Responsable de la configuration du SGBD, du chiffrement des données, de la gestion des identités (IAM) et du respect des réglementations (RGPD).

--------------------------------------------------------------------------------

## VI. Audit, Détection et Continuité

### 1. Database Activity Monitoring (DAM)

L'audit doit répondre aux 5 questions fondamentales : **Qui ? Quoi ? Quand ? Comment ? Où ?**

- **Traçabilité :** Enregistrer les modifications de structure, les changements de droits et les accès aux tables sensibles.
- **Détection d'anomalies :** Identifier les connexions hors horaires, les volumes de requêtes atypiques ou les échecs de connexion répétés.

### 2. Gestion des Clés (KMS)

Un _Key Management System_ centralise la génération, le stockage, le remplacement et la destruction des clés de chiffrement.

- **Modes de déploiement :**
    - _On-premise :_ Souveraineté maximale, intégration complexe.
    - _SaaS :_ Déploiement rapide, sécurité dépendante du fournisseur Cloud.
    - _Hybride :_ Clés sauvegardées localement avec applicatif Cloud.

### 3. Résilience et Plan de Reprise d'Activité (PRA)

- **Sauvegardes :** Doivent être chiffrées, isolées et testées régulièrement.
- **Procédure d'incident :** Containment (endiguement), Investigation, Éradication et Retour d'expérience.

## Conclusion

Une administration sécurisée repose sur un engagement continu. La sécurité n'est pas une destination mais un processus itératif combinant des mesures techniques (chiffrement, audit), organisationnelles (principes d'accès) et humaines (formation des équipes). Un SGBD, qu'il soit MySQL, PostgreSQL ou MongoDB, correctement configuré et surveillé, demeure une solution fiable pour les données critiques.









Voici les réponses aux questions de votre partie théorique, basées sur les sources fournies :

### 1. Importance de la sécurité d'une base de données (04 points)

La sécurité des bases de données est vitale pour une entreprise pour les raisons suivantes :

- **Protection des informations sensibles :** Elle permet de mettre à l'abri les données confidentielles telles que les informations d'identification personnelle (PII), les données financières et la propriété intellectuelle contre le vol ou l'accès non autorisé.
- **Garantie de l'intégrité et de la disponibilité :** Elle assure que les données ne sont pas modifiées de manière frauduleuse ou accidentelle et qu'elles restent accessibles aux utilisateurs légitimes pour assurer la continuité des activités.
- **Conformité réglementaire :** Les entreprises doivent respecter des cadres légaux stricts, comme le **RGPD**, sous peine d'amendes substantielles et de poursuites judiciaires en cas de violation de données.
- **Préservation de la réputation :** Une faille de sécurité peut entraîner une perte de confiance durable de la part des clients, des partenaires et des actionnaires, nuisant gravement à l'image de marque.

### 2. Moyens de garantir la sécurité (03 définitions au choix) (1,5 pt)

- **Authentification :** Ce mécanisme consiste à **vérifier l'identité** d'un utilisateur, d'une application ou d'une machine avant de lui accorder l'accès au système. Elle peut s'appuyer sur des mots de passe, des certificats ou l'authentification multi-facteurs.
- **Autorisation :** Une fois l'identité vérifiée, l'autorisation définit précisément les **droits et privilèges** accordés (lecture, écriture, suppression) sur des ressources spécifiques, souvent selon le principe du moindre privilège.
- **Audit et Journalisation :** Il s'agit du processus d'enregistrement automatique et d'examen des événements survenus sur la base (connexions, requêtes, modifications). Cela permet d'assurer la **traçabilité** des actions et de détecter des comportements suspects.

### 3. Types et objectifs de l'injection SQL (1,5 pt)

**Types d'injection SQL :** Les sources identifient trois catégories principales :

1. **En bande (In-band) :** La forme la plus simple où l'attaquant utilise le même canal pour lancer l'attaque et récupérer les résultats (ex: basées sur les **erreurs** ou sur l'opération **UNION**).
2. **Inférentielle (Blind) :** Plus sournoise, elle consiste à observer le comportement de l'application (réponses **booléennes** ou basées sur le **temps/délai**) pour déduire des informations.
3. **Hors bande (Out-of-band) :** L'attaquant utilise des canaux auxiliaires comme des requêtes DNS ou HTTP pour extraire les données. _Note : Les sources mentionnent également l'injection SQL **stockée**, où le code malveillant est enregistré pour être exécuté plus tard._

**Objectifs d'une injection SQL :**

- Visualiser ou voler des données privées (mots de passe, cartes de crédit).
- Contourner les écrans d'authentification.
- Modifier ou supprimer des données dans la base.
- Prendre le contrôle total du serveur de base de données.

### 4. Types de sauvegarde de base de données (1,5 pt)

La sauvegarde est le dernier rempart contre la perte totale de données. Les sources mentionnent :

- **Sauvegardes régulières et automatisées :** Pour assurer un maintien de versions historiques (journalières, hebdomadaires, mensuelles) selon les besoins métier.
- **Sauvegardes hors site ou isolées :** Stockées dans un lieu physique différent ou un cloud sécurisé pour prévenir les sinistres locaux, souvent selon la règle **3-2-1** (3 copies, 2 supports, 1 hors site).
- **Sauvegardes immuables :** Protégées en écriture pour empêcher leur chiffrement ou destruction lors d'une attaque par ransomware.

_Information non présente dans les sources :_ En milieu professionnel, on distingue classiquement la sauvegarde **complète** (tout le système), **incrémentale** (données modifiées depuis la dernière sauvegarde) et **différentielle** (données modifiées depuis la dernière sauvegarde complète).

### 5. Relation entre menace, risque et vulnérabilité (0,5 pt)

La relation peut s'établir ainsi : une **menace** (danger potentiel ou acteur malveillant) cherche à exploiter une **vulnérabilité** (faiblesse ou faille de configuration du système) pour engendrer un **risque**. Le risque est alors mesuré par la combinaison de la **probabilité** que la menace réussisse et de l'**impact** (gravité des dommages) qu'elle causerait sur l'organisation.