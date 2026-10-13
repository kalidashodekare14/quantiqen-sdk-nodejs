# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.1] - 2026-10-13

### Added

- Troubleshooting guidance for `401 INVALID_API_KEY` (well-formed but
  unknown/revoked key — check the key exists and is active in the
  organization settings).

### Changed

- API-key validation now enforces the full platform contract
  `qtn_{live|test|dev}_…` client-side, so a malformed key fails instantly
  with a clear error instead of a server round-trip.
- README: removed the stale "not yet published to npm" notice; `@quantiqen/sdk`
  installs from the public registry.

## [0.1.0] - 2026-10-12

### Added

- Initial release: `QuantiqenClient` with in-memory batching (default 100 events,
  500-event chunk cap), automatic retry with exponential backoff and jitter on
  network errors / 5xx / 429, `Retry-After` handling, and
  `X-RateLimit-Remaining`-aware throttling.
- `POST /api/v1/events/bulk` ingestion against the QUANTIQEN platform
  (`https://quantiqen-backend.onrender.com` by default).
- Automatic event metadata enrichment (`sdkLanguage`, `sdkVersion`, `userAgent`).
- Fire-and-forget semantics: `sendEvent()`/`sendEvents()` never throw; failures
  are logged via the configurable `logger`.
- Telemetry ingestion helpers: `sendMetric()` / `sendSpan()` / `sendPerformance()` /
  `sendLog()` plus strict builders (`buildMetricEvent`, `buildSpanEvent`,
  `buildPerformanceEvent`, `buildLogEvent`) and typed models. Records ride the
  existing event envelope (`eventCategory: "telemetry"`,
  `telemetry.{metric|span|performance|log}`) with a discriminated
  `payload.telemetryType`; auto-UUID `idempotencyKey` per record; `sendLog`
  maps log level to severity.

### Fixed

- Requests now carry a 30s timeout (`AbortSignal`), matching the Python SDK.
- Retries exhausted on HTTP 5xx now surface an `HTTP <status>` error instead of
  possibly throwing `undefined`.
- Non-retryable HTTP 4xx failures are logged instead of dropped silently.
- A `Retry-After` pause is no longer re-applied after it has elapsed.

[0.1.1]: https://github.com/kalidashodekare14/quantiqen-sdk-nodejs/releases/tag/v0.1.1
[0.1.0]: https://github.com/kalidashodekare14/quantiqen-sdk-nodejs/releases/tag/v0.1.0
