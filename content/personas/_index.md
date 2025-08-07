---
title: "Personas"
type: "docs"
weight: 3
---

Nauthera is designed to meet the unique needs of different teams involved in the software development lifecycle. By providing a clear separation of concerns and a declarative, GitOps-native workflow, Nauthera empowers each team to work more effectively and securely.

```mermaid
graph TD
    subgraph sg_dev ["Developer"]
        direction LR
        A[AuthClient]
    end

    subgraph sg_sec ["Security Team"]
        direction LR
        B[AuthPolicy]
        C[UserStore]
    end

    subgraph sg_plat ["Platform Team"]
        direction LR
        D[AuthServer]
        E[Operator Deployment]
    end

    A -- "requests" --> D
    D -- "governed by" --> B
    D -- "connects to" --> C

    style sg_dev fill:#cde4ff,stroke:#333,stroke-width:2px
    style sg_sec fill:#d4edda,stroke:#333,stroke-width:2px
    style sg_plat fill:#f8d7da,stroke:#333,stroke-width:2px
```

This section explores the specific benefits and workflows for each role:

*   **[For Developers](./developers/)**: Learn how Nauthera enables developer self-service and reduces friction.
*   **[For Security Teams](./security/)**: Discover how to enforce robust, consistent security policies across your organization.
*   **[For Platform Teams](./platform-teams/)**: See how Nauthera simplifies the management of a scalable, cloud-native authentication platform.
