# Festically API Contract

## Versionnement
L’API utilise un versionnement par URL :
`/api/v1/...`

## Format des réponses

### Succès
```json
{
  "status": "success",
  "data": {},
  "meta": {}
}
```

### Erreur
```json
{
  "status": "error",
  "code": "ERROR_CODE",
  "message": "Message explicatif",
  "details": []
}
```

## Conventions

- Endpoints : kebab-case
- JSON : camelCase
- Dates : ISO 8601
- Pagination : page + limit
- Authentification : JWT Bearer Token

## Codes d’erreurs

- VALIDATION_ERROR
- EVENT_NOT_FOUND
- NOT_ENOUGH_CAPACITY
- UNAUTHORIZED
- FORBIDDEN

## Questionnaire

### Ressources & Hiérarchie
Ressources principales

`/events`

`/reservations`

`/auth`

`/users (via /auth/me et relations indirectes)`

Relations métier

- Un User peut avoir plusieurs Reservations

- Un Event peut avoir plusieurs Reservations

- Une Reservation appartient à :

    - 1 User

    - 1 Event

Hiérarchie REST retenue

```
/events
/events/{id}

/reservations

/auth/register
/auth/login
/auth/me
```

### Opérations & Codes HTTP
**Events**

| Opération | Méthode | Code |
|:-------- |:--------:| --------:|
| Liste     | GET /events   | 200    |
| Détail     | GET /events/{id}   | 200    |
| Création     | POST /events   | 201    |
| Modification     | PATCH /events/{id}   | 200    |
| Suppression     | DELETE /events/{id}   | 204    |

**Reservations**

| Opération | Méthode | Code |
|:-------- |:--------:| --------:|
| Création     | POST /reservations   | 201    |

**Authentification**

| Opération | Méthode | Code |
|:-------- |:--------:| --------:|
| Inscription     | POST /auth/register   | 201    |
| Login     | POST /auth/login   | 200    |
| Création     | GET /auth/me   | 202    |

**Codes d’erreurs utilisés**

400 → Validation error

401 → Non authentifié

403 → Non autorisé

404 → Ressource inexistante

409 → Conflit métier (ex: capacité insuffisante)

500 → Erreur serveur

### Format de Réponse Standard

Toutes les réponses suivent ce format :

**Succès :**
```json
{
  "status": "success",
  "data": {},
  "meta": {}
}
```

**Erreur :**
```json
{
  "status": "error",
  "code": "ERROR_CODE",
  "message": "Description claire",
  "details": []
}
```

Ce format est défini dans components/schemas de l’OpenAPI.

### Pagination

Stratégie choisie : **page + limit**

Exemple :
```
GET /events?page=1&limit=10
```

Réponse :
```json
{
  "status": "success",
  "data": [...],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 45,
    "totalPages": 5
  }
}
```

### Filtrage & Tri

Paramètres disponibles sur /events :

| Paramètre | Type | Description |
|:-------- |:--------| :--------|
| page     | integer   | Nombre résultats    |
| sort     | string   | Ex: -date    |
| dateFrom | date  | Filtre date min    |
| dateTo | date  | Filtre date max    |
| location | string  | Filtre lieu    |

### Gestion des Erreurs

Format standard :
```json
{
  "status": "error",
  "code": "NOT_ENOUGH_CAPACITY",
  "message": "Plus assez de places disponibles",
  "details": []
}
```

**Catalogue défini**

- VALIDATION_ERROR

- EVENT_NOT_FOUND

- NOT_ENOUGH_CAPACITY

- INVALID_CREDENTIALS

- UNAUTHORIZED

- FORBIDDEN

Fichier dédié :

`errors/error-catalog.json`


### Conventions de Nommage

- Endpoints → kebab-case

- Collections → pluriel

- JSON → camelCase

- Dates → ISO 8601 obligatoire

- Versionnement → /api/v1/...


### Versionnement

Stratégie choisie : versionnement par URL

`/api/v1/events`

Avantages :

- Clair

- Standard REST

- Compatible Swagger

- Facile à maintenir

Les changements incompatibles créeront :

`/api/v2/`

### Composants Réutilisables

Dans `components` :

`schemas` :

- Event

- EventInput

- Reservation

- ReservationInput

- User

- AuthResponse

- ErrorResponse

`parameters` :

- PageParam

- LimitParam

- SortParam

- IdParam

`responses` :

- ValidationError

- NotFound

- CapacityError

Cela évite la duplication et facilite la maintenance.

### Exemples Complets

Deux ressources principales entièrement spécifiées :

1️⃣ **Events**

- GET liste (pagination + filtres)

- GET détail

- POST création

- PATCH modification

- DELETE suppression

- Exemples request + response fournis

2️⃣ **Réservations**

- POST réservation

- Gestion capacité (409)

- Exemple succès

- Exemple erreur