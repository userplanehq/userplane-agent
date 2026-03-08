# Userplane Sensitive Data — Third-Party Compatibility Reference

## Third-party tool compatibility

The SDK recognizes privacy attributes from 16 providers. If you already tag elements for another tool, they are automatically blurred in Userplane recordings too — no re-tagging required.

### Mask selectors

| Provider | Mask selectors |
|----------|---------------|
| FullStory | `.fs-exclude`, `.fs-block`, `.fs-mask`, `.fs-hide` |
| Hotjar | `[data-hj-suppress]`, `[data-hj-masked]`, `.hj-suppress` |
| Sentry | `.sentry-block`, `[data-sentry-mask]`, `[data-sentry-block]` |
| OpenReplay | `[data-openreplay-obscured]`, `[data-openreplay-block]`, `.openreplay-block` |
| Heap | `[data-heap-redact-text]`, `[data-heap-redact-attributes]`, `[data-heap-ignore]`, `.heap-ignore` |
| Amplitude | `[data-amp-mask]`, `.amp-mask` |
| rrweb | `.rr-block`, `.rr-mask`, `.rr-ignore`, `[data-rr-mask]` |
| LogRocket | `[data-logrocket-hidden]`, `.lr-hide`, `.lr-block` |
| Microsoft Clarity | `[data-clarity-mask]`, `.clarity-mask` |
| ContentSquare | `[data-cs-mask]` |
| Quantum Metric | `[data-qm-mask]` |
| Glassbox | `[data-glassbox-mask]` |
| Smartlook | `[data-recording-disable]`, `[data-recording-ignore]` |
| Mouseflow | `[data-mouseflow-mask]`, `.mouseflow-mask` |
| Inspectlet | `[data-inspectlet-sensitive]`, `.__ipt_sensitive__` |
| Lucky Orange | `[data-lo-mask]`, `.lo-mask` |

### Unmask selectors

| Provider | Unmask selectors |
|----------|-----------------|
| FullStory | `.fs-unmask` |
| Microsoft Clarity | `[data-clarity-unmask]` |
| ContentSquare | `[data-cs-capture]` |
| Amplitude | `[data-amp-unmask]` |

## Generic privacy attributes

In addition to provider-specific attributes, these generic selectors are also recognized:

| Selector | Effect |
|----------|--------|
| `[data-private]` | Blur |
| `.private` | Blur |
| `[data-sensitive]` | Blur |
| `.sensitive` | Blur |
