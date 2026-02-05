

# Guide Complet Dart - Du Débutant à l'Expert

## 1. Introduction à Dart

**Dart** est un langage de programmation développé par Google, utilisé principalement pour :

- **Flutter** (applications mobiles iOS/Android)
- **Applications web** (avec compilation en JavaScript)
- **Applications serveur** (backend)

**Caractéristiques :**

- Orienté objet
- Fortement typé (avec inférence de type)
- Syntaxe similaire à Java/JavaScript/C#
- Compilé en code natif (très performant)

---

## 2. Variables et Types de Données

### Déclaration de Variables

```dart
// var - Type inféré automatiquement
var nom = 'John';  // String
var age = 25;      // int

// Type explicite (recommandé)
String prenom = 'Doe';
int annee = 2024;
double prix = 19.99;
bool estActif = true;

// final - Valeur assignée une seule fois (runtime)
final String ville = 'Paris';
// ville = 'Lyon';  // ❌ ERREUR

// const - Constante de compilation (compile-time)
const double PI = 3.14159;
const int MAX_USERS = 100;

// late - Initialisation différée
late String description;
description = 'Initialisé plus tard';

// Nullable types (peut être null)
String? email;  // Peut être null
email = null;   // ✅ OK
email = 'test@example.com';  // ✅ OK

// Non-nullable types (ne peut PAS être null)
String username = 'admin';
// username = null;  // ❌ ERREUR
```

### Types Primitifs

```dart
// Nombres
int entier = 42;
double decimal = 3.14;
num nombre = 10;  // Peut être int ou double

// Chaînes de caractères
String simple = 'Hello';
String double = "World";
String multiline = '''
  Texte
  sur plusieurs
  lignes
''';

// Interpolation de chaînes
String nom = 'Alice';
String message = 'Bonjour $nom';  // Bonjour Alice
String calcul = 'La somme est ${2 + 3}';  // La somme est 5

// Booléens
bool estVrai = true;
bool estFaux = false;

// Runes (caractères Unicode)
Runes emoji = Runes('\u{1F600}');  // 😀

// Symbols (identifiants)
Symbol sym = #mySymbol;
```

---

## 3. Collections

### List (Tableau dynamique)

```dart
// Déclaration
List<String> fruits = ['Pomme', 'Banane', 'Orange'];
var nombres = [1, 2, 3, 4, 5];
List<int> vide = [];

// Accès
print(fruits[0]);  // Pomme
print(fruits.length);  // 3
print(fruits.first);  // Pomme
print(fruits.last);   // Orange

// Modification
fruits.add('Fraise');  // Ajoute à la fin
fruits.insert(1, 'Kiwi');  // Insère à l'index 1
fruits.remove('Banane');  // Supprime par valeur
fruits.removeAt(0);  // Supprime par index
fruits.clear();  // Vide la liste

// Opérations
List<int> nums = [1, 2, 3];
nums.contains(2);  // true
nums.indexOf(3);   // 2
nums.reversed.toList();  // [3, 2, 1]
nums.sort();  // Trie en place

// List immuable (non modifiable)
const List<String> constList = ['A', 'B', 'C'];
// constList.add('D');  // ❌ ERREUR

// Spread operator (...)
List<int> a = [1, 2];
List<int> b = [3, 4];
List<int> combined = [...a, ...b];  // [1, 2, 3, 4]

// Collection if
bool includeZero = true;
List<int> numbers = [
  if (includeZero) 0,
  1,
  2,
  3,
];

// Collection for
List<int> doubled = [
  for (var i in [1, 2, 3]) i * 2
];  // [2, 4, 6]
```

### Set (Ensemble - valeurs uniques)

```dart
// Déclaration
Set<String> tags = {'dart', 'flutter', 'mobile'};
var nombres = <int>{1, 2, 3};

// Ajout (pas de doublons)
tags.add('web');
tags.add('dart');  // Ignoré (déjà présent)

// Opérations ensemblistes
Set<int> a = {1, 2, 3};
Set<int> b = {3, 4, 5};

a.union(b);        // {1, 2, 3, 4, 5}
a.intersection(b); // {3}
a.difference(b);   // {1, 2}

// Vérifications
tags.contains('dart');  // true
tags.length;  // nombre d'éléments
```

