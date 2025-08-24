# Choix techniques – TurboDex

Ce document permet de présenter les choix techniques retenus pour la mise en place de TurboDex, en cohérence avec l’architecture et les objectifs du projet.

---

## 1. Infrastructure Cloud (Azure)

- **Azure Blob Storage** : stockage des images capturées par les utilisateurs.  
- **Azure Functions (optionnel)** : exécution de requêtes entre l'API et les différents services.
- **Cosmos DB (option futur)** : base NoSQL hautement scalable, envisagée pour des fonctionnalités temps réel et globales.  
- Pouquoi Azure : Nous disposons de crédits étudiants, et il offre une intégration simple avec nos besoins (storage + containers + DB).

---

## 2. Backend API – FastAPI

- **Framework choisi** : [FastAPI](https://fastapi.tiangolo.com/).  
- Rôles :
  - Fournir une API REST rapide/typée pour l’app et la landing page.  
  - Gérer l’authentification, les fiches véhicules et le Pokédex utilisateur.  
- Justification : FastAPI est moderne, asynchrone, supporte bien la documentation auto et s’intègre facilement avec PostgreSQL et Redis.

---

## 3. Traitements asynchrones – Redis + Celery

- **Redis** : utilisé comme broker et cache.  
- **Celery** : exécution de tâches longues (pré-traitement images, floutage, mise à jour DB).  
- Justification : permet de ne pas bloquer les appels API et d’assurer une meilleure réactivité côté utilisateur.

---

## 4. Détection & Vision – YOLOv8

- **YOLOv8** : utilisé pour la détection et le floutage automatique des plaques d’immatriculation et visages.  
- Complémentaire au modèle de classification (Azure Custom Vision ou ONNX).  
- Justification : YOLOv8 est un standard open-source performant et facile à intégrer avec PyTorch/ONNX Runtime.

---

## 5. Conteneurisation & Déploiement

- **Full containerization** : chaque composant (API, worker Celery, modèle ML) est packagé dans un conteneur Docker.  
- **Azure Container Apps** : hébergement scalable des conteneurs (alternative : Web App for Containers).  
- Justification : conteneurisation = portabilité, facilité de CI/CD, déploiement homogène. Azure Container Apps agit comme un service managé sans overhead de gestion de VM.

---

## 6. Base de données relationnelle

- **PostgreSQL** : utilisé pour stocker les informations suivantes :
  - Utilisateurs & authentification.  
  - Fiches véhicules (caractéristiques, images associées).  
  - Pokédex personnel (collections, favoris).  
- Justification : robuste, open-source, supporte SQL standard et migrations.

---

## 7. Résumé des choix

| Domaine              | Choix retenu                       | Justification principale |
|----------------------|------------------------------------|--------------------------|
| Stockage             | Azure Blob Storage                 | Intégration + coûts réduits |
| API                  | FastAPI (Python)                   | Rapidité + doc auto + async |
| Tâches asynchrones   | Redis + Celery                     | Découplage traitements lourds |
| Vision IA            | YOLOv8 (+ Custom Vision/ONNX)      | Détection plaques/visages |
| Conteneurisation     | Docker                             | Portabilité + CI/CD |
| Hébergement          | Azure Container Apps               | Managé + scalable |
| Base de données      | PostgreSQL                         | Standard SQL + scalable |

---
