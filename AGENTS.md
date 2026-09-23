# Property Portal Engineering Boundaries

This file defines the stable engineering boundaries for the Property Portal workspace. Read it before changing any project. Product behavior belongs in [SPEC.md](SPEC.md); user-facing setup belongs in the README for the relevant project.

## Workspace topology

The workspace contains three sibling projects. They are independent Git repositories, not a monorepo:

```text
property-portal/
├── AGENTS.md
├── CLAUDE.md
├── SPEC.md
├── README.md
├── api/       # Express + Prisma + SQLite API repository
├── app/       # React web application repository
└── mobile/    # React Native + Expo application repository
```

Keep the projects independently installable, testable, and deployable. Do not add a workspace package manager or imports between sibling repositories. Share domain contracts through an API specification or generated client/types, not source-level coupling.

## Stable technology choices

- TypeScript is the language across all projects.
- The API uses Express, Prisma 7, SQLite, and Zod for runtime validation.
- The web application uses React with Vite and shadcn/ui. Preserve shadcn/ui preset `b7ClNFsdU`.
- The mobile application uses React Native with Expo.
- Node.js 22 or newer is the baseline for the API; pin exact versions in each project manifest.
- The API is the reusable source of business behavior for both clients.

## Project boundaries

### API

Own authentication and authorization, catalog and listing APIs, persistence, migrations, seed data, validation, error handling, audit logging, and API documentation. Expose versioned REST endpoints under `/api/v1`.

### Web app

Own the responsive public marketplace and role-aware web experiences. It may provide presentation and client state, but must not be the authority for permissions, listing ownership, status transitions, or other business rules.

### Mobile app

Own the future Expo experience and consume the API contract. Mobile work must not duplicate server business logic or block web delivery.

## Architectural guardrails

- Keep API modules organized by feature rather than concentrating behavior in one route or controller file.
- Validate request bodies, query parameters, path parameters, and uploaded-media metadata at API boundaries.
- Enforce authorization in the API for every protected resource and action; never rely only on client checks.
- Use soft deletion or archival for business records where recovery or audit history matters.
- Store timestamps in UTC and localize their display in clients.
- Use stable, non-sequential public identifiers where practical.
- Paginate list endpoints from the first implementation.
- Keep a centralized error response shape and request correlation IDs.
- Store media references/metadata in the database rather than large binary content.
- Preserve an auditable history for moderation and listing status changes.

## Development expectations

- Each project owns its own `package.json`, TypeScript configuration, environment example, tests, linting, and eventual CI.
- Never commit real credentials or personal data. Seed and demo data must be synthetic and clearly non-production.
- When a command, environment variable, port, or user-facing workflow changes, update that project's README.
- When a product behavior or acceptance criterion changes, update SPEC.md rather than this file.
- Keep this file limited to boundaries and choices that should remain stable across feature iterations.
