# GitOps Platform on AWS EKS

Production-grade GitOps platform built with Terraform, EKS, ArgoCD, Prometheus & Grafana.

## Project Highlights
- 3-node EKS cluster provisioned with Terraform (S3 backend + DynamoDB locking)
- GitOps delivery using ArgoCD with auto-sync, drift detection & self-healing
- Prometheus + Grafana stack with custom dashboards and 15+ alerting rules
- 3 defined SLOs with real-time error budget tracking
- Team namespaces & least-privilege RBAC

## Tech Stack
- AWS EKS • Terraform • ArgoCD • Helm • Prometheus • Grafana • GitHub Actions

## Architecture
## Architecture Diagram

```mermaid
flowchart TD
    %% Git Layer
    GitHub[GitHub Repository] 
    Terraform[Terraform IaC\nEKS Cluster + State]
    ArgoManifests[ArgoCD Applications\n+ Helm Charts]
    Dashboards[Grafana Dashboards + Alerts]

    %% AWS Layer
    AWS[AWS Cloud]
    S3[(S3 Remote State)]
    Dynamo[(DynamoDB Lock)]

    subgraph EKSCluster ["AWS EKS Cluster (3 Nodes)"]
        Control[EKS Control Plane]
        
        subgraph RBAC ["Namespaces & RBAC"]
            TeamAlpha[team-alpha\n+ Least-Privilege SA]
            TeamBeta[team-beta\n+ Least-Privilege SA]
        end

        subgraph GitOpsLayer ["GitOps - ArgoCD"]
            ArgoCD[ArgoCD Server]
        end

        subgraph Observability ["Monitoring & Observability"]
            Prometheus[Prometheus]
            Grafana[Grafana\n+ SLO Dashboards]
        end

        Workloads[Sample Workloads\n+ Future Apps]
    end

    GitHub --> Terraform
    GitHub --> ArgoManifests
    GitHub --> Dashboards

    Terraform --> S3
    Terraform --> Dynamo
    Terraform --> EKSCluster

    ArgoManifests --> ArgoCD
    ArgoCD -->|Auto Sync + Self-Heal| RBAC
    ArgoCD -->|Auto Sync + Self-Heal| Workloads
    ArgoCD -->|Auto Sync + Self-Heal| Observability

    Prometheus -->|Scrapes Metrics| Grafana
    Grafana -->|Visualizations + Alerts| User[Developers / Platform Team]

    style ArgoCD fill:#22c55e,stroke:#166534,color:white
    style Grafana fill:#a855f7,stroke:#6b21a8,color:white
    style Prometheus fill:#f59e0b,stroke:#b45309,color:white


## Quick Start

## Screenshots

## SLOs Implemented
- Availability: 99.9%
- Latency p99: < 500ms
- Error Rate: < 0.5%
