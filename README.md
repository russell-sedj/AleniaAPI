# Alenia API — ASP.NET Core 8

API REST de la plateforme **Alenia**, une application de mise en relation entre établissements de santé et professionnels intérimaires. Le projet couvre l'authentification, les utilisateurs, les missions, les candidatures, les certifications et les évaluations.

## Stack

- **Framework** : ASP.NET Core 8 (Web API)
- **ORM** : Entity Framework Core 8
- **Base de données** : MySQL 8.2 (Pomelo.EntityFrameworkCore.MySql)
- **Auth** : JWT Bearer, BCrypt
- **Documentation** : Swagger / OpenAPI
- **CORS** : frontend Angular sur `http://localhost:4200`

## Rôles

| Rôle | Accès |
|---|---|
| `admin` | Administration complète |
| `etablissement` | Gestion de l'établissement, des missions et des candidatures |
| `interimaire` | Profil, missions, candidatures, certifications |

## Fonctionnalités

- Inscription et connexion avec JWT
- Gestion des utilisateurs, intérimaires et établissements
- Gestion des missions
- Candidatures aux missions
- Certifications des intérimaires
- Évaluations
- Administration

## Endpoints principaux

Chaque contrôleur expose un CRUD REST classique (`GET`, `GET /{id}`, `POST`, `PUT /{id}`, `DELETE /{id}`) :

| Contrôleur | Route |
|---|---|
| Auth | `/api/auth` (`POST /register`, `POST /login`) |
| Admin | `/api/admin` |
| Utilisateur | `/api/utilisateur` |
| Interimaire | `/api/interimaire` |
| Etablissement | `/api/etablissement` |
| Mission | `/api/mission` |
| Candidature | `/api/candidature` |
| Certification | `/api/certification` |
| Evaluation | `/api/evaluation` |

La documentation Swagger est disponible en développement sur `/swagger`.

## Lancer le projet

```bash
dotnet restore
```

Configurer la base MySQL dans `appsettings.json` (ou via `ConnectionStrings__DefaultConnection`) :

```json
"ConnectionStrings": {
  "DefaultConnection": "Server=localhost;Database=alenia_db;User=root;Password=;"
}
```

Configurer la clé JWT **hors du dépôt** :

```bash
dotnet user-secrets init
dotnet user-secrets set "Jwt:Key" "votre-cle-locale"
```

Ou avec une variable d'environnement :

```bash
$env:Jwt__Key = "votre-cle-locale"
```

Appliquer les migrations puis lancer l'API :

```bash
dotnet ef database update
dotnet run
```

L'API tourne sur `https://localhost:7134` et Swagger est accessible en développement.

## Sécurité

- La clé `Jwt:Key` ne doit **jamais** être commitée. Utiliser `dotnet user-secrets` ou la variable d'environnement `Jwt__Key`.
- `appsettings.Example.json` fournit un modèle de configuration sans secret.
- Les mots de passe sont hachés avec BCrypt.
- Les rôles sont vérifiés via des policies (`AdminOnly`, `EtablissementOnly`, `InterimaireOnly`).
- CORS est restreint au frontend Angular local.

## Structure

```text
Controllers/       # Auth, Admin, Utilisateur, Interimaire, Etablissement,
                   # Mission, Candidature, Certification, Evaluation
Models/            # Entités EF Core
Data/              # DbContext
Migrations/        # Migrations EF Core
Program.cs         # Configuration, JWT, CORS, Swagger
appsettings.json   # Configuration locale (sans secret)
appsettings.Example.json
```

## Auteur

Russell Dossou-Yovo — [GitHub](https://github.com/russell-sedj)
