

# Guide Complet Flutter - Des Bases aux Appels API

## 1. Architecture Flutter

```
┌─────────────────────────────────────────────────────────┐
│                      APPLICATION                         │
├─────────────────────────────────────────────────────────┤
│  main.dart (Point d'entrée)                             │
│  MyApp (Widget racine)                                  │
└─────────────────────────────────────────────────────────┘
           │
           ├── SCREENS/PAGES
           │   ├── home_screen.dart
           │   ├── admin_screen.dart
           │   └── profile_screen.dart
           │
           ├── WIDGETS (Composants réutilisables)
           │   ├── custom_button.dart
           │   ├── admin_card.dart
           │   └── loading_widget.dart
           │
           ├── SERVICES (Logique métier + API)
           │   ├── admin_service.dart
           │   └── auth_service.dart
           │
           └── MODELS (Classes de données)
               ├── admin.dart
               └── user.dart
```

---

## 2. Structure de Base d'une Application Flutter

```dart
// main.dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());  // Point d'entrée de l'application
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Mon App Flutter',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const HomeScreen(),  // Page d'accueil
    );
  }
}
```

---

## 3. Types de Widgets

### StatelessWidget

**Rôle :** Widget immuable (ne change pas)  
**Fonction :**

- Pour les interfaces statiques
- Pas de gestion d'état interne
- Plus performant

```dart
class MyStaticWidget extends StatelessWidget {
  final String title;
  
  const MyStaticWidget({super.key, required this.title});

  @override
  Widget build(BuildContext context) {
    return Text(title);  // Ne changera jamais
  }
}
```

### StatefulWidget

**Rôle :** Widget avec état mutable (peut changer)  
**Fonction :**

- Pour les interfaces dynamiques
- Possède un State qui peut être mis à jour
- Utilise `setState()` pour rafraîchir l'UI

```dart
class Counter extends StatefulWidget {
  const Counter({super.key});

  @override
  State<Counter> createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int _count = 0;

  void _increment() {
    setState(() {  // ⚠️ Déclenche le rebuild du widget
      _count++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Count: $_count'),
        ElevatedButton(
          onPressed: _increment,
          child: const Text('Increment'),
        ),
      ],
    );
  }
}
```

---

## 4. Widgets de Layout Essentiels

### Container

**Rôle :** Boîte polyvalente pour le styling  
**Fonction :**

- Padding, margin, bordures, couleur de fond
- Taille fixe ou flexible

```dart
Container(
  width: 200,
  height: 100,
  padding: const EdgeInsets.all(16),  // Espacement interne
  margin: const EdgeInsets.only(top: 20),  // Espacement externe
  decoration: BoxDecoration(
    color: Colors.blue,
    borderRadius: BorderRadius.circular(10),
    boxShadow: [
      BoxShadow(
        color: Colors.grey.withOpacity(0.5),
        spreadRadius: 2,
        blurRadius: 5,
      ),
    ],
  ),
  child: const Text('Hello'),
)
```

### Column (Disposition verticale)

**Rôle :** Empile les widgets verticalement  
**Fonction :**

- Aligne les enfants de haut en bas

```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center,  // Alignement vertical
  crossAxisAlignment: CrossAxisAlignment.start, // Alignement horizontal
  children: [
    const Text('Premier'),
    const Text('Deuxième'),
    const Text('Troisième'),
  ],
)
```

### Row (Disposition horizontale)

**Rôle :** Empile les widgets horizontalement  
**Fonction :**

