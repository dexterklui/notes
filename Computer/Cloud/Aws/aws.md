---
date: 2024-05-09 (Thu)
---

# AWS

## Notes

### General

- [AWS CLI](aws-cli.md)
- [AWS Terms and Acronyms](aws-terms-and-acronyms.md)

## New Accounts

See [aws: secure your AWS account]

1. Create Account. AWS will provide you with one root account.
2. Enable Multi-Factor Authentiation (MFA) for the root account.
3. Enable IAM Identity Center, and create an AWS organization in the process.
4. Set up users in IAM Identity Center, and assign them to group.
5. Add the new group to your AWS account, attaching a permission set to the
   group (e.g. AWS predefined Administrator Permission Set).
6. The user will receive an invitation email. Set up the password and MFA.
7. Record the **Username** and **AWS access portal URL**, where the user is able
   to login. (Note: login with username, not with email).

## 🧭 Navigation

- [🔼 Back to top](#aws)
- [◀️ Back](../../../index.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
