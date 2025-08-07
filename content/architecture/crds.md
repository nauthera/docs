---
title: "Custom Resources (CRDs)"
type: "docs"
weight: 6
---

The operator introduces a set of `CustomResourceDefinitions` (CRDs) that allow administrators to manage Nauthera resources declaratively using the Kubernetes API. These CRDs are the building blocks for configuring the authentication platform and are designed to mirror the underlying API structure.

By creating, updating, or deleting these custom resources, administrators can manage the entire lifecycle of their authentication setup, including:

*   **Realms**: Define isolated environments for users, clients, and roles.
*   **Clients**: Configure OAuth 2.0 / OIDC clients that can request authentication.
*   **Users and Groups**: Manage user identities and their group memberships (if not using an external LDAP).
*   **Roles and Mappers**: Define fine-grained permissions and map user attributes.

The operator continuously monitors these resources and reconciles the state of the cluster to match the desired configuration defined in the CRDs. This GitOps-friendly approach allows for version-controlled, automated, and repeatable deployments.