### Map (Dictionnaire clé-valeur)

```dart
// Déclaration
Map<String, int> ages = {
  'Alice': 25,
  'Bob': 30,
  'Charlie': 35,
};

// Autre syntaxe
var scores = <String, double>{
  'Math': 15.5,
  'Français': 14.0,
};

// Accès
print(ages['Alice']);  // 25
print(ages['Unknown']);  // null

// Modification
ages['David'] = 28;  // Ajoute
ages['Alice'] = 26;  // Modifie
ages.remove('Bob');  // Supprime

// Méthodes utiles
ages.containsKey('Alice');  // true
ages.containsValue(30);  // true
ages.keys;  // Iterable des clés
ages.values;  // Iterable des valeurs
ages.length;  // nombre d'entrées

// Parcours
ages.forEach((key, value) {
  print('$key a $value ans');
});

// Valeur par défaut si clé absente
var age = ages.putIfAbsent('Eve', () => 22);
```

---

## 4. Opérateurs

### Opérateurs Arithmétiques

```dart
int a = 10, b = 3;

a + b;   // 13 - Addition
a - b;   // 7  - Soustraction
a * b;   // 30 - Multiplication
a / b;   // 3.333... - Division (retourne double)
a ~/ b;  // 3  - Division entière
a % b;   // 1  - Modulo (reste)

// Incrémentation/Décrémentation
int count = 0;
count++;  // 1
count--;  // 0
++count;  // 1 (pré-incrémentation)
```

### Opérateurs de Comparaison

```dart
int x = 5, y = 10;

x == y;  // false - Égalité
x != y;  // true  - Différent
x > y;   // false - Supérieur
x < y;   // true  - Inférieur
x >= y;  // false - Supérieur ou égal
x <= y;  // true  - Inférieur ou égal
```

### Opérateurs Logiques

```dart
bool a = true, b = false;

a && b;  // false - ET logique
a || b;  // true  - OU logique
!a;      // false - NON logique
```

### Opérateurs Spécifiques à Dart

```dart
// ?? (if-null) - Retourne la valeur de gauche si non-null, sinon celle de droite
String? nom;
String display = nom ?? 'Anonyme';  // 'Anonyme'

// ??= - Assigne seulement si null
String? titre;
titre ??= 'Sans titre';  // titre = 'Sans titre'
titre ??= 'Nouveau';     // Pas d'effet (déjà non-null)

// ?. (null-aware) - Appelle seulement si non-null
String? text;
int? longueur = text?.length;  // null (pas d'erreur)

// ! (null assertion) - Force le non-null (attention aux erreurs)
String? email = 'test@example.com';
int len = email!.length;  // OK si email n'est pas null

// .. (cascade) - Enchaîne les opérations sur le même objet
var list = [1, 2, 3]
  ..add(4)
  ..add(5)
  ..remove(1);  // [2, 3, 4, 5]

// as (cast) - Conversion de type
dynamic value = 'Hello';
String text = value as String;

// is / is! - Vérification de type
var obj = 'Hello';
if (obj is String) {
  print('C\'est une chaîne');
}
if (obj is! int) {
  print('Ce n\'est pas un entier');
}
```

---

## 5. Structures de Contrôle

### if / else

```dart
int age = 18;

if (age >= 18) {
  print('Majeur');
} else if (age >= 13) {
  print('Adolescent');
} else {
  print('Enfant');
}

// Expression ternaire
String status = age >= 18 ? 'Adulte' : 'Mineur';

// if-null
String? nom;
String display = nom ?? 'Anonyme';
```

### switch / case

```dart
String grade = 'A';

switch (grade) {
  case 'A':
    print('Excellent');
    break;
  case 'B':
    print('Bien');
    break;
  case 'C':
    print('Moyen');
    break;
  default:
    print('Insuffisant');
}

// Switch avec énumérations (plus propre)
enum Status { pending, approved, rejected }

Status current = Status.pending;

switch (current) {
  case Status.pending:
    print('En attente');
    break;
  case Status.approved:
    print('Approuvé');
    break;
  case Status.rejected:
    print('Rejeté');
    break;
}
```

