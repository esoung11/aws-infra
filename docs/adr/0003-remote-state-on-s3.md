# 3. Store Terraform state remotely in S3

Date: 2026-10-02 (recorded 2026-10-19)
Status: Accepted

## Context
Terraform state is the map between my config and the real AWS resources. By
default it's a local file on the ThinkPad. If the ThinkPad dies, the state is
gone and Terraform no longer knows what it built. State can also contain secrets
(e.g. database passwords), so where it's stored matters.
Options: (a) local file, (b) S3 backend, (c) Terraform Cloud.

## Decision
Use the S3 backend (`esoung-tfstate-4821`) with `encrypt = true` and
`use_lockfile = true`. The bucket is versioned, encrypted and public-access-blocked.

## Consequences
- State survives losing the ThinkPad.
- Encryption protects the state (and any secrets in it) at rest if the bucket is breached.
- Locking stops two `terraform apply` runs writing state at the same time and corrupting it.
- Versioning lets me roll back to an earlier state file.
- Chicken-and-egg: the state bucket is managed by the same config that stores state in it.
- Anyone with S3 read on the bucket can read the state, so bucket access must stay tight.