- Aligne les enfants de gauche à droite

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween,  // Alignement horizontal
  crossAxisAlignment: CrossAxisAlignment.center,      // Alignement vertical
  children: [
    const Icon(Icons.home),
    const Text('Accueil'),
    const Icon(Icons.arrow_forward),
  ],
)
```

### Stack

**Rôle :** Superpose les widgets les uns sur les autres  
**Fonction :**

- Comme des calques (z-index)
- Positionne avec `Positioned`

```dart
Stack(
  children: [
    Container(color: Colors.blue, width: 200, height: 200),
    Positioned(
      top: 10,
      right: 10,
      child: const Icon(Icons.star, color: Colors.yellow),
    ),
  ],
)
```

### Expanded

**Rôle :** Prend tout l'espace disponible dans Row/Column  
**Fonction :**

- Flex automatique
- Utilise `flex` pour la proportion

```dart
Row(
  children: [
    Expanded(
      flex: 2,  // Prend 2/3 de l'espace
      child: Container(color: Colors.red),
    ),
    Expanded(
      flex: 1,  // Prend 1/3 de l'espace
      child: Container(color: Colors.blue),
    ),
  ],
)
```

### Padding

**Rôle :** Ajoute un espacement interne  
**Fonction :**

- Enveloppe un widget avec du padding

```dart
Padding(
  padding: const EdgeInsets.all(16.0),  // Tous les côtés
  // padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 10),
  // padding: const EdgeInsets.only(left: 10, top: 5),
  child: const Text('Texte avec padding'),
)
```

### SizedBox

**Rôle :** Boîte avec dimensions fixes ou espacement  
**Fonction :**

- Contrôle précis de la taille
- Espacement entre widgets

```dart
// Espacement vertical
const SizedBox(height: 20),

// Taille fixe
SizedBox(
  width: 100,
  height: 50,
  child: ElevatedButton(
    onPressed: () {},
    child: const Text('Button'),
  ),
)
```

---

## 5. MainAxisAlignment & CrossAxisAlignment

### MainAxisAlignment (Axe principal)

**Rôle :** Aligne les enfants le long de l'axe principal

- Pour `Column` → Axe principal = **Vertical** ↕️
- Pour `Row` → Axe principal = **Horizontal** ↔️

**Options :**

```dart
MainAxisAlignment.start       // Début (haut pour Column, gauche pour Row)
MainAxisAlignment.center      // Centre
MainAxisAlignment.end         // Fin (bas pour Column, droite pour Row)
MainAxisAlignment.spaceBetween // Espace égal ENTRE les widgets
MainAxisAlignment.spaceAround  // Espace égal AUTOUR des widgets
MainAxisAlignment.spaceEvenly  // Espace parfaitement égal partout
```

**Exemple visuel :**

```dart
Column(
  mainAxisAlignment: MainAxisAlignment.spaceBetween,
  children: [
    Container(color: Colors.red, height: 50),    // ⬆️ En haut
    Container(color: Colors.green, height: 50),  // 🔲 Au milieu
    Container(color: Colors.blue, height: 50),   // ⬇️ En bas
  ],
)
```

### CrossAxisAlignment (Axe secondaire)

**Rôle :** Aligne les enfants perpendiculairement à l'axe principal

- Pour `Column` → Axe secondaire = **Horizontal** ↔️
- Pour `Row` → Axe secondaire = **Vertical** ↕️

**Options :**

```dart
CrossAxisAlignment.start    // Début (gauche pour Column, haut pour Row)
CrossAxisAlignment.center   // Centre
CrossAxisAlignment.end      // Fin (droite pour Column, bas pour Row)
CrossAxisAlignment.stretch  // S'étire pour remplir tout l'espace
```

**Exemple visuel :**

```dart
Column(
  crossAxisAlignment: CrossAxisAlignment.start,  // Aligne à gauche
  children: [
    const Text('Court'),
    const Text('Texte plus long'),
    const Text('X'),
  ],
)
```

---

## 6. Widgets Interactifs

### ElevatedButton

**Rôle :** Bouton avec élévation (ombre)

```dart
ElevatedButton(
  onPressed: () {
    print('Bouton cliqué !');
  },
  style: ElevatedButton.styleFrom(
    backgroundColor: Colors.blue,
    padding: const EdgeInsets.symmetric(horizontal: 30, vertical: 15),
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.circular(10),
    ),
  ),
  child: const Text('Cliquez-moi'),
)
```

### TextButton

**Rôle :** Bouton plat sans élévation

```dart
TextButton(
  onPressed: () {},
  child: const Text('Texte Bouton'),
)
```

### IconButton

**Rôle :** Bouton avec une icône

```dart
IconButton(
  icon: const Icon(Icons.favorite),
  color: Colors.red,
  onPressed: () {},
)
```

### TextField

**Rôle :** Champ de saisie de texte

```dart
TextField(
  decoration: const InputDecoration(
    labelText: 'Nom',
    hintText: 'Entrez votre nom',
    prefixIcon: Icon(Icons.person),
    border: OutlineInputBorder(),
  ),
  onChanged: (value) {
    print('Valeur: $value');
  },
)
```

### Switch

**Rôle :** Interrupteur on/off

```dart
bool _isEnabled = false;

