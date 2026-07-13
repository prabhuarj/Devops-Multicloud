# Getting Started with AWS Solutions Architect Associate (SAA-C03)

**Author:** Prabhu Arjunan

---

# AWS IAM (Identity and Access Management)

## What is AWS IAM?

AWS Identity and Access Management (IAM) is a **global AWS service** that enables you to securely manage access to AWS resources. IAM provides **authentication** (who you are) and **authorization** (what you can do).

IAM answers the following questions:

- Who (user, application, or service) can access AWS resources?
- What actions can they perform?
- Which AWS resources can they access?
- Under what conditions can they access those resources?

> **Note:** AWS IAM follows the **Principle of Least Privilege**, which means users and applications should receive only the minimum permissions required to perform their tasks.

---

# IAM Users, Groups, Roles, and Policies

## IAM User

An IAM User represents an individual person or application that requires access to AWS resources.

Each IAM User has a unique username within an AWS account.

### Examples

- alice
- bob
- automation-user

An IAM User can authenticate using:

- Username & Password (AWS Management Console)
- Access Keys (AWS CLI / SDK)

---

## IAM Group

An IAM Group is a collection of IAM Users with the same permissions.

Think of it as a team within an organization.

### Example Groups

- Platform Engineers
- Application Developers
- Database Administrators
- Security Team

Example:

```text
ApplicationDevelopers
├── Alice
├── Bob
└── Charlie
```

Instead of assigning permissions individually, attach the policy to the group.

---

## IAM Policy Evaluation Logic

AWS evaluates permissions in the following order:

1. **Explicit Deny**
2. **Explicit Allow**
3. **Implicit Deny**

### Example

**Application Developers**

Allowed:

- Launch EC2 Instances
- Deploy Lambda Functions

Denied:

- Modify Amazon RDS Databases

**Database Administrators**

Allowed:

- Manage Amazon RDS
- Perform Database Backups

Denied:

- Launch or Terminate EC2 Instances

> **Remember:** Explicit Deny always overrides Allow.

---

# IAM Role

An IAM Role is an AWS identity that provides **temporary security credentials**.

Unlike an IAM User, a role **does not have long-term credentials**.

Roles are commonly assumed by:

- AWS Services (EC2, Lambda, ECS)
- Federated Users (Microsoft Entra ID, Okta)
- Another AWS Account
- Applications using AWS STS

Example:

```text
EC2 Instance
      │
      ▼
Assume EC2Role
      │
      ▼
STS Temporary Credentials
      │
      ▼
Amazon S3
```

### Common Use Cases

- EC2 accessing Amazon S3
- Lambda accessing DynamoDB
- ECS Tasks accessing Secrets Manager
- GitHub Actions deploying infrastructure
- Cross-account access
- Identity Federation

---

# IAM Principals

An IAM Principal is any identity that can authenticate and make requests to AWS.

Examples:

- IAM User
- IAM Role
- Federated User
- AWS Service

---

# Assuming an IAM Role

To gain temporary permissions, an identity must **assume an IAM Role**.

Example:

```text
Development Account
        │
        ▼
Developer assumes ProductionAccessRole
        │
        ▼
AWS STS
        │
        ▼
Temporary Credentials
        │
        ▼
Production AWS Resources
```

The target role must trust the source identity using a **Trust Policy**.

---

# IAM Policies

Policies define **what permissions** an IAM Principal receives.

Policies are written in **JSON**.

Policies can be attached to:

- IAM Users
- IAM Groups
- IAM Roles

AWS recommends using **Customer Managed Policies** or **AWS Managed Policies** instead of creating many inline policies.

---

# Key Elements of an IAM Policy

## Version

Specifies the policy language version.

```json
"Version": "2012-10-17"
```

---

## Statement

Contains one or more permission statements.

---

## Effect

Defines whether the action is:

- Allow
- Deny

---

## Action

Specifies AWS API operations.

Examples:

```
ec2:RunInstances
ec2:DescribeInstances
s3:GetObject
lambda:InvokeFunction
```

---

## Resource

Specifies which AWS resource the policy applies to.

Examples:

- Specific S3 Bucket
- Specific EC2 Instance
- All Resources (`*`)

---

## Condition

Adds fine-grained restrictions.

Examples:

- Source IP
- AWS Region
- MFA
- Time
- Resource Tags

---

# IAM Access Analyzer and Credential Reports

## Credential Report

Provides a downloadable report containing:

- IAM Users
- Password Status
- MFA Status
- Access Keys
- Password Age
- Access Key Age

Useful for security audits.

---

## IAM Access Advisor

Shows which AWS services a user or role has recently accessed.

Useful for removing unused permissions.

---

## IAM Access Analyzer

Analyzes IAM and resource-based policies to identify:

- Public Access
- Cross-Account Access
- External Access
- Overly Permissive Policies

---

# Access Keys

Access Keys provide programmatic access to AWS.

Each Access Key consists of:

## Access Key ID

Acts like a username.

## Secret Access Key

Acts like a password.

Used by:

- AWS CLI
- AWS SDK
- Third-party Applications

---

## Best Practices

