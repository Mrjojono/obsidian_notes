# GUIDE COMPLET PL/SQL

## 1. Qu'est-ce que PL/SQL ?

**PL/SQL (Procedural Language extension to SQL)** est le langage procédural d'Oracle qui combine SQL (accès aux données) et la programmation (variables, conditions, boucles).

**Utilisations principales :**

- Automatiser des traitements
- Créer de la logique métier côté base de données
- Améliorer les performances

---

## 2. Structure d'un bloc PL/SQL

```sql
DECLARE
  -- Déclarations de variables (section optionnelle)
BEGIN
  -- Instructions exécutables (section obligatoire)
EXCEPTION
  -- Gestion des erreurs (section optionnelle)
END;
/
```

### Exemple simple

```sql
BEGIN
  DBMS_OUTPUT.PUT_LINE('Hello PL/SQL');
END;
/
```

**Note :** Le `/` final est obligatoire dans SQL*Plus pour exécuter le bloc.

---

## 3. Types de blocs PL/SQL

### Bloc anonyme

- Non stocké en base de données
- Exécuté ponctuellement
- Idéal pour des tests rapides

```sql
BEGIN
  DBMS_OUTPUT.PUT_LINE('Bloc anonyme');
END;
/
```

### Procédure

- Nommée et stockée en base
- Ne retourne pas de valeur
- Réutilisable

**Création :**

```sql
CREATE OR REPLACE PROCEDURE afficher_message IS
BEGIN
  DBMS_OUTPUT.PUT_LINE('Bonjour depuis une procédure');
END;
/
```

**Appel :**

```sql
BEGIN
  afficher_message;
END;
/
```

### Fonction

- Nommée et stockée en base
- Retourne obligatoirement une valeur
- Utilisable dans des requêtes SQL

**Création :**

```sql
CREATE OR REPLACE FUNCTION doubler_salaire(p_sal NUMBER)
RETURN NUMBER IS
BEGIN
  RETURN p_sal * 2;
END;
/
```

**Utilisation :**

```sql
SELECT doubler_salaire(5000) FROM dual;
```

---

## 4. Variables

### Déclaration et initialisation

```sql
DECLARE
  v_nom VARCHAR2(50);
  v_age NUMBER := 20;
  v_actif BOOLEAN := TRUE;
BEGIN
  v_nom := 'Kekeli';
  DBMS_OUTPUT.PUT_LINE(v_nom || ' a ' || v_age || ' ans');
END;
/
```

### Règles de nommage

- Commence par une lettre
- Maximum 30 caractères
- Pas de mots réservés SQL
- Convention : préfixe `v_` pour les variables

### Initialisation obligatoire

Les variables déclarées `NOT NULL` doivent être initialisées immédiatement.

---

## 5. Types de données

### Types scalaires principaux

|Type|Description|Exemple|
|---|---|---|
|`VARCHAR2(n)`|Chaîne de caractères variable|`VARCHAR2(100)`|
|`NUMBER(p,s)`|Nombre décimal|`NUMBER(10,2)`|
|`DATE`|Date et heure|`DATE`|
|`BOOLEAN`|Valeur logique|`TRUE/FALSE/NULL`|
|`PLS_INTEGER`|Entier optimisé|Pour compteurs|
|`TIMESTAMP`|Date avec précision|Millisecondes|

### Type ancré `%TYPE`

Utilise automatiquement le type d'une colonne de table.

```sql
DECLARE
  v_salaire employees.salary%TYPE;
  v_nom employees.first_name%TYPE;
BEGIN
  SELECT salary, first_name INTO v_salaire, v_nom
  FROM employees
  WHERE employee_id = 100;
END;
/
```

**Avantage :** Si le type de la colonne change en base, le code reste compatible.

### Type enregistrement `%ROWTYPE`

Récupère une ligne complète avec tous ses champs.

```sql
DECLARE
  v_emp employees%ROWTYPE;
BEGIN
  SELECT * INTO v_emp
  FROM employees
  WHERE employee_id = 100;
  
  DBMS_OUTPUT.PUT_LINE(v_emp.first_name || ' - ' || v_emp.salary);
END;
/
```

