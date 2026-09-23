# Property Portal

Property Portal is a Myanmar-focused marketplace for discovering, listing, renting, and selling property in Yangon and Mandalay. The web experience is the current delivery focus; the API is shared with a future Expo mobile app.

## Projects

| Project | Purpose | Local URL |
| --- | --- | --- |
| [`api/`](api/README.md) | Express API, Prisma schema, SQLite database, and seed data | `http://localhost:4000` |
| [`app/`](app/README.md) | Responsive React marketplace and role-aware demo flows | `http://localhost:5173` |
| [`mobile/`](mobile/README.md) | Independent Expo app reserved for the mobile phase | Expo dev server |

Each directory is its own Git repository and has its own install and development commands.

## Clone the workspace

Clone the root repository with its three independent projects:

```bash
git clone --recurse-submodules https://github.com/Nel-Well/property.git
```

If the root repository is already cloned, initialize its submodules with `git submodule update --init --recursive`.

## Documentation

- [AGENTS.md](AGENTS.md) — stable architecture, technology choices, and project boundaries
- [CLAUDE.md](CLAUDE.md) — instruction entry point for Claude Code
- [SPEC.md](SPEC.md) — evolving product and feature specification
- [API README](api/README.md) — API setup and development information
- [Web app README](app/README.md) — web setup and demo information
- [Mobile README](mobile/README.md) — Expo setup and current scope

## Run the current web experience

In one terminal, start the API:

```bash
cd api
npm install
cp .env.example .env
npm run prisma:generate
npm run db:seed
npm run dev
```

In another terminal, start the web app:

```bash
cd app
npm install
cp .env.example .env
npm run dev
```

Open `http://localhost:5173`. The web app uses the API when it is available and falls back to local demo listings when it is not.

## Current product direction

The marketplace starts with sale and rental listings, core property categories, Yangon and Mandalay locations, owner/agent workflows, buyer/renter engagement, and staff moderation. The mobile project follows API and web workflow stabilization.
