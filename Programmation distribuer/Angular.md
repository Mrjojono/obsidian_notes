
# Guide Complet Angular - Des Bases aux Appels API

## 1. Architecture Angular

```
┌─────────────────────────────────────────────────────────┐
│                      APPLICATION                         │
├─────────────────────────────────────────────────────────┤
│  app.component.ts (Composant racine)                    │
│  app.routes.ts (Configuration des routes)               │
│  app.config.ts (Configuration globale)                  │
└─────────────────────────────────────────────────────────┘
           │
           ├── MODULES/COMPOSANTS
           │   ├── admin.component.ts
           │   ├── user.component.ts
           │   └── dashboard.component.ts
           │
           ├── SERVICES (Logique métier + API)
           │   ├── admin.service.ts
           │   └── auth.service.ts
           │
           └── MODELS (Interfaces TypeScript)
               ├── admin.model.ts
               └── user.model.ts
```

---

## 2. Décorateurs Angular Essentiels

### `@Component`

**Rôle :** Définit un composant Angular (brique UI de base)  
**Fonction :**

- Lie un template HTML, un style CSS et une classe TypeScript
- Crée un élément réutilisable

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-admin',           // Balise HTML: <app-admin></app-admin>
  templateUrl: './admin.component.html',  // Template HTML
  styleUrls: ['./admin.component.css'],   // Styles CSS
  standalone: true,                // Angular 15+ : composant autonome
  imports: [CommonModule, FormsModule]    // Modules nécessaires
})
export class AdminComponent {
  nom: string = 'John Doe';
  age: number = 30;

  direBonjour() {
    console.log('Bonjour ' + this.nom);
  }
}
```

### `@Injectable`

**Rôle :** Marque une classe comme service injectable  
**Fonction :**

- Permet l'injection de dépendances
- `providedIn: 'root'` = singleton global

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({
  providedIn: 'root'  // Service disponible partout, instance unique
})
export class AdminService {
  private apiUrl = 'http://localhost:8080/api/admins';

  constructor(private http: HttpClient) {}

  getAllAdmins() {
    return this.http.get<Admin[]>(this.apiUrl);
  }
}
```

### `@Input` (Communication Parent → Enfant)

**Rôle :** Recevoir des données du composant parent  
**Fonction :**

- Propriété bindée depuis le parent

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-admin-card',
  template: `
    <div class="card">
      <h3>{{ admin.nom }}</h3>
      <p>{{ admin.email }}</p>
    </div>
  `
})
export class AdminCardComponent {
  @Input() admin!: Admin;  // Reçoit admin du parent
}

// Utilisation dans le parent:
// <app-admin-card [admin]="monAdmin"></app-admin-card>
```

### `@Output` (Communication Enfant → Parent)

**Rôle :** Émettre des événements vers le composant parent  
**Fonction :**

- Utilise EventEmitter pour envoyer des données

```typescript
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-admin-form',
  template: `
    <button (click)="onSubmit()">Envoyer</button>
  `
})
export class AdminFormComponent {
  @Output() adminCreated = new EventEmitter<Admin>();

  onSubmit() {
    const newAdmin: Admin = { nom: 'Test', email: 'test@test.com' };
    this.adminCreated.emit(newAdmin);  // Envoie vers le parent
  }
}

// Utilisation dans le parent:
// <app-admin-form (adminCreated)="onAdminCreated($event)"></app-admin-form>
```

---

## 3. Directives Essentielles (dans les Templates)

### `*ngIf` - Affichage conditionnel

```html
<div *ngIf="isLoggedIn">
  Bienvenue !
</div>

<div *ngIf="admins.length > 0; else noData">
  Il y a {{ admins.length }} admins
</div>
<ng-template #noData>
  <p>Aucun admin trouvé</p>
</ng-template>
```

### `*ngFor` - Boucle sur une liste

```html
<ul>
  <li *ngFor="let admin of admins; let i = index">
    {{ i + 1 }}. {{ admin.nom }} - {{ admin.email }}
  </li>
