
# Guide des Annotations Spring Boot & JPA

## Annotations JPA (Persistence)

### `@Entity`

**Rôle :** Marque une classe comme entité JPA  
**Fonction :**

- Indique à Hibernate/JPA que cette classe doit être mappée à une table de base de données
- Crée automatiquement une table avec le nom de la classe (ou personnalisable avec `@Table`)
- Chaque instance de la classe représente une ligne dans la table

```java
@Entity
public class Admin extends User {
    // Sera mappé à la table "admin" en BD
}
```

---

## Annotations Lombok

### `@Data`

**Rôle :** Génère automatiquement le code boilerplate  
**Fonction :** Crée automatiquement :

- `@Getter` pour tous les champs
- `@Setter` pour tous les champs non-final
- `@ToString`
- `@EqualsAndHashCode`
- `@RequiredArgsConstructor`

```java
@Data
public class Admin {
    private String nom;
    // Lombok génère getNom(), setNom(), toString(), equals(), hashCode()
}
```

### `@NoArgsConstructor`

**Rôle :** Génère un constructeur sans paramètres  
**Fonction :**

- Crée `public Admin() {}`
- Requis par JPA pour instancier les entités

```java
@NoArgsConstructor
public class Admin {
    // Génère: public Admin() {}
}
```

### `@EqualsAndHashCode(callSuper = true)`

**Rôle :** Génère les méthodes `equals()` et `hashCode()`  
**Fonction :**

- `callSuper = true` : inclut les champs de la classe parent (User) dans la comparaison
- Utile pour comparer des objets et les utiliser dans des collections (Set, Map)

```java
@EqualsAndHashCode(callSuper = true)
public class Admin extends User {
    // equals() et hashCode() incluent les champs de User
}
```

### `@RequiredArgsConstructor`

**Rôle :** Génère un constructeur avec les champs obligatoires  
**Fonction :**

- Crée un constructeur avec tous les champs `final` et `@NonNull`
- Très utilisé avec l'injection de dépendances

```java
@RequiredArgsConstructor
public class UserService {
    private final UserRepository repository; // final = requis
    
    // Lombok génère: 
    // public UserService(UserRepository repository) {
    //     this.repository = repository;
    // }
}
```

---

## Annotations Spring

### `@Service`

**Rôle :** Marque une classe comme service métier  
**Fonction :**

- Composant Spring détecté automatiquement par le scan
- Contient la logique métier de l'application
- Singleton par défaut (une seule instance)

```java
@Service
@RequiredArgsConstructor
public class AdminService {
    private final AdminRepository adminRepository;
    
    public Admin creerAdmin(Admin admin) {
        // Logique métier
        return adminRepository.save(admin);
    }
}
```

### `@Repository`

**Rôle :** Marque une classe comme couche d'accès aux données  
**Fonction :**

- Composant Spring pour les DAO/Repository
- Traduction automatique des exceptions de persistence en exceptions Spring
- Gère les interactions avec la base de données

```java
@Repository
public interface AdminRepository extends JpaRepository<Admin, Long> {
    Optional<Admin> findByEmail(String email);
}
```

### `@Controller` / `@RestController`

**Rôle :** Marque une classe comme contrôleur web  
**Fonction :**

- `@Controller` : retourne des vues (HTML)
- `@RestController` = `@Controller` + `@ResponseBody` : retourne du JSON/XML

```java
@RestController
@RequestMapping("/api/admins")
@RequiredArgsConstructor
public class AdminController {
    private final AdminService adminService;
    
    @GetMapping
    public List<Admin> getAllAdmins() {
        return adminService.findAll();
    }
}
```

### `@Component`

**Rôle :** Annotation générique pour tout composant Spring  
**Fonction :**

- Classe détectée et gérée par Spring
- `@Service`, `@Repository`, `@Controller` sont des spécialisations de `@Component`

---

## Autres Annotations Courantes

### `@Autowired` (moins utilisé maintenant)

**Rôle :** Injection de dépendances  
**Fonction :**

- Spring injecte automatiquement les dépendances
- `@RequiredArgsConstructor` est préféré (injection par constructeur)

### `@Transactional`

**Rôle :** Gestion des transactions  
**Fonction :**

- Ouvre une transaction au début de la méthode
- Commit si succès, rollback si exception

```java
@Service
@RequiredArgsConstructor
public class AdminService {
    
    @Transactional
    public void updateAdmin(Long id, String newName) {
        Admin admin = adminRepository.findById(id).orElseThrow();
        admin.setNom(newName);
        // Sauvegarde automatique au commit
    }
}
```

---

## Architecture Typique

```
┌─────────────────────────────────────┐
│  @RestController                    │  ← API REST (JSON)
│  @RequestMapping("/api/admins")     │
└──────────────┬──────────────────────┘
               │ appelle
               ▼
┌─────────────────────────────────────┐
│  @Service                           │  ← Logique métier
│  @RequiredArgsConstructor           │
└──────────────┬──────────────────────┘
               │ appelle
               ▼
┌─────────────────────────────────────┐
│  @Repository                        │  ← Accès base de données
│  extends JpaRepository              │
└──────────────┬──────────────────────┘
               │ manipule
               ▼
┌─────────────────────────────────────┐
│  @Entity                            │  ← Modèle de données
│  @Data @NoArgsConstructor           │
└─────────────────────────────────────┘
```

---

## Résumé Rapide

| Annotation                 | Type   | Usage Principal                |
| -------------------------- | ------ | ------------------------------ |
| `@Entity`                  | JPA    | Classe = Table BD              |
| `@Data`                    | Lombok | Getters/Setters automatiques   |
| `@NoArgsConstructor`       | Lombok | Constructeur vide              |
| `@RequiredArgsConstructor` | Lombok | Constructeur avec champs final |
| `@Service`                 | Spring | Logique métier                 |
| `@Repository`              | Spring | Accès données                  |
| `@RestController`          | Spring | API REST                       |
| `@Transactional`           | Spring | Gestion transactions           |
|                            |        |                                |
|                            |        |                                |
|                            |        |                                |
|                            |        |                                |
|                            |        |                                |
|                            |        |                                |
|                            |        |                                |