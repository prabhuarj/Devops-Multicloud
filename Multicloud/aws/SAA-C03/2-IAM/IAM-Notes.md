Getting Started with AWS Solutions Architect Associate (SAA-C03)

Author: Prabhu Arjunan

AWS IAM – Identity and Access Management
What is AWS IAM?

AWS Identity and Access Management (IAM) is a global AWS service that enables you to securely manage access to AWS resources. It controls who can access AWS resources, what actions they can perform, and under what conditions they can perform them.

IAM answers the following questions:

Who (user, application, or service) can access AWS resources?
What actions can they perform?
Which AWS resources can they access?
Under what conditions is access allowed?

Note: IAM follows the Principle of Least Privilege, meaning users and applications should receive only the minimum permissions required to perform their tasks.

IAM Users, Groups, Roles, and Policies
IAM User

An IAM User represents an individual person or application that requires access to AWS resources.

Each IAM User has a unique username within an AWS account.

Examples

alice
bob
automation-user

Users can authenticate using:

Username and Password (AWS Management Console)
Access Keys (AWS CLI or SDK)
IAM Group

An IAM Group is a collection of IAM Users who require the same permissions.

Think of it as a team within an organization.

Example Groups:

Platform Engineers
Application Developers
Database Administrators
Security Team

Example:

ApplicationDevelopers
    ├── Alice
    ├── Bob
    └── Charlie

Instead of assigning permissions individually, you assign permissions once to the group.

IAM Policy Evaluation

IAM evaluates permissions in the following order:

Explicit Deny
Explicit Allow
Implicit Deny

Example

Suppose you have two teams:

Application Developers

Allowed:

Launch EC2 instances
Deploy Lambda functions

Denied:

Modify Amazon RDS databases
Database Administrators (DBA)

Allowed:

Manage Amazon RDS
Perform database backups

Denied:

Launch or terminate EC2 instances

Even if another policy allows an action, an Explicit Deny always overrides it.

IAM Role

An IAM Role is an AWS identity that provides temporary permissions.

Unlike an IAM User, a role has no permanent credentials.

Roles are commonly assumed by:

AWS Services (EC2, Lambda, ECS)
Federated Users (Microsoft Entra ID, Okta)
Another AWS Account
Applications using STS

Example:

EC2 Instance

↓

Assume EC2Role

↓

Temporary Credentials (STS)

↓

Access Amazon S3

Common use cases:

EC2 accessing Amazon S3
Lambda accessing DynamoDB
GitHub Actions deploying infrastructure
Cross-account access
Federated users accessing AWS
IAM Principals

An IAM Principal is any identity that can authenticate and make requests to AWS.

Examples:

IAM User
IAM Role
Federated User
AWS Service
Assuming a Role

To access another AWS resource using temporary credentials, an identity must assume an IAM Role.

Example:

Development Account

↓

Developer assumes ProductionAccessRole

↓

STS

↓

Temporary Credentials

↓

Production AWS Resources

The target IAM Role must include a Trust Policy allowing the source account, user, or service to assume the role.

IAM Policies

Policies define what permissions are granted to IAM Principals.

Policies are written in JSON.

Policies can be attached to:

IAM Users
IAM Groups
IAM Roles

AWS recommends using Customer Managed Policies or AWS Managed Policies instead of creating many inline policies.

Key Elements of an IAM Policy
Version

Specifies the policy language version.

Current version:

"Version": "2012-10-17"
Statement

Contains one or more permission statements.

Effect

Specifies whether the action is:

Allow
Deny
Action

Specifies the API operations allowed or denied.

Examples:

ec2:RunInstances

ec2:DescribeInstances

s3:GetObject

lambda:InvokeFunction
Resource

Specifies which AWS resource the action applies to.

Example:

Specific S3 Bucket

Specific EC2 Instance

All Resources (*)
Condition

Provides fine-grained access control.

Examples:

Source IP
AWS Region
Time of Day
MFA Required
IAM Access Analyzer and Credential Reports
Credential Report

Provides a downloadable report containing:

IAM Users
Password status
MFA status
Access Keys
Password last changed
Access key age

Useful for security audits.

IAM Access Advisor

Shows which AWS services a user or role has recently accessed.

Useful for removing unused permissions.

IAM Access Analyzer

Analyzes IAM and resource-based policies to identify:

Public access
Cross-account access
External access
Overly permissive policies
Access Keys

Access Keys provide programmatic access to AWS.

Each Access Key consists of:

Access Key ID

Acts like a username.

Secret Access Key

Acts like a password.

Used by:

AWS CLI
AWS SDK
Third-party applications
Best Practices
Rotate access keys regularly.
Never commit keys to Git repositories.
Never share access keys.
Use IAM Roles instead of long-term access keys whenever possible.
Common Use Cases
Local development using AWS CLI
Applications running outside AWS
Third-party monitoring tools
On-premises applications accessing AWS services
Security Best Practices
Avoid using the Root User for daily activities.
Enable MFA for the Root User.
Create individual IAM Users (if not using federation).
Never share credentials.
Use IAM Roles for AWS services.
Follow the Principle of Least Privilege.
Enforce strong password policies.
Rotate credentials regularly.
Use CloudTrail for auditing.
Regularly review IAM Access Analyzer findings.
Advanced IAM Topics
Identity Federation

Allows users from external Identity Providers such as:

Microsoft Entra ID
Okta
Google Workspace

to access AWS without creating IAM Users.

Authentication is performed by the Identity Provider.

AWS authorizes access by assigning an IAM Role and issuing temporary STS credentials.

Resource-Based Policies

Some AWS resources support policies attached directly to the resource.

Examples:

Amazon S3 Bucket Policy
SNS Topic Policy
SQS Queue Policy
KMS Key Policy

These policies define who can access the resource and what actions they can perform.

Cross-Account Access

Allows users or roles from one AWS account to access resources in another AWS account.

Example:

Developer (Development Account)

↓

Assume ProductionAccessRole

↓

STS

↓

Temporary Credentials

↓

Access Production Resources

No IAM User needs to be created in the target account.

IAM Condition Keys

Condition keys provide fine-grained access control.

Common examples:

aws:SourceIp → Restrict access from specific IP addresses.
aws:RequestedRegion → Restrict access to selected AWS Regions.
aws:MultiFactorAuthPresent → Require MFA.
s3:prefix → Restrict access to specific folders within an S3 bucket.
Overall Feedback