Switch(
  value: _isEnabled,
  onChanged: (bool value) {
    setState(() {
      _isEnabled = value;
    });
  },
)
```

### Checkbox

**Rôle :** Case à cocher

```dart
bool _isChecked = false;

Checkbox(
  value: _isChecked,
  onChanged: (bool? value) {
    setState(() {
      _isChecked = value ?? false;
    });
  },
)
```

---

## 7. Listes et Scroll

### ListView

**Rôle :** Liste scrollable verticale  
**Fonction :**

- Affiche une liste de widgets
- Scroll automatique

```dart
ListView(
  children: const [
    ListTile(
      leading: Icon(Icons.person),
      title: Text('John Doe'),
      subtitle: Text('john@example.com'),
      trailing: Icon(Icons.arrow_forward),
    ),
    ListTile(
      leading: Icon(Icons.person),
      title: Text('Jane Smith'),
      subtitle: Text('jane@example.com'),
      trailing: Icon(Icons.arrow_forward),
    ),
  ],
)
```

### ListView.builder (Optimisé pour grandes listes)

**Rôle :** Crée les widgets à la demande  
**Fonction :**

- Meilleure performance
- Ne crée que les items visibles

```dart
ListView.builder(
  itemCount: admins.length,
  itemBuilder: (context, index) {
    final admin = admins[index];
    return ListTile(
      title: Text(admin.nom),
      subtitle: Text(admin.email),
      onTap: () {
        print('Admin ${admin.id} cliqué');
      },
    );
  },
)
```

### GridView

**Rôle :** Grille d'éléments

```dart
GridView.count(
  crossAxisCount: 2,  // 2 colonnes
  crossAxisSpacing: 10,
  mainAxisSpacing: 10,
  children: List.generate(20, (index) {
    return Container(
      color: Colors.blue,
      child: Center(child: Text('Item $index')),
    );
  }),
)
```

### SingleChildScrollView

**Rôle :** Rend un widget unique scrollable

```dart
SingleChildScrollView(
  child: Column(
    children: [
      // Beaucoup de contenu...
    ],
  ),
)
```

---

## 8. Navigation

### Navigation de Base

```dart
// Page de départ
class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Home')),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Navigation vers une nouvelle page
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => const SecondPage()),
            );
          },
          child: const Text('Aller à la page 2'),
        ),
      ),
    );
  }
}

// Page de destination
class SecondPage extends StatelessWidget {
  const SecondPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Page 2')),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Retour à la page précédente
            Navigator.pop(context);
          },
          child: const Text('Retour'),
        ),
      ),
    );
  }
}
```

### Routes Nommées (Recommandé)

```dart
// main.dart
MaterialApp(
  initialRoute: '/',
  routes: {
    '/': (context) => const HomePage(),
    '/admins': (context) => const AdminListPage(),
    '/profile': (context) => const ProfilePage(),
  },
)

// Navigation avec route nommée
Navigator.pushNamed(context, '/admins');

// Navigation avec remplacement
Navigator.pushReplacementNamed(context, '/profile');
```

### Passer des Données

```dart
// Envoyer des données
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => DetailPage(admin: selectedAdmin),
  ),
);

// Recevoir des données
class DetailPage extends StatelessWidget {
  final Admin admin;
  
  const DetailPage({super.key, required this.admin});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(admin.nom)),
      body: Text(admin.email),
    );
  }
}
```

---

## 9. Models (Classes de Données)

```dart
// lib/models/admin.dart
class Admin {
  final int? id;
  final String nom;
  final String prenom;
  final String email;
  final DateTime? dateCreation;

