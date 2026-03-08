---
name: userplane-best-practices
description: Cross-cutting best practices for Userplane integration. Use when user asks about general Userplane guidance, correct integration patterns, SSR safety across frameworks, environment variable naming, metadata conventions, or privacy best practices.
license: MIT
metadata:
  author: Userplane
  version: 0.0.1
  category: reference
  tags:
    - userplane
    - screen-recording
---

# Userplane Best Practices

Cross-cutting best practices for integrating Userplane across frameworks. Covers installation, SDK patterns, metadata, privacy, and common pitfalls.

## When to use

- Reviewing a Userplane integration for correctness
- Looking for general guidance that isn't framework-specific
- Checking env var naming conventions across frameworks
- Understanding SSR safety across different setups

## Examples

### Example 1: Review an integration for correctness
User says: "Is my Userplane setup correct?" or "What are common Userplane integration mistakes?" or "General Userplane best practices"
Check the Installation, SDK integration patterns, and Metadata sections below for the recommended patterns per framework.

### Example 2: Environment variable naming
User says: "What's the right env var prefix for Next.js?" or "How do I expose the workspace ID in Vite?" or "NUXT_PUBLIC vs PUBLIC_ prefix"
See the Environment variable naming table in the SDK integration patterns section.

### Example 3: SSR safety guidance
User says: "Is it safe to import the SDK in a server component?" or "Where should I call initialize() in Nuxt?" or "Will Userplane crash during SSR?"
See the SSR safety table in the SDK integration patterns section and the per-framework skill for framework-specific patterns.

## Installation best practices

### Script placement

Place the CDN script as early as possible in `<head>`, before other scripts. Userplane can only capture console logs and network requests after it initializes. Do not use `async` or `defer`.

### Preserve URL parameters through redirects

All `userplane-` prefixed query parameters must survive redirects (especially auth redirects). The recording session cannot start if `userplane-token` and `userplane-action` are lost. Every framework skill includes a redirect preservation example for that framework's middleware/guard pattern.

### Content-Security-Policy

If your site sets CSP, add `*.userplane.io` to both `frame-src` and `script-src`. See the `userplane-cdn` skill for exact syntax.

## SDK integration patterns

### SSR safety

`@userplane/sdk` is SSR-safe to import — it does not reference `window` or `document` at module evaluation time. However, `initialize()` must be called in a browser context:

| Framework | Where to call `initialize()` |
|-----------|------------------------------|
| React (Vite) | `useEffect` in provider component |
| Next.js | `useEffect` in `'use client'` provider |
| Vue 3 (Vite) | Vue plugin `install` function |
| Angular | `APP_INITIALIZER` factory |
| Nuxt 3 | `.client.ts` plugin |
| Astro | `<script>` block in layout |
| SvelteKit | `onMount` in `+layout.svelte` |
| TanStack Start | `useEffect` in provider |
| Static HTML | CDN script handles it automatically |

### Environment variable naming

Each framework has its own convention for client-exposed env vars:

| Framework | Prefix | Example |
|-----------|--------|---------|
| Vite (React, Vue) | `VITE_` | `VITE_USERPLANE_WORKSPACE_ID` |
| Next.js | `NEXT_PUBLIC_` | `NEXT_PUBLIC_USERPLANE_WORKSPACE_ID` |
| Nuxt 3 | `NUXT_PUBLIC_` | `NUXT_PUBLIC_USERPLANE_WORKSPACE_ID` |
| Astro | `PUBLIC_` | `PUBLIC_USERPLANE_WORKSPACE_ID` |
| SvelteKit | `PUBLIC_` | `PUBLIC_USERPLANE_WORKSPACE_ID` |
| Angular | N/A | Use `environment.ts` files |
| TanStack Start | `VITE_` | `VITE_USERPLANE_WORKSPACE_ID` |

### Call `initialize()` once

The SDK ignores subsequent calls to `initialize()`. Place it at the app's entry point (root layout, provider, plugin) so it runs once on page load.

## Metadata best practices

- Use `set()` for stable values known at initialization: user ID, account name, plan, app version, environment
- Use `metadata()` for values that change during the session: current page, feature flags, cart contents
- Call `clearMetadata()` on logout to prevent user data leaking into subsequent recordings
- Keep keys descriptive — your support team sees them as-is in the Info Panel
- Avoid large objects — metadata should be concise context, not a state dump
- SDK metadata takes priority over URL parameter metadata for the same key

See the `userplane-metadata-sdk` skill for the full API.

## Privacy best practices

- Use `<meta name="userplane:blur">` tags to centralize blur rules rather than scattering attributes across components
- If you already use FullStory, Hotjar, Sentry, or other session replay tools, their privacy attributes are automatically respected — no re-tagging needed
- Enable **Hide sensitive fields** in domain preferences for automatic detection of password and credit card inputs
- Use `data-userplane-blur="false"` to exclude specific elements when a parent is blurred
- Test blur by creating a recording and playing it back

See the `userplane-sensitive-data` skill for the full reference.

## TanStack Start caveat

TanStack Router validates search parameters against a Zod schema. You **must** add `userplane-token`, `userplane-action`, and `userplane-workspace` to your root route's search schema, or the router throws a `notFoundError`. See the `userplane-tanstack-start` skill for details.

## Domain preferences reference

For detailed domain recording preferences (capture controls, sensitive field hiding), see `references/REFERENCE.md`.
