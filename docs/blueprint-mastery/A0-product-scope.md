# Phase A0 — Product & scope (artifact)

**Source:** [hiregypt_technical_blueprint.docx](../hiregypt_technical_blueprint.docx) (product narrative, actors, user stories) — cross-checked with [README.md](../../README.md) and the development roadmap.

## Problem

Employers need a **multi-tenant job board** to post listings, manage applicants, and optionally pay for featured visibility. 
Candidates need to **discover jobs**, apply, and track status. 
Admins need **moderation and metrics**.

## Who it is for

| Actor | Primary goals |
|--------|----------------|
| **Company** | Register (approval), post/publish/close jobs, review applicants, payments for featuring |
| **Candidate** | Register, search jobs, apply, track applications, optional CV / saved jobs |
| **Admin** | Approve companies, manage listings, view platform metrics and audit |

## MVP boundary (first shippable loop)

1. **Auth:** company + candidate registration, email verification, login, refresh, logout (JWT).
2. **Jobs:** create draft / publish / public search / close.
3. **Applications:** apply, company views/updates status, candidate tracks, email + outbox.
4. **Payments:** feature listing (Paymob) where required by product.
5. **Later:** SignalR, CV upload, saved jobs, full Hangfire suite, admin hardening.

## Out of scope for early slices

- Building every domain entity before a story needs it (YAGNI).
- Full observability/rate limits until admin/polish phase (add incrementally).

## One-page checkpoint

Before coding a slice, answer: **Which actor? Which user story id? What endpoint(s)? What DB rows change?**
