```mermaid
flowchart TB
  subgraph Azure
    API[FastAPI API]
    DB[(PostgreSQL DB)]
    BLOB[(Azure Blob Storage)]
    AI["AI Services & Model Deployment"]
  end

  Dev[Developers GitHub/GitLab] -->|CI/CD| API
  API --> DB
  API --> BLOB
  API --> AI
```
