---
title: API Security Integrations
description: Overview of API Security capabilities and supported integrations.
hide:
  - toc
---


<style>
h2 {
  color: #000025;
  font-size: 1.5rem !important;
}
.nt-card .nt-card-image{
  color: #005BFF;

}

 .nt-card-title {
    text-align: -webkit-center;
}
</style>

# API Security Integrations

The **API Security** module provides deep visibility and continuous risk assessment for your APIs by analyzing live traffic, identifying unknown endpoints, and highlighting security exposures.

::cards:: cols=2

- title: AWS API Gateway
  image: ./aws-api-gateway.png
  url: ../integrations/api-aws.md
  description: Monitor and secure APIs running on AWS API Gateway

- title: Kubernetes API Proxy
  image: ./k8s.png
  url: ../integrations/api-k8s.md
  description: Secure APIs in Kubernetes environments

- title: Istio
  image: ./istio.png
  url: ../integrations/api-istio.md
  description: Integrate with Istio service mesh for API security

- title: Nginx Ingress
  image: ./nginx.png
  url: ../integrations/api-nginx.md
  description: Protect APIs exposed via Nginx Ingress Controller

::/cards::


## Key Capabilities

- **Real-Time Inventory**: Automatically discovers APIs from traffic with risk scoring based on authentication, exposure, and sensitive data
- **Risk Detection**: Identifies Shadow, Zombie, and Orphan APIs through continuous traffic analysis
- **Logical Grouping**: Organize APIs for efficient tracking and management

![alt text](image-7.png)