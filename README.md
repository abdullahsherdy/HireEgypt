# HireEgypt

Multi-tenant job board backend (Clean Architecture). See `docs/hiregypt_technical_blueprint.docx` for the full spec.

## What’s in this repo (quick map)

| Item | Why it exists |
|------|----------------|
| `src/HireEgypt.Domain` | Entities, value objects, domain events — **no** database or framework code. |
| `src/HireEgypt.Application` | Use cases (commands/queries later), DTOs, validators — depends only on Domain. |
| `src/HireEgypt.Infrastructure` | EF Core, Hangfire, email, Paymob, Redis, etc. — **implements** interfaces the Application layer defines. |
| `src/HireEgypt.API` | HTTP host: controllers, middleware, DI wiring — thin layer. |
| `docker-compose.yml` | Local **SQL Server** and **Redis** so you can run the app without installing them on the machine. |
| `DECISIONS.md` | Short notes on **why** we chose patterns (Clean Architecture, Outbox, Hangfire). Useful for you and for reviews. |

You only need **one** Infrastructure project: `HireEgypt.Infrastructure`. The old folder name `HireEgypt.Infrustructure` was a typo and has been removed.

## Run locally (later)

```bash
docker compose up -d
dotnet build src/HireEgypt.API/HireEgypt.sln
```

Change the SQL `SA_PASSWORD` in `docker-compose.yml` for anything beyond local dev.