### Boucles

```dart
// for classique
for (int i = 0; i < 5; i++) {
  print(i);  // 0, 1, 2, 3, 4
}

// for-in (itération sur collection)
List<String> fruits = ['Pomme', 'Banane', 'Orange'];
for (var fruit in fruits) {
  print(fruit);
}

// forEach (méthode sur collections)
fruits.forEach((fruit) {
  print(fruit);
});

// while
int count = 0;
while (count < 5) {
  print(count);
  count++;
}

// do-while
int num = 0;
do {
  print(num);
  num++;
} while (num < 5);

// break et continue
for (int i = 0; i < 10; i++) {
  if (i == 3) continue;  // Passe à l'itération suivante
  if (i == 7) break;     // Sort de la boucle
  print(i);
}
```

---

## 6. Fonctions

### Fonctions de Base

```dart
// Fonction simple
void direBonjour() {
  print('Bonjour !');
}

// Fonction avec paramètres
int additionner(int a, int b) {
  return a + b;
}

// Fonction avec expression (arrow function)
int multiplier(int a, int b) => a * b;

// Fonction sans retour (void)
void afficher(String message) {
  print(message);
}

// Appel
direBonjour();
int somme = additionner(5, 3);
int produit = multiplier(4, 2);
```

### Paramètres Nommés

```dart
// Paramètres nommés optionnels
void creerUtilisateur({String? nom, int? age, String? email}) {
  print('Nom: $nom, Age: $age, Email: $email');
}

// Appel avec paramètres nommés
creerUtilisateur(nom: 'Alice', age: 25);
creerUtilisateur(email: 'alice@example.com');

// Paramètres nommés requis
void afficherInfos({required String nom, required int age}) {
  print('$nom a $age ans');
}

// Appel (obligatoire de fournir tous les paramètres)
afficherInfos(nom: 'Bob', age: 30);

// Valeurs par défaut
void creerProduit({String nom = 'Sans nom', double prix = 0.0}) {
  print('$nom: $prix€');
}

creerProduit();  // Sans nom: 0.0€
creerProduit(nom: 'Laptop', prix: 999.99);
```

### Paramètres Positionnels Optionnels

```dart
// Entre crochets []
String saluer([String nom = 'Invité', String titre = 'Mr']) {
  return 'Bonjour $titre $nom';
}

print(saluer());  // Bonjour Mr Invité
print(saluer('Alice'));  // Bonjour Mr Alice
print(saluer('Alice', 'Mme'));  // Bonjour Mme Alice
```

### Fonctions Anonymes (Lambda)

```dart
// Fonction anonyme
var multiplier = (int a, int b) => a * b;
print(multiplier(3, 4));  // 12

// Dans une méthode
List<int> nombres = [1, 2, 3, 4];
nombres.forEach((n) {
  print(n * 2);
});

// Avec map
var doubles = nombres.map((n) => n * 2).toList();
print(doubles);  // [2, 4, 6, 8]
```

### Fonctions de Première Classe

```dart
// Fonction en tant que paramètre
void executerOperation(int a, int b, Function operation) {
  print(operation(a, b));
}

int additionner(int x, int y) => x + y;
int multiplier(int x, int y) => x * y;

executerOperation(5, 3, additionner);   // 8
executerOperation(5, 3, multiplier);    // 15

// Fonction qui retourne une fonction
Function creerMultiplicateur(int facteur) {
  return (int n) => n * facteur;
}

var doubler = creerMultiplicateur(2);
var tripler = creerMultiplicateur(3);

print(doubler(5));  // 10
print(tripler(5));  // 15
```

---

## 7. Classes et Objets

### Classe de Base

