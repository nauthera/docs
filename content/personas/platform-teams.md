---
title: "For Platform Teams"
type: "docs"
weight: 3
---

Nauthera provides platform and operations teams with a lightweight, scalable, and easy-to-manage authentication platform that integrates seamlessly with their existing cloud-native tooling.

## The Burden of Traditional Identity Providers

Traditional identity providers are often monolithic, stateful, and complex to manage, creating a significant operational burden for platform teams:

*   **Complex Deployments**: Deploying and configuring a traditional identity provider in a high-availability, multi-cluster environment is a complex and time-consuming task.
*   **Difficult to Scale**: Scaling a monolithic, stateful application can be challenging and often requires specialized expertise.
*   **High Maintenance Overhead**: Platform teams are responsible for patching, upgrading, and backing up the identity provider, which can be a significant ongoing effort.
*   **Poor Observability**: Many traditional identity providers offer limited observability, making it difficult to monitor their health and performance.

## The Nauthera Advantage: A Lightweight, Cloud-Native Platform

Nauthera is designed to be a lightweight, stateless, and easy-to-manage platform that is a natural fit for the cloud-native ecosystem.

{{% steps %}}

### Deploy with a Single Helm Chart

Deploy the entire Nauthera platform with a single Helm chart. You can choose between a cluster-wide or single-namespace installation model to fit your organization's needs.

### Configure with CRDs

Manage the entire configuration of the platform, including `AuthServer` and `UserStore` resources, through declarative YAML manifests.

### Scale with Ease

Because the Nauthera operator and authentication server are stateless, you can easily scale them horizontally to meet the demands of your workload.

### Monitor and Audit

Nauthera exposes metrics in the Prometheus format and supports OpenTelemetry for distributed tracing, allowing you to monitor the platform with your existing observability stack. All changes to authentication resources are also captured in the Kubernetes audit log, providing a complete and immutable record for compliance.

{{% /steps %}}

### Key Benefits for Platform Teams

*   **Simplified Operations**: Deploy, configure, and manage the entire platform with the same tools and workflows you use for your other Kubernetes applications.
*   **Reduced Overhead**: The lightweight, stateless architecture of Nauthera reduces the operational burden of managing a complex, stateful identity provider.
*   **Enhanced Scalability and Resilience**: Easily scale the platform to meet the needs of your organization and benefit from the self-healing capabilities of Kubernetes.
*   **Improved Observability**: Gain deep insights into the health and performance of the platform with out-of-the-box support for Prometheus and OpenTelemetry.
