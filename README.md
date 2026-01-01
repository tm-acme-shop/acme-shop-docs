# A Company Manufacturing Everything (ACME) Shop

## Overview

> *"A Company Manufacturing Everything"* — Since 1949, ACME Corporation has been the trusted supplier of anvils, rocket-powered roller skates, and giant magnets to cartoon characters worldwide. Now, we're pivoting to e-commerce.

**ACME Shop** is our fictional e-commerce SaaS platform — a sprawling, realistic codebase complete with:
- 🏗️ Multi-repo, multi-language microservices architecture
- 🔧 Legacy code migrations in progress (some... more in progress than others)
- 🐛 Intentionally planted bugs, security issues, and anti-patterns
- 📜 The kind of technical debt that makes engineers weep

This demo environment showcases all Sourcegraph features through the lens of a codebase that has seen things. Things like MD5 password hashing. Things like `0.0.0.0/0` security groups. Things that keep security teams up at night.

### The Backstory

The ACME Shop engineering team is mid-migration on *everything*:
- **Auth**: Moving from MD5 → bcrypt (yes, really, MD5 was in prod)
- **APIs**: v1 → v2 migration (v1 endpoints marked deprecated but still... everywhere)
- **Logging**: printf-style → structured logging
- **Payments**: Legacy client → new client (bank transfers still use the old one 🙃)
- **Infrastructure**: "We'll fix it after launch" → actually fixing it

---

## Repository Architecture

### Custom Repositories (10 total)

| Repository | Language | Purpose | Key Demo Patterns |
|------------|----------|---------|-------------------|
| `acme-shop-gateway` | Go | API gateway, routing, auth middleware | Cross-repo imports, header patterns |
| `acme-shop-users-service` | Go | User management, authentication | Password hashing migration (md5→bcrypt) |
| `acme-shop-orders-service` | Go | Order lifecycle, payments integration | Legacy vs new payment client |
| `acme-shop-payments-service` | Python | Payment orchestration | Deprecated endpoints, logging migration |
| `acme-shop-notifications-service` | TypeScript | Email/SMS/Push notifications | Deprecated sendEmailLegacy() |
| `acme-shop-frontend-web` | React/TypeScript | Customer-facing web UI | API client v1→v2 migration |
| `acme-shop-shared-go` | Go | Cross-service interfaces, domain models | Interface definitions for navigation |
| `acme-shop-shared-ts` | TypeScript | DTOs, API client, utilities | Shared types for navigation |
| `acme-shop-infra` | Terraform/Helm/YAML | Infrastructure as Code | Security misconfigs, CI migrations |
| `acme-shop-analytics-etl` | Python | ETL batch jobs | SQL anti-patterns, PII handling |

### Search Contexts

Search contexts let you scope your Sourcegraph searches to a specific set of repositories. Instead of searching across all repos, you can target just the ones relevant to your current task.

| Context | Description |
|---------|-------------|
| `context:@solutions-eng/acme-shop-all` | Contains all acme-shop repos |
| `context:@solutions-eng/acme-shop-backend` | Backend repos: gateway, users-service, orders-service, payments-service, analytics-etl, shared-go |
| `context:@solutions-eng/acme-shop-frontend` | Frontend repos: frontend-web, shared-ts, notifications-service |

---

### Dependency Matrix

| Consumer | Depends On | What's Used |
|----------|-----------|-------------|
| `acme-shop-gateway` | `acme-shop-shared-go` | logging, middleware, utils, models |
| `acme-shop-users-service` | `acme-shop-shared-go` | logging, middleware, models, interfaces, errors, utils |
| `acme-shop-orders-service` | `acme-shop-shared-go` | logging, middleware, models, interfaces, errors |
| `acme-shop-frontend-web` | `acme-shop-shared-ts` | ApiClient, User/Order/Payment models, utilities |
| `acme-shop-notifications-service` | `acme-shop-shared-ts` | Shared types and utilities |
| `acme-shop-payments-service` | — | Standalone (Python) |
| `acme-shop-analytics-etl` | — | Standalone (Python) |
| `acme-shop-infra` | — | Standalone (IaC) |