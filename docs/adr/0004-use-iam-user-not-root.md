# 4. Use a dedicated IAM user for Terraform, not the root account

Date: 2026-10-02 (recorded 2026-10-19)
Status: Accepted

## Context
Terraform needs AWS credentials. The root account has unlimited power: it can
do anything, including closing the account and changing billing, and it can't be
restricted by IAM policies. If root keys leak, the attacker has everything.
Options: (a) root access keys, (b) a dedicated IAM user, (c) IAM Identity Center (SSO) with short-lived credentials.

## Decision
Create a dedicated IAM user `terraform-cli` with access keys in `~/.aws`. Root is
used only for account-level tasks and never has access keys.

## Consequences
- A leaked key can be revoked without touching root.
- Activity is attributable to `terraform-cli` in CloudTrail.
- Weak point: `terraform-cli` has `AdministratorAccess`, so it's not least-privilege.
  A leak is still close to full control of AWS resources.
- Long-lived access keys on disk; SSO with short-lived credentials would be better.
- Follow-up: scope the policy down to what Terraform actually manages.
