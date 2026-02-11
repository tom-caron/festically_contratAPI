# Festically API Contract

## Versionnement
L’API utilise un versionnement par URL :
`/api/v1/...`

## Format des réponses

### Succès
{
  "status": "success",
  "data": {},
  "meta": {}
}

### Erreur
{
  "status": "error",
  "code": "ERROR_CODE",
  "message": "Message explicatif",
  "details": []
}

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