  Admin({
    this.id,
    required this.nom,
    required this.prenom,
    required this.email,
    this.dateCreation,
  });

  // Convertir JSON → Admin
  factory Admin.fromJson(Map<String, dynamic> json) {
    return Admin(
      id: json['id'],
      nom: json['nom'],
      prenom: json['prenom'],
      email: json['email'],
      dateCreation: json['dateCreation'] != null 
          ? DateTime.parse(json['dateCreation'])
          : null,
    );
  }

  // Convertir Admin → JSON
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'nom': nom,
      'prenom': prenom,
      'email': email,
      'dateCreation': dateCreation?.toIso8601String(),
    };
  }
}
```

---

## 10. Services & Appels API

### Installation du Package HTTP

```yaml
# pubspec.yaml
dependencies:
  flutter:
    sdk: flutter
  http: ^1.1.0  # ⚠️ Ajouter cette ligne
```

Puis exécuter : `flutter pub get`

### Service Complet avec CRUD

```dart
// lib/services/admin_service.dart
import 'dart:convert';
import 'package:http/http.dart' as http;
import '../models/admin.dart';

class AdminService {
  static const String baseUrl = 'http://localhost:8080/api/admins';

  // GET - Récupérer tous les admins
  Future<List<Admin>> getAllAdmins() async {
    try {
      final response = await http.get(Uri.parse(baseUrl));

      if (response.statusCode == 200) {
        final List<dynamic> jsonData = json.decode(response.body);
        return jsonData.map((json) => Admin.fromJson(json)).toList();
      } else {
        throw Exception('Erreur lors du chargement des admins');
      }
    } catch (e) {
      throw Exception('Erreur réseau: $e');
    }
  }

  // GET - Récupérer un admin par ID
  Future<Admin> getAdminById(int id) async {
    final response = await http.get(Uri.parse('$baseUrl/$id'));

    if (response.statusCode == 200) {
      return Admin.fromJson(json.decode(response.body));
    } else {
      throw Exception('Admin non trouvé');
    }
  }

  // POST - Créer un admin
  Future<Admin> createAdmin(Admin admin) async {
    final response = await http.post(
      Uri.parse(baseUrl),
      headers: {'Content-Type': 'application/json'},
      body: json.encode(admin.toJson()),
    );

    if (response.statusCode == 201 || response.statusCode == 200) {
      return Admin.fromJson(json.decode(response.body));
    } else {
      throw Exception('Erreur lors de la création');
    }
  }

  // PUT - Modifier un admin
  Future<Admin> updateAdmin(int id, Admin admin) async {
    final response = await http.put(
      Uri.parse('$baseUrl/$id'),
      headers: {'Content-Type': 'application/json'},
      body: json.encode(admin.toJson()),
    );

    if (response.statusCode == 200) {
      return Admin.fromJson(json.decode(response.body));
    } else {
      throw Exception('Erreur lors de la modification');
    }
  }

  // DELETE - Supprimer un admin
  Future<void> deleteAdmin(int id) async {
    final response = await http.delete(Uri.parse('$baseUrl/$id'));

    if (response.statusCode != 200 && response.statusCode != 204) {
      throw Exception('Erreur lors de la suppression');
    }
  }
}
```

---

## 11. Page Complète avec Appel API

```dart
// lib/screens/admin_list_screen.dart
import 'package:flutter/material.dart';
import '../models/admin.dart';
import '../services/admin_service.dart';

class AdminListScreen extends StatefulWidget {
  const AdminListScreen({super.key});

  @override
  State<AdminListScreen> createState() => _AdminListScreenState();
}

class _AdminListScreenState extends State<AdminListScreen> {
  final AdminService _adminService = AdminService();
  List<Admin> _admins = [];
  bool _isLoading = false;
  String? _errorMessage;

  @override
  void initState() {
    super.initState();
    _loadAdmins();
  }

  Future<void> _loadAdmins() async {
    setState(() {
      _isLoading = true;
      _errorMessage = null;
    });

    try {
      final admins = await _adminService.getAllAdmins();
      setState(() {
        _admins = admins;
        _isLoading = false;
      });
    } catch (e) {
      setState(() {
        _errorMessage = e.toString();
        _isLoading = false;
      });
    }
  }

