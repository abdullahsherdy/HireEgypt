# HireEgypt.Domain

## Purpose of This Layer

The **Domain layer** is the heart of the application. It contains entities, value objects, enums, domain events, and repository interfaces. It has **zero external dependencies**—no NuGet packages beyond the BCL. All business rules and invariants are expressed here.

**Responsibilities:**
- Define entities and their behavior (e.g. `Company.CanPostJob()`, `JobListing.Publish()`)
- Define value objects (Email, Money, Slug, TenantId, PhoneNumber)
- Define domain events (CompanyRegisteredEvent, ApplicationSubmittedEvent, etc.)
- Define repository and unit-of-work interfaces for persistence
- Enforce invariants and status transitions

---

## Folder Architecture

```
HireEgypt.Domain/
├── Entities/        # Aggregate roots and entities (User, Company, Candidate, JobListing, etc.)
├── Enums/           # UserRole, JobType, ExperienceLevel, ListingStatus, ApplicationStatus, etc.
├── Events/          # Domain events (CompanyRegisteredEvent, ListingPublishedEvent, etc.)
├── Interfaces/     # IRepository<T>, IUnitOfWork, IJobListingRepository, ICompanyRepository, etc.
├── ValueObjects/   # Email, Money, Slug, TenantId, PhoneNumber
└── README.md       # This file
```

| Folder         | Purpose |
|----------------|---------|
| `Entities/`    | Domain entities: `User`, `Company`, `Candidate`, `JobListing`, `Application`, `Payment`, `OutboxMessage`, `SubscriptionPlan`, `SavedJob`, `AuditLog`. Includes base `BaseEntity` with Id, CreatedAt, domain events. Business logic lives here (e.g. status transitions). |
| `Enums/`       | Domain enumerations: `UserRole`, `JobType`, `ExperienceLevel`, `ListingStatus` (DRAFT/ACTIVE/CLOSED/EXPIRED), `ApplicationStatus`, `PaymentStatus`, `PaymentType`. |
| `Events/`      | Domain events raised by entities: `CompanyRegisteredEvent`, `CandidateRegisteredEvent`, `ListingPublishedEvent`, `ApplicationSubmittedEvent`, `PaymentCompletedEvent`, etc. Consumed by outbox/background processors. |
| `Interfaces/`  | Persistence abstractions: `IRepository<T>`, `IUnitOfWork`, `IJobListingRepository`, `ICandidateRepository`, `ICompanyRepository`, `IApplicationRepository`. Implemented by Infrastructure. |
| `ValueObjects/`| Immutable value types: `Email`, `Money`, `Slug`, `TenantId`, `PhoneNumber`. Encapsulate validation and formatting. |

---

## Design Principles

1. **No infrastructure** — No EF Core, no Redis, no external APIs.
2. **Rich domain** — Entities enforce rules (e.g. `Application.UpdateStatus()` validates allowed transitions).
3. **Events over direct calls** — Side effects (emails, notifications) are triggered by domain events via the outbox.
4. **Testable** — Pure C#; unit tests need no database or mocks for domain logic.

---

## Clean Architecture Position

```
     ┌─────────────────────────────────────┐
     │         HireEgypt.Domain            │  ← You are here (center)
     │  (Entities, Value Objects, Events)  │
     └─────────────────────────────────────┘
                       ▲
                       │ depends on
         ┌─────────────┴─────────────┐
         │                           │
   Application                 Infrastructure
```

Domain is the **innermost** layer; nothing in Domain depends on other projects.
