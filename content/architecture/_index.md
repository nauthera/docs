---
title: "Architecture"
type: "docs"
weight: 2
---

This document outlines the architecture of the Nauthera platform. The platform is designed to be a cloud-native, scalable, and secure authentication solution that runs on Kubernetes.

```mermaid
graph TD
    subgraph sg_cluster ["Kubernetes Cluster"]
        subgraph sg_nauthera ["Nauthera Platform"]
            Operator[Nauthera Operator]
            AuthServer[Authentication Server]
        end

        subgraph sg_prereq ["Prerequisites (User Provided)"]
            Ingress[Ingress / Gateway]
            CertManager[cert-manager]
            PostgreSQL[PostgreSQL]
        end

        subgraph sg_optional ["Optional Components"]
            Redis[Redis / Valkey]
        end

        CRDs(CRDs)
    end

    subgraph sg_external ["External Systems"]
        LDAP[LDAP Server]
        Admin[Admin]
        EndUser[End User]
    end

    Admin -- "kubectl apply" --> CRDs
    EndUser -- "HTTPS" --> Ingress
    Ingress -- "Forwards traffic" --> AuthServer
    CRDs -- "Defines desired state" --> Operator
    Operator -- "Manages" --> AuthServer
    Operator -- "gRPC (mTLS)" --> AuthServer
    CertManager -- "Issues Certificates" --> Operator
    CertManager -- "Issues Certificates" --> AuthServer
    CertManager -- "Issues Certificates" --> Ingress
    AuthServer -- "Stores data (2FA, consents)" --> PostgreSQL
    AuthServer -- "Caches tokens/claims (optional)" --> Redis
    AuthServer -- "User/Group federation (optional)" --> LDAP

    style sg_nauthera fill:#cde4ff,stroke:#333,stroke-width:2px
    style sg_prereq fill:#d4edda,stroke:#333,stroke-width:2px
    style sg_optional fill:#fff3cd,stroke:#333,stroke-width:2px
    style sg_external fill:#f8d7da,stroke:#333,stroke-width:2px
```

This section provides a detailed look into the different aspects of the platform's architecture:

*   **[Installation](./installation/)**: Describes the different deployment models.
*   **[Components](./components/)**: Details the core components of the platform.
*   **[Custom Resources](./crds/)**: Explains how to manage the platform declaratively.
*   **[Observability](./observability/)**: Covers metrics, logging, and tracing.
