# 🏥 Mini Système de Gestion de Pharmacie

Un système de gestion de pharmacie moderne et complet développé avec Next.js 15, TypeScript, et Turso (SQLite). Ce projet permet de gérer efficacement les médicaments d'une petite pharmacie avec toutes les opérations CRUD (Créer, Lire, Mettre à jour, Supprimer).

## 📋 Table des matières

- [Fonctionnalités](#-fonctionnalités)
- [Technologies utilisées](#-technologies-utilisées)
- [Structure de la base de données](#-structure-de-la-base-de-données)
- [Installation](#-installation)
- [Utilisation](#-utilisation)
- [API Endpoints](#-api-endpoints)
- [Tests](#-tests)

## ✨ Fonctionnalités

### Opérations CRUD complètes
- ✅ **Ajouter un médicament** : Formulaire de création avec validation
- ✅ **Modifier un médicament** : Mise à jour des informations existantes
- ✅ **Supprimer un médicament** : Suppression avec confirmation
- ✅ **Rechercher un médicament** : Recherche en temps réel par nom
- ✅ **Afficher la liste complète** : Tableau lisible avec toutes les informations

### Interface utilisateur moderne
- 🎨 Interface graphique élégante avec Shadcn/UI
- 🔍 Recherche instantanée avec mise en évidence
- 📱 Design responsive (mobile, tablette, desktop)
- 🌙 Support du mode sombre
- ⚡ Chargement rapide avec indicateurs de progression
- ✨ Animations fluides et transitions

### Fonctionnalités avancées
- 🚨 Alertes visuelles pour les stocks faibles (< 20 unités)
- ⚠️ Mise en évidence des stocks épuisés (quantité = 0)
- 📅 Format de date français (JJ/MM/AAAA)
- 💶 Format de prix en euros (€)
- ✔️ Validation des formulaires en temps réel
- 🔄 Actualisation automatique après chaque opération

## 🛠 Technologies utilisées

- **Framework** : Next.js 15 (App Router)
- **Langage** : TypeScript
- **Base de données** : Turso (SQLite distribué)
- **ORM** : Drizzle ORM
- **UI Components** : Shadcn/UI + Radix UI
- **Styling** : Tailwind CSS v4
- **Forms** : React Hook Form
- **Icons** : Lucide React
- **Runtime** : Bun

## 🗃 Structure de la base de données

### Table : `medicaments`

```sql
CREATE TABLE medicaments (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  nom TEXT NOT NULL,
  prix REAL NOT NULL,
  quantite INTEGER NOT NULL,
  date_expiration TEXT NOT NULL,
  created_at TEXT NOT NULL,
  updated_at TEXT NOT NULL
);
```

### Schéma Drizzle (TypeScript)

```typescript
export const medicaments = sqliteTable('medicaments', {
  id: integer('id').primaryKey({ autoIncrement: true }),
  nom: text('nom').notNull(),
  prix: real('prix').notNull(),
  quantite: integer('quantite').notNull(),
  dateExpiration: text('date_expiration').notNull(),
  createdAt: text('created_at').notNull(),
  updatedAt: text('updated_at').notNull(),
});
```

### Colonnes

| Colonne | Type | Description | Contraintes |
|---------|------|-------------|-------------|
| `id` | INTEGER | Identifiant unique | PRIMARY KEY, AUTO_INCREMENT |
| `nom` | TEXT | Nom du médicament | NOT NULL, min 2 caractères |
| `prix` | REAL | Prix en euros | NOT NULL, > 0 |
| `quantite` | INTEGER | Quantité en stock | NOT NULL, >= 0 |
| `date_expiration` | TEXT | Date d'expiration (ISO 8601) | NOT NULL, format DATE |
| `created_at` | TEXT | Date de création | NOT NULL, ISO timestamp |
| `updated_at` | TEXT | Date de modification | NOT NULL, ISO timestamp |

### Données de test (Seeder)

Le projet inclut 5 médicaments de test :

1. **Paracétamol** - 5,50€ - Quantité : 100 - Expiration : 31/12/2025
2. **Ibuprofène** - 7,80€ - Quantité : 75 - Expiration : 15/10/2025
3. **Aspirine** - 4,20€ - Quantité : 120 - Expiration : 20/03/2026
4. **Amoxicilline** - 12,50€ - Quantité : 50 - Expiration : 30/08/2025
5. **Doliprane** - 6,00€ - Quantité : 90 - Expiration : 10/01/2026

## 📦 Installation

### Prérequis

- **Bun** >= 1.0.0 (ou Node.js >= 18.0.0)
- Un compte **Turso** (gratuit) : [https://turso.tech](https://turso.tech)

### Étapes d'installation

1. **Cloner le projet**
```bash
git clone <url-du-repo>
cd pharmacy-management
```

2. **Installer les dépendances**
```bash
bun install
# ou
npm install
```

3. **Configuration de la base de données**

Le fichier `.env` est déjà configuré avec une base de données Turso prête à l'emploi.

4. **Lancer le serveur de développement**
```bash
bun dev
# ou
npm run dev
```

5. **Ouvrir l'application**

Accédez à [http://localhost:3000](http://localhost:3000) dans votre navigateur.

## 💻 Utilisation

### Menu principal

L'interface principale présente un tableau avec les colonnes suivantes :
- **ID** : Identifiant unique
- **Nom** : Nom du médicament
- **Prix** : Prix en euros
- **Quantité** : Stock disponible (avec alertes visuelles)
- **Date d'expiration** : Date limite d'utilisation
- **Actions** : Boutons modifier et supprimer

### 1. Ajouter un médicament

1. Cliquez sur le bouton **"Ajouter un médicament"**
2. Remplissez le formulaire :
   - **Nom** : Minimum 2 caractères
   - **Prix** : Valeur positive supérieure à 0
   - **Quantité** : Nombre entier positif ou nul
   - **Date d'expiration** : Sélectionnez une date
3. Cliquez sur **"Ajouter"**

### 2. Rechercher un médicament

- Utilisez la barre de recherche en haut du tableau
- La recherche est instantanée et insensible à la casse
- Les résultats se filtrent automatiquement

### 3. Modifier un médicament

1. Cliquez sur l'icône **crayon (✏️)** dans la colonne Actions
2. Modifiez les champs souhaités
3. Cliquez sur **"Mettre à jour"**

### 4. Supprimer un médicament

1. Cliquez sur l'icône **corbeille (🗑️)** dans la colonne Actions
2. Confirmez la suppression dans la boîte de dialogue
3. Le médicament est supprimé définitivement

## 🔌 API Endpoints

### Base URL : `/api/medicaments`

#### 📝 GET - Lister tous les médicaments
```http
GET /api/medicaments
```

**Réponse (200)** :
```json
[
  {
    "id": 1,
    "nom": "Paracétamol",
    "prix": 5.5,
    "quantite": 100,
    "dateExpiration": "2025-12-31",
    "createdAt": "2024-01-07T10:30:00.000Z",
    "updatedAt": "2024-01-07T10:30:00.000Z"
  }
]
```

#### 🔍 GET - Rechercher par nom
```http
GET /api/medicaments?search=para
```

#### 📄 GET - Récupérer un médicament par ID
```http
GET /api/medicaments?id=1
```

#### ➕ POST - Créer un médicament
```http
POST /api/medicaments
Content-Type: application/json

{
  "nom": "Vitamine C",
  "prix": 8.5,
  "quantite": 200,
  "dateExpiration": "2025-06-30"
}
```

**Réponse (201)** :
```json
{
  "id": 6,
  "nom": "Vitamine C",
  "prix": 8.5,
  "quantite": 200,
  "dateExpiration": "2025-06-30",
  "createdAt": "2024-01-07T11:00:00.000Z",
  "updatedAt": "2024-01-07T11:00:00.000Z"
}
```

#### ✏️ PUT - Modifier un médicament
```http
PUT /api/medicaments/1
Content-Type: application/json

{
  "prix": 6.0,
  "quantite": 95
}
```

#### ❌ DELETE - Supprimer un médicament
```http
DELETE /api/medicaments/1
```

**Réponse (200)** :
```json
{
  "message": "Medication deleted successfully",
  "medication": { ... }
}
```

### Codes de réponse

| Code | Description |
|------|-------------|
| 200 | Succès |
| 201 | Création réussie |
| 400 | Erreur de validation |
| 404 | Médicament non trouvé |
| 500 | Erreur serveur |

## 🧪 Tests

### Tests manuels à effectuer

#### ✅ Test 1 : Ajout d'un médicament

**Objectif** : Vérifier qu'un médicament ajouté apparaît dans la base

**Étapes** :
1. Ouvrir l'application
2. Cliquer sur "Ajouter un médicament"
3. Remplir le formulaire :
   - Nom : "Test Médicament"
   - Prix : 10.50
   - Quantité : 50
   - Date : 2025-12-31
4. Cliquer sur "Ajouter"

**Résultat attendu** :
- ✅ Le médicament apparaît dans le tableau
- ✅ Le tableau se rafraîchit automatiquement

#### ✅ Test 2 : Suppression d'un médicament

**Objectif** : Vérifier que la suppression retire bien le médicament

**Étapes** :
1. Repérer un médicament dans le tableau
2. Cliquer sur l'icône corbeille
3. Confirmer la suppression

**Résultat attendu** :
- ✅ Le médicament disparaît du tableau
- ✅ La liste se met à jour automatiquement

#### ✅ Test 3 : Recherche par nom

**Objectif** : Vérifier que la recherche retourne le bon résultat

**Étapes** :
1. Dans la barre de recherche, taper "para"
2. Observer les résultats filtrés

**Résultat attendu** :
- ✅ Seul "Paracétamol" est affiché
- ✅ La recherche est insensible à la casse

#### ✅ Test 4 : Modification d'un médicament

**Objectif** : Vérifier que les modifications sont enregistrées

**Étapes** :
1. Cliquer sur l'icône crayon d'un médicament
2. Modifier le prix (par exemple : 7.99)
3. Cliquer sur "Mettre à jour"

**Résultat attendu** :
- ✅ Le nouveau prix s'affiche dans le tableau
- ✅ Les autres champs restent inchangés

#### ✅ Test 5 : Validation du formulaire

**Objectif** : Vérifier que les validations fonctionnent

**Étapes** :
1. Ouvrir le formulaire d'ajout
2. Essayer de soumettre avec des champs vides
3. Essayer un prix négatif

**Résultat attendu** :
- ✅ Messages d'erreur appropriés sous chaque champ
- ✅ Le formulaire ne se soumet pas si invalide

### Tests API avec curl

```bash
# Test GET - Lister tous
curl http://localhost:3000/api/medicaments

# Test GET - Rechercher
curl "http://localhost:3000/api/medicaments?search=para"

# Test POST - Créer
curl -X POST http://localhost:3000/api/medicaments \
  -H "Content-Type: application/json" \
  -d '{"nom":"Test","prix":5.5,"quantite":10,"dateExpiration":"2025-12-31"}'

# Test PUT - Modifier
curl -X PUT http://localhost:3000/api/medicaments/1 \
  -H "Content-Type: application/json" \
  -d '{"prix":6.5}'

# Test DELETE - Supprimer
curl -X DELETE http://localhost:3000/api/medicaments/1
```

## 📁 Structure du projet

```
pharmacy-management/
├── src/
│   ├── app/
│   │   ├── api/
│   │   │   └── medicaments/
│   │   │       ├── route.ts          # API GET, POST, PUT, DELETE
│   │   │       └── [id]/
│   │   │           └── route.ts      # API PUT, DELETE par ID
│   │   ├── globals.css               # Styles globaux
│   │   ├── layout.tsx                # Layout principal
│   │   └── page.tsx                  # Page d'accueil
│   ├── components/
│   │   ├── ui/                       # Composants Shadcn/UI
│   │   ├── MedicationDialog.tsx      # Dialogue ajout/modification
│   │   └── PharmacyDashboard.tsx     # Dashboard principal
│   ├── db/
│   │   ├── index.ts                  # Configuration DB
│   │   ├── schema.ts                 # Schéma Drizzle
│   │   └── seeds/
│   │       └── medicaments.ts        # Données de test
│   └── lib/
│       └── utils.ts                  # Fonctions utilitaires
├── .env                              # Variables d'environnement
├── drizzle.config.ts                 # Configuration Drizzle
├── package.json                      # Dépendances
└── README.md                         # Ce fichier
```

## 🎯 Fonctionnalités implémentées

- [x] Base de données SQLite avec Turso
- [x] Schéma de table `medicaments` complet
- [x] API REST complète (CRUD)
- [x] Interface utilisateur moderne
- [x] Formulaire d'ajout avec validation
- [x] Formulaire de modification
- [x] Suppression avec confirmation
- [x] Recherche en temps réel
- [x] Affichage en tableau lisible
- [x] Format de date français
- [x] Format de prix en euros
- [x] Alertes visuelles (stocks faibles)
- [x] Mode responsive
- [x] Mode sombre
- [x] Loading states
- [x] Error handling
- [x] Données de test (seeder)
- [x] Documentation complète

## 🚀 Déploiement

Le projet peut être déployé sur :
- **Vercel** (recommandé pour Next.js)
- **Netlify**
- **Railway**
- **Fly.io**

La base de données Turso est déjà hébergée dans le cloud et accessible depuis n'importe où.

## 📝 Notes importantes

- La base de données utilise **Turso** (SQLite distribué dans le cloud)
- Les données sont **persistantes** et partagées
- Le projet est **prêt à l'emploi** sans configuration supplémentaire
- Toutes les opérations incluent une **validation côté serveur**
- L'interface est **traduite en français**

## 👨‍💻 Auteur

Développé comme mini-projet de gestion de base de données avec Next.js et TypeScript.

---

**Bon développement ! 🎉**#   S I I  
 