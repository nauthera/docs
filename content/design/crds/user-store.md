---
title: "UserStore"
type: "docs"
weight: 4
---

The `UserStore` resource defines a connection to a backend user directory, such as an LDAP server or a SQL database. This allows the `AuthServer` to be decoupled from the specifics of how user data is stored and retrieved.

## Purpose

*   **Abstract User Directories**: Provides a consistent interface for the `AuthServer` to interact with different types of user stores.
*   **Centralized User Store Configuration**: Manages the configuration for connecting to user directories in a single place.
*   **Dynamic User Federation**: Enables the `AuthServer` to federate user identities from external sources in real-time.

## Example

This example shows a `UserStore` configured to connect to an LDAP server.

```yaml
apiVersion: auth.nauthera.io/v1alpha1
kind: UserStore
metadata:
  name: corporate-ldap
  namespace: security
spec:
  type: ldap
  ldap:
    server: "ldaps://ldap.example.com:636"
    bindDN: "cn=admin,dc=example,dc=com"
    bindPasswordSecretRef:
      name: ldap-bind-password
      key: password
    userSearch:
      baseDN: "ou=users,dc=example,dc=com"
      filter: "(objectClass=inetOrgPerson)"
      usernameAttribute: "uid"
    groupSearch:
      baseDN: "ou=groups,dc=example,dc=com"
      filter: "(objectClass=groupOfNames)"
      nameAttribute: "cn"
```

## Spec Fields

| Field  | Type     | Description                                                                       |
| ------ | -------- | --------------------------------------------------------------------------------- |
| `type` | `string` | The type of user store. Currently supports `ldap`.                                |
| `ldap` | `object` | Configuration for an LDAP user store. This field is required if `type` is `ldap`. |
