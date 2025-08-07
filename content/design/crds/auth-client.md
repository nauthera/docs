---
title: "AuthClient"
type: "docs"
weight: 1
---

The `AuthClient` is the primary resource for developers to provision and manage an OAuth 2.0 / OIDC client for their application. It is designed to be simple and focused on the application's needs, abstracting away the underlying security complexity.

## Purpose

*   **Developer Self-Service**: Enables developers to request an OIDC client without needing to understand the intricacies of the authentication server or security policies.
*   **Application-Specific Configuration**: Defines the essential properties of an application's client, such as its name, redirect URIs, and required scopes.
*   **Secret Generation**: The operator automatically generates and manages the client credentials, storing them in a Kubernetes `Secret` for the application to consume.

## Example

```yaml
apiVersion: auth.nauthera.io/v1alpha1
kind: AuthClient
metadata:
  name: my-webapp
  namespace: my-app-dev
spec:
  displayName: "My Awesome Web App"
  authServerRef:
    name: production-server
    namespace: security
  redirectUris:
    - "https://my-webapp.dev.example.com/callback"
  scopes:
    - openid
    - profile
    - email
    - "api:read"
status:
  conditions:
    - type: Ready
      status: "True"
      reason: ClientProvisioned
      message: "OIDC client has been successfully provisioned."
  clientSecretRef:
    name: my-webapp-client-secret
```

## Spec Fields

| Field           | Type                | Description                                                                                                                                 |
| --------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `displayName`   | `string`            | A human-readable name for the client application. This may be shown on consent screens.                                                     |
| `authServerRef` | `object`            | A reference to the `AuthServer` that this client will be associated with. The `name` and `namespace` of the `AuthServer` must be provided. |
| `redirectUris`  | `array` of `string` | A list of allowed redirect URIs for the client. The operator will ensure these are registered with the OIDC provider.                       |
| `scopes`        | `array` of `string` | The list of OAuth 2.0 scopes that the client is allowed to request. These are validated against the `allowedScopes` in the `AuthPolicy`.    |

## Status Fields

The `status` sub-resource provides feedback on the state of the `AuthClient`.

| Field             | Type     | Description                                                                                                                            |
| ----------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `conditions`      | `array`  | A list of conditions that describe the current state of the resource. The `Ready` condition indicates if the client is provisioned.    |
| `clientSecretRef` | `object` | A reference to the `Secret` containing the generated `client_id` and `client_secret`. The `name` of the secret is provided here. |

{{< callout type="tip" >}}
Your application's deployment can use the `clientSecretRef.name` from the status to automatically mount the secret without needing to know the secret's name in advance.
{{< /callout >}}
