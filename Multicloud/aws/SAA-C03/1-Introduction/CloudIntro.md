# Getting Started with AWS Certified Solutions Architect – Associate (SAA-C03)

**Author:** Prabhu Arjunan

## Certification Overview

* **Exam:** AWS Certified Solutions Architect – Associate (SAA-C03)
* **Duration:** 130 minutes
* **Format:** 65 Multiple Choice and Multiple Response questions
* **Exam Fee:** USD 150

---

# What is Cloud Computing?

Cloud computing is the on-demand delivery of computing resources such as compute, storage, networking, databases, and applications over the internet on a pay-as-you-go pricing model.

---

# Who is a Cloud Engineer?

A Cloud Engineer is an IT professional who designs, builds, automates, and maintains cloud infrastructure and services. They ensure the availability, scalability, security, and operational continuity of cloud environments.

---

# Exam Domains and Weightage

| Domain                               | Weightage |
| ------------------------------------ | --------- |
| Design Secure Architectures          | 30%       |
| Design Resilient Architectures       | 26%       |
| Design High-Performing Architectures | 24%       |
| Design Cost-Optimized Architectures  | 20%       |

---

# Types of Cloud Deployment Models

## Public Cloud

Infrastructure and services are owned and managed by a cloud provider and delivered over the internet to multiple customers.

**Examples:** AWS, Azure, Google Cloud.

## Private Cloud

Cloud infrastructure dedicated to a single organization. It may be hosted on-premises or by a third-party provider.

Unlike traditional on-premises environments, private clouds leverage virtualization, automation, and resource pooling, providing better resiliency and scalability.

## Hybrid Cloud

A Hybrid Cloud is a computing environment that combines on-premises infrastructure with public cloud services, allowing applications and data to move between them based on business requirements.

Examples:

An organization runs its production database in its on-premises data center but hosts its web application on AWS.
A company stores sensitive customer data on-premises while using AWS for backup and disaster recovery.
An e-commerce company uses its private data center for regular workloads and automatically scales additional resources to AWS during festive sales such as Diwali.

---

# AWS Global Infrastructure

AWS global infrastructure consists of:

1. Regions
2. Availability Zones (AZs)
3. Edge Locations and Regional Edge Caches

---

## AWS Regions

A Region is a physical geographic area containing multiple isolated Availability Zones.

Each region is independent and provides fault isolation and high availability.

**Examples:**

• Asia Pacific (Mumbai) – ap-south-1
• Asia Pacific (Hyderabad) – ap-south-2

---

## Availability Zones (AZs)

An Availability Zone is one or more discrete data centers with redundant power, networking, and connectivity.

Characteristics:

* Physically separated from other AZs
* Connected through low-latency, high-bandwidth private networking
* Designed for fault isolation and high availability
* Multiple AZs within a region enable resilient and highly available architectures

**Example:** ap-south-1a

---

## Edge Locations

Edge Locations are smaller AWS sites distributed globally and located closer to end users.

They:

* Cache frequently accessed content
* Reduce latency
* Lower the load on origin servers
* Improve user experience

Services such as Amazon CloudFront use Edge Locations to deliver content with low latency.

---

## How Edge Caching Works

1. User requests content.
2. CloudFront checks whether the content exists in the cache.
3. If there is a cache hit, content is served directly from the Edge Location.
4. If there is a cache miss, CloudFront retrieves the content from the origin server, stores it in the cache, and serves it to the user.

---

## Point of Presence (POP)

A Point of Presence (POP) is an AWS network site that consists of:

* Edge Locations
* Regional Edge Caches

POPs cache content closer to users and leverage the AWS global private network to improve performance and reduce internet transit latency.

**Services using POPs:**

* Amazon CloudFront
* Amazon Route 53
* AWS WAF
* AWS Global Accelerator

---

## Why Are Resources Region-Specific?

Organizations choose specific AWS regions for:

* Regulatory and compliance requirements (for example, GDPR)
* Data residency requirements
* Lower latency and proximity to users
* Business continuity and disaster recovery
* Cost optimization

---

# Shared Responsibility Model

## On-Premises

The customer manages everything:

* Networking
* Storage
* Servers
* Virtualization
* Operating Systems
* Runtime
* Applications
* Data
* Security

Requires significant operational expertise and capital expenditure (CapEx).

---

## Infrastructure as a Service (IaaS)

AWS manages:

* Physical infrastructure
* Networking
* Storage
* Virtualization

Customer manages:

* Operating System
* Runtime
* Applications
* Data

**Example:** Amazon EC2

---

## Platform as a Service (PaaS)

AWS manages:

* Infrastructure
* Networking
* Operating System
* Runtime
* Middleware

Customer manages:

* Applications
* Data

**Examples:**

* AWS Elastic Beanstalk
* AWS App Runner

---

## Function as a Service (FaaS)

Customers only write and deploy application code as functions while AWS manages the entire underlying infrastructure.

**Example:** AWS Lambda

---

## Software as a Service (SaaS)

The service provider manages everything, and users simply consume the application.

**Examples:**

* Amazon QuickSight
* Amazon Chime
* Microsoft 365
* Salesforce

> Note: Amazon Rekognition is generally classified as an AI/ML managed service (Managed AI Service) rather than a traditional SaaS application.
