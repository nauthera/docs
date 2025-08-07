---
title: "Prerequisites & Integrations"
type: "docs"
weight: 9
---

## Prerequisites

Before deploying the Nauthera platform, the following components must be available in your Kubernetes cluster:

*   **Ingress/Gateway**: A Kubernetes Ingress controller or Gateway implementation to expose the authentication server.
*   **PostgreSQL**: A running PostgreSQL instance is required.
*   **cert-manager**: `cert-manager` must be installed to handle the lifecycle of TLS certificates for secure communication (mTLS) between components.

## External Integrations

### LDAP

For user and group management, the platform can be integrated with an external LDAP server. If an LDAP server is not configured, user and group information will be stored in the PostgreSQL database.
