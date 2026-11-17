# Mesa · Internal Support Management

[Español](README.md) | **English**

A web application for registering, organizing, and tracking technology support requests.

## Features

- Create and edit open requests.
- Search, filter by status, priority and category, sort, and paginate.
- Change history with actor, timestamp, and observations.
- Problem image at creation and solution evidence at resolution.
- Responsive interface for desktop, tablet, and mobile.

```text
Pendiente → En progreso → Resuelta
Pendiente → Cancelada
```

These API values mean Pending, In progress, Resolved, and Cancelled. The interface and API values are in Spanish. Resolved and cancelled requests remain available for reference. Critical requests require an observation to resolve. Saved images cannot be replaced or deleted.

## Stack

Next.js, React, TypeScript, and Tailwind CSS; a NestJS REST API; Supabase PostgreSQL and Storage. A transactional SQL function saves operations and their history together.

## Installation

Requirements: Node.js 24, npm, and a Supabase project.

Clone this repository and run from its directory:

```sh
npm ci
```

Create `.env.local` at the repository root:

```dotenv
SUPABASE_URL=https://YOUR_PROJECT.supabase.co
SUPABASE_SECRET_KEY=sb_secret_YOUR_KEY
PORT=3001
FRONTEND_ORIGIN=http://localhost:3000
```

Find the URL under Supabase → Settings → Data API and the key under Settings → API Keys → Secret keys. The secret key is used exclusively by the backend.

Run the scripts in [supabase/migrations](supabase/migrations) through the SQL Editor, from `001` to `006` in order.

```sh
npm run dev
```

- Application: [localhost:3000](http://localhost:3000).
- API: [localhost:3001/api/requests](http://localhost:3001/api/requests).
- Health: [localhost:3001/api/health](http://localhost:3001/api/health).

## Verification

```sh
npm test
npm run typecheck
npm run build
```

Tests use embedded PostgreSQL and mocked services; Supabase credentials are not required.

## Deployment

Deploy the monorepo as two Vercel projects:

| Project | Directory | Variables |
|---|---|---|
| NestJS API | `backend` | `SUPABASE_URL`, `SUPABASE_SECRET_KEY`, `FRONTEND_ORIGIN` |
| Next.js application | `frontend` | `API_URL` |

Set `API_URL` to the backend HTTPS origin, without `/api`. See the [deployment guide](docs/VERCEL.md).

## Documentation

Detailed guides are in Spanish:

- [Architecture](docs/ARQUITECTURA.md)
- [REST API](docs/API.md)
- [Images](docs/IMAGENES.md)
- [Manual verification](docs/VERIFICACION.md)

## Access

The current version has no authentication or roles; actors are entered manually. Use fictitious data on the public instance. Authentication and authorization are required before handling real information.
