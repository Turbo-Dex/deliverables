```mermaid
  erDiagram
  USER ||--o{ PHOTO : "capture"
  PHOTO ||--|| VEHICLE : "classify"
  VEHICLE {
    string marque
    string modele
    int annee
  }
  USER {
    int id
    string email
    string passwordHash
  }
  PHOTO {
    int id
    string url
    datetime timestamp
  }
```
