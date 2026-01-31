



Très bien, on va poser ça calmement, brique par brique 🧱. Oracle aime les rituels précis, mais une fois compris, tout coule.

---

## 1️⃣ Le principe général (la carte du territoire)

Pour te connecter **à distance** à une base Oracle qui tourne sur une **machine virtuelle**, il faut que **3 portes** soient ouvertes et alignées :

1. 🟢 **La base Oracle est démarrée**
    
2. 🟢 **Le listener Oracle écoute sur l’IP de la VM**
    
3. 🟢 **Le réseau autorise l’accès (IP + port 1521)**
    

Si un seul maillon casse, Oracle te regarde sans répondre 👀.

---

## 2️⃣ Vérifier que la base Oracle tourne

Sur la VM :

```bash
sqlplus / as sysdba
```

Puis :

```sql
SELECT status FROM v$instance;
```

Résultat attendu :

```text
OPEN
```

Sinon :

```sql
STARTUP;
```

---

## 3️⃣ Le LISTENER : le portier de la base 🚪

👉 **Oui, tu dois avoir un listener**, et **il doit écouter sur l’IP de la machine virtuelle**, pas seulement `localhost`.

### Vérifier l’état du listener

```bash
lsnrctl status
```

S’il n’existe pas ou ne tourne pas :

```bash
lsnrctl start
```

---

## 4️⃣ Configurer le listener sur l’IP de la VM

Fichier :

```bash
$ORACLE_HOME/network/admin/listener.ora
```

### Exemple correct (important 🔥) :

```ini
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.50)(PORT = 1521))
    )
  )

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = orcl)
      (ORACLE_HOME = /u01/app/oracle/product/19c/dbhome_1)
      (SID_NAME = orcl)
    )
  )
```

📌 `192.168.1.50` = **IP réelle de la VM**  
📌 `1521` = port standard Oracle

Ensuite :

```bash
lsnrctl stop
lsnrctl start
```

---

## 5️⃣ Vérifier que la base est bien enregistrée

```bash
lsnrctl status
```

Tu dois voir quelque chose comme :

```text
Service "orcl" has 1 instance(s).
```

Sinon, dans SQL*Plus :

```sql
ALTER SYSTEM REGISTER;
```

---

## 6️⃣ Configurer le client (ta machine locale)

Sur **ta machine**, édite :

```bash
tnsnames.ora
```

(emplacement dépend de ton OS)

### Exemple :

```ini
ORCL_VM =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.50)(PORT = 1521))
    (CONNECT_DATA =
      (SERVICE_NAME = orcl)
    )
  )
```

---

## 7️⃣ 🔍 C’est quoi `tnsping` ?

🧠 **tnsping ≠ ping réseau**

👉 `tnsping` vérifie :

- que le **listener Oracle est joignable**
    
- que la **résolution TNS fonctionne**
    

### Utilisation :

```bash
tnsping ORCL_VM
```

Résultat attendu :

```text
OK (20 msec)
```

❌ Si ça échoue :

- IP incorrecte
    
- port 1521 bloqué
    
- listener arrêté
    
- mauvais SERVICE_NAME
    

---

## 8️⃣ Tester la connexion distante

```bash
sqlplus user/password@ORCL_VM
```

Ou directement :

```bash
sqlplus user/password@192.168.1.50:1521/orcl
```

---

## 9️⃣ Réseau & VM (très important ⚠️)

Selon ton hyperviseur :

### VirtualBox / VMware

- Mode **Bridge** ou **NAT + Port Forwarding**
    
- Port **1521 ouvert**
    
- Pare-feu Linux :
    

```bash
sudo firewall-cmd --add-port=1521/tcp --permanent
sudo firewall-cmd --reload
```

---

## 🔥 Résumé ultra simple

|Élément|Rôle|
|---|---|
|Listener|Réceptionne les connexions|
|listener.ora|Définit IP + port|
|tnsnames.ora|Alias côté client|
|tnsping|Test Oracle (pas réseau)|
|Port 1521|Autoroute Oracle|

---

Si tu veux, dis-moi :

- ton **OS sur la VM**
    
- Oracle **XE, 19c, 21c ?**
    
- **VirtualBox / VMware / Cloud ?**
    

Je te fais ensuite **une config exacte adaptée à ton cas** 💻✨