# Phase A3 — API contracts & workflows (artifact)

**Sources:** blueprint API section; auth & job/application flows in roadmap.

## Top 3 flows (sequence notes)

### 1) Register company → verify email → login (US-AUTH-01 / -03)

1. `POST /api/v1/auth/register/company` — create `User` (role Company) + `Company` (`IsApproved=false`), send verification email (or log stub in dev).
2. `POST /api/v1/auth/verify-email` — mark email verified; **no JWT until verified** (per roadmap).
3. `POST /api/v1/auth/login` — **Company**: require `EmailVerified` **and** `Company.IsApproved` (admin approves later in admin phase; until then test data may pre-approve).
4. `POST /api/v1/auth/refresh` / `logout` — rotate/invalidate refresh token.

### 2) Create job → publish → public search (US-CO-01 / US-CA-01)

1. `POST /api/v1/jobs` — company creates **DRAFT**.
2. `PUT /api/v1/jobs/{id}/publish` — validate subscription/approval; **ACTIVE**; optional cache invalidation later.
3. `GET /api/v1/jobs` — public filters; featured sort first (after payments slice).

### 3) Submit application (US-CA-02)

1. `POST /api/v1/jobs/{id}/applications` — candidate; reject if duplicate or job not active.
2. Domain event / outbox → email company (+ SignalR in later phase).

## Reading order in blueprint

1. Global conventions: versioning (`/api/v1/`), auth scheme, error format (Problem Details).
2. Auth endpoints and status codes (409 duplicate, 422 validation).
3. Job & application endpoints tied to your MVP phases.
4. Integrations: Paymob webhook security; email provider assumptions.

## Artifact output

Use this file as **story index**: when picking a vertical slice, paste the relevant endpoints and expected statuses into your story notes before coding.