```dart
class Personne {
  // Propriétés (attributs)
  String nom;
  int age;
  String? email;  // Optionnel

  // Constructeur
  Personne(this.nom, this.age, [this.email]);

  // Méthode
  void sePresenter() {
    print('Je m\'appelle $nom et j\'ai $age ans');
  }

  // Méthode avec retour
  bool estMajeur() {
    return age >= 18;
  }
}

// Utilisation
var p = Personne('Alice', 25, 'alice@example.com');
p.sePresenter();  // Je m'appelle Alice et j'ai 25 ans
print(p.estMajeur());  // true
```

### Constructeurs

```dart
class Produit {
  String nom;
  double prix;
  int stock;

  // Constructeur principal
  Produit(this.nom, this.prix, this.stock);

  // Constructeur nommé
  Produit.gratuit(this.nom)
      : prix = 0.0,
        stock = 1000;

  // Constructeur nommé avec paramètres
  Produit.fromJson(Map<String, dynamic> json)
      : nom = json['nom'],
        prix = json['prix'],
        stock = json['stock'];

  // Constructeur factory
  factory Produit.creer(String type) {
    if (type == 'basique') {
      return Produit('Produit basique', 9.99, 100);
    } else {
      return Produit('Produit premium', 99.99, 10);
    }
  }
}

// Utilisation
var p1 = Produit('Laptop', 999.99, 5);
var p2 = Produit.gratuit('eBook');
var p3 = Produit.fromJson({'nom': 'Phone', 'prix': 599.0, 'stock': 20});
var p4 = Produit.creer('basique');
```

### Getters et Setters

```dart
class Rectangle {
  double largeur;
  double hauteur;

  Rectangle(this.largeur, this.hauteur);

  // Getter
  double get aire => largeur * hauteur;

  // Getter avec logique
  double get perimetre {
    return 2 * (largeur + hauteur);
  }

  // Setter
  set dimensions(List<double> dims) {
    largeur = dims[0];
    hauteur = dims[1];
  }
}

// Utilisation
var rect = Rectangle(10, 5);
print(rect.aire);  // 50 (appelé comme une propriété)
print(rect.perimetre);  // 30

rect.dimensions = [20, 10];
print(rect.aire);  // 200
```

### Propriétés Privées

```dart
class CompteBancaire {
  String _titulaire;  // _ = privé
  double _solde;

  CompteBancaire(this._titulaire, this._solde);

  // Getter public
  double get solde => _solde;
  String get titulaire => _titulaire;

  // Méthode publique
  void deposer(double montant) {
    if (montant > 0) {
      _solde += montant;
    }
  }

  void retirer(double montant) {
    if (montant > 0 && montant <= _solde) {
      _solde -= montant;
    }
  }
}

// Utilisation
var compte = CompteBancaire('Alice', 1000);
print(compte.solde);  // 1000
// compte._solde = 5000;  // ❌ ERREUR (privé)
compte.deposer(500);
print(compte.solde);  // 1500
```

### Héritage

```dart
// Classe parent
class Animal {
  String nom;
  int age;

  Animal(this.nom, this.age);

  void faireDuBruit() {
    print('L\'animal fait du bruit');
  }

  void dormir() {
    print('$nom dort');
  }
}

// Classe enfant
class Chien extends Animal {
  String race;

  // Appel du constructeur parent avec super
  Chien(String nom, int age, this.race) : super(nom, age);

  // Override (redéfinition)
  @override
  void faireDuBruit() {
    print('$nom aboie: Woof!');
  }

  // Méthode spécifique
  void rapporter() {
    print('$nom rapporte la balle');
  }
}

// Utilisation
var chien = Chien('Rex', 3, 'Labrador');
chien.faireDuBruit();  // Rex aboie: Woof!
chien.dormir();  // Rex dort (hérité)
chien.rapporter();  // Rex rapporte la balle
```

### Classes Abstraites

