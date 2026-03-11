# Technology Stack

## Architecture

Monolithic Web Architecture using Next.js App Router (Server Actions/APIs) deployed on Vercel.

## Core Technologies

- **Language**: TypeScript
- **Framework**: React, Next.js (App Router)
- **Runtime**: Node.js 20+
- **Styling**: TailwindCSS

## Key Libraries

- **Database**: Supabase (PostgreSQL)
- **Validation**: Zod
- **Charts/Visualization**: Recharts (or Chart.js)
- **Component Primitives**: shadcn/ui

## Development Standards

### Type Safety
Strict TypeScript mode. No `any` allowed. Use discriminated unions for error handling where applicable.

### Code Quality
Use ESLint and Prettier for strict formatting and linting. Next.js recommended rules apply.

### Testing
Use Jest for unit testing logic (e.g., specific rules for log validation and collection unlocks).

## Development Environment

### Required Tools
- Node.js 20+
- npm (or pnpm/yarn)
- Supabase CLI (optional for local DB, or connect to remote)
- Vercel CLI (for deployments)

### Common Commands
```bash
# Dev: npm run dev
# Build: npm run build
# Test: npm test
```

## Key Technical Decisions

- **Supabase as DB**: Adopted Supabase (PostgreSQL) for a cloud-native robust persistence layer, seamlessly integrating with Vercel serverless functions environment.
- **Vercel Deployment**: Chosen for zero-config Next.js deployments, giving us fast CI/CD and edge/serverless caching mechanisms.
- **Server Actions**: Handled using standard Next.js Server Actions with Zod validations for a robust and typed mutation layer, minimizing client-server boilerplate.

---
_Document standards and patterns, not every dependency_
