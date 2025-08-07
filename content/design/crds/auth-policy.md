---
title: "AuthPolicy"
type: "docs"
weight: 2
---

The `AuthPolicy` is a cluster-scoped or namespace-scoped resource managed by the security team. It defines the security constraints and default settings that govern a group of `AuthClient` resources.

## Purpose

*   **Centralized Security Governance**: Allows security teams to define and enforce security policies across multiple applications and teams.
*   **Enforce Best Practices**: Ensures that all clients adhere to organizational security standards, such as token lifetimes and the use of PKCE.
*   **Scope and Grant Control**: Provides a whitelist of allowed scopes and grant types, preventing clients from requesting permissions they shouldn't have.

## Example

```yaml
apiVersion: auth.nauthera.io/v1alpha1
kind: AuthPolicy
metadata:
  name: web-apps-policy
  namespace: security
spec:
  token:
    accessTokenTTL: "15m"
    refreshTokenTTL: "24h"
    idTokenTTL: "5m"
  allowedGrantTypes:
    - "authorization_code"
    - "refresh_token"
  enforcePKCE: true
  allowedScopes:
    - openid
    - profile
    - email
    - "api:read"
    - "api:write"
  clientAuthentication:
    method: "client_secret_basic"
    secretRotation:
      enabled: true
      schedule: "0 0 * * *" # Daily at midnight
```

## Spec Fields

| Field                  | Type                | Description                                                                                                                                                           |
| ---------------------- | ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `token`                | `object`            | Contains settings for token lifetimes. `accessTokenTTL`, `refreshTokenTTL`, and `idTokenTTL` are specified as strings (e.g., "15m", "24h").                 |
| `allowedGrantTypes`    | `array` of `string` | A list of the OAuth 2.0 grant types that are permitted for clients associated with this policy (e.g., "authorization_code", "client_credentials").         |
| `enforcePKCE`          | `boolean`           | If `true`, all clients must use PKCE (Proof Key for Code Exchange). This is a critical security feature for public clients and is highly recommended.      |
| `allowedScopes`        | `array` of `string` | A comprehensive list of all scopes that can be requested by clients governed by this policy.                                                                        |
| `clientAuthentication` | `object`            | Defines how the client authenticates to the token endpoint. Includes the `method` (e.g., `client_secret_basic`) and optional `secretRotation` configuration. |

{{< callout type="warning" >}}
Changing an `AuthPolicy` can affect all the `AuthClient` resources that are associated with it. Always review the potential impact before applying changes to a policy in a production environment.
{{< /callout >}}
