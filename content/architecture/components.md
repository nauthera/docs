---
title: "Components"
type: "docs"
weight: 5
---

The Nauthera platform is composed of several key components that work together to provide a robust authentication and authorization solution.

## Authentication Server

The authentication server is a critical component of the Nauthera platform, responsible for handling all aspects of user authentication and authorization. It is designed as a stateless service, which makes it robust, secure, and easy to scale horizontally.

### Deployment Strategy

The server is deployed as a Kubernetes `Deployment`, ensuring high availability and easy scaling. It is exposed via a `Service` to be accessible within the cluster and, if needed, externally through an `Ingress`.

### Configuration

Configuration is managed through a `ConfigMap` and `Secrets`. This allows for easy management of environment-specific settings without modifying the container image. Key configuration options include:

*   **Database Connection**: Credentials for connecting to the PostgreSQL database.
*   **Redis/Valkey Connection**: Details for connecting to the Redis/Valkey instance.
*   **OAuth 2.0 / OIDC settings**: Client secrets, redirect URIs, and other provider-specific information.

### Session Management

The server is stateless and uses JSON Web Tokens (JWTs) for handling sessions and cookies. This approach eliminates the need for server-side session storage, simplifying the architecture and improving scalability.

### High Availability

To ensure high availability, it is recommended to run at least two replicas of the authentication server. Because the server is stateless, scaling the number of replicas up or down is a straightforward process. A `PodDisruptionBudget` is configured to prevent all replicas from being down simultaneously during voluntary disruptions.

## PostgreSQL

PostgreSQL is used to store operational data such as user preferences, consents, and 2FA (Two-Factor Authentication) details. Client configurations and other static settings are generally stored as `CustomResourceDefinitions` (CRDs) within the Kubernetes cluster. User and group information can also be stored in PostgreSQL if an external LDAP server is not used.

### Deployment Strategy

PostgreSQL is a prerequisite for the Nauthera platform and must be provided by the user. For production environments, a managed PostgreSQL service from a cloud provider (e.g., Amazon RDS, Google Cloud SQL, or Azure Database for PostgreSQL) is highly recommended.

## Redis / Valkey (Optional)

Redis/Valkey can be utilized for caching tokens, claims, and other metadata to reduce database load and improve performance. Its use is recommended for large-scale deployments where high performance is a critical requirement. Since sessions are handled by stateless JWTs, Redis/Valkey is not used for session management.

### Deployment Strategy

Similar to PostgreSQL, using a managed Redis/Valkey service (e.g., Amazon ElastiCache, Google Cloud Memorystore, or Azure Cache for Redis) is the recommended approach for production deployments.

For in-cluster deployments, Redis/Valkey can be deployed as a `StatefulSet` to provide stable network identity. For a highly available setup, a Redis/Valkey cluster with sentinel nodes can be configured.

### Configuration

The connection details for Redis/Valkey are provided to the authentication server and other components via a `Secret`. This includes the host, port, and any required authentication credentials.

## Operator

The Nauthera Operator is a Kubernetes operator responsible for managing the lifecycle of the Nauthera platform components. It automates the deployment, configuration, and management of the authentication server. The operator communicates with the authentication server securely using gRPC with mutual TLS (mTLS).

### Deployment Strategy

The operator is deployed as a Kubernetes `Deployment`. The specific permissions it requires depend on the installation model:
*   **Cluster-Wide**: The operator is deployed with a `ClusterRole` and `ClusterRoleBinding` to manage resources across all namespaces.
*   **Single-Namespace**: The operator is deployed with a `Role` and `RoleBinding` scoped to its specific namespace.

### Key Responsibilities

*   **Automated Deployment**: The operator automates the deployment of the authentication server, PostgreSQL, and Redis/Valkey based on the custom resource specifications.
*   **Configuration Management**: It manages the `ConfigMaps` and `Secrets` required by the various components.
*   **Upgrades and Maintenance**: The operator simplifies the process of upgrading the platform components and handling routine maintenance tasks.
