# Project Structure

## Directory Layout

```text
/src
  /app         # Next.js App Router endpoints and pages
  /components  # Reusable UI React components (e.g. dashboards, charts)
  /lib         # Utility functions, Supabase clients
  /actions     # Next.js Server Actions for mutations
  /types       # Zod schemas, TypeScript types
/public        # Static assets
/.kiro         # Kiro specifications and steering
```

## Architectural Boundaries

- **UI vs Logic**: Server Actions in `/actions` carry out all business logic, while `/components` are strictly presentational.
- **Database Access**: Must happen only on the server, leveraging the Supabase client initialized via `/lib` or through Next.js secure environment vars.

## Naming Conventions

- **Files**: kebab-case or PascalCase (React components) depending on the framework standard. (`my-component.tsx` or `MyComponent.tsx`).
- **Interfaces**: PascalCase without 'I' prefix (`HealthLogService` instead of `IHealthLogService`).
- **Endpoints**: Route directories in Next.js App Router must be kebab-case (e.g., `/app/dashboard/collection-items/page.tsx`).

## Code Boundaries

- Client components (`"use client"`) should fetch data via proper React hooks or accept server-fetched data via props from (`page.tsx`). Server Actions handle all form submissions and API mutations safely.
