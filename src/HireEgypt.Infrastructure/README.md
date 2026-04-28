# HireEgypt.Infrastructure

## Purpose of This Layer

The **Infrastructure layer** implements all technical concerns: persistence, caching, background jobs, real-time hubs, and external integrations. It depends on Domain (and Application where interfaces are defined there) and **implements** the abstractions they define.

**Responsibilities:**
- Implement `IRepository<T>`, `IUnitOfWork`, and specialized repositories
- Configure EF Core (DbContext, migrations, entity configurations)
- Implement caching (Redis)
- Run background jobs (Hangfire): outbox processor, listing expiry, digest emails
- Host SignalR hubs for real-time notifications
- Integrate with external services (Paymob, SendGrid, Azure Blob)

---

## Folder Architecture

```
HireEgypt.Infrastructure/
├── BackgroundJobs/   # Hangfire jobs (OutboxProcessor, ListingExpiryJob, FeaturedExpiryJob, etc.)
├── Cache/            # Redis cache implementation, cache invalidation
├── Hubs/             # SignalR hubs (NotificationHub for company real-time updates)
├── Integrations/     # Paymob, email (SendGrid/MailKit), Azure Blob, CV parsing
├── Persistence/      # AppDbContext, EF configurations, repository implementations, migrations
└── README.md         # This file
```

| Folder            | Purpose |
|-------------------|---------|
| `BackgroundJobs/` | Hangfire recurring jobs: `OutboxProcessor` (dispatches domain events), `ListingExpiryJob`, `FeaturedExpiryJob`, `CandidateDigestJob`, `CompanyWeeklyReport`, `MetricsCacheWarmup`, `CvParseJob`. |
| `Cache/`          | Redis caching for search results, metrics, rate limits. Cache invalidation on publish/close. |
| `Hubs/`           | SignalR `NotificationHub` — company-group messaging for new applications and status updates. |
| `Integrations/`   | External service adapters: Paymob (payments), email provider (SendGrid/MailKit), Azure Blob (CV uploads), CV parsing logic. |
| `Persistence/`    | `AppDbContext`, Fluent API entity configs, `Repository<T>`, `UnitOfWork`, specialized repositories (`JobListingRepository`, etc.), EF Core migrations. |

---

## Key Patterns

| Pattern              | Usage |
|----------------------|-------|
| **Transactional Outbox** | Domain events written to `OutboxMessage` in same transaction; `OutboxProcessor` dispatches them asynchronously. |
| **Hangfire**         | Background and scheduled jobs instead of custom hosted services. |
| **Repository**        | Abstracts EF Core; Application uses `IRepository<T>` and `IUnitOfWork`. |

---

## Dependencies

- **HireEgypt.Domain** — Entities, interfaces
- **HireEgypt.Application** — Application service interfaces (when defined there)

---

## Clean Architecture Position

```
     ┌─────────────────────────────────────┐
     │      HireEgypt.Infrastructure        │  ← You are here
     │  (EF Core, Hangfire, SignalR, Redis) │
     └─────────────────┬───────────────────┘
                       │
         ┌─────────────┼─────────────┐
         ▼             ▼             ▼
     Domain      Application    (external APIs)
```
