# EKS Multi-Team Bootstrap with ArgoCD, KRO & Gateway API

Laboratory simulating multiple teams working on a shared EKS cluster, each with their own bootstrap and application repositories.

## 🎯 What This Lab Demonstrates

- **Multi-Tenancy**: Multiple teams deploying on a shared cluster
- **Per-Team Bootstrap**: Each team manages their own ArgoCD bootstrap
- **GitOps with ArgoCD**: Declarative infrastructure management per team
- **KRO (Kubernetes Resource Operator)**: Standardized application deployments
- **Gateway API**: Gateway shared across Team 1 applications
- **Multi-Repository Simulation**: Each team has their own independent repositories

## 🏗️ Multi-Team Architecture

```
                    ┌─────────────────────────────────────┐
                    │      Shared EKS Cluster             │
                    │                                     │
                    │  ┌───────────────────────────────┐  │
                    │  │   Controllers                 │  │
                    │  │   (ALB, KRO, Gateway API)     │  │
                    │  └───────────────────────────────┘  │
                    └──────────────┬──────────────────────┘
                                   │
        ┌──────────────────────────┼──────────────────────────┐
        │                          │                          │
┌───────▼────────┐        ┌────────▼────────┐       ┌────────▼────────┐
│  Team DevOps   │        │    Team 1       │       │    Team 2       │
│                │        │  + Gateway      │       │                 │
│                │        │                 │       │                 │
│ Repo:          │        │ Repos:          │       │ Repos:          │
│ bootstrap-k8s/ │        │ bootstrap-k8s/  │       │ bootstrap-k8s/  │
│  └─ ArgoCD +   │        │  └─ Shared      │       │  └─ Shared      │
│     Bootstrap  │        │     manifest    │       │     manifest    │
│     Controllers│        │     files       │       │     files       │
│                │        │                 │       │                 │
│                │        │ mondamail/      │       │ mondasample/    │
│                │        │  └─ src/        │       │  └─ src/        │
│                │        │  └─ manifest    │       │  └─ manifest    │
│                │        │     files       │       │     files       │
│                │        │                 │       │                 │
│                │        │ mondareader/    │       │                 │
│                │        │  └─ src/        │       │                 │
│                │        │  └─ manifest    │       │                 │
│                │        │     files       │       │                 │
└────────────────┘        └─────────────────┘       └─────────────────┘

  Namespaces:             Namespace: team-1         Namespace: team-2
  - argocd
  - gateway-system
```

## 📂 Project Structure

```
k8s/
└── repos/                          # Simulates multiple repositories
    ├── team-devops/
    │   └── bootstrap-k8s/          # Shared infrastructure bootstrap
    │       ├── controllers/        # ALB, EBS, EFS, KRO
    │       └── applications.yaml   # App of Apps
    │
    ├── team-1/
    │   ├── bootstrap-k8s/          # Team 1 bootstrap
    │   │   ├── gateway/            # Team 1 Gateway
    │   │   └── applications.yaml   # Team 1 apps
    │   ├── mondamail/              # Application 1
    │   │   ├── src/
    │   │   ├── Dockerfile
    │   │   └── application.yaml    # Manifest files
    │   └── mondareader/            # Application 2
    │       ├── src/
    │       └── application.yaml
    │
    └── team-2/
        ├── bootstrap-k8s/          # Team 2 bootstrap
        │   └── applications.yaml   # Team 2 apps
        └── mondasample/            # Application 1
            ├── src/
            └── application.yaml
```
