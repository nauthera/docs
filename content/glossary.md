---
title: "Glossary"
type: "docs"
weight: 10
---

This page provides definitions for common technical terms used in the Nauthera documentation.

### CRD (Custom Resource Definition)
A Kubernetes feature that allows you to define your own resource types. Nauthera uses CRDs to manage authentication resources like `AuthClient` and `AuthPolicy` as native Kubernetes objects.

### GitOps
A modern, declarative approach to continuous delivery where Git is the single source of truth for both application and infrastructure configuration. Changes are made via pull requests and automatically applied to the cluster.

### Grafana
A popular open-source platform for monitoring and observability, often used to visualize metrics collected by Prometheus.

### Jaeger
An open-source, end-to-end distributed tracing system that helps you monitor and troubleshoot transactions in complex distributed systems.

### JWT (JSON Web Token)
A compact, URL-safe means of representing claims to be transferred between two parties. Nauthera's authentication server uses JWTs to handle sessions in a stateless manner.

### LDAP (Lightweight Directory Access Protocol)
A standard application protocol for accessing and maintaining distributed directory information services over an IP network. It's a common way to store user and group information in a centralized location.

### mTLS (Mutual TLS)
Mutual Transport Layer Security is a method for mutual authentication. It ensures that both parties at each end of a network connection are who they claim to be by verifying that they both have the correct private key. Nauthera uses mTLS for secure communication between the operator and the authentication server.

### OAuth 2.0
An authorization framework that enables applications to obtain limited access to user accounts on an HTTP service. It is not an authentication protocol itself but is often used in conjunction with protocols like OIDC.

### OIDC (OpenID Connect)
A simple identity layer built on top of the OAuth 2.0 protocol. It allows clients to verify the identity of the end-user based on the authentication performed by an authorization server, as well as to obtain basic profile information about the end-user.

### OpenTelemetry (OTEL)
An open-source observability framework for instrumenting, generating, collecting, and exporting telemetry data (metrics, logs, and traces) to help you analyze your software’s performance and behavior.

### Operator
A method of packaging, deploying, and managing a Kubernetes application. A Kubernetes operator is an application-specific controller that extends the Kubernetes API to create, configure, and manage instances of complex stateful applications on behalf of a Kubernetes user.

### PKCE (Proof Key for Code Exchange)
An extension to the Authorization Code flow to prevent certain attacks and to be able to securely perform the OAuth flow from public clients. It is a security best practice for modern applications.

### Prometheus
An open-source systems monitoring and alerting toolkit. It collects and stores its metrics as time series data, i.e., metrics information is stored with the timestamp at which it was recorded.
