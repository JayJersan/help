---
title: AWS API Gateway Integration
description: Integration guide for AWS API Gateway with AccuKnox API Security.
---

# AWS API Gateway Integration

This guide provides the steps required to connect AWS API Gateway to AccuKnox API Security.

!!! info "Quick Rundown of Steps"
    - Deploy the CloudFormation template. Enable logging.
    - Logs move through CloudWatch and Lambda. The AccuKnox API Agent receives them.
    - The Control Plane builds your real-time inventory.
    - Scan endpoints against your OpenAPI spec.

![alt text](<Screenshot 2025-11-28 132113.png>)

## 1. Deploy the AccuKnox CloudFormation Template

1. Download the CloudFormation template from the AccuKnox console.
2. In your AWS account, open **CloudFormation → Create Stack**.
3. Upload the template.
4. Provide the required parameters for your AccuKnox environment.
5. Launch the stack.

The stack creates:

- A **CloudWatch Log Group** for API Gateway logs.
- A **Lambda subscription function** to forward logs to the AccuKnox API Agent.

## 2. Enable Logging in AWS API Gateway

1. Open **API Gateway → Your API → Stages**.
2. Select the stage you want to secure.
3. Enable execution/access logging.
4. Point logging to the CloudWatch Log Group created by the AccuKnox stack.

Once enabled, API Gateway begins writing API request and response logs to CloudWatch.

## 3. Allow Lambda to Forward Logs

The CloudFormation-created Lambda function automatically subscribes to the Log Group.
No manual changes are needed.
This function pushes all API Gateway log events to the **AccuKnox API Agent**.

## 4. AccuKnox API Agent Receives Logs

The API Agent (EC2 instance created during onboarding) listens on its configured port and accepts logs from the Lambda function.
The agent sends processed data to the **AccuKnox Control Plane (EKS)**.

## 5. Real-Time API Inventory in AccuKnox

As soon as logs reach the control plane:

- API endpoints appear in the **Inventory** tab.
- Inventory shows method, path, request and response bodies, and sensitive-data classification.
- External and internal calls are labeled accordingly.

Inventory updates continuously as logs arrive.

## 6. Create API Collections

If you want to group related endpoints:

1. Go to **API Security → Collections**.
2. Create a collection using host, path, or method conditions.

Collections help organize endpoints before scanning.

## 7. Upload Your OpenAPI Specification

1. Navigate to **API Security → Specifications**.
2. Upload your OpenAPI (YAML or JSON) file.

This file defines the expected API structure and is used only during scan comparison.

## 8. Scan API Endpoints

1. Go to **API Security → Scans → New Scan**.
2. Select the OpenAPI spec you uploaded.
3. Choose the inventory or a collection.
4. Run the scan.

The scan compares runtime traffic with your specification.

## 9. Review Findings

AccuKnox displays four types of findings:

- **zombie APIs**
  Detected when request or response bodies include sensitive fields.

- **Shadow APIs**
  Endpoints seen in runtime traffic but not present in the spec.

- **Orphan APIs**
  Endpoints defined in the spec but never seen in traffic.

- **Active APIs**
  Endpoints currently receiving traffic.

Each finding includes request, response, and occurrence details.
