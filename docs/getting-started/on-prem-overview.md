---
title: On-Premises Overview
description: Overview of On-Premises Deployment for AccuKnox
---

## High-Level Architecture Overview

![](./images/on-prem/3.png)

AccuKnox onprem deployment is based on Kubernetes native architecture.

### AccuKnox OnPrem k8s components

#### Microservices

Microservices implement the API logic and provide the corresponding service endpoints. AccuKnox uses Golang-based microservices for handling streaming data (such as alerts and telemetry) and Python-based microservices for other control-plane services.

#### Databases

PostgreSQL is used as a relational database and MongoDB is used for storing JSON events such as alerts and telemetry. Ceph storage is used to keep periodic scanned reports and the Ceph storage is deployed and managed using the Rook storage operator.

#### Secrets Management

Within the on-prem setup, there are several cases where sensitive data and credentials have to be stored. Hashicorp's Vault is used to store internal (such as DB username/password) and user secrets (such as registry tokens). The authorization is managed purely using the k8s native model of service accounts. Every microservice has its service account and uses its service account token automounted by k8s to authenticate and subsequently authorize access to the secrets.

#### Scaling

K8s native horizontal and vertical pod autoscaling is enabled for most microservices with upper limits for resource requirements.

#### AccuKnox-Agents

Agents need to be deployed in target k8s clusters and virtual machines that have to be secured at runtime and to get workload forensics. Agents use Linux native technologies such as eBPF for workload telemetry and LSMs (Linux Security Modules) for preventing attacks/unknown execution in the target workloads. The security policies are orchestrated from the AccuKnox onprem control plane. AccuKnox leverages SPIFFE/SPIRE for workload/node attestation and certificate provisioning. This ensures that the credentials are not hardcoded and automatically rotated. This also ensures that if the cluster/virtual machine has to be deboarded then the control lies with the AccuKnox control plane.

## Onboarding Steps for AccuKnox

The onboarding process for AccuKnox's on-prem security solution consists of four key steps that the user must complete. Let's go through each step in a thorough, step-by-step manner:

![on-prem](images/on-prem/user_journey.png)

### **Step 1: Hardware & Prerequisites**

- Verify hardware, email user, and domain configurations.
- Ensure your environment meets all requirements.
- Time estimate: **Varies**, allocate sufficient time for review and adjustments.

### **Step 2: Staging AccuKnox Container Images** _(For airgapped environments only)_

- Stage AccuKnox container images in the airgapped setup.
- Reconfirm hardware, email user, and domain requirements.
- Time estimate: **~1 hour**.

### **Step 3: Installation**

- Install the AccuKnox system within your environment.
- Ensure all prerequisites remain satisfied.
- Time estimate: **~45 minutes**.

### **Step 4: Verification/Validation**

- Confirm all previous steps were completed successfully.
- Validate hardware, email user, and domain configurations.
- Time estimate: **~1 hour**.

AccuKnox onprem deployment is based on Kubernetes native architecture.