</ul>
```

### `[property]` - Property Binding (TS → HTML)

```html
<!-- Lie une propriété TypeScript à un attribut HTML -->
<img [src]="imageUrl" [alt]="imageDescription">
<button [disabled]="isLoading">Envoyer</button>
```

### `(event)` - Event Binding (HTML → TS)

```html
<!-- Appelle une méthode au clic -->
<button (click)="deleteAdmin(admin.id)">Supprimer</button>
<input (keyup)="onKeyUp($event)">
```

### `[(ngModel)]` - Two-Way Binding

```typescript
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-admin-form',
  standalone: true,
  imports: [FormsModule],  // ⚠️ Obligatoire !
  template: `
    <input [(ngModel)]="nom" placeholder="Nom">
    <p>Vous avez tapé: {{ nom }}</p>
  `
})
export class AdminFormComponent {
  nom: string = '';
}
```

---

## 4. Models (Interfaces TypeScript)

```typescript
// src/app/models/admin.model.ts
export interface Admin {
  id?: number;           // ? = optionnel
  nom: string;
  prenom: string;
  email: string;
  dateCreation?: Date;
}

// src/app/models/user.model.ts
export interface User {
  id?: number;
  username: string;
  email: string;
  role: 'ADMIN' | 'USER';  // Union type
}
```

---

## 5. Services & Appels API

### Configuration HttpClient (Angular 17+)

```typescript
// src/app/app.config.ts
import { ApplicationConfig } from '@angular/core';
import { provideHttpClient } from '@angular/common/http';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes),
    provideHttpClient()  // ⚠️ Active HttpClient
  ]
};
```

### Service Complet avec CRUD

```typescript
// src/app/services/admin.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError, map } from 'rxjs/operators';
import { Admin } from '../models/admin.model';

@Injectable({
  providedIn: 'root'
})
export class AdminService {
  private apiUrl = 'http://localhost:8080/api/admins';

  constructor(private http: HttpClient) {}

  // GET - Récupérer tous les admins
  getAllAdmins(): Observable<Admin[]> {
    return this.http.get<Admin[]>(this.apiUrl)
      .pipe(
        catchError(this.handleError)
      );
  }

  // GET - Récupérer un admin par ID
  getAdminById(id: number): Observable<Admin> {
    return this.http.get<Admin>(`${this.apiUrl}/${id}`)
      .pipe(
        catchError(this.handleError)
      );
  }

  // POST - Créer un admin
  createAdmin(admin: Admin): Observable<Admin> {
    const headers = new HttpHeaders({ 'Content-Type': 'application/json' });
    return this.http.post<Admin>(this.apiUrl, admin, { headers })
      .pipe(
        catchError(this.handleError)
      );
  }

  // PUT - Modifier un admin
  updateAdmin(id: number, admin: Admin): Observable<Admin> {
    return this.http.put<Admin>(`${this.apiUrl}/${id}`, admin)
      .pipe(
        catchError(this.handleError)
      );
  }

