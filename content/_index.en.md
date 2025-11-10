---
title: "SportShop E-Commerce Platform"
description: "A Secure, Scalable AWS Three-Tier Architecture for Online Sports Retail"
date: 2025-11-09
---

# SportShop E-Commerce Platform

## A Secure, Scalable AWS Three-Tier Architecture for Online Sports Retail

---

### Executive Summary

The SportShop E-Commerce Platform is built to modernize and streamline online retail operations for sports equipment and apparel businesses. It implements a three-tier AWS architecture (Edge → Application → Data) using ReactJS, Spring Boot, and Amazon RDS MySQL. The platform supports product CRUD operations, user authentication via Amazon Cognito with OTP (email and SMS), VNPay (sandbox) payment integration, and real-time chat.

By leveraging AWS managed and serverless services such as Elastic Beanstalk, RDS, S3, CloudFront, and Cognito, combined with an automated CI/CD pipeline (GitLab → CodePipeline → CodeBuild → Elastic Beanstalk), the system achieves high availability, security, and cost efficiency. This architecture ensures continuous delivery, simplified management, and rapid deployment cycles for future scalability.

---

### Problem Statement

**The Problem:**

Traditional manual deployments are slow, inconsistent, and insecure. Credentials are often stored in plain text, increasing risks of leaks. There is no centralized monitoring, no automatic rollback, and no CI/CD integration.

**The Solution:**
SportShop leverages **AWS managed services** to automate infrastructure, secure configurations, and ensure scalability.

The architecture is divided into three layers:

- **Edge:** Route 53, CloudFront (with ACM), Cognito, SES, SNS
- **Application:** Elastic Beanstalk and Application Load Balancer
- **Data:** RDS MySQL (Single-AZ), S3, CloudWatch

Sensitive credentials (database, VNPay keys) are stored securely in **AWS Systems Manager Parameter Store.**

The CI/CD flow uses **GitLab → CodePipeline → CodeBuild** for continuous deployment to Elastic Beanstalk.

**Benefits:** Deployment time drops dramatically with CI/CD automation, end-to-end HTTPS improves security, and the design remains cost-optimized for practical lab and demo use.

---

### Solution Architecture

Solution Architecture Overview
The SportShop platform adopts a **three-tier AWS cloud architecture** (Edge, Application, Data) designed for secure, scalable, and cost-optimized e-commerce operations.

Static web content is served globally through CloudFront and S3, while backend logic is deployed on Elastic Beanstalk with load balancing and auto-scaling. RDS MySQL provides reliable data storage, and Cognito manages authentication with OTP verification. The architecture follows AWS best practices for security, availability, and operational efficiency. (Architecture diagram shown below.)

<img src="/images/diagram.jpg">

#### AWS Services Used:

- **Amazon Route 53**: Hosts the public DNS zone and routes user traffic to CloudFront.
- **Amazon CloudFront**: Delivers the static ReactJS frontend and media assets over HTTPS globally.
- **AWS Certificate Manager (ACM)**: Issues and manages the SSL/TLS certificate used by CloudFront for end-to-end HTTPS encryption.
- **Amazon Cognito**: Handles user authentication, registration, and token management with OTP through SES and SNS.
- **Amazon Simple Email Service (SES)**: Sends email-based OTPs and registration messages.
- **Amazon Simple Notification Service (SNS)**: Sends SMS notifications and alerts.
- **AWS Elastic Beanstalk**: Hosts the Spring Boot backend (REST + WebSocket) with automatic scaling and environment management.
- **Application Load Balancer (ALB)**: Routes HTTPS (port 443) traffic from CloudFront to the Beanstalk instances.
- **AWS Systems Manager Parameter Store**: Stores encrypted database credentials and VNPay API keys using KMS.
- **NAT Gateway**: Provides secure outbound Internet access for the Beanstalk environment (e.g., VNPay API, external libraries).
- **Amazon RDS MySQL (Single-AZ)**: Stores application data such as users, products, orders, and chat messages.
- **Amazon S3**: Hosts frontend static files, user-uploaded images, and backup files (encrypted and versioned).
- **VPC Gateway Endpoint for S3**: Allows internal access to S3 from Beanstalk without routing through the Internet.
- **Amazon CloudWatch**: Collects metrics and logs from EB, ALB, and RDS for monitoring and alerting.
- **Amazon SNS**: Delivers CloudWatch alarms to administrators via email or SMS.
- **AWS CodePipeline and CodeBuild**: Automate CI/CD integration with GitLab for continuous deployment to Elastic Beanstalk.

**Component Design:**

- **Edge Layer (Content Delivery & Authentication):** Route 53 manages the domain and routes traffic to CloudFront, which caches and delivers the ReactJS frontend globally over HTTPS (ACM). Cognito secures user login, while SES and SNS provide OTP delivery for email and SMS.
- **Application Layer (Backend Logic & APIs):** Elastic Beanstalk hosts the Spring Boot backend, with ALB distributing HTTPS traffic evenly across instances. The environment scales automatically and accesses VNPay and Cognito JWKS securely via NAT Gateway. Application secrets and credentials are fetched from Parameter Store.
- **Data Layer (Storage & Monitoring):** RDS MySQL manages relational data. S3 stores static content, images, and backups, accessible internally via the VPC Gateway Endpoint. CloudWatch monitors EB, ALB, and RDS performance, and sends alerts through SNS for proactive maintenance.

