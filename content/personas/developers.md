---
title: "For Developers"
type: "docs"
weight: 1
---

Nauthera is designed to make life easier for developers by providing a simple, declarative, and self-service workflow for managing OIDC clients.

## The Traditional Workflow: A Story of Friction

In many organizations, the process of getting OIDC credentials for a new application is a slow and frustrating experience:

1.  **File a Ticket**: The developer starts by filing a ticket with the security or platform team, requesting a new OIDC client.
2.  **Fill out a Form**: The developer is then asked to fill out a lengthy form with dozens of fields, many of which they may not understand.
3.  **Wait for Approval**: The ticket goes into a queue, and the developer has to wait for a manual review and approval process.
4.  **Receive Credentials**: After a few days (or weeks), the developer receives the `client_id` and `client_secret`, often via an insecure channel like email or Slack.
5.  **Repeat for Changes**: If the developer needs to add a redirect URI or change a scope, they have to go through the whole process again.

This workflow is a major source of friction and a significant drag on developer velocity.

## The Nauthera Workflow: Declarative Self-Service

Nauthera replaces this manual, ticket-based workflow with a simple, declarative, and GitOps-native process:

{{% steps %}}

### Define Your Client in YAML

Create a simple `AuthClient` manifest in your application's repository. You only need to define the properties that are relevant to your app.

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
```

### Commit and Push

Commit the file to your Git repository.

```bash
git add client.yaml
git commit -m "feat: add OIDC client for my-webapp"
git push
```

### Get Your Credentials

The Nauthera operator automatically provisions the client and creates a Kubernetes `Secret` with your credentials. You can then mount this secret directly into your application's `Deployment`.

{{% /steps %}}

### Key Benefits for Developers

*   **Velocity**: Get the credentials you need in minutes, not days.
*   **Simplicity**: No more complex UIs or confusing forms. Just simple, declarative YAML.
*   **Ownership**: Manage your client configuration alongside your application code in the same repository.
*   **Instant Feedback**: Get immediate feedback on the status of your client directly from the Kubernetes API.
