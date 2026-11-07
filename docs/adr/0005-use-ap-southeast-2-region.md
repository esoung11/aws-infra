# 5. Use ap-southeast-2 (Sydney) as the AWS region

Date: 2026-11-07
Status: Accepted. Supersedes ADR-0002.

## Context
ADR-0002 chose eu-west-1 because I was targeting EU fintech roles. I'm based in
NZ and now targeting cloud security roles here, so the platform should run where
NZ companies actually run. Only the Terraform state bucket existed, so moving
now was cheap; later it would mean rebuilding everything.
Options: (a) stay in eu-west-1 Ireland, (b) ap-southeast-6 Auckland, (c) ap-southeast-2 Sydney.

## Decision
Use ap-southeast-2. It's mature, with the full set of services, and it's where
most NZ companies already run. I'm starting out and want the project to run as
smoothly and realistically as possible.

## Consequences
- Matches what I'd see in an NZ job; docs, tutorials and services all available.
- Data leaves NZ (sits in Australia). NZ organisations with strict data
  sovereignty requirements would want Auckland.
- Auckland rejected for now: newer, fewer services, opt-in region. Revisit if a
  project needs data kept in NZ.
- The move: new state bucket `esoung-tfstate-apse2-project` in `bootstrap/`; the
  Ireland bucket `esoung-tfstate-4821` and its config were destroyed.