```dart
// Classe abstraite (ne peut pas être instanciée)
abstract class Forme {
  // Méthode abstraite (doit être implémentée)
  double calculerAire();
  
  // Méthode concrète
  void afficher() {
    print('Aire: ${calculerAire()}');
  }
}

class Cercle extends Forme {
  double rayon;

  Cercle(this.rayon);

  @override
  double calculerAire() {
    return 3.14159 * rayon * rayon;
  }
}

class Carre extends Forme {
  double cote;

  Carre(this.cote);

  @override
  double calculerAire() {
    return cote * cote;
  }
}

// Utilisation
// var forme = Forme();  // ❌ ERREUR (abstraite)
var cercle = Cercle(5);
var carre = Carre(4);

print(cercle.calculerAire());  // 78.53975
print(carre.calculerAire());   // 16
```

### Interfaces (implements)

```dart
// Toute classe peut servir d'interface
class Volant {
  void voler() {}
  void atterrir() {}
}

class Nageur {
  void nager() {}
}

// Implémentation multiple
class Canard implements Volant, Nageur {
  @override
  void voler() {
    print('Le canard vole');
  }

  @override
  void atterrir() {
    print('Le canard atterrit');
  }

  @override
  void nager() {
    print('Le canard nage');
  }
}

var canard = Canard();
canard.voler();
canard.nager();
```

### Mixins (Réutilisation de code)

```dart
// Mixin (avec keyword mixin)
mixin Marcheur {
  void marcher() {
    print('Je marche');
  }
}

mixin Coureur {
  void courir() {
    print('Je cours');
  }
}

// Classe utilisant des mixins
class Humain with Marcheur, Coureur {
  String nom;
  
  Humain(this.nom);
}

var personne = Humain('Bob');
personne.marcher();  // Je marche
personne.courir();   // Je cours
```

### Static (Membres de classe)

```dart
class MathUtils {
  // Propriété statique
  static const double PI = 3.14159;

  // Méthode statique
  static int additionner(int a, int b) {
    return a + b;
  }

  static double aireCercle(double rayon) {
    return PI * rayon * rayon;
  }
}

// Utilisation (sans instanciation)
print(MathUtils.PI);  // 3.14159
print(MathUtils.additionner(5, 3));  // 8
print(MathUtils.aireCercle(5));  // 78.53975
```

---

## 8. Énumérations (Enum)

```dart
// Énumération simple
enum Jour {
  lundi,
  mardi,
  mercredi,
  jeudi,
  vendredi,
  samedi,
  dimanche
}

// Utilisation
Jour aujourd hui = Jour.lundi;

if (aujourd hui == Jour.samedi || aujourd hui == Jour.dimanche) {
  print('C\'est le week-end !');
}

// Switch avec enum
switch (aujourd hui) {
  case Jour.lundi:
    print('Début de semaine');
    break;
  case Jour.vendredi:
    print('Presque le week-end');
    break;
  default:
    print('Jour normal');
}

// Énumération enrichie (Dart 2.17+)
enum Status {
  pending('En attente', '⏳'),
  approved('Approuvé', '✅'),
  rejected('Rejeté', '❌');

  final String label;
  final String icon;

  const Status(this.label, this.icon);
}

// Utilisation
Status current = Status.approved;
print('${current.icon} ${current.label}');  // ✅ Approuvé

// Parcourir toutes les valeurs
for (var status in Status.values) {
  print('${status.icon} ${status.label}');
}
```

---

## 9. Généricité (Generics)

```dart
// Classe générique
class Boite<T> {
  T contenu;

  Boite(this.contenu);

  T obtenir() => contenu;

  void modifier(T nouveau) {
    contenu = nouveau;
  }
}

// Utilisation
var boiteInt = Boite<int>(42);
print(boiteInt.obtenir());  // 42
boiteInt.modifier(100);

var boiteString = Boite<String>('Hello');
print(boiteString.obtenir());  // Hello

// Fonction générique
T premier<T>(List<T> liste) {
  return liste[0];
}

print(premier<int>([1, 2, 3]));  // 1
print(premier<String>(['a', 'b']));  // a

// Contraintes de type (extends)
class Cache<T extends num> {
  T valeur;
  
  Cache(this.valeur);
  
  T doubler() => (valeur * 2) as T;
}

var cacheInt = Cache<int>(5);
var cacheDouble = Cache<double>(3.14);
// var cacheString = Cache<String>('test');  // ❌ ERREUR (String n'extend pas num)
```

