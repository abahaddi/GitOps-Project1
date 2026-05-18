📄 README — Voting App Microservices (GitOps / Kubernetes)
🧠 Vue d’ensemble

Cette application est une voting app distribuée basée sur une architecture microservices orchestrée avec Kubernetes et déployée en mode GitOps via Argo CD.

Elle permet aux utilisateurs de voter (Cats vs Dogs), de traiter les votes en temps réel, et d’afficher les résultats.

🏗️ Architecture globale
🔄 Flux applicatif
User
  │
  ▼
Vote UI (NodePort 31000)
  │
  ▼
Redis (queue temporaire)
  │
  ▼
Worker (processing engine)
  │
  ▼
PostgreSQL (db)
  │
  ▼
Result UI (NodePort 31001)
🧩 Microservices
🗳️ Vote Service
Image : dockersamples/examplevotingapp_vote
Port : 31000 (NodePort)
Rôle :
Interface utilisateur
Envoi des votes vers Redis
📊 Result Service
Image : dockersamples/examplevotingapp_result
Port : 31001 (NodePort)
Rôle :
Affichage des résultats
Lecture depuis PostgreSQL
⚙️ Worker Service
Image : dockersamples/examplevotingapp_worker
Rôle :
Consomme les votes depuis Redis
Traite les messages
Stocke les résultats dans PostgreSQL
🔴 Redis (cache / queue)
Image : redis:alpine
Port : 6379
Rôle :
Queue temporaire des votes
Buffer entre vote et worker
🗄️ Database (PostgreSQL)
Image : postgres:15-alpine
Port : 5432
Rôle :
Stockage persistant des résultats
Note :
utilise emptyDir → non persistant (dev only)
🌐 Services Kubernetes
Vote Service
Type : NodePort
Port : 8080 → 31000
Result Service
Type : NodePort
Port : 8081 → 31001
Redis & DB
Type : ClusterIP (interne cluster uniquement)
⚙️ Architecture technique
🧠 Pattern utilisé
Event-driven architecture
Queue-based processing (Redis)
Microservices découplés
Stateless frontend services
🔁 Communication interne
Source	Destination	Type
Vote	Redis	write
Worker	Redis	read
Worker	DB	write
Result	DB	read
☸️ Kubernetes Design
Deployments pour chaque microservice
Services pour exposition réseau
NodePort pour accès externe
ClusterIP pour services internes
🚀 GitOps avec ArgoCD

Le déploiement est automatisé via Argo CD.

Processus :
GitHub Repo
   ↓
ArgoCD détecte changements
   ↓
Sync Kubernetes
   ↓
Deploy automatique des services
