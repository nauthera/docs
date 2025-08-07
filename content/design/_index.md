---
title: "Design Philosophy"
type: "docs"
weight: 3
---

Nauthera is built on a lean, opinionated, and security-first design philosophy that aims to simplify authentication for developers while providing robust governance for security and platform teams.

Our core principles are:

*   **Separation of Concerns**: Developers, security teams, and operators have distinct roles and responsibilities, managed through separate Custom Resources. Developers declare *what* they need, while security defines *how* it's secured.
*   **Declarative & GitOps-Native**: All configuration is managed through version-controlled YAML manifests. This enables automated, repeatable, and auditable workflows.
*   **Secure by Default**: We provide sensible defaults and enforce security best practices through policies, ensuring that even the simplest client configuration is secure.
*   **Developer Self-Service**: Empower developers to configure authentication for their applications frictionlessly without requiring deep security expertise or waiting on support tickets.

This section provides a detailed look into our design:

*   **[Core Concepts](./core-concepts/)**: The foundational principles that guide our architecture.
*   **CRD Specifications**:
    *   **[AuthClient](./crds/auth-client/)**: For developers to request OIDC clients.
    *   **[AuthPolicy](./crds/auth-policy/)**: For security teams to define governance.
    *   **[AuthServer](./crds/auth-server/)**: For platform teams to manage server instances.
    *   **[UserStore](./crds/user-store/)**: For configuring backend user directories.
*   **[Example Workflows](./workflows/)**: Practical examples of how these resources work together.