- Rotate access keys regularly.
- Never commit access keys to Git repositories.
- Never share access keys.
- Store secrets securely using AWS Secrets Manager or Parameter Store.
- Prefer IAM Roles over long-term access keys.

---

## Common Use Cases

- Local Development
- AWS CLI
- Applications running outside AWS
- Third-party monitoring tools
- On-premises applications accessing AWS services

---

# IAM Security Best Practices

- Do not use the Root User for daily activities.
- Enable MFA for the Root User.
- Create individual IAM Users (if not using federation).
- Never share credentials.
- Use IAM Roles for AWS services.
- Follow the Principle of Least Privilege.
- Enforce strong password policies.
- Rotate credentials regularly.
- Enable CloudTrail for auditing.
- Regularly review IAM Access Analyzer findings.

---

# Advanced IAM Topics

## Identity Federation

Identity Federation allows users from external Identity Providers such as:

- Microsoft Entra ID
- Okta
- Google Workspace

to access AWS **without creating IAM Users**.

Authentication is performed by the Identity Provider.

AWS authorizes the user by allowing them to assume an IAM Role and issuing temporary credentials through AWS STS.

### Authentication Flow

```text
User
    │
    ▼
Microsoft Entra ID / Okta
    │
    ▼
SAML Assertion / OIDC Token
    │
    ▼
AWS STS
    │
    ▼
Temporary Credentials
    │
    ▼
AWS Resources
```

---

## Resource-Based Policies

Some AWS resources allow policies to be attached directly to the resource.

Examples:

- Amazon S3 Bucket Policy
- Amazon SNS Topic Policy
- Amazon SQS Queue Policy
- AWS KMS Key Policy

These policies define:

- Who can access the resource
- What actions they can perform

---

## Cross-Account Access

Cross-account access allows users or roles from one AWS account to access resources in another AWS account.

Example:

```text
Developer (Development Account)
          │
          ▼
Assume ProductionAccessRole
          │
          ▼
AWS STS
          │
          ▼
Temporary Credentials
          │
          ▼
Production AWS Resources
```

No IAM User needs to be created in the target account.

---

## IAM Condition Keys

Condition Keys provide fine-grained access control.

### Common Examples

| Condition Key | Description |
|---------------|-------------|
| `aws:SourceIp` | Restrict access to specific IP addresses |
| `aws:RequestedRegion` | Restrict actions to specific AWS Regions |
| `aws:MultiFactorAuthPresent` | Require MFA |
| `aws:CurrentTime` | Restrict access based on time |
| `s3:prefix` | Restrict access to specific folders in an S3 bucket |

---

# Exam Tips

✅ IAM is a **Global Service**

✅ IAM follows the **Principle of Least Privilege**

✅ **Explicit Deny** always overrides Allow.

✅ IAM Roles provide **temporary credentials** using AWS STS.

✅ Prefer **IAM Roles** over long-term Access Keys.

✅ IAM Users are recommended only for individual users who require long-term credentials.

✅ Federated users receive permissions through **IAM Roles**, not IAM Users.

✅ Access Analyzer answers:

> **Who can access my resources?**

✅ Policy Simulator answers:

> **What can this user or role access?**
>
> ---

# IAM Troubleshooting Tools

In enterprise environments, IAM permission issues are rarely resolved by manually reading every IAM policy. AWS provides several tools that help administrators quickly determine **why an action is allowed or denied** by evaluating the user's effective permissions.

## IAM Policy Simulator

The **IAM Policy Simulator** allows you to test whether a user, group, or role can perform a specific action on an AWS resource.

**Question it answers:**

> **"Can this user or role perform this action?"**

**Example:**

```text
Can Alice launch an EC2 instance?

Result:
✔ Allowed

or

❌ Denied (Explicit Deny)
```

---

## AWS CLI - simulate-principal-policy

AWS also provides a CLI/API to simulate the effective permissions of a principal.

```bash
aws iam simulate-principal-policy \
--policy-source-arn arn:aws:iam::123456789012:user/Alice \
--action-names ec2:RunInstances
```

The command evaluates all applicable IAM policies and returns whether the requested action is:

- Allowed
- Explicitly Denied
- Implicitly Denied

This is especially useful for automation and troubleshooting from the command line.

---

## IAM Access Analyzer

IAM Access Analyzer helps identify **who can access your AWS resources** by analyzing IAM and resource-based policies.

It detects:

- Public access
- Cross-account access
- External access
- Overly permissive policies

**Question it answers:**

> **"Who can access this resource?"**

---

## CloudTrail

AWS CloudTrail records every API call made in your AWS account.

When troubleshooting an **AccessDenied** error, CloudTrail helps identify:

- The IAM user or role making the request
- The API action that failed
- The timestamp of the request
- The reason for the failure (where applicable)

---

### Enterprise Best Practice

In enterprise environments, IAM permission issues are typically resolved within a few minutes by using tools such as **IAM Policy Simulator**, **IAM Access Analyzer**, **CloudTrail**, and the **`simulate-principal-policy`** CLI/API. These tools evaluate the user's **effective permissions**, eliminating the need to manually inspect every IAM policy attached to users, groups, and roles.
