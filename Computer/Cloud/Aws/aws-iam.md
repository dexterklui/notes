---
date: 2024-10-05 (Sat)
---

# AWS IAM

## Overview

### Why IAM?

- Root user cannot be restricted and is fully trusted by its account.
- To manage access to AWS resources, IAM comes in
- Each AWS account has its own IAM
- IAM is a **global service** - one global database for an AWS account
  - It is a globally resilient service, so can be cope with failures of large
    sections of the AWS infrastructure

### Three Main Functions

- Create and manage [_identities_](#identities) as an ID Provider (**_IDP_**)
- Authenticate identities
- Authorize identities with permissions

### General Features

- Generally, IAM is fully trusted by its AWS account, just like a root user,
  except some billing control and closing the account.
- No cost
- There are limits to the number of IAM entities you can create
- Allow or Deny its identities through policies
- No direct control on external accounts or users
- Support Identity federation and MFA

## Identities

- **User**: represents humans or applications that need access
- **Group**: a collection of related users. E.g. dev, finance, HR
- **Role**: Used by AWS services or for granting external access

Usually you use _user_ if you can identify the entity using that user, but you
use _role_ if you are granting a varying number of entities access.

## IAM Policy

An **_IAM policy_** or **_policy document_** is a JSON document that defines
access permissions, and when they are attached to an identity to actually grant
permissions.

## IAM User Login URL

- In IAM Dashboard > AWS Account, you see the sign-in URL for IAM users in this
  account. The URL is in the format:
  `https://<account-alias/account-id>.signin.aws.amazon.com/console`

## IAM Long Term Credentials

- **_Long-term credentials_** are not regularly rotated or changed
- Access keys are long-term credentials for programmatic access
- It is used by AWS CLI, SDKs, and calls to AWS API
- Access keys can be created, deleted, made inactive or made active
- Access key also have two parts:
  - Access Key ID: the public part
  - Secret Access Key: the private part - can only be seen once when created
- An IAM user can have at most two access keys
  - Two because you can **_rotate_** the keys - create a new one to replace the
    old one, then delete the old one
- Other than root user (not recommended) and IAM users, no one uses access keys

## 🧭 Navigation

- [🔼 Back to top](#aws-iam)
- [◀️ Back](aws.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