---

## 10. Gestion des Exceptions

```dart
// Lancer une exception
void verifierAge(int age) {
  if (age < 0) {
    throw Exception('L\'âge ne peut pas être négatif');
  }
  if (age < 18) {
    throw FormatException('Vous devez avoir 18 ans minimum');
  }
}

// Try-catch
void exemple1() {
  try {
    verifierAge(-5);
  } catch (e) {
    print('Erreur: $e');
  }
}

// Try-catch avec type spécifique
void exemple2() {
  try {
    verifierAge(15);
  } on FormatException catch (e) {
    print('Format invalide: $e');
  } on Exception catch (e) {
    print('Erreur générale: $e');
  }
}

// Try-catch-finally
void exemple3() {
  try {
    int resultat = 10 ~/ 0;  // Division par zéro
  } catch (e) {
    print('Erreur: $e');
  } finally {
    print('Toujours exécuté (nettoyage)');
  }
}

// Créer une exception personnalisée
class AgeInvalideException implements Exception {
  final String message;
  
  AgeInvalideException(this.message);
  
  @override
  String toString() => 'AgeInvalideException: $message';
}

void validerAge(int age) {
  if (age < 0 || age > 150) {
    throw AgeInvalideException('L\'âge doit être entre 0 et 150');
  }
}
```

---

## 11. Asynchrone (Async/Await)

### Future (Tâche asynchrone)

```dart
// Future simple
Future<String> chargerDonnees() {
  return Future.delayed(
    const Duration(seconds: 2),
    () => 'Données chargées',
  );
}

// Utilisation avec then
void exemple1() {
  print('Début');
  chargerDonnees().then((resultat) {
    print(resultat);
  });
  print('Fin (exécuté avant le résultat)');
}

// Utilisation avec async/await (recommandé)
Future<void> exemple2() async {
  print('Début');
  String resultat = await chargerDonnees();
  print(resultat);
  print('Fin');
}

// Gestion d'erreurs avec async/await
Future<void> exemple3() async {
  try {
    String data = await chargerDonnees();
    print(data);
  } catch (e) {
    print('Erreur: $e');
  }
}

// Future.wait (attendre plusieurs futures)
Future<void> exemple4() async {
  var futures = [
    Future.delayed(Duration(seconds: 1), () => 'A'),
    Future.delayed(Duration(seconds: 2), () => 'B'),
    Future.delayed(Duration(seconds: 1), () => 'C'),
  ];

  var resultats = await Future.wait(futures);
  print(resultats);  // ['A', 'B', 'C'] après 2 secondes
}
```

### Exemple Réaliste (API)

```dart
import 'dart:convert';
import 'package:http/http.dart' as http;

class User {
  final int id;
  final String name;
  final String email;

  User({required this.id, required this.name, required this.email});

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'],
      name: json['name'],
      email: json['email'],
    );
  }
}

class UserService {
  Future<List<User>> fetchUsers() async {
    final response = await http.get(
      Uri.parse('https://jsonplaceholder.typicode.com/users'),
    );

    if (response.statusCode == 200) {
      List<dynamic> jsonData = json.decode(response.body);
      return jsonData.map((json) => User.fromJson(json)).toList();
    } else {
      throw Exception('Échec du chargement');
    }
  }

  Future<User> fetchUserById(int id) async {
    final response = await http.get(
      Uri.parse('https://jsonplaceholder.typicode.com/users/$id'),
    );

    if (response.statusCode == 200) {
      return User.fromJson(json.decode(response.body));
    } else {
      throw Exception('Utilisateur non trouvé');
    }
  }
}

// Utilisation
void main() async {
  var service = UserService();
  
  try {
    List<User> users = await service.fetchUsers();
    for (var user in users) {
      print('${user.name} - ${user.email}');
    }
  } catch (e) {
    print('Erreur: $e');
  }
}
```

### Stream (Flux de données)

