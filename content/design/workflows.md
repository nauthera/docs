---
title: "Example Workflows"
type: "docs"
weight: 7
---

This section illustrates how the different Nauthera CRDs work together in common scenarios.

## Onboarding a New Application

This workflow shows how a developer can onboard a new application and get OIDC credentials without needing to interact directly with the security team.

**Personas:**
*   **Developer**: Member of the `my-app-dev` team.
*   **Security Admin**: Manages security policies for the organization.
*   **Platform Admin**: Manages the Nauthera platform.

**Prerequisites:**
1.  The Platform Admin has already deployed an `AuthServer` named `production-server`.
2.  The Security Admin has already created an `AuthPolicy` named `web-apps-policy` that governs web applications.

{{% steps %}}

### Developer Defines the `AuthClient`

The developer creates a simple `AuthClient` manifest for their new web application. They only need to specify the details relevant to their app.

```yaml
# my-app/client.yaml
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
```

### Developer Commits to Git

The developer commits this file to their application's Git repository.

```bash
git add my-app/client.yaml
git commit -m "feat: add OIDC client for my-webapp"
git push
```

### GitOps Controller Applies the Manifest

A GitOps controller (like Argo CD or Flux) detects the change and applies the `AuthClient` manifest to the Kubernetes cluster.

### Nauthera Operator Takes Over

1.  The Nauthera operator sees the new `AuthClient` resource.
2.  It retrieves the referenced `AuthServer` (`production-server`) and its associated `AuthPolicy` (`web-apps-policy`).
3.  It **validates** the `AuthClient` against the `AuthPolicy`.
4.  It **provisions** the new OIDC client in the backend authentication server.
5.  It **creates** a Kubernetes `Secret` with the client credentials.
6.  It **updates** the `AuthClient`'s status to `Ready`.

### Application Consumes the Secret

The application's `Deployment` can now mount the generated secret and start the OIDC flow.

{{% /steps %}}

## Updating a Security Policy

This workflow shows how a security admin can update a security policy and have it automatically enforced for all associated clients.

{{% steps %}}

### Security Admin Updates the `AuthPolicy`

The security team decides to shorten the access token lifetime for all web apps from 15 minutes to 5 minutes. The admin updates the `AuthPolicy` manifest.

```yaml
# security-policies/web-apps-policy.yaml
apiVersion: auth.nauthera.io/v1alpha1
kind: AuthPolicy
metadata:
  name: web-apps-policy
  namespace: security
spec:
  token:
    # Changed from "15m"
    accessTokenTTL: "5m"
    refreshTokenTTL: "24h"
    idTokenTTL: "5m"
  # ... other fields remain the same
```

### Security Admin Commits to Git

The admin commits the change to the security policies repository.

```bash
git add security-policies/web-apps-policy.yaml
git commit -m "sec: reduce access token lifetime for web apps"
git push
```

### GitOps Controller Applies the Manifest

The GitOps controller applies the updated `AuthPolicy`.

### Nauthera Operator Reconciles Clients

1.  The operator detects the change to the `AuthPolicy`.
2.  It identifies all `AuthClient` resources that are governed by this policy.
3.  It **re-reconciles** each of these clients, updating their configuration to reflect the new token lifetime.

{{% /steps %}}

The change is rolled out automatically and consistently, without any action required from the development teams.
