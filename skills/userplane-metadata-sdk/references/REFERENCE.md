# Userplane Metadata SDK — Type Definitions and URL Parameters

## TypeScript type definitions

### Serializable

```typescript
type Serializable =
  | string
  | number
  | boolean
  | null
  | undefined
  | Serializable[]
  | { [key: string]: Serializable };
```

### SerializableObject

```typescript
interface SerializableObject {
  [key: string]: Serializable;
}
```

## URL parameter metadata format

When you cannot use the SDK (helpdesk macros, server-rendered pages, third-party tools), attach metadata via the `userplane-meta` URL parameter.

### Format

```
?userplane-meta=key1%3Dval1,key2%3Dval2
```

Decoded: `key1=val1,key2=val2` — comma-separated key-value pairs, `=` as delimiter, URL-encoded.

### Example

Helpdesk macro link:

```
https://record.userplane.io/r/abc123?userplane-meta=ticketId%3D98765,priority%3Dhigh
```

This attaches `ticketId: 98765` and `priority: high` to the recording.

## Merge priority table

When multiple metadata sources provide the same key, the following priority order applies:

| Priority | Source | Method |
|----------|--------|--------|
| 1 (highest) | Dynamic SDK metadata | `metadata(() => ...)` |
| 2 | Static SDK metadata | `set(key, value)` |
| 3 (lowest) | URL parameter metadata | `?userplane-meta=...` |

Higher-priority values always override lower-priority values for the same key. SDK metadata set after the URL parameter is read will always win.
