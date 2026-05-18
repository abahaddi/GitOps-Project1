# 🗳️ Voting App - Microservices (Kubernetes + GitOps)

## 📌 Overview

This project is a **distributed voting application** built using a microservices architecture deployed on Kubernetes and managed via GitOps with Argo CD.

It allows users to vote (Cats vs Dogs), processes votes asynchronously, and displays real-time results.

---

## 🏗️ Architecture Overview

### 🔄 System Flow
<img width="1536" height="1024" alt="argocd" src="https://github.com/user-attachments/assets/5d935296-d379-4057-9193-15a9bf2242c1" />

---

## 🧩 Microservices

### 🗳️ Vote Service
- Image: `dockersamples/examplevotingapp_vote`
- Type: NodePort
- Port: `31000`
- Role:
  - User interface for voting
  - Sends votes to Redis

---

### 📊 Result Service
- Image: `dockersamples/examplevotingapp_result`
- Type: NodePort
- Port: `31001`
- Role:
  - Displays real-time results
  - Reads data from PostgreSQL

---

### ⚙️ Worker Service
- Image: `dockersamples/examplevotingapp_worker`
- Role:
  - Consumes votes from Redis
  - Processes data
  - Writes results to PostgreSQL

---

### 🔴 Redis (Message Queue)
- Image: `redis:alpine`
- Port: `6379`
- Role:
  - Temporary vote storage
  - Message queue between vote and worker

---

### 🗄️ PostgreSQL Database
- Image: `postgres:15-alpine`
- Port: `5432`
- Role:
  - Persistent storage of results


---

## 🌐 Kubernetes Services

| Service  | Type       | Port Mapping |
|----------|-----------|--------------|
| vote     | NodePort  | 8080 → 31000 |
| result   | NodePort  | 8081 → 31001 |
| redis    | ClusterIP | 6379         |
| db       | ClusterIP | 5432         |

---

## ⚙️ Architecture Style

- Microservices architecture
- Event-driven system
- Asynchronous processing (Redis queue)
- Stateless frontend services

---

## 🔁 Communication Flow

| From   | To        | Type     |
|--------|----------|----------|
| Vote   | Redis    | Write    |
| Worker | Redis    | Read     |
| Worker | DB       | Write    |
| Result | DB       | Read     |

---

## ☸️ Kubernetes Design

- Deployments for all services
- Services for internal/external access
- NodePort for frontend exposure
- ClusterIP for internal communication



