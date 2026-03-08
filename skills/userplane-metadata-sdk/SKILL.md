---
name: userplane-metadata-sdk
description: Attach custom key-value metadata to Userplane recordings. Use when user asks how to send user context to recordings, set user ID or plan on a session, use dynamic metadata, clear metadata on logout, or pass metadata via URL parameters.
license: MIT
metadata:
  author: Userplane
  version: 0.0.1
  category: reference
  tags:
    - userplane
    - screen-recording
---

# Userplane Metadata SDK

Attach custom key-value data to recordings so your support team sees user context (user ID, plan, feature flags, environment) in the Info Panel without asking.

## When to use

- Attaching user context to Userplane recordings
- Setting static or dynamic metadata via the SDK
- Passing metadata via URL parameters (helpdesk macros, server-rendered pages)
- Clearing metadata on logout
- Debugging metadata with `getCustomMetadata()`

## Examples

### Example 1: Attach user context at login
User says: "How do I send user ID to Userplane?" or "Set the plan on a recording" or "Add user metadata to screen recordings"
Call `set()` with stable values after authentication. See the `set()` section.

### Example 2: Use dynamic metadata
User says: "How do I send the current page URL in recordings?" or "Capture feature flags at recording time" or "Dynamic metadata that updates during the session"
Register a function with `metadata()` — it is called at submission time with the latest values. See the `metadata()` section.

### Example 3: Clear metadata on logout
User says: "How do I clear metadata when a user logs out?" or "Prevent user data leaking between sessions" or "Remove all metadata after sign-out"
Call `clearMetadata()` in your logout handler. See the `clearMetadata()` section.

## API reference

### `set(key, value)`

Sets a static metadata key-value pair. Use for values known at initialization that don't change frequently.

| Parameter | Type | Description |
|-----------|------|-------------|
| `key` | `string` | Metadata key. Appears as a label in the Info Panel. |
| `value` | `Serializable` | String, number, boolean, null, array, or nested object. |

```javascript
import { set } from '@userplane/sdk';

set('userId', 'usr_12345');
set('plan', 'business');
set('environment', 'production');
```

Static metadata provides defaults. If a dynamic metadata function returns the same key, the function value takes priority.

### `metadata(fn)`

Registers a dynamic metadata function. Called when a recording is being submitted, capturing the most current values.

| Parameter | Type | Description |
|-----------|------|-------------|
| `fn` | `() => SerializableObject` | Returns key-value pairs. Called at recording submission time. |

```javascript
import { metadata } from '@userplane/sdk';

metadata(() => ({
  userId: getCurrentUser().id,
  plan: getCurrentUser().plan,
  page: window.location.pathname,
  featureFlags: getActiveFlags(),
}));
```

Calling `metadata()` again overwrites the previous function. Only one metadata function can be registered at a time.

### `clearMetadata(keyOrType?)`

Clears metadata. Behavior depends on the argument:

| Argument | Behavior |
|----------|----------|
| _(no argument)_ | Clears everything — metadata function and all static pairs |
| `'function'` | Clears only the registered metadata function |
| `'static'` | Clears all static key-value pairs set via `set()` |
| Any other string | Clears that specific static key |

```javascript
import { clearMetadata } from '@userplane/sdk';

clearMetadata();            // Clear everything
clearMetadata('function');  // Clear only the metadata function
clearMetadata('static');    // Clear all static metadata
clearMetadata('userId');    // Clear a specific key
```

### `getCustomMetadata()`

Returns the merged metadata object (static + function), or `null` if no metadata is set. Useful for debugging.

```javascript
import { getCustomMetadata } from '@userplane/sdk';

const meta = getCustomMetadata();
// { userId: 'usr_12345', plan: 'business', page: '/dashboard', ... }
```

## URL parameter metadata (`userplane-meta`)

When you cannot use the SDK (helpdesk macros, server-rendered pages, third-party tools), attach metadata via the `userplane-meta` URL parameter. Format: `?userplane-meta=key1%3Dval1,key2%3Dval2` (comma-separated key=value pairs, URL-encoded).

**Merge priority:** dynamic SDK (`metadata()`) > static SDK (`set()`) > URL params. SDK values always win for the same key.

For the complete type definitions, URL parameter encoding details, and merge priority table, see `references/REFERENCE.md`.

## Common patterns

- **Login context** — Call `set()` with user ID, email, account name, plan, and role after authentication
- **Logout clearing** — Call `clearMetadata()` on logout to prevent user data leaking into subsequent recordings
- **Feature flags** — Use `metadata()` to capture active feature flags at recording submission time
- **Error context** — Listen for `window.error` events and call `set('lastError', event.message)` to attach recent errors
- **Page tracking in SPAs** — Call `set('currentRoute', path)` in your router's after-navigation hook
- **Static + dynamic combo** — Use `set()` for stable values (app version, environment, region) and `metadata()` for changing values (current page, cart items, active experiments)
