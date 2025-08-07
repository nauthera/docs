---
title: "Core Concepts"
type: "docs"
---

Nauthera's design is guided by a set of core concepts that ensure a secure, scalable, and developer-friendly authentication platform.

## Separation of Concerns

At the heart of Nauthera is a strict separation of concerns between different roles within an organization. This is enforced through a set of distinct Custom Resource Definitions (CRDs), each with a clear purpose and owner.

*   **Developers**: Focus on their application's needs. They define an `AuthClient` resource, specifying only the necessary details like redirect URIs and required scopes. They don't need to be security experts to get a secure client.
*   **Security Teams**: Define the rules of the road. They create `AuthPolicy` resources that set constraints on token lifetimes, grant types, and allowed scopes. They can enforce security best practices across the organization without blocking developer velocity.
*   **Platform/Ops Teams**: Manage the underlying infrastructure. They are responsible for deploying and maintaining the `AuthServer` instances and their dependencies, such as databases and certificate management.

This model prevents developers from making security-critical decisions they aren't equipped for, while allowing security teams to govern without becoming a bottleneck.

## Policy-Driven Configuration

Instead of embedding security settings directly into a client's configuration, Nauthera uses a policy-driven model. An `AuthClient` is linked to an `AuthPolicy`. The operator then merges and validates these resources, ensuring the resulting OIDC client adheres to the security team's standards.

This has several advantages:
*   **Centralized Governance**: Security policies are defined in one place and can be applied to many clients.
*   **Consistency**: All clients under a given policy share the same security posture.
*   **Agility**: Security teams can update a policy, and the changes are automatically rolled out to all associated clients by the operator.

## Secure by Default

Nauthera is designed to be secure out of the box. The `AuthPolicy` CRD enforces modern security best practices, such as requiring PKCE (Proof Key for Code Exchange) for all public clients. By making security the default, we reduce the risk of misconfiguration.

## Declarative and GitOps-Native

Every aspect of the Nauthera platform is configured through declarative YAML manifests. This makes it a perfect fit for GitOps workflows.

*   **Version Control**: All authentication configuration can be stored in Git, providing a full audit trail of who changed what and when.
*   **Automated Deployments**: Changes to CRDs in Git can be automatically applied to the cluster via tools like Argo CD or Flux.
*   **Repeatability**: The entire authentication setup can be easily replicated across different environments (dev, staging, prod) by applying the same set of manifests.
