# DECISIONS

## 2026-03-13 - Clean Architecture Baseline

The project uses Clean Architecture with four projects: `HireEgypt.API`, `HireEgypt.Application`, `HireEgypt.Domain`, and `HireEgypt.Infrastructure`. Dependencies are directed inward to protect domain logic from framework details and external services. This setup keeps business rules testable and allows infrastructure choices (database, email provider, cache) to evolve with minimal impact on core logic.

## 2026-03-13 - CQRS in Application Layer

The application layer will use command/query separation through MediatR handlers. This keeps API controllers thin, centralizes use-case logic, and provides a clean place for validation and pipeline behaviors. It also fits naturally with async workflows and outbox-driven side effects planned for later phases.

## 2026-03-13 - Transactional Outbox Pattern

Domain events that require side effects (emails, SignalR notifications, payment reactions) will be written as outbox records in the same transaction as business data. A background processor will dispatch unprocessed records. This approach improves reliability and avoids data/message inconsistency during failures.

## 2026-03-13 - Hangfire for Background Processing

Background and scheduled workloads (outbox processing, listing expiry, digests, CV parsing) will run via Hangfire instead of hand-rolled hosted services. Hangfire provides persistence, retries, observability, and scheduling features that reduce custom operational code and support production behavior.