```dart
// Stream simple
Stream<int> compteur() async* {
  for (int i = 1; i <= 5; i++) {
    await Future.delayed(const Duration(seconds: 1));
    yield i;  // Émet une valeur
  }
}

// Utilisation avec await for
Future<void> exemple1() async {
  await for (var valeur in compteur()) {
    print(valeur);  // 1, 2, 3, 4, 5 (avec 1s entre chaque)
  }
}

// Utilisation avec listen
void exemple2() {
  compteur().listen(
    (valeur) {
      print('Valeur: $valeur');
    },
    onDone: () {
      print('Stream terminé');
    },
    onError: (error) {
      print('Erreur: $error');
    },
  );
}

// StreamController (pour créer des streams personnalisés)
import 'dart:async';

void exemple3() {
  final controller = StreamController<String>();

  // Écouter le stream
  controller.stream.listen((data) {
    print('Reçu: $data');
  });

  // Ajouter des données
  controller.add('Message 1');
  controller.add('Message 2');
  controller.add('Message 3');

  // Fermer le stream
  controller.close();
}

// Transformation de stream
void exemple4() async {
  var stream = Stream.fromIterable([1, 2, 3, 4, 5]);

  // map
  var doubled = stream.map((n) => n * 2);
  
  // where (filter)
  var evenOnly = stream.where((n) => n % 2 == 0);
  
  await for (var value in doubled) {
    print(value);  // 2, 4, 6, 8, 10
  }
}
```

---

## 12. Extensions

```dart
// Ajouter des méthodes à des types existants
extension StringExtension on String {
  // Capitaliser la première lettre
  String capitalize() {
    if (isEmpty) return this;
    return '${this[0].toUpperCase()}${substring(1)}';
  }

  // Vérifier si c'est un email
  bool isEmail() {
    return contains('@') && contains('.');
  }

  // Inverser la chaîne
  String reverse() {
    return split('').reversed.join('');
  }
}

// Utilisation
void main() {
  String nom = 'alice';
  print(nom.capitalize());  // Alice

  String email = 'test@example.com';
  print(email.isEmail());  // true

  String text = 'hello';
  print(text.reverse());  // olleh
}

// Extension sur List
extension ListExtension<T> on List<T> {
  T? get firstOrNull => isEmpty ? null : first;
  
  List<T> unique() {
    return toSet().toList();
  }
}

// Utilisation
void main() {
  List<int> nombres = [];
  print(nombres.firstOrNull);  // null (pas d'erreur)

  List<int> duplicates = [1, 2, 2, 3, 3, 4];
  print(duplicates.unique());  // [1, 2, 3, 4]
}
```

---

## 13. Null Safety

```dart
// Types nullable vs non-nullable
String nonNullable = 'Hello';  // Ne peut PAS être null
String? nullable = null;        // PEUT être null

// Vérification de null
void afficher(String? texte) {
  if (texte != null) {
    print(texte.length);  // Safe après vérification
  }
}

// Opérateur ??
String? nom;
String display = nom ?? 'Anonyme';  // Utilise 'Anonyme' si nom est null

// Opérateur ??=
String? titre;
titre ??= 'Sans titre';  // Assigne seulement si null

// Opérateur ?.
String? email;
int? longueur = email?.length;  // null si email est null

// Opérateur ! (null assertion - à utiliser avec précaution)
String? username = 'admin';
int len = username!.length;  // Crash si username est null

// late (initialisation différée)
late String description;

void initialiser() {
  description = 'Initialisé';
}

void utiliser() {
  print(description);  // Crash si pas initialisé avant
}
```

---

## 14. Méthodes Utiles sur Collections

### List

