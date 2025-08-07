---
title: "Installation"
type: "docs"
weight: 4
---

The Nauthera platform can be deployed in two primary models, managed via a Helm chart:

## Cluster-Wide Installation

In this model, a single instance of the Nauthera Operator is deployed to manage authentication services across all namespaces in the Kubernetes cluster. This is the recommended approach for most use cases as it provides centralized management and reduces overhead. The operator uses a `ClusterRole` and `ClusterRoleBinding` to gain the necessary permissions.

## Single-Namespace Installation

For environments with stricter multi-tenancy requirements, the operator can be deployed in a single-namespace model. In this setup, the operator is restricted to managing resources only within its own namespace. This requires a separate installation of the operator for each namespace that needs to run an authentication service.