  Future<void> _deleteAdmin(int id) async {
    final confirm = await showDialog<bool>(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('Confirmation'),
        content: const Text('Voulez-vous vraiment supprimer cet admin ?'),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context, false),
            child: const Text('Annuler'),
          ),
          TextButton(
            onPressed: () => Navigator.pop(context, true),
            child: const Text('Supprimer'),
          ),
        ],
      ),
    );

    if (confirm == true) {
      try {
        await _adminService.deleteAdmin(id);
        _loadAdmins(); // Recharger la liste
        if (mounted) {
          ScaffoldMessenger.of(context).showSnackBar(
            const SnackBar(content: Text('Admin supprimé avec succès')),
          );
        }
      } catch (e) {
        if (mounted) {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(content: Text('Erreur: $e')),
          );
        }
      }
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Liste des Admins'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: _loadAdmins,
          ),
        ],
      ),
      body: _buildBody(),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          // Navigation vers le formulaire de création
          Navigator.pushNamed(context, '/admin/create');
        },
        child: const Icon(Icons.add),
      ),
    );
  }

  Widget _buildBody() {
    if (_isLoading) {
      return const Center(child: CircularProgressIndicator());
    }

    if (_errorMessage != null) {
      return Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Icon(Icons.error, color: Colors.red, size: 60),
            const SizedBox(height: 16),
            Text(_errorMessage!, textAlign: TextAlign.center),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: _loadAdmins,
              child: const Text('Réessayer'),
            ),
          ],
        ),
      );
    }

    if (_admins.isEmpty) {
      return const Center(
        child: Text('Aucun admin trouvé'),
      );
    }

    return RefreshIndicator(
      onRefresh: _loadAdmins,
      child: ListView.builder(
        itemCount: _admins.length,
        itemBuilder: (context, index) {
          final admin = _admins[index];
          return Card(
            margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 8),
            child: ListTile(
              leading: CircleAvatar(
                child: Text(admin.nom[0].toUpperCase()),
              ),
              title: Text('${admin.nom} ${admin.prenom}'),
              subtitle: Text(admin.email),
              trailing: IconButton(
                icon: const Icon(Icons.delete, color: Colors.red),
                onPressed: () => _deleteAdmin(admin.id!),
              ),
              onTap: () {
                // Navigation vers les détails
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => AdminDetailScreen(admin: admin),
                  ),
                );
              },
            ),
          );
        },
      ),
    );
  }
}
```

---

## 12. Formulaires avec Validation

```dart
class AdminFormScreen extends StatefulWidget {
  const AdminFormScreen({super.key});

  @override
  State<AdminFormScreen> createState() => _AdminFormScreenState();
}

class _AdminFormScreenState extends State<AdminFormScreen> {
  final _formKey = GlobalKey<FormState>();
  final _nomController = TextEditingController();
  final _prenomController = TextEditingController();
  final _emailController = TextEditingController();
  final AdminService _adminService = AdminService();
  bool _isLoading = false;

  @override
  void dispose() {
    _nomController.dispose();
    _prenomController.dispose();
    _emailController.dispose();
    super.dispose();
  }

