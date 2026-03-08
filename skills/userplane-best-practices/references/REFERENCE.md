# Domain Preferences Reference

Each verified domain in your Userplane workspace has configurable recording preferences that control what data is captured alongside the video.

## Available preferences

| Preference | Description | Default |
|------------|-------------|---------|
| **Hide sensitive fields** | Automatically blur sensitive form fields and inputs (passwords, credit card fields) during recording. | Enabled |
| **Capture console logs** | Record browser console output (errors, warnings, messages). | Enabled |
| **Capture client actions** | Record user interactions (clicks, inputs, navigation). | Enabled |
| **Capture network logs** | Record HTTP requests and responses (URLs, status codes, timing). | Enabled |
| **Capture backend logs** | Record server-side logs forwarded during the session. | Enabled |
| **Capture voice and device audio** | Record the user's voice and device system audio alongside the video. | Disabled |

## Configuring preferences

1. Go to **Workspace Settings > Domains**
2. Select a **verified** domain
3. Toggle preferences — changes take effect immediately for new recordings

Domain preferences are only available for verified domains. Pending or revoked domains must complete domain verification first.

## Capture controls interaction with SDK

Domain preferences apply at the recording level. They work alongside SDK-level configuration:

- **`captureEnabled` option** in `initialize()` controls whether the capture iframe is mounted on the client. If set to `false`, no console or network data is captured regardless of domain preferences.
- **Domain preferences** control what the Userplane backend accepts and stores. Even if the client captures data, the backend respects domain-level toggles.

Both must be enabled for data to appear in the recording.

## Sensitive field auto-detection

When **Hide sensitive fields** is enabled, the following are automatically blurred:

- `<input type="password">`
- Inputs with `autocomplete` values: `cc-number`, `cc-exp`, `cc-csc`, `cc-name`, `cc-type`
- Other browser-identified sensitive input types

This is additive with manual blur attributes (`data-userplane-blur`, `.userplane-mask`, `<meta>` tags). Manual blur always applies regardless of this preference.

## Integration with embed script vs SDK

| Feature | Embed script (CDN) | npm SDK |
|---------|-------------------|---------|
| Domain preferences | Applied automatically | Applied automatically |
| Manual blur attributes | Respected | Respected |
| Custom metadata | Not available | Available via `set()` and `metadata()` |
| Programmatic recorder control | Not available | Available via `open()`, `unmount()` |
| Recording state queries | Not available | Available via `getRecordingState()`, etc. |

Both integration paths respect domain preferences and blur configuration identically. The SDK adds programmatic capabilities on top.
