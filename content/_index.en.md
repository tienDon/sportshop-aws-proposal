---
title: "SportShop E-Commerce Platform"
description: "A Secure, Scalable AWS Three-Tier Architecture for Online Sports Retail"
date: 2025-11-09
---

# SportShop E-Commerce Platform

## A Secure, Scalable AWS Three-Tier Architecture for Online Sports Retail

---

### Executive Summary

The SportShop E-Commerce Platform is built to modernize and streamline online retail operations for sports equipment and apparel businesses. It implements a three-tier AWS architecture (Edge → Application → Data) using ReactJS, Spring Boot, and Amazon RDS MySQL.

The platform supports:

- Product CRUD operations
- User authentication via Amazon Cognito with OTP (email and SMS)
- VNPay (sandbox) payment integration
- Real-time chat functionality

By leveraging AWS managed and serverless services such as Elastic Beanstalk, RDS, S3, CloudFront, and Cognito, combined with an automated CI/CD pipeline (GitLab → CodePipeline → CodeBuild → Elastic Beanstalk), the system achieves **high availability, security, and cost efficiency**. This architecture ensures continuous delivery, simplified management, and rapid deployment cycles for future scalability.

---

### Problem Statement

**The Problem:**

- Traditional manual deployments are slow, inconsistent, and insecure
- Credentials often stored in plain text, increasing risks of leaks
- No centralized monitoring, automatic rollback, or CI/CD integration

**The Solution:**
SportShop leverages AWS managed services to automate infrastructure, secure configurations, and ensure scalability.

**Architecture Layers:**

- **Edge:** Route 53, CloudFront (with ACM), Cognito, SES, SNS
- **Application:** Elastic Beanstalk and Application Load Balancer
- **Data:** RDS MySQL (Single-AZ), S3, CloudWatch

**Security:** Sensitive credentials (database, VNPay keys) are stored securely in AWS Systems Manager Parameter Store.

**CI/CD:** GitLab → CodePipeline → CodeBuild for continuous deployment to Elastic Beanstalk.

**Benefits:** Deployment time drops dramatically with CI/CD automation, end-to-end HTTPS improves security, and the design remains cost-optimized for practical lab and demo use.

---

### Solution Architecture

The SportShop platform adopts a **three-tier AWS cloud architecture** (Edge, Application, Data) designed for secure, scalable, and cost-optimized e-commerce operations.

**Key Components:**

- **Static web content** served globally through CloudFront and S3
- **Backend logic** deployed on Elastic Beanstalk with load balancing and auto-scaling
- **RDS MySQL** provides reliable data storage
- **Cognito** manages authentication with OTP verification

#### AWS Services Used

| Service                       | Purpose                                  |
| ----------------------------- | ---------------------------------------- |
| Amazon Route 53               | DNS management and traffic routing       |
| Amazon CloudFront             | Global content delivery with HTTPS       |
| AWS Certificate Manager (ACM) | SSL/TLS certificate management           |
| Amazon Cognito                | User authentication and token management |
| Amazon SES/SNS                | Email and SMS OTP delivery               |
| AWS Elastic Beanstalk         | Spring Boot backend hosting              |
| Application Load Balancer     | HTTPS traffic distribution               |
| AWS Parameter Store           | Encrypted credential storage             |
| Amazon RDS MySQL              | Application data storage                 |
| Amazon S3                     | Static files and backups                 |
| Amazon CloudWatch             | Monitoring and alerting                  |

---

### Technical Implementation

**Tech Stack:**

- **Frontend:** ReactJS on S3 + CloudFront (HTTPS via ACM)
- **Backend:** Spring Boot (Java 17) on Elastic Beanstalk
- **Database:** RDS MySQL (Single-AZ)
- **CI/CD:** GitLab → CodePipeline → CodeBuild
- **Security:** ACM, IAM, Parameter Store (KMS), CloudWatch Alerts

**Implementation Phases:**

1. **Architecture Design:** Define VPC subnets, routing, security groups, and IAM roles
2. **Cost Optimization:** Use AWS Free Tier resources with Single-AZ RDS and single NAT Gateway
3. **Deployment:** Host backend on Elastic Beanstalk and frontend on S3 + CloudFront
4. **Monitoring & CI/CD:** Integrate GitLab with CodePipeline and enable CloudWatch alerts

---

### Timeline & Budget

**Project Timeline:**

- **Month 0:** System review and AWS architecture preparation
- **Month 1:** Set up VPC, ALB, RDS, S3, CloudFront, Route 53, and CI/CD pipeline
- **Month 2:** Integrate Cognito, SES, SNS, and VNPay sandbox; deploy frontend and backend
- **Month 3:** Testing, performance monitoring, and production launch

**Budget Estimation:** **$106.42/month** (~$1,292/year + $15 upfront)

**Key Cost Components:**

- NAT Gateway: $43.36/month (largest cost)
- ALB: $19.57/month
- RDS MySQL: $21.84/month
- EC2 (Elastic Beanstalk): $11.68/month
- Other services: <$10/month combined

---

### Risk Assessment & Mitigation

| Risk              | Impact | Probability | Mitigation                      |
| ----------------- | ------ | ----------- | ------------------------------- |
| Instance Failure  | Medium | Low         | Elastic Beanstalk auto-recovery |
| Database Downtime | High   | Low         | RDS automated backups           |
| Pipeline Failure  | Medium | Medium      | CloudWatch + SNS monitoring     |
| Cost Overruns     | Medium | Medium      | AWS Budget alerts               |

**Contingency Plans:**

- Revert deployments using CodePipeline previous versions
- Restore from latest RDS snapshots
- Temporary NAT Gateway disable for cost control

---

### Expected Outcomes

**Technical Improvements:**

- ✅ CI/CD automation reduces deployment time from hours to minutes
- ✅ End-to-end HTTPS with ACM enhances data security and user trust
- ✅ Centralized monitoring through CloudWatch enables proactive maintenance
- ✅ Architecture supports auto-scaling and efficient cost control

**Long-Term Value:**

- 🎯 Reusable AWS reference design for future e-commerce systems
- 🎯 Low-cost, secure, scalable foundation for startups and academic projects
- 🎯 Hands-on experience with AWS cloud-native tools and distributed application design

---

_This proposal provides a complete roadmap for implementing a modern, secure, and scalable e-commerce platform using AWS best practices and cost-effective solutions._