---

## 6. Instructions SQL en PL/SQL

### SELECT INTO

Récupère une valeur depuis la base de données.

```sql
DECLARE
  v_salaire employees.salary%TYPE;
BEGIN
  SELECT salary INTO v_salaire
  FROM employees
  WHERE employee_id = 100;

  DBMS_OUTPUT.PUT_LINE('Salaire : ' || v_salaire);
END;
/
```

**Contrainte importante :** `SELECT INTO` doit retourner exactement une ligne, sinon :

- Aucune ligne : exception `NO_DATA_FOUND`
- Plusieurs lignes : exception `TOO_MANY_ROWS`

### Manipulation de données (DML)

```sql
BEGIN
  -- Insertion
  INSERT INTO departments VALUES (50, 'IT', NULL, NULL);
  
  -- Mise à jour
  UPDATE employees 
  SET salary = salary * 1.1 
  WHERE department_id = 50;
  
  -- Suppression
  DELETE FROM departments WHERE department_id = 50;
  
  -- Validation
  COMMIT;
END;
/
```

---

## 7. Structures de contrôle

### IF simple

```sql
IF v_salaire > 5000 THEN
  DBMS_OUTPUT.PUT_LINE('Salaire élevé');
END IF;
```

### IF / ELSIF / ELSE

```sql
DECLARE
  v_salaire NUMBER := 4500;
  v_categorie VARCHAR2(20);
BEGIN
  IF v_salaire < 3000 THEN
    v_categorie := 'Bas';
  ELSIF v_salaire < 6000 THEN
    v_categorie := 'Moyen';
  ELSE
    v_categorie := 'Élevé';
  END IF;
  
  DBMS_OUTPUT.PUT_LINE('Catégorie : ' || v_categorie);
END;
/
```

### CASE simple

```sql
DECLARE
  v_dept_id NUMBER := 10;
  v_nom_dept VARCHAR2(30);
BEGIN
  CASE v_dept_id
    WHEN 10 THEN v_nom_dept := 'Administration';
    WHEN 20 THEN v_nom_dept := 'Informatique';
    WHEN 30 THEN v_nom_dept := 'Ventes';
    ELSE v_nom_dept := 'Autre';
  END CASE;
  
  DBMS_OUTPUT.PUT_LINE(v_nom_dept);
END;
/
```

### CASE recherché

```sql
CASE
  WHEN v_salaire < 3000 THEN v_categorie := 'Junior';
  WHEN v_salaire < 6000 THEN v_categorie := 'Confirmé';
  ELSE v_categorie := 'Senior';
END CASE;
```

---

## 8. Boucles

### LOOP basique

```sql
DECLARE
  v_compteur NUMBER := 1;
BEGIN
  LOOP
    DBMS_OUTPUT.PUT_LINE('Itération : ' || v_compteur);
    v_compteur := v_compteur + 1;
    EXIT WHEN v_compteur > 5;
  END LOOP;
END;
/
```

### WHILE

```sql
DECLARE
  v_i NUMBER := 1;
BEGIN
  WHILE v_i <= 5 LOOP
    DBMS_OUTPUT.PUT_LINE('Compteur : ' || v_i);
    v_i := v_i + 1;
  END LOOP;
END;
/
```

### FOR (la plus utilisée)

```sql
BEGIN
  FOR i IN 1..10 LOOP
    DBMS_OUTPUT.PUT_LINE('Nombre : ' || i);
  END LOOP;
END;
/
```

**Boucle FOR inversée :**

```sql
FOR i IN REVERSE 1..5 LOOP
  DBMS_OUTPUT.PUT_LINE(i);  -- Affiche 5, 4, 3, 2, 1
END LOOP;
```

---

## 9. Curseurs

### Curseur explicite

Permet de traiter plusieurs lignes ligne par ligne.

