# Phase A2 — Data model & ERD (artifact)

**You should read the blueprint ERD (and related sections) before coding.** This document captures the **intent** of the model and maps **user stories → persistence** so vertical slices stay small.

## Why ERD first

- Prevents wrong cardinalities (e.g. User–Company–Candidate).
- Surfaces **constraints**: unique email, FK cascade rules, indexes for search.
- Clarifies **tenant** and **ownership** (who may update which row).

## Conceptual core (auth & tenancy)

Derived from roadmap + typical job-board shape (align names/columns with blueprint Section 5 / ERD when implementing).

| Story cluster | Tables / entities touched (minimal slice) |
|----------------|--------------------------------------------|
| US-AUTH-01 Register company | `Users`, `Companies` (+ optional `SubscriptionPlans` later) |
| US-AUTH-02 Register candidate | `Users`, `Candidates` |
| US-AUTH-03 Login / refresh | `Users` (password, verification, refresh token fields) |
| US-CO-01 / US-CA-01 Jobs | `JobListings`, `Companies`, enums/status |
| US-CA-02 Applications | `Applications`, `JobListings`, `Candidates` |
| US-CO-04 Payments | `Payments`, `JobListings` |
| Notifications | `OutboxMessages` (later slice) |

## Story → entity mapping (checkpoint)

Before migrating, list for **this story only**:

1. New entities or columns?
2. Unique/index needs?
3. Who owns the aggregate root?

## Note

When the blueprint ERD uses different table names or columns, **treat the ERD as source of truth** and adjust entity configurations in Infrastructure to match.
