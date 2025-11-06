---
title: Secrets Management
description: Learn about AccuKnox Secrets Manager, a Vault-compatible solution for secure secret storage and management across multi-cloud environments.
---

# AccuKnox Secrets Manager (Vault-Compatible)

AccuKnox Secrets Manager provides centralized, secure storage and lifecycle management for secrets across multi-cloud environments. It is designed as a **drop-in, API-compatible replacement for HashiCorp Vault**, allowing existing applications and scripts to continue working with minimal or no changes.

## What It Does

* Stores secrets in an encrypted, versioned key/value store
* Issues dynamic, short-lived credentials (AWS, Kubernetes, databases)
* Provides Transit encryption APIs for encrypt/decrypt/sign/verify operations
* Manages certificates and PKI (root and intermediate CAs)
* Implements identity-based access control (OIDC, LDAP, Okta, Kubernetes Auth, tokens, AppRole)
* Generates full audit logs for every request
* Supports multi-tenant namespaces for clean separation of teams or environments

## HashiCorp Vault Compatibility

AccuKnox Secrets Manager maintains compatibility with commonly used Vault APIs:

### Migration from Hashicorp Vault

Switching from Vault is straightforward since most operational patterns remain unchanged.

1. Map existing Vault namespaces to AccuKnox namespaces.
2. Reuse or translate existing policies.
3. Update applications to point to the new endpoint.
4. Validate dynamic secrets roles and auth configurations.
5. Move traffic gradually if needed.

### Supported (No Major Code Changes Required)

* **KV Secrets Engine** (read, write, list, delete)
* **Transit Engine** (encrypt, decrypt, sign, verify)
* **PKI Engine** (CSR signing, issuing, revocation)
* **Dynamic Secrets** (similar workflow for roles and leases)
* **Authentication Methods** (OIDC, LDAP, Kubernetes, JWT, AppRole, tokens)
* **Policy Model** (identity/role-based rules similar to Vault ACLs)

If an application already talks to Vault using standard APIs, it can typically point to the AccuKnox endpoint and continue working.

## Key Features (Quick View)

| Feature                       | Summary                                        |
| -- | - |
| **Secure Secret Storage**     | Encrypted at rest, versioned secrets.          |
| **Dynamic Secrets**           | Automatically created short-lived credentials. |
| **Transit Encryption**        | Offload cryptographic operations over an API.  |
| **PKI Management**            | Certificate issuance and lifecycle control.    |
| **Identity & Access Control** | Granular permissions for users and services.   |
| **Audit Logging**             | Records all actions, paths, and identities.    |
| **Multi-Tenancy**             | Per-tenant namespaces with isolated policies.  |

## Deployment Notes

* Runs in high-availability mode
* Supports cloud, on-prem, and air-gapped deployments
* All instances hardened with AccuKnox CWPP
* Supports regional deployments as required
