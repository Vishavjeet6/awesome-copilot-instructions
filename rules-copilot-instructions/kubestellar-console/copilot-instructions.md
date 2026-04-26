# KubeStellar Console Copilot Instructions

## Project Overview
KubeStellar Console is a multi-cluster Kubernetes dashboard with AI-powered operations, real-time observability, and 20+ CNCF project integrations. Frontend: React + TypeScript (`/web/`). Backend: Go with Fiber v2 (root directory).

## Build & Lint
Always run before committing: `cd web && npm run build && npm run lint`

## Card Development Patterns
- All data fetching MUST use `useCache`/`useCached*` hooks from `hooks/useCachedData.ts`
- Always destructure `isDemoData` and `isRefreshing` from hooks and pass to `useCardLoadingState()`
- Never show demo data during loading: use `isDemoFallback && !isLoading`
- Hook ordering: call `useCardLoadingState` AFTER hooks that provide `isDemoData`

## Array Safety
Never call `.join()`, `.map()`, `.filter()`, `.forEach()`, or `for...of` on values that might be undefined. Always guard: `(data || []).map(...)`.

## No Magic Numbers
Every numeric literal must be a named constant. Use constants from `lib/constants/` (time.ts, network.ts, ui.ts).

## Styling (Tailwind CSS)
- Use `cn()` utility from `lib/cn.ts` for merging classNames
- Use semantic Tailwind classes (`text-foreground`, `bg-primary`) — never raw hex colors
- Status colors: green-400 (success), yellow-400 (warning), red-400 (error), cyan-400 (info)

## Internationalization
- All user-facing strings use `t()` from `react-i18next`
- Translation keys in `web/src/locales/en/` JSON files
- Never use raw strings for UI text

## Go Backend (Fiber v2)
- Handlers: `func(c *fiber.Ctx) error`
- Errors: `fiber.NewError(statusCode, message)`
- Slices: `make([]T, 0)` not `var x []T` (nil serializes to `null`)
- Logging: `log/slog` structured logging
- Multi-cluster: goroutines + `sync.WaitGroup` for parallel requests
- Demo mode: every endpoint checks `isDemoMode(c)` first

## Cluster Safety
Always use `DeduplicatedClusters()` when iterating clusters — multiple kubeconfig contexts can point to the same physical cluster.

## State Management
Pure React (Context + hooks). No Redux, Zustand, or Jotai. 12 focused Context providers in `App.tsx`.
