# Next.js App — CLAUDE.md Template

Generic rules for a Next.js App Router project. Fill in stack, commands, and folder structure for the specific project.

---

## Key Commands

```bash
npm run dev          # Dev server
npm run validate     # type-check + lint + test  ← run after every change
npm run lint:fix     # auto-fix lint
npm run test:watch   # watch mode
```

## After Every Code Change

```bash
npm run validate  # REQUIRED before commit — fix types, lint, tests first
```

Bug workflow: write failing test → fix → verify pass.
New feature: write tests for utils, hooks, stores. TDD for complex logic.

---

## API Routes

Use `NextRequest` / `NextResponse`. Treat as production code.

- Validate required params/body first — return `400` immediately if invalid.
- Wrap every async op in `try/catch` — never throw unhandled.
- Always `console.error("[route/METHOD] msg", err)` in catch — never silently swallow.
- Return `{ error: "..." }` with correct status: `400` bad input · `401` auth · `404` not found · `500` unexpected.
- Handle known Prisma codes (`P2002` unique, `P2003` FK) before the generic 500.

---

## Code Rules

**Memoization:** React Compiler is enabled (`babel-plugin-react-compiler`). Do not add `useMemo`, `useCallback`, or `memo` — the compiler handles this automatically. Remove them when encountered in new code.

**Imports:** Cross-feature → `@/alias`. Within feature → relative paths. No barrel files (no `export * from`).

**No `_` prefixed files in app routes.** Co-located components belong in `src/features/<feature>/components/`, not as `_Component.tsx` next to page files.

**No middleware file convention.** The `middleware.ts` convention is deprecated — use `proxy` instead.

**Data flow:**
1. Read from Zustand store directly — no prop drilling beyond 1 level
2. Set meaningful defaults in stores — never guard against `undefined`
3. No defensive null checks — trust schema defaults, let it fail fast
4. Trust validated data — no `if (!config.field)` guards on validated JSON

```typescript
// ❌ Bad
if (!config.severityBands) return;
if (cfg?.enabled && val && cfg.factors[type] !== undefined) { ... }

// ✅ Good
const { severityBands } = config;
if (cfg.enabled) { ... }
```

**No typed catch checks:** Do not inspect error codes or types inside catch blocks (e.g. `typeof err === "object"`, `(err as T).code`). Let errors propagate to the generic handler.

**Avoid redundant computation:** Compute derived values once and pass them as parameters rather than recomputing in callees. Hoist expensive object construction (e.g. `Intl.DateTimeFormat`) outside of callbacks — don't create per-call.

**No unnecessary spreads:** `array.map(...)` not `[...array.map(...)]`. Extract repeated boolean expressions to a named variable rather than evaluating inline twice.

**No comments that restate function names** — delete JSDoc that just describes what the signature already says.

---

## Testing

Jest + React Testing Library. Tests in `__tests__/` next to source files.

- Pattern: `should <behavior> when <condition>` · Arrange-Act-Assert · cleanup in `afterEach`
- Pure functions: all branches · Zustand: `act()` + clear localStorage · React Query: `renderHook` + `createQueryWrapper()` · Components: accessible roles
- Global mocks in `jest.setup.ts`: `next-intl`, `echarts-for-react`, `localStorage`, `window.matchMedia`
- Per-test: `jest.mock('axios')`
- Utilities: `/src/__tests__/utils/` and `/src/__tests__/mocks/`
- Validate JSON edits — no trailing commas

---

## Git

**Commits:** `type(scope): description` — types: `feat` `fix` `refactor` `chore` `docs` `test`

**PRs:** rebase before merge

---

## Behavior

- Implement changes directly; don't plan unless asked
- Follow user's placement instructions exactly (hook vs component vs store)
- Keep this file concise — prefer updating existing sections over adding new ones
- When updating this file: be short and clear — bullet points over prose, no code examples unless essential
- After AI-assisted sessions, reflect new patterns/decisions back into this file
