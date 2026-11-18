# api-gateway-backend

The single entry point.

Part of [Finance Master](https://github.com/financemaster-proj/platform) — a
personal finance system built as four backend services behind an API gateway.

## What this service owns

**Nothing.** It has no database and no business logic. It routes requests, validates JWTs at the edge, applies Redis-backed rate limiting, and injects correlation IDs.

## What it does not own

Any domain data or authorisation decision beyond "is this token valid". Services validate the token again themselves — defence in depth, and it keeps them safe if reached directly.

## Interface

| | |
|---|---|
| Port | `8080` |
| Database | `— (none)` |
| Emits | — |
| Consumes | — |

## Running locally

Infrastructure lives in the [`platform`](https://github.com/financemaster-proj/platform)
repo. Clone it as a sibling of this one, then:

```bash
cd ../platform && docker compose -f infra/docker-compose.yml up -d
cd - && cp .env.example .env && ./gradlew bootRun
```

## Conventions

This service follows the project-wide rules in
[`platform/docs`](https://github.com/financemaster-proj/platform/tree/main/docs) —
in particular **no synchronous calls to another service**
([ADR-0004](https://github.com/financemaster-proj/platform/blob/main/docs/adr/0004-no-synchronous-service-calls.md)),
package by feature, Flyway for every schema change, and money as `BigDecimal`
with an explicit currency.
