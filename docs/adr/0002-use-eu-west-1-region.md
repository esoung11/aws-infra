# 2. Use eu-west-1 (Ireland) as the AWS region

Date: 2026-10-02 (recorded 2026-10-19)
Status: Superseded by ADR-0005

## Context
Every AWS resource lives in a region, and moving later means rebuilding. The
platform is fintech-style and aimed at EU roles, so where the data lives matters.
Options: (a) eu-west-1 Ireland, (b) eu-central-1 Frankfurt, (c) eu-west-2 London.

## Decision
Use eu-west-1. It keeps data inside the EU (GDPR), it's AWS's oldest EU region so
it gets new services first, and it's usually the cheapest EU region.

## Consequences
- Data stays in the EU; good fit for EU fintech.
- Lower cost than Frankfurt for the same resources.
- London was ruled out: outside the EU since Brexit.
- Some German clients/regulators may prefer Frankfurt; moving would mean a rebuild.
- Region is hard-coded in `main.tf` (backend + provider); a change touches both.
