# Setup TODO

## 🎯 Goal
Transform this Shopify React Router app into a modern stack with tRPC, Drizzle, Zod, Tailwind, and Vercel AI SDK.

---

## ✅ Already Complete
- [x] React Router 7 setup
- [x] Shopify App Bridge integration
- [x] Polaris UI components
- [x] Basic folder structure (`src/app`, `src/components`, `src/features`, `src/lib`, `src/server`, `src/trpc`)
- [x] Drizzle ORM installed (but not configured)
- [x] PostgreSQL driver (`pg`) installed
- [x] TypeScript configured
- [x] ESLint configured

---

## 🔧 Phase 1: Cleanup & Prerequisites

### Remove Unused Files
- [ ] Delete `Dockerfile`
- [ ] Delete `.dockerignore`
- [ ] Remove `docker-start` script from `package.json` (line 13)
- [ ] Decide: Keep or remove `.editorconfig` (optional)

### Update Dependencies
- [ ] Upgrade ESLint to v9 (already done ✅)
- [ ] Remove old TypeScript ESLint packages if not needed:
  - `@typescript-eslint/eslint-plugin` (v6 - old)
  - `@typescript-eslint/parser` (v6 - old)
  - Keep `typescript-eslint` (v8 - new unified package)

---

## 🗄️ Phase 2: Database Migration (Prisma → Drizzle)

### Install Drizzle Session Storage
- [ ] Install `@shopify/shopify-app-session-storage-drizzle`
  ```bash
  pnpm add @shopify/shopify-app-session-storage-drizzle
  ```

### Set Up Drizzle
- [ ] Create `src/server/db/index.ts` with Drizzle connection
  - Use PostgreSQL connection pool
  - Add dev caching (similar to current Prisma setup)
- [ ] Create `src/server/db/schema.ts` with Session table schema
  - Match Prisma Session model structure
  - Use Drizzle's `pgTable` syntax
- [ ] Create `drizzle.config.ts` at root for migrations
- [ ] Generate initial migration for Session table
- [ ] Update `src/app/shopify.server.ts` to use `DrizzleSessionStorage`

### Migrate Database
- [ ] Create Drizzle migration for Session table
- [ ] Test session storage works with Drizzle
- [ ] Update `package.json` scripts:
  - Replace `prisma generate` with `drizzle-kit generate`
  - Replace `prisma migrate deploy` with `drizzle-kit migrate`

### Remove Prisma (After Migration Verified)
- [ ] Remove `@prisma/client`
- [ ] Remove `prisma`
- [ ] Remove `@shopify/shopify-app-session-storage-prisma`
- [ ] Delete `prisma/` directory
- [ ] Remove Prisma scripts from `package.json`
- [ ] Update `src/app/db.server.ts` to export Drizzle instance

---

## 🔌 Phase 3: tRPC Setup

### Install tRPC Dependencies
- [ ] Install core tRPC packages:
  ```bash
  pnpm add @trpc/server @trpc/client @trpc/react-query @trpc/server@next
  pnpm add -D @trpc/react-router
  ```
- [ ] Install React Query (required for tRPC client):
  ```bash
  pnpm add @tanstack/react-query
  ```

### Set Up tRPC Server
- [ ] Create `src/server/trpc/trpc.ts` - Initialize tRPC with context
  - Include Shopify session/auth in context
  - Add error formatting
- [ ] Create `src/server/trpc/context.ts` - Context creator
  - Extract session from request
  - Provide database instance
  - Provide Shopify admin API client
- [ ] Create `src/server/trpc/router.ts` - Root router
  - Export `appRouter` type
  - Set up router structure

### Set Up tRPC Client
- [ ] Create `src/trpc/client.ts` - Client configuration
  - Set up React Query client
  - Configure tRPC React Query hooks
- [ ] Create `src/trpc/utils.ts` - Helper utilities
  - `getBaseUrl()` function
  - Client creation helpers

### Integrate with React Router
- [ ] Create `src/app/routes/api.trpc.$.tsx` - tRPC API route handler
  - Handle tRPC requests
  - Pass React Router request context
- [ ] Update `src/app/root.tsx` to include tRPC provider
  - Wrap app with React Query provider
  - Add tRPC provider

### Create First tRPC Router
- [ ] Create `src/server/routers/example.router.ts` - Example router
  - Simple query procedure
  - Simple mutation procedure
- [ ] Add to root router in `src/server/trpc/router.ts`
- [ ] Test tRPC endpoint works

---

## ✅ Phase 4: Zod Integration

### Install Zod
- [ ] Install Zod:
  ```bash
  pnpm add zod
  ```

### Set Up Schema Structure
- [ ] Create `src/schemas/` directory
- [ ] Create `src/schemas/index.ts` - Export all schemas
- [ ] Create example schemas:
  - `src/schemas/user.schema.ts`
  - `src/schemas/product.schema.ts` (if needed)

### Integrate with tRPC
- [ ] Update tRPC routers to use Zod for input validation
- [ ] Create shared schemas for common types
- [ ] Use Zod for form validation (if using forms)

---

## 🎨 Phase 5: Tailwind CSS Setup

### Install Tailwind
- [ ] Install Tailwind and dependencies:
  ```bash
  pnpm add -D tailwindcss postcss autoprefixer
  ```
- [ ] Initialize Tailwind:
  ```bash
  npx tailwindcss init -p
  ```

### Configure Tailwind
- [ ] Update `tailwind.config.js`:
  - Add content paths: `["./src/**/*.{js,jsx,ts,tsx}"]`
  - Configure to work with Polaris (avoid conflicts)
  - Add custom colors/themes if needed
- [ ] Create `src/styles/tailwind.css` - Tailwind directives
- [ ] Import Tailwind CSS in `src/app/root.tsx`

### Optional: shadcn/ui Components
- [ ] Install shadcn/ui CLI:
  ```bash
  pnpm add -D shadcn
  ```
- [ ] Initialize shadcn:
  ```bash
  npx shadcn init
  ```
- [ ] Configure `components.json` for your setup
- [ ] Add initial components to `src/components/ui/`:
  - Button
  - Card
  - Input
  - (Add more as needed)

---

## 🤖 Phase 6: Vercel AI SDK Integration

### Install AI SDK
- [ ] Install Vercel AI SDK:
  ```bash
  pnpm add ai @ai-sdk/openai
  # Or other providers: @ai-sdk/anthropic, @ai-sdk/google, etc.
  ```

### Set Up AI Infrastructure
- [ ] Create `src/lib/ai/config.ts` - AI configuration
  - API keys management
  - Model selection
- [ ] Create `src/lib/ai/utils.ts` - AI helper functions
  - Streaming utilities
  - Prompt templates

### Integrate with tRPC
- [ ] Create `src/server/routers/ai.router.ts` - AI router
  - Text generation procedures
  - Streaming procedures
  - Agent procedures (if needed)
- [ ] Add AI router to root router
- [ ] Create example AI feature in `src/features/`

---

## 📁 Phase 7: Folder Structure & Organization

### Set Up Feature Modules
- [ ] Create first feature module in `src/features/`:
  - Example: `src/features/products/`
    - `products.page.tsx` - Main page component
    - `products.hooks.ts` - Custom hooks (tRPC queries)
    - `products.schemas.ts` - Zod schemas
    - `products.logic.ts` - Business logic
    - `products.types.ts` - TypeScript types

### Set Up Shared Components
- [ ] Create `src/components/ui/` for shadcn components (if using)
- [ ] Create `src/components/shared/` for custom shared components
- [ ] Create layout components if needed

### Set Up Lib Utilities
- [ ] Create `src/lib/env.ts` - Environment variable validation (with Zod)
- [ ] Create `src/lib/utils.ts` - General utilities
- [ ] Create `src/lib/constants.ts` - App constants
- [ ] Create `src/lib/errors.ts` - Custom error classes

### Set Up Schemas
- [ ] Organize `src/schemas/` by domain:
  - `src/schemas/session.schema.ts`
  - `src/schemas/shopify.schema.ts`
  - Domain-specific schemas

---

## 🔐 Phase 8: Environment & Configuration

### Environment Variables
- [ ] Create `.env.example` with all required variables:
  - Database URL
  - Shopify API keys
  - AI API keys
  - tRPC base URL
- [ ] Set up `src/lib/env.ts` with Zod validation
- [ ] Document all env vars in README

### TypeScript Configuration
- [ ] Update `tsconfig.json` paths if needed:
  - `@/` alias for `src/`
  - Feature aliases if desired
- [ ] Ensure all imports work correctly

---

## 🧪 Phase 9: Testing & Validation

### Test Database
- [ ] Verify Drizzle migrations work
- [ ] Test session storage with Drizzle
- [ ] Test database queries

### Test tRPC
- [ ] Test tRPC queries from client
- [ ] Test tRPC mutations
- [ ] Verify type safety end-to-end
- [ ] Test error handling

### Test AI Integration
- [ ] Test AI text generation
- [ ] Test streaming responses
- [ ] Verify error handling

### Test UI
- [ ] Verify Tailwind works
- [ ] Test Polaris + Tailwind compatibility
- [ ] Test shadcn components (if added)

---

## 🚀 Phase 10: Deployment Preparation

### Vercel Configuration
- [ ] Create `vercel.json` if needed
- [ ] Configure environment variables in Vercel dashboard
- [ ] Set up database (Vercel Postgres or external)
- [ ] Test build locally: `pnpm build`
- [ ] Verify all environment variables are set

### Update Scripts
- [ ] Update `package.json` build scripts if needed
- [ ] Add database migration script for production
- [ ] Add health check endpoint if needed

### Documentation
- [ ] Update README with new stack information
- [ ] Document folder structure
- [ ] Document tRPC usage patterns
- [ ] Document AI integration patterns

---

## 📝 Notes

### Architecture Decisions
- **React Router**: Thin routing layer, most logic in features/
- **tRPC**: Single API layer for all backend ↔ frontend communication
- **Drizzle**: Single source of truth for database (replaces Prisma)
- **Zod**: All validation + shared types
- **Tailwind**: Custom UI alongside Polaris
- **AI SDK**: Integrated in feature routers

### Potential Issues to Watch
- Polaris + Tailwind CSS conflicts (use CSS modules or scoped styles)
- tRPC + React Router integration complexity
- Drizzle migration from Prisma data
- Vercel serverless function timeouts for long AI operations

### Next Steps After Setup
- Create first feature module
- Set up authentication patterns
- Create reusable UI components
- Set up error boundaries
- Add logging/monitoring
