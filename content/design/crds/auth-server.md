---
title: "AuthServer"
type: "docs"
weight: 3
---

The `AuthServer` is a cluster-scoped resource managed by the platform or operations team. It represents a logical instance of the authentication server and serves as the central point of configuration for a set of related `AuthClient` and `AuthPolicy` resources.

## Purpose

*   **Centralized Configuration**: Defines the core properties of an authentication server instance, such as its issuer URL and endpoint locations.
*   **Trust and Federation**: Establishes a trust domain and provides a place to configure federation with external identity providers.
*   **Infrastructure Binding**: Links the logical authentication server to the underlying infrastructure, such as TLS certificates and database connections.

## Example

```yaml
apiVersion: auth.nauthera.io/v1alpha1
kind: AuthServer
metadata:
  name: production-server
  namespace: security
spec:
  issuerUrl: "https://auth.prod.example.com"
  policySelector:
    matchLabels:
      nauthera.io/environment: production
  tls:
    certSecretRef:
      name: auth-tls-cert
  userStoreRef:
    name: corporate-ldap
    namespace: security
```

## Spec Fields

| Field            | Type     | Description                                                                                                                            |
| ---------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `issuerUrl`      | `string` | The public-facing URL of the authentication server. This is used to construct the OIDC discovery document.                             |
| `policySelector` | `object` | A label selector that determines which `AuthPolicy` resources apply to this server. This allows for flexible policy management.        |
| `tls`            | `object` | Configuration for TLS termination. The `certSecretRef` field points to a Kubernetes `Secret` containing the TLS certificate and key. |
| `userStoreRef`   | `object` | A reference to the `UserStore` that this server will use for user authentication and group lookups.                                    |
