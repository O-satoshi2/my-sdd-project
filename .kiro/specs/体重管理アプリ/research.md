# Research & Design Decisions: 体重管理アプリ

---
**Purpose**: Capture discovery findings, architectural investigations, and rationale that inform the technical design.
---

## Summary
- **Feature**: 体重管理アプリ (Weight Management App with Collection Elements)
- **Discovery Scope**: New Feature (Greenfield)
- **Key Findings**:
  - The application requires a robust data model to handle daily weight/body fat logs, user-defined collection themes, and collection items.
  - A robust backend architecture is suitable. Since this is a standalone web application, a modern React framework (Next.js) with a cloud relational database (Supabase / PostgreSQL) provides excellent development speed, data integrity, and a clear path for deployment on Vercel.
  - The collection unlock mechanism requires a rules engine or evaluation logic that triggers after each daily log is saved.

## Research Log

### Architecture Stack Selection
- **Context**: Choosing the right technical stack for a greenfield weight management application.
- **Sources Consulted**: Next.js (Vercel deployment), Supabase, Prisma, Vite PWA patterns.
- **Findings**: 
  - A Fullstack React framework like Next.js allows unified strongly-typed APIs (tRPC or Server Actions).
  - Supabase provides an easy-to-use PostgreSQL database with built-in remote hosting, ideal for serverless Next.js apps deployed to Vercel.
  - Using a UI library like TailwindCSS + shadcn/ui enables rapid prototyping of dashboards and forms.
- **Implications**: We will proceed with Next.js (App Router) deployed on Vercel, TypeScript, Supabase (PostgreSQL), and TailwindCSS.

### Collection Evaluation Logic
- **Context**: How to implement the logic that unlocks collection items based on weight logs.
- **Findings**:
  - The unlock condition needs to be flexible. For MVP, we can support simple triggers like "logged N days in a row" or "reached target weight X".
  - The evaluation should be executed synchronously or as a background job immediately after saving a new weight log.
- **Implications**: The `LogService` must emit an event or call `CollectionEvaluatorService` to process the latest state against the unlocking criteria.

## Architecture Pattern Evaluation

| Option | Description | Strengths | Risks / Limitations | Notes |
|--------|-------------|-----------|---------------------|-------|
| Client-side Only (IndexedDB) | React App using IndexedDB for storage. | No backend needed, offline by default. | Data loss risk to user if browser data is cleared. Hard to sync. | Not ideal for long-term health data storage. |
| Next.js + Supabase (Monolith on Vercel) | Next.js App Router with Server Actions backing to a Supabase PostgreSQL database. | Easy deployment via Vercel, typed API contracts, built-in cloud persistence. | Relies on external services (Vercel/Supabase). | Perfect fit for modern web apps wanting fast iteration and zero infra. |

## Design Decisions

### Decision: Backend Architecture
- **Context**: Need a reliable way to store health and collection data.
- **Selected Approach**: Next.js App Router using Server Actions with Supabase (PostgreSQL), deployed on Vercel.
- **Rationale**: Best-in-class developer experience, excellent type safety (via Prisma or Supabase Client), and native seamless deployment on Vercel.
- **Trade-offs**: Requires internet connection to access the database compared to a local SQLite file, but data safety and cross-device syncing are automatic.
- **Follow-up**: Ensure schema is well defined and Supabase project is created before starting UI implementation.

## Risks & Mitigations
- **Data Integrity** — Ensure negative values or unrealistic weight values are blocked at both client and server layers using Zod validation.
- **Complex UI State** — Managing the dashboard, graphs, and collection unlock animations may result in complex React component trees. Mitigation: Isolate responsibilities into pure components and use custom hooks for business logic.

## References
- [Next.js Documentation](https://nextjs.org/docs) — Framework foundation
- [Supabase Documentation](https://supabase.com/docs) — Database schema and APIs
- [Vercel Documentation](https://vercel.com/docs) — Deployment and Serverless architecture