```dart
List<int> nombres = [1, 2, 3, 4, 5];

// map - Transformer chaque élément
var doubles = nombres.map((n) => n * 2).toList();
print(doubles);  // [2, 4, 6, 8, 10]

// where - Filtrer
var pairs = nombres.where((n) => n % 2 == 0).toList();
print(pairs);  // [2, 4]

// reduce - Réduire à une seule valeur
var somme = nombres.reduce((a, b) => a + b);
print(somme);  // 15

// fold - Comme reduce mais avec valeur initiale
var produit = nombres.fold(1, (prev, n) => prev * n);
print(produit);  // 120

// any - Au moins un élément correspond
bool hasEven = nombres.any((n) => n % 2 == 0);
print(hasEven);  // true

// every - Tous les éléments correspondent
bool allPositive = nombres.every((n) => n > 0);
print(allPositive);  // true

// firstWhere / lastWhere
var premier = nombres.firstWhere((n) => n > 3);
print(premier);  // 4

// take / skip
var premiers3 = nombres.take(3).toList();
print(premiers3);  // [1, 2, 3]

var sansLes2Premiers = nombres.skip(2).toList();
print(sansLes2Premiers);  // [3, 4, 5]

// sort
List<String> mots = ['banane', 'pomme', 'cerise'];
mots.sort();
print(mots);  // [banane, cerise, pomme]

// Tri personnalisé
List<int> nums = [5, 2, 8, 1, 9];
nums.sort((a, b) => b.compareTo(a));  // Ordre décroissant
print(nums);  // [9, 8, 5, 2, 1]
```

---

## 15. Concepts Avancés

### Typedef (Alias de Type)

```dart
// Définir un alias pour une fonction
typedef Operation = int Function(int a, int b);

int additionner(int a, int b) => a + b;
int multiplier(int a, int b) => a * b;

void executer(int x, int y, Operation op) {
  print(op(x, y));
}

executer(5, 3, additionner);  // 8
executer(5, 3, multiplier);   // 15

// Alias pour des types complexes
typedef UserMap = Map<String, dynamic>;

UserMap creerUtilisateur(String nom, int age) {
  return {'nom': nom, 'age': age};
}
```

### Callable Classes

```dart
// Classe qui peut être appelée comme une fonction
class Multiplicateur {
  final int facteur;

  Multiplicateur(this.facteur);

  // Méthode call
  int call(int valeur) {
    return valeur * facteur;
  }
}

// Utilisation
var doubler = Multiplicateur(2);
print(doubler(5));  // 10 (appelé comme une fonction)
print(doubler.call(5));  // 10 (équivalent)
```

---

## Résumé des Concepts Clés

|Concept|Description|Exemple|
|---|---|---|
|`var`|Type inféré|`var nom = 'John'`|
|`final`|Valeur assignée une fois|`final PI = 3.14`|
|`const`|Constante compile-time|`const MAX = 100`|
|`late`|Initialisation différée|`late String desc`|
|`String?`|Type nullable|`String? email = null`|
|`List`|Tableau dynamique|`[1, 2, 3]`|
|`Map`|Dictionnaire|`{'key': 'value'}`|
|`Set`|Ensemble (unique)|`{1, 2, 3}`|
|`class`|Définir une classe|`class User {}`|
|`extends`|Héritage|`class Admin extends User`|
|`implements`|Interface|`class A implements B`|
|`mixin`|Réutilisation de code|`with Marcheur`|
|`async/await`|Asynchrone|`await fetchData()`|
|`Future`|Tâche asynchrone|`Future<String>`|
|`Stream`|Flux de données|`Stream<int>`|

---

## Bonnes Pratiques

✅ **Toujours utiliser `const` quand possible** (performance)  
✅ **Préférer `final` à `var`** pour les variables non modifiées  
✅ **Utiliser null safety** (`String?` vs `String`)  
✅ **Nommer les classes en PascalCase** (`UserService`)  
✅ **Nommer les variables en camelCase** (`userName`)  
✅ **Nommer les constantes en SCREAMING_SNAKE_CASE** (`MAX_USERS`)  
✅ **Préférer `async/await` à `.then()`**  
✅ **Utiliser les extensions** pour ajouter des fonctionnalités  
✅ **Gérer les exceptions avec try-catch**  
✅ **Commenter le code complexe**

---

## 📚 Ressources Utiles

- [Documentation officielle Dart](https://dart.dev/guides)
- [DartPad (Playground en ligne)](https://dartpad.dev/)
- [Effective Dart](https://dart.dev/guides/language/effective-dart)
- [Dart Packages](https://pub.dev/)

---

**Créé avec ❤️ pour maîtriser Dart rapidement**