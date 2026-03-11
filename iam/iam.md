# Identity and Access Management

IAM (Identity and Access Management) is a **global service**

## Users & Groups

- **Root account** created by default should not be used or shared
- **Users** are people within your organization and can be **grouped**
- **Groups** only contains users
- User don't have to belong to a group and can belong to multiple groups

## Permissions

### Policies

JSON documents that define a set of permissions for making requests to AWS services, and can be used by IAM Users, User Groups, and IAM Roles.

- Users & Groups can be assigned JSON-formatted **policies**
- These policies define the permissions of users
- **Least privilege principle** recommended to be applied

### Policies Structure

Policy consists of:

- **Version**: policy language version
- **Id**: an identifier for the policy *(optional)*
- **Statement**: one or more individual statements
    - **Sid**: an identifier for the statement *(optional)*
    - **Effect**: whether the statement allows or denies access
    - **Principal**: account/user/role to which this policy is applied to
    - **Action**: list of actions this policy allows or denies
    - **Resource**: list of resources to which the actions are applied to
    - **Condition**: conditions for when this policy is in effect *(optional)*

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "elasticloadbalancing:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:ListMetrics",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:Describe*"
            ],
            "Resource": "*"
        }
    ]
}
```

- Example: AdministratorAccess permission policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

## Password Policy

When setting up a password policy for users it is possible to:
- specify minimum password length
- require specific character types
- allow IAM users to change their own passwords
- require users to change their password after some time
- prevent password re-use
- enable multi factor authentication

### MFA device options

- Virtual MFA device
    - Google Authenticator
    - Authy
    - support for multiple tokens on a single device
- Universal 2nd Factor (U2F) Security Key
    - YubiKey
    - support for multiple root and IAM users using a single security key
- Hardware Key Fob MFA Device
    - Gemalto
    - SurePassID

## Different ways to access AWS

- There are three options to access AWS:
    - AWS Management Console - protected by password + MFA
    - AWS Command Line Interface (CLI) - protected by access keys
    - AWS Software Development Kit (SDK) - protected by access keys

### Access Keys

- Access Keys are generated through the AWS Console
- Users manage their own access keys
- Access Key ID is the username equivalent while Secret Access Key is similar to password

### AWS CLI

- enables interacting with AWS using commands in command line shell
- direct access to the public APIs of AWS
- enables scripting
- it's [open source](https://github.com/aws/aws-cli)
- alternative to management console

### AWS SDK

- Language-specific APIs (set of libraries)
- Enables to access and manage AWS programmatically
- Embedded within application code
- Supports
    - SDKs (JS, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++)
    - Mobile SDKs (Android, iOS, ...)
    - IoT Device SDKs (Embedded C, Arduino, ...)
- AWS CLI is built with AWS SDK for Python

## IAM Roles for Services

- Some AWS services will need to perform actions on your behalf
- To do so, permissions have to be assigned to services through IAM Roles
- Common roles:
    - EC2 instance roles
    - Lambda function roles
    - CloudFormation roles

### IAM Roles

- **identities** that can be created with specific permissions
- similar to IAM users, but instead of being associated with one person, they **are assumable by anyone who needs them**
- **does not have long-term credentials**, provides temporary ones instead
- can be used to **delegate access to users, applications, or services** that don't normally have access to AWS resources 

### IAM Role Trust Policy

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "ec2.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

## IAM Security Tools

### IAM Credentials Report (account-level)

- a report that lists all your account's users and the status of their various credentials

### IAM Access Advisor (user-level)

- shows the service permissions granted to a user and when those services were last accessed

## IAM Guidelines & Best Practices

- Don’t use the root account except for AWS account setup
- One physical user = One AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce the use of Multi Factor Authentication (MFA)
- Create and use Roles for giving permissions to AWS services
- Use Access Keys for Programmatic Access (CLI / SDK)
- Audit permissions of your account using IAM Credentials Report & IAM Access Advisor
- Never share IAM users & Access Keys

## Shared Responsibility Model for IAM

AWS is responsible for the infrastructure. The user is responsible for the use of this infrastructure.

### AWS Responsibility

- Infrastructure (global network security)
- Configuration and vulnerability analysis
- Compliance validation 

### User Responsibility

- Users, Groups, Roles, Policies management and monitoring
- Enable MFA on all accounts
- Rotate all your keys often
- Use IAM tools to apply appropriate permissions
- Analyze access patterns & review permissions

## IAM Section – Summary

- **Users**: 
    - mapped to a physical user
    - each has a password for AWS Console.
- **Groups**: 
    - contains users only
- **Policies**: 
    - JSON document that outlines permissions for users or groups
- **Roles**: 
    - identities that can be created with specific permissions and assumed by users, applications, or services
- **Security**: 
    - MFA + Password Policy
- **AWS CLI**: 
    - manage your AWS services using the command-line
- **AWS SDK**: 
    - manage your AWS services using a programming language
- **Access Keys**: 
    - access AWS using the CLI or SDK
- **Audit**: 
    - IAM Credential Reports & IAM Access Advisor

