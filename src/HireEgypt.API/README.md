# HireEgypt.API

## Purpose of This Layer

The **API layer** is the HTTP host and entry point of the application. It exposes REST endpoints, configures authentication, wires up dependency injection, and applies cross-cutting middleware. This layer stays **thin**—controllers delegate to the Application layer via MediatR and do not contain business logic.

**Responsibilities:**
- Expose API endpoints (`/api/v1/...`)
- Configure JWT authentication and Swagger
- Register middleware (exception handling, tenant resolution, rate limiting)
- Dependency injection setup for all layers

---

## Folder Architecture

```
HireEgypt.API/
├── Controllers/          # HTTP controllers — thin, call MediatR
├── Middleware/           # Custom middleware (ExceptionHandler, TenantResolver, etc.)
├── Properties/           # launchSettings.json, assembly metadata
├── Program.cs            # Host configuration, DI, pipeline
├── appsettings.json     # Configuration (env-specific overrides in appsettings.Development.json)
├── HireEgypt.http        # HTTP file for manual API testing
└── README.md             # This file
```

| Folder / File      | Purpose |
|--------------------|---------|
| `Controllers/`     | ASP.NET Core controllers. Each controller action sends a command/query to MediatR and returns the result. No business logic here. |
| `Middleware/`      | Custom middleware components: global exception handling (RFC 7807 Problem Details), tenant resolution from headers/claims, rate limiting. |
| `Properties/`      | Build and launch settings (ports, environment variables). |
| `Program.cs`       | Application bootstrap: services registration, middleware pipeline, JWT config, Swagger. |

---

## Dependencies

- **HireEgypt.Application** — Commands, queries, DTOs, validators
- **HireEgypt.Infrastructure** — Database, cache, external integrations, background jobs

---

## Clean Architecture Position

```
     ┌─────────────────────────────────────┐
     │         HireEgypt.API               │  ← You are here
     │  (Controllers, Middleware, DI)      │
     └─────────────────┬───────────────────┘
                       │
         ┌─────────────┴─────────────┐
         ▼                           ▼
  Application                  Infrastructure
```