  Future<void> _submitForm() async {
    if (_formKey.currentState!.validate()) {
      setState(() {
        _isLoading = true;
      });

      final admin = Admin(
        nom: _nomController.text,
        prenom: _prenomController.text,
        email: _emailController.text,
      );

      try {
        await _adminService.createAdmin(admin);
        if (mounted) {
          Navigator.pop(context);
          ScaffoldMessenger.of(context).showSnackBar(
            const SnackBar(content: Text('Admin créé avec succès')),
          );
        }
      } catch (e) {
        if (mounted) {
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(content: Text('Erreur: $e')),
          );
        }
      } finally {
        if (mounted) {
          setState(() {
            _isLoading = false;
          });
        }
      }
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Créer un Admin'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: ListView(
            children: [
              TextFormField(
                controller: _nomController,
                decoration: const InputDecoration(
                  labelText: 'Nom',
                  border: OutlineInputBorder(),
                  prefixIcon: Icon(Icons.person),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Le nom est requis';
                  }
                  if (value.length < 2) {
                    return 'Minimum 2 caractères';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _prenomController,
                decoration: const InputDecoration(
                  labelText: 'Prénom',
                  border: OutlineInputBorder(),
                  prefixIcon: Icon(Icons.person_outline),
                ),
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'Le prénom est requis';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 16),
              TextFormField(
                controller: _emailController,
                decoration: const InputDecoration(
                  labelText: 'Email',
                  border: OutlineInputBorder(),
                  prefixIcon: Icon(Icons.email),
                ),
                keyboardType: TextInputType.emailAddress,
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'L\'email est requis';
                  }
                  if (!value.contains('@')) {
                    return 'Email invalide';
                  }
                  return null;
                },
              ),
              const SizedBox(height: 24),
              ElevatedButton(
                onPressed: _isLoading ? null : _submitForm,
                style: ElevatedButton.styleFrom(
                  padding: const EdgeInsets.symmetric(vertical: 16),
                ),
                child: _isLoading
                    ? const CircularProgressIndicator()
                    : const Text('Créer', style: TextStyle(fontSize: 16)),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

---

## 13. Gestion d'État avec setState

```dart
class CounterWidget extends StatefulWidget {
  const CounterWidget({super.key});

  @override
  State<CounterWidget> createState() => _CounterWidgetState();
}

class _CounterWidgetState extends State<CounterWidget> {
  int _counter = 0;
  bool _isVisible = true;

  void _increment() {
    setState(() {  // ⚠️ Reconstruit le widget avec les nouvelles valeurs
      _counter++;
    });
  }

  void _toggleVisibility() {
    setState(() {
      _isVisible = !_isVisible;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Counter: $_counter'),
        ElevatedButton(
          onPressed: _increment,
          child: const Text('Increment'),
        ),
        if (_isVisible) const Text('Je suis visible !'),
        ElevatedButton(
          onPressed: _toggleVisibility,
          child: Text(_isVisible ? 'Masquer' : 'Afficher'),
        ),
      ],
    );
  }
}
```

---

## 14. Widgets Utiles

### CircularProgressIndicator

**Rôle :** Indicateur de chargement circulaire

```dart
const Center(
  child: CircularProgressIndicator(),
)
```

### AlertDialog

**Rôle :** Boîte de dialogue modale

```dart
showDialog(
  context: context,
  builder: (context) => AlertDialog(
    title: const Text('Confirmation'),
    content: const Text('Voulez-vous continuer ?'),
    actions: [
      TextButton(
        onPressed: () => Navigator.pop(context),
        child: const Text('Annuler'),
      ),
      TextButton(
        onPressed: () {
          Navigator.pop(context);
          // Action
        },
        child: const Text('OK'),
      ),
    ],
  ),
)
```

### SnackBar

**Rôle :** Notification en bas de l'écran

```dart
ScaffoldMessenger.of(context).showSnackBar(
  const SnackBar(
    content: Text('Opération réussie !'),
    duration: Duration(seconds: 2),
  ),
)
```

### Image

**Rôle :** Afficher des images

```dart
// Image depuis un réseau
Image.network('https://example.com/image.jpg')

// Image depuis les assets
Image.asset('assets/images/logo.png')

// Image avec gestion d'erreur
Image.network(
  'https://example.com/image.jpg',
  loadingBuilder: (context, child, progress) {
    if (progress == null) return child;
    return const CircularProgressIndicator();
  },
  errorBuilder: (context, error, stackTrace) {
    return const Icon(Icons.error);
  },
)
```

### Card

**Rôle :** Carte avec élévation

```dart
Card(
  elevation: 4,
  margin: const EdgeInsets.all(8),
  child: Padding(
    padding: const EdgeInsets.all(16),
    child: Column(
      children: [
        const Text('Titre', style: TextStyle(fontSize: 18)),
        const SizedBox(height: 8),
        const Text('Contenu de la carte'),
      ],
    ),
  ),
)
```

---

## 15. Lifecycle d'un StatefulWidget

```dart
class LifecycleDemo extends StatefulWidget {
  const LifecycleDemo({super.key});

  @override
  State<LifecycleDemo> createState() => _LifecycleDemoState();
}

class _LifecycleDemoState extends State<LifecycleDemo> {
  
  @override
  void initState() {
    super.initState();
    // ⚠️ Appelé UNE SEULE FOIS à la création
    print('initState: Widget créé');
    // Idéal pour : appels API, initialisation de variables
  }

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    // Appelé après initState et quand les dépendances changent
    print('didChangeDependencies');
  }

  @override
  Widget build(BuildContext context) {
    // ⚠️ Appelé à CHAQUE setState()
    print('build: Widget reconstruit');
    return const Text('Widget');
  }

  @override
  void dispose() {
    // ⚠️ Appelé avant la destruction du widget
    print('dispose: Nettoyage');
    // Libérer les ressources : controllers, listeners, timers
    super.dispose();
  }
}
```

---

## 16. Astuces et Bonnes Pratiques

### const Constructors (Performance)

```dart
// ✅ BON - Utilise const quand possible
const Text('Hello')
const SizedBox(height: 20)
const Icon(Icons.home)

// ❌ MAUVAIS - Widget reconstruit inutilement
Text('Hello')
```

### Extract Widgets (Lisibilité)

```dart
// ✅ BON - Widget extrait
class _AdminCard extends StatelessWidget {
  final Admin admin;
  const _AdminCard({required this.admin});

  @override
  Widget build(BuildContext context) {
    return Card(
      child: ListTile(
        title: Text(admin.nom),
        subtitle: Text(admin.email),
      ),
    );
  }
}

// ❌ MAUVAIS - Tout dans build()
@override
Widget build(BuildContext context) {
  return ListView.builder(
    itemBuilder: (context, index) {
      return Card(
        child: ListTile(
          title: Text(admins[index].nom),
          // 50 lignes de code...
        ),
      );
    },
  );
}
```

### Dispose Controllers

```dart
// ✅ BON
class MyWidget extends StatefulWidget {
  @override
  State<MyWidget> createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  final _controller = TextEditingController();

  @override
  void dispose() {
    _controller.dispose();  // ⚠️ Libère la mémoire
    super.dispose();
  }
}
```

---

## Résumé des Concepts Clés

|Concept|Description|Usage|
|---|---|---|
|`StatelessWidget`|Widget immuable|UI statique|
|`StatefulWidget`|Widget avec état|UI dynamique|
|`setState()`|Met à jour l'UI|Après modification de variables|
|`Column`|Layout vertical|Empiler verticalement|
|`Row`|Layout horizontal|Empiler horizontalement|
|`Container`|Boîte de style|Padding, margin, couleur|
|`ListView.builder`|Liste optimisée|Grandes listes|
|`Navigator`|Navigation|Changer de page|
|`Future`|Asynchrone|Appels API|
|`async/await`|Attendre une Future|Gérer l'asynchrone|

---

## Checklist Projet Flutter

✅ Créer les models avec `fromJson()` et `toJson()`  
✅ Ajouter le package `http` dans `pubspec.yaml`  
✅ Créer les services pour les appels API  
✅ Utiliser `StatefulWidget` pour les pages dynamiques  
✅ Gérer les états avec `setState()`  
✅ Ajouter un `CircularProgressIndicator` pendant le chargement  
✅ Gérer les erreurs avec try/catch  
✅ Utiliser `const` pour optimiser les performances  
✅ Libérer les `TextEditingController` dans `dispose()`  
✅ Tester sur Android/iOS avec `flutter run`

---

## 📚 Ressources Utiles

- [Documentation officielle Flutter](https://flutter.dev/docs)
- [Dart Language Tour](https://dart.dev/guides/language/language-tour)
- [Flutter Packages](https://pub.dev/)
- [Flutter Widget Catalog](https://flutter.dev/docs/development/ui/widgets)

---

**Créé avec ❤️ pour faciliter l'apprentissage de Flutter**