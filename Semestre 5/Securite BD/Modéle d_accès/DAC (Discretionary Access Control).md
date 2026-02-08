
Le principe du DAC est que le propriétaire  d'un objet est libre de décider qui peut y accéder . 
c'est un modèle  de sécurité  décentralisé  où  la responsabilité  est délégué  au createur des données 


**entité  ou composants clés :**  

| Sujets    | Les utilisateurs ou le processus qui demandent l'accès                                      |
| --------- | ------------------------------------------------------------------------------------------- |
| Objets    | Les ressources  protégées, comme les tables ou les colonnes                                 |
| Privilège | Les permissions qui définissent les actions possibles(lecture, écriture, exécution, etc...) |

Fonctionnalités : 

Les droits sont représentés dans un matrice , une matrice DAC est une tables où  les lignes représentent  les sujets et les colonnes  représente  les objets, Indiquand les permission s de chaque sujet pour chaque objets  

exemple de simple matrice DAC : 

|               | Fichier 1          | Fichier 2 | Fichier 3 |
| ------------- | ------------------ | --------- | --------- |
| Utilisateur A | Lecture , Ecriture | Lecture   | Interdit  |

exemple plus realiste  avec des rôles 

|        | Dossier Patient A(Chirurgie) | Dossier Patient B(Radio) | Dossier Patient C (Générale) |
| ------ | ---------------------------- | ------------------------ | ---------------------------- |
| E_CH   | Lecture,Ecriture             | Lecture,                 | Lecture                      |
| E_RA   | Lecture                      | Lecture, Ecriture        | Lecture                      |
| Assana | Lecture                      | Lecture                  | Lecture , ecriture           |
| Kodjo  | Lecture , ecriture           | Lecture , ecriture       | Lecture , ecriture           |
E_CH ( Equipe chirurgie)
E_RA( Equipe Radiologie)


- Grant 
GRANT liste_privilège  
ON table ou vue 
TO liste_utilisateurs
WITH GRANT OPTION - facultatif permet de délégué des droits 

- revoke (Révoquer) : Le propriétaire  peut retirer les privilège  qu'il a accordés 
REVOKE GRANT OPTION FOR liste_privilège  
ON table ou vue 
FROM liste_utilisateur 


SECURITE  OBLIGATOIRE (MAC)