  // DELETE - Supprimer un admin
  deleteAdmin(id: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`)
      .pipe(
        catchError(this.handleError)
      );
  }

  // Gestion des erreurs
  private handleError(error: any) {
    console.error('Erreur API:', error);
    return throwError(() => new Error('Une erreur est survenue'));
  }
}
```

---

## 6. Composant Utilisant le Service

```typescript
// src/app/components/admin-list/admin-list.component.ts
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { AdminService } from '../../services/admin.service';
import { Admin } from '../../models/admin.model';

@Component({
  selector: 'app-admin-list',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './admin-list.component.html',
  styleUrls: ['./admin-list.component.css']
})
export class AdminListComponent implements OnInit {
  admins: Admin[] = [];
  isLoading: boolean = false;
  errorMessage: string = '';

  constructor(private adminService: AdminService) {}

  ngOnInit(): void {
    this.loadAdmins();
  }

  loadAdmins(): void {
    this.isLoading = true;
    this.adminService.getAllAdmins().subscribe({
      next: (data) => {
        this.admins = data;
        this.isLoading = false;
      },
      error: (error) => {
        this.errorMessage = 'Erreur lors du chargement';
        this.isLoading = false;
        console.error(error);
      }
    });
  }

  deleteAdmin(id: number): void {
    if (confirm('Êtes-vous sûr ?')) {
      this.adminService.deleteAdmin(id).subscribe({
        next: () => {
          this.admins = this.admins.filter(a => a.id !== id);
          alert('Admin supprimé avec succès');
        },
        error: (error) => {
          alert('Erreur lors de la suppression');
          console.error(error);
        }
      });
    }
  }
}
```

### Template HTML Correspondant

```html
<!-- admin-list.component.html -->
<div class="admin-container">
  <h2>Liste des Administrateurs</h2>

  <!-- Loader -->
  <div *ngIf="isLoading" class="spinner">Chargement...</div>

  <!-- Message d'erreur -->
  <div *ngIf="errorMessage" class="alert alert-danger">
    {{ errorMessage }}
  </div>

  <!-- Liste des admins -->
  <div *ngIf="!isLoading && admins.length > 0">
    <table class="table">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nom</th>
          <th>Email</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr *ngFor="let admin of admins">
          <td>{{ admin.id }}</td>
          <td>{{ admin.nom }} {{ admin.prenom }}</td>
          <td>{{ admin.email }}</td>
          <td>
            <button (click)="deleteAdmin(admin.id!)" class="btn-danger">
              Supprimer
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- Message si aucun admin -->
  <div *ngIf="!isLoading && admins.length === 0" class="no-data">
    Aucun administrateur trouvé.
  </div>
</div>
```

---

## 7. Formulaires avec Validation

### Reactive Forms (Recommandé)

```typescript
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators, ReactiveFormsModule } from '@angular/forms';
import { CommonModule } from '@angular/common';
import { AdminService } from '../../services/admin.service';
import { Admin } from '../../models/admin.model';

@Component({
  selector: 'app-admin-form',
  standalone: true,
  imports: [CommonModule, ReactiveFormsModule],
  templateUrl: './admin-form.component.html'
})
export class AdminFormComponent {
  adminForm: FormGroup;
  submitted = false;

  constructor(
    private fb: FormBuilder,
    private adminService: AdminService
  ) {
    this.adminForm = this.fb.group({
      nom: ['', [Validators.required, Validators.minLength(2)]],
      prenom: ['', Validators.required],
      email: ['', [Validators.required, Validators.email]]
    });
  }

  // Getters pour accéder aux contrôles facilement
  get f() { return this.adminForm.controls; }

  onSubmit(): void {
    this.submitted = true;

    if (this.adminForm.invalid) {
      return;
    }

    const newAdmin: Admin = this.adminForm.value;

    this.adminService.createAdmin(newAdmin).subscribe({
      next: (response) => {
        alert('Admin créé avec succès !');
        this.adminForm.reset();
        this.submitted = false;
      },
      error: (error) => {
        alert('Erreur lors de la création');
        console.error(error);
      }
    });
  }
}
```

### Template du Formulaire

```html
<!-- admin-form.component.html -->
<form [formGroup]="adminForm" (ngSubmit)="onSubmit()">
  
  <!-- Nom -->
  <div class="form-group">
    <label>Nom</label>
    <input type="text" formControlName="nom" class="form-control"
           [class.is-invalid]="submitted && f['nom'].errors">
    
    <div *ngIf="submitted && f['nom'].errors" class="invalid-feedback">
      <div *ngIf="f['nom'].errors['required']">Le nom est requis</div>
      <div *ngIf="f['nom'].errors['minlength']">Minimum 2 caractères</div>
    </div>
  </div>

  <!-- Email -->
  <div class="form-group">
    <label>Email</label>
    <input type="email" formControlName="email" class="form-control"
           [class.is-invalid]="submitted && f['email'].errors">
    
    <div *ngIf="submitted && f['email'].errors" class="invalid-feedback">
      <div *ngIf="f['email'].errors['required']">L'email est requis</div>
      <div *ngIf="f['email'].errors['email']">Email invalide</div>
    </div>
  </div>

  <button type="submit" class="btn btn-primary">Créer</button>
</form>
```

---

## 8. Routing

```typescript
// src/app/app.routes.ts
import { Routes } from '@angular/router';
import { AdminListComponent } from './components/admin-list/admin-list.component';
import { AdminFormComponent } from './components/admin-form/admin-form.component';
import { DashboardComponent } from './components/dashboard/dashboard.component';

export const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'dashboard', component: DashboardComponent },
  { path: 'admins', component: AdminListComponent },
  { path: 'admins/new', component: AdminFormComponent },
  { path: '**', redirectTo: '/dashboard' }  // Route par défaut (404)
];
```

### Navigation dans le Template

```html
<!-- app.component.html -->
<nav>
  <a routerLink="/dashboard" routerLinkActive="active">Dashboard</a>
  <a routerLink="/admins" routerLinkActive="active">Admins</a>
  <a routerLink="/admins/new" routerLinkActive="active">Nouveau</a>
</nav>

<router-outlet></router-outlet>  <!-- Affiche le composant correspondant -->
```

### Navigation Programmatique

```typescript
import { Router } from '@angular/router';

constructor(private router: Router) {}

goToAdmins(): void {
  this.router.navigate(['/admins']);
}
```

---

## 9. Lifecycle Hooks (Cycle de Vie)

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';

export class AdminComponent implements OnInit, OnDestroy {
  
  ngOnInit(): void {
    // Appelé après l'initialisation du composant
    console.log('Composant initialisé');
    this.loadData();
  }

  ngOnDestroy(): void {
    // Appelé avant la destruction du composant
    console.log('Composant détruit');
    // Nettoyage (désabonnement, etc.)
  }
}
```

**Hooks principaux:**

- `ngOnInit()` - Initialisation (appel API, etc.)
- `ngOnChanges()` - Quand `@Input()` change
- `ngOnDestroy()` - Nettoyage avant destruction
- `ngAfterViewInit()` - Après l'initialisation de la vue

---

## 10. Observables & RxJS

```typescript
import { Observable } from 'rxjs';
import { map, filter, catchError } from 'rxjs/operators';

// Transformation de données
this.adminService.getAllAdmins()
  .pipe(
    map(admins => admins.filter(a => a.email.includes('@admin.com'))),
    catchError(error => {
      console.error(error);
      return [];
    })
  )
  .subscribe(filteredAdmins => {
    this.admins = filteredAdmins;
  });
```

---

## Résumé des Concepts Clés

|Concept|Description|Exemple|
|---|---|---|
|`@Component`|Définit un composant UI|`selector: 'app-admin'`|
|`@Injectable`|Service injectable|`providedIn: 'root'`|
|`@Input`|Parent → Enfant|`@Input() admin: Admin`|
|`@Output`|Enfant → Parent|`@Output() clicked = new EventEmitter()`|
|`HttpClient`|Appels HTTP|`http.get()`, `http.post()`|
|`Observable`|Flux de données asynchrone|`.subscribe()`|
|`ngOnInit`|Hook d'initialisation|Chargement de données|
|`RouterModule`|Navigation entre pages|`routerLink="/admins"`|

---

## Checklist Projet Angular

✅ Installer HttpClient: `provideHttpClient()` dans `app.config.ts`  
✅ Créer les models (interfaces)  
✅ Créer les services avec `@Injectable`  
✅ Créer les composants avec `@Component`  
✅ Configurer le routing  
✅ Utiliser les directives (`*ngIf`, `*ngFor`)  
✅ Gérer les formulaires (ReactiveFormsModule)  
✅ Gérer les erreurs avec `catchError`

---

## 📚 Ressources Utiles

- [Documentation officielle Angular](https://angular.io/docs)
- [RxJS Documentation](https://rxjs.dev/)
- [Angular CLI](https://angular.io/cli)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)

---

**Créé avec ❤️ pour faciliter l'apprentissage d'Angular**