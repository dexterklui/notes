---
date: 2024-10-04 (Fri)
---

# AWS Accounts

## AWS Account

- An AWS account is a **container** for identities (users) and resources.
- Creation of an AWS account needs a **unique** email address and a credit card.
  - This email address will be for the **root** user.
- As an container, an AWS account can isolate its resources from other accounts,
  so that when something goes wrong, it doesn't propagate to other accounts. So
  it is good to have multiple accounts for:
  - Different **environments** (e.g. dev, test, prod)
  - Different teams
  - Different products

## IAM

**_Identity and Access Management_** (**_IAM_**) is a service that manages
identities, e.g. users, groups and roles, with full or limited permissions.

An IAM identity is scoped with the AWS account in which it is created. They
start with no permissions.

## Setting Up

1. Create an AWS account
2. Go to account settings
3. Set up _Alternate Contacts_ for billing, operations and security
4. Under _IAM User and Role Access to Billing Information_, enable access. If
   this is not enabled, then even if we give permission to a user to access the
   billing information, it still won't be able to access it.
5. Securing the AWS Account using MFA
6. Set up billing method - e.g. billed currency
7. Set up invoice delivery and billing alerts
8. Set up budget

### Securing the Root Account

**_Multi-Factor Authentication_** (**_MFA_**)

- **_Knowledge_**: username and password
- **_Possession_**: a physical device, e.g. a phone, bank card, MFA app
- **_Inherent_**: biometrics, e.g. fingerprint, face
- **_Location_**: geolocation, network

### Billing Settings

1. Enable PDF invoices delivery by mail
2. Enable AWS Free Tier alerts
3. Enable CloudWatch billing alerts

## 🧭 Navigation

- [🔼 Back to top](#aws-accounts)
- [◀️ Back](aws.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
