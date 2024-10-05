---
date: 2024-09-27 (Fri)
---

# AWS CLI

## Help

- `aws help`
- `aws <command> help`
- `aws <command> <subcommand> help`

## Install

Currently, on Arch Linux, you can install AWS CLI v2 with `aws-cli-v2` package
from the AUR.

See
[Install or update to latest version of the AWS CLI - AWS](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
for more info.

## Configuration

Follow instructions from
[Configure the AWS CLI with IAM Identity Center authentication - AWS](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html)
to configure the AWS CLI with IAM Identity Center authentication.

Run `aws configure --profile iamadmin-general` to configure a AWS CLI profile
using an IAM user access key pair. Without the `--profile` option, the default
profile is configured.

After configuring with the access keys, you can remove where the access keys is
restored, to prevent the keys from being exposed.

## 🧭 Navigation

- [🔼 Back to top](#aws-cli)
- [◀️ Back](aws.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