```sql
DECLARE
  CURSOR c_employes IS 
    SELECT first_name, salary 
    FROM employees 
    WHERE department_id = 50;
    
  v_nom employees.first_name%TYPE;
  v_sal employees.salary%TYPE;
BEGIN
  OPEN c_employes;
  
  LOOP
    FETCH c_employes INTO v_nom, v_sal;
    EXIT WHEN c_employes%NOTFOUND;
    
    DBMS_OUTPUT.PUT_LINE(v_nom || ' : ' || v_sal);
  END LOOP;
  
  CLOSE c_employes;
END;
/
```

### Attributs de curseur

|Attribut|Description|
|---|---|
|`%FOUND`|TRUE si FETCH a récupéré une ligne|
|`%NOTFOUND`|TRUE si FETCH n'a pas récupéré de ligne|
|`%ROWCOUNT`|Nombre de lignes récupérées|
|`%ISOPEN`|TRUE si le curseur est ouvert|

### Curseur FOR (syntaxe simplifiée)

Gestion automatique de l'ouverture, du fetch et de la fermeture.

```sql
BEGIN
  FOR emp IN (SELECT first_name, salary FROM employees WHERE department_id = 50) LOOP
    DBMS_OUTPUT.PUT_LINE(emp.first_name || ' gagne ' || emp.salary);
  END LOOP;
END;
/
```

### Curseur avec paramètres

```sql
DECLARE
  CURSOR c_emp_dept(p_dept_id NUMBER) IS
    SELECT first_name FROM employees WHERE department_id = p_dept_id;
BEGIN
  FOR emp IN c_emp_dept(50) LOOP
    DBMS_OUTPUT.PUT_LINE(emp.first_name);
  END LOOP;
END;
/
```

---

## 10. Gestion des exceptions

### Exceptions prédéfinies courantes

```sql
DECLARE
  v_salaire employees.salary%TYPE;
BEGIN
  SELECT salary INTO v_salaire
  FROM employees
  WHERE employee_id = 999999;
  
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    DBMS_OUTPUT.PUT_LINE('Aucun employé trouvé');
  WHEN TOO_MANY_ROWS THEN
    DBMS_OUTPUT.PUT_LINE('Plusieurs employés trouvés');
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('Erreur : ' || SQLERRM);
END;
/
```

### Principales exceptions Oracle

|Exception|Description|
|---|---|
|`NO_DATA_FOUND`|SELECT INTO sans résultat|
|`TOO_MANY_ROWS`|SELECT INTO avec plusieurs lignes|
|`ZERO_DIVIDE`|Division par zéro|
|`VALUE_ERROR`|Erreur de conversion|
|`INVALID_CURSOR`|Opération sur curseur fermé|

### Exception personnalisée

```sql
DECLARE
  e_salaire_negatif EXCEPTION;
  v_salaire NUMBER := -1000;
BEGIN
  IF v_salaire < 0 THEN
    RAISE e_salaire_negatif;
  END IF;
  
EXCEPTION
  WHEN e_salaire_negatif THEN
    DBMS_OUTPUT.PUT_LINE('Erreur : le salaire ne peut pas être négatif');
END;
/
```

### RAISE_APPLICATION_ERROR

Crée des erreurs personnalisées avec codes (entre -20000 et -20999).

```sql
BEGIN
  IF v_age < 18 THEN
    RAISE_APPLICATION_ERROR(-20001, 'L''âge doit être supérieur ou égal à 18 ans');
  END IF;
END;
/
```

---

## 11. Packages

Un package regroupe des procédures, fonctions et variables connexes dans une unité logique.

### Spécification (interface publique)

```sql
CREATE OR REPLACE PACKAGE pkg_gestion_employes IS
  -- Déclaration des éléments publics
  PROCEDURE augmenter_salaire(p_emp_id NUMBER, p_pourcentage NUMBER);
  FUNCTION calculer_prime(p_emp_id NUMBER) RETURN NUMBER;
END pkg_gestion_employes;
/
```

### Corps (implémentation)

