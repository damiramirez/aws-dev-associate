# aws-dev-associate
Repo for https://learn.cantrill.io/p/aws-certified-developer-associate

## AWS Accounts

### Account

An AWS Account is a container for identities (users) and Resources.

- You need an account name, unique email address and credit card
- There is a unique Root user - Full control over all of the AWS Account
- Resource bill to the AWS Account payment method as they are consumed
- By default all access to an AWS Account and resources is denied except for the Root User -> You can granted access to external identities

You can create additional identities inside the AWS Account that can be restricted

- IAM users, Groups and Roles
- Any IAM identity starts off with no permissions

A good practice is to use separate accounts for separate things (DEV, TEST, PROD) or teams or products or client. Another good practice is not to use the root account as it has full privileges. Instead, it is recommended to create other identities using IAM (Identity and Access Management) and grant them the necessary permissions to perform their tasks.

### MFA

AWS Multi-Factor Authentication (MFA) is a simple best practice that adds an extra layer of protection on top of your user name and password. With MFA enabled, when a user signs in to an AWS Management Console, they will be prompted for their user name and password (the first factor—what they know), as well as for an authentication code from their AWS MFA device (the second factor—what they have). Taken together, these multiple factors provide increased security for your AWS account settings and resources.

### IAM

*Manages identities - Authenticate - Authorize*

IAM is what allows additional identities to be created within an AWS account - identities which can be given restricted levels of access. IAM identities start with no permissions on an AWS Account, but can be granted permissions (almost) up to those held by the Account Root User.

With IAM you can create:

1. IAM User - Human or application that need access to your account
2. IAM Group - Collection of related users (development team, finance or HR)
3. IAM Role - Use by AWS Service or for granting external access to your account

IAM Policy Document can be used to allow or deny access to AWS Services only when they are attached to Users, Groups and Roles.

There is no cost associated with creating users, groups or roles but there is a limit. IAM is a global resilient service. Only controls what identities can do via policies. No direct control on external accounts or users. Identity federation(Facebook, Twitter, Google, etc) and MFA.

### IAM Access Keys

Access keys are how the AWS Command Line Tools (CLI Tools) interact with AWS accounts.

- Access Key ID
- Secret Access Key