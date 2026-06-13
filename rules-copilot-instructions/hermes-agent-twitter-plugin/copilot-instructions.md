# Hermes Agent + Hermes Tweet Copilot Instructions

## Context
- Build Hermes Agent plugins and skills that use `hermes-tweet` for X/Twitter automation.
- Prefer the published `hermes-tweet` Python package and repository manifests over custom X API clients.
- Keep explore, read, and action workflows separate so write operations stay explicit.
- Use public configuration names only: `XQUIK_API_KEY` and `HERMES_TWEET_ENABLE_ACTIONS`.

## Setup
- Use Python 3.12 or newer.
- Install `hermes-tweet` with the project package manager.
- Require `XQUIK_API_KEY` before read or action tools call the network.
- Keep action tools disabled unless `HERMES_TWEET_ENABLE_ACTIONS=true`.
- Never hard-code API keys, tokens, cookies, or account identifiers.

## Hermes Agent Patterns
- Register bundled skills with `ctx.register_skill`.
- Keep tool handlers JSON-serializable and return strings when the agent runtime expects strings.
- Accept `**kwargs` in handlers for forward-compatible runtime metadata.
- Keep non-network exploration tools available without credentials.
- Gate read tools on API availability and `XQUIK_API_KEY`.
- Gate write tools on action enablement, `XQUIK_API_KEY`, and `HERMES_TWEET_ENABLE_ACTIONS=true`.

## X/Twitter Safety
- Treat posting, liking, following, reposting, deleting, and replying as write actions.
- Ask for confirmation before generating code paths that can publish or mutate accounts.
- Prefer dry-run previews, explicit payload summaries, and clear error messages.
- Do not add fallback write routes when a write service reports an error.
- Do not log private profile data, credentials, cookies, or raw request headers.

## Code Quality
- Use typed request and response models instead of unstructured dictionaries.
- Isolate network clients behind small functions or dependency injection.
- Add tests for credential gating, action gating, invalid payloads, and JSON serialization.
- Keep documentation examples minimal, public-safe, and copy-pasteable.
- Preserve Hermes Agent compatibility when editing plugin manifests or install docs.