```sql
CREATE OR REPLACE PACKAGE BODY pkg_gestion_employes IS
  
  PROCEDURE augmenter_salaire(p_emp_id NUMBER, p_pourcentage NUMBER) IS
  BEGIN
    UPDATE employees
    SET salary = salary * (1 + p_pourcentage/100)
    WHERE employee_id = p_emp_id;
    COMMIT;
  END;
  
  FUNCTION calculer_prime(p_emp_id NUMBER) RETURN NUMBER IS
    v_salaire NUMBER;
  BEGIN
    SELECT salary INTO v_salaire
    FROM employees
    WHERE employee_id = p_emp_id;
    
    RETURN v_salaire * 0.1;
  END;
  
END pkg_gestion_employes;
/
```

### Utilisation

```sql
BEGIN
  pkg_gestion_employes.augmenter_salaire(100, 10);
  DBMS_OUTPUT.PUT_LINE('Prime : ' || pkg_gestion_employes.calculer_prime(100));
END;
/
```

**Avantages des packages :**

- Organisation du code
- Encapsulation (éléments publics/privés)
- Performance (chargement en mémoire)
- Maintenance facilitée

---

## 12. Outils et environnement

### Outils de développement

- **SQL Developer** : IDE graphique Oracle gratuit
- **SQL*Plus** : Outil en ligne de commande
- **Oracle JDeveloper** : IDE complet pour développement Oracle
- **TOAD** : Outil commercial populaire

### Configuration SQL*Plus

```sql
SET SERVEROUTPUT ON SIZE 1000000
SET LINESIZE 200
SET PAGESIZE 50
```

### Commandes utiles

```sql
-- Voir le code source d'un objet
SELECT text FROM user_source WHERE name = 'PROCEDURE_NAME' ORDER BY line;

-- Lister les erreurs de compilation
SHOW ERRORS

-- Décrire une table ou procédure
DESC employees
DESC nom_procedure
```

---

## 13. Cheat Sheet - Référence rapide

### Opérateurs et syntaxe

```sql
:=              -- Affectation de valeur
||              -- Concaténation de chaînes
%TYPE           -- Type d'une colonne
%ROWTYPE        -- Type d'une ligne complète
%FOUND          -- Curseur a trouvé une ligne
%NOTFOUND       -- Curseur n'a pas trouvé de ligne
%ROWCOUNT       -- Nombre de lignes affectées
```

### Structures de base

```sql
-- Condition
IF condition THEN ... ELSIF ... ELSE ... END IF;

-- Boucles
LOOP ... EXIT WHEN ... END LOOP;
WHILE condition LOOP ... END LOOP;
FOR i IN 1..10 LOOP ... END LOOP;

-- Curseur
CURSOR c IS SELECT ...;
FOR rec IN c LOOP ... END LOOP;

-- Exception
EXCEPTION WHEN exception_name THEN ...
```

---

## 14. Bonnes pratiques

1. **Nommage cohérent** : Utiliser des préfixes (`v_` pour variables, `p_` pour paramètres, `c_` pour curseurs)
    
2. **Gestion des exceptions** : Toujours traiter les exceptions possibles
    
3. **Transactions** : Utiliser COMMIT/ROLLBACK de manière appropriée
    
4. **Performance** :
    
    - Préférer BULK COLLECT pour traiter de grands volumes
    - Utiliser des index appropriés
    - Limiter les requêtes dans les boucles
5. **Documentation** : Commenter le code complexe
    
6. **Réutilisation** : Utiliser des packages pour le code partagé
    

---

## 15. Conclusion

PL/SQL est un langage essentiel pour :

- Le développement d'applications Oracle
- L'implémentation de logique métier robuste
- L'optimisation des performances base de données
- L'automatisation des tâches administratives

**Points forts :**

- Intégration native avec Oracle Database
- Performance élevée pour les traitements de masse
- Gestion avancée des transactions
- Très demandé dans le monde professionnel

---

**Pour aller plus loin :**

- Triggers (déclencheurs)
- Collections (VARRAY, TABLE, ASSOCIATIVE ARRAY)
- BULK COLLECT et FORALL
- Dynamic SQL
- Programmation orientée objet en PL/SQL
