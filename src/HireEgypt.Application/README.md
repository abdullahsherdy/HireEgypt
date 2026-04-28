# HireEgypt.Application

## Purpose of This Layer

The **Application layer** (Use Cases) contains all business workflows. It receives commands and queries from the API, coordinates with the Domain, and uses Infrastructure services via interfaces. This layer implements **CQRS** through MediatR handlers—commands change state, queries return data.

**Responsibilities:**
- Define use cases as commands and queries
- Implement handlers (business orchestration, validation)
- Define DTOs for requests and responses
- Validate input with FluentValidation
- Depend only on Domain interfaces (no infrastructure details)

---

## Folder Architecture

```
HireEgypt.Application/
├── Auth/                 # Authentication use cases (Register, Login, Refresh, VerifyEmail)
├── Common/               # Shared DTOs, behaviors, mapper profiles
├── Interfaces/           # Application-layer interfaces (e.g. IEmailService, ITokenService)
├── (future) Jobs/        # Job listing commands & queries
├── (future) Applications/# Application submission & status commands & queries
├── (future) Payments/    # Payment initiation & webhook handling
└── README.md             # This file
```

| Folder           | Purpose |
|------------------|---------|
| `Auth/`          | Authentication use cases: `RegisterCompany`, `RegisterCandidate`, `Login`, `RefreshToken`, `Logout`, `VerifyEmail` — commands, handlers, validators. |
| `Common/`        | Shared types: base DTOs, mapping profiles, pipeline behaviors, common validators. |
| `Interfaces/`    | Application-level service abstractions implemented by Infrastructure (e.g. email, tokens, storage). Domain interfaces live in Domain; these are for app-level concerns. |

---

## CQRS Pattern

| Type     | Purpose                           | Example |
|----------|-----------------------------------|---------|
| **Command** | Mutates state, returns minimal data | `RegisterCompanyCommand`, `PublishJobListingCommand` |
| **Query**   | Reads data, no side effects        | `SearchJobListingsQuery`, `GetCompanyJobListingsQuery` |

Handlers are registered with MediatR; controllers send `IRequest<T>` and receive `T`.

---

## Dependencies

- **HireEgypt.Domain** — Entities, value objects, domain events, repository interfaces

**No dependency on:** Infrastructure, API (to keep business logic testable and decoupled).

---

## Clean Architecture Position

```
     ┌─────────────────────────────────────┐
     │      HireEgypt.Application          │  ← You are here
     │  (Commands, Queries, DTOs, Handlers) │
     └─────────────────┬───────────────────┘
                       │
                       ▼
                   Domain
```