---

### Technical Implementation

**Tech Stack:**

- **Frontend:** ReactJS on S3 + CloudFront (HTTPS via ACM)
- **Backend:** Spring Boot (Java 17) on Elastic Beanstalk
- **Database:** RDS MySQL (Single-AZ)
- **CI/CD:** GitLab → CodePipeline → CodeBuild
- **Security:** ACM, IAM, Parameter Store (KMS), CloudWatch Alerts

**The implementation follows four main phases:**

1. **Architecture Design:** Define VPC subnets, routing, security groups, and IAM roles.
2. **Cost Optimization:** Use AWS Free Tier resources with Single-AZ RDS and a single NAT Gateway to minimize expense.
3. **Deployment:** Host the backend on Elastic Beanstalk and the frontend on S3 + CloudFront. Configure ACM, Route 53, Cognito, SES, and SNS.
4. **Monitoring & CI/CD:** Integrate GitLab with CodePipeline and CodeBuild for automated builds, and enable CloudWatch + SNS alerts for runtime monitoring.

---

### Timeline & Milestones

**Project Timeline:**

- **Pre-Implementation**
  - **Month 0:** One month for system review and AWS architecture preparation.
- **Implementation (Months 1–3):**
  - **Month 1:** Set up VPC, ALB, RDS, S3, CloudFront, Route 53, and CI/CD pipeline.
  - **Month 2:** Integrate Cognito, SES, SNS, and VNPay sandbox; deploy frontend and backend.
  - **Month 3:** Conduct testing, performance monitoring, and production launch.
    Post-Launch: Continue optimization, monitoring, and research for scalability.

---

### Budget Estimation

You can find the budget estimation on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?utm_source=chatgpt.com) Or you can download the <a href="/files/SportShop-Budget-Estimation.pdf" download>Budget Estimation PDF</a>.

**Infrastructure Costs AWS Services:**

- **Amazon RDS for MySQL**: $21.84/month (db.t2.micro, 20 GB storage)
- **Amazon EC2 (Elastic Beanstalk)**: $11.68/month (t3.micro, 20 GB EBS)
- **S3 Standard**: $0.26/month (10 GB, typical requests)
- **Data Transfer (Internet)**: $0.60/month (1 GB inbound, 5 GB outbound)
- **Amazon CloudFront**: $0.88/month (10 GB, 10k HTTPS requests)
- **Amazon Route 53**: $0.90/month (1 hosted zone)
- **AWS Certificate Manager (ACM)**: $0.00/month (free public cert; $15 one-time)
- **Amazon Cognito**: $0.00/month (≈500 MAU)
- **Amazon Simple Email Service (SES)**: $0.10/month (≈1,000 emails)
- **Amazon Simple Notification Service (SNS – topics)**: $0.00/month
- **Amazon CloudWatch (metrics & APIs)**: $3.02/month (≈10 metrics, basic APIs)
- **Network Address Translation (NAT Gateway)**: $43.36/month (1 gateway, 720 h)
- **NAT Data Transfer**: $1.30/month (≈10 GB outbound, 5 GB intra-region)
- **Application Load Balancer (ALB)**: $19.57/month (HTTPS routing, ~0.2 LCU)
- **AWS CodePipeline**: $0.00/month (1 pipeline, Free Tier)
- **AWS CodeBuild**: $0.50/month (≈10 builds × 10 min, general1.small)
- **AWS Systems Manager Parameter Store**: $0.00/month (≈10 standard parameters)
- **Amazon CloudWatch (logs)**: $1.41/month (≈2 GB ingested, 1-month retention)
- **AWS Key Management Service (KMS)**: $1.00/month (1 CMK, optional)

**Total: $106.42/month, $1,292.04/12 months, $15.00 upfront.**

---

### Risk Assessment & Mitigation

- **Risk Matrix**

  - **Network or Instance Failure**: Medium impact, low probability
  - **Database Downtime or Data Loss**: High impact, low probability
  - **CI/CD Pipeline or Build Failure**: Medium impact, medium probability
  - **NAT Gateway Cost Overruns**: Medium impact, medium probability

- **Mitigation Strategies**

  - **Network/Instance**: Use Elastic Beanstalk rolling updates and health checks for auto-recovery
  - **Database**: Enable RDS automated backups and snapshots for fast restoration
  - **Pipeline**: Monitor CodePipeline and CodeBuild using CloudWatch alarms integrated with SNS alerts
  - **Cost**: Configure AWS Budget alerts and apply lifecycle rules to S3 to reduce storage costs

- **Contingency Plans**
  - **If Elastic Beanstalk fails**: Revert deployment using previous application versions in CodePipeline
  - **If RDS downtime occurs**: Restore the latest automated snapshot
  - **In case of cost escalation**: Disable NAT Gateway temporarily and switch to manual outbound access for testing

---

### Expected Outcomes

**Technical Improvements:**

- CI/CD automation reduces deployment time from hours to minutes.
- End-to-end HTTPS with ACM enhances data security and user trust.
- Centralized monitoring through CloudWatch and SNS enables proactive maintenance.
- Architecture supports auto-scaling and efficient cost control.

**Long-Term Value:**

- Reusable AWS reference design for future e-commerce systems.
- Low-cost, secure, scalable foundation for startups and academic projects.
- Hands-on experience with AWS cloud-native tools and distributed application design.
