# Repository instructions

These instructions apply to every file in this repository. More specific `AGENTS.md` files may add rules for their subtree; they may not weaken these rules.

## Read first

Before changing this repository, read:

1. this file;
2. [`README.md`](README.md);
3. [`docs/AGENTS.md`](docs/AGENTS.md) for any documentation change;
4. [`docs/contracts-and-source-of-truth.md`](docs/contracts-and-source-of-truth.md); and
5. [`docs/roadmap/STATUS.md`](docs/roadmap/STATUS.md).

If work depends on Kumwe behavior, inspect the corresponding core source, executable tests, generated OpenAPI contract, and stable core documentation at the exact supported revision. Do not infer a contract from a screenshot, Twig template, route name, or historical planning prompt.

## Repository state

This is currently a documentation-first repository with no Flutter app. The separate
[`kumwe/dart-sdk`](https://github.com/kumwe/dart-sdk) repository contains an early SDK foundation; do not duplicate
its package or generated clients here. Do not initialize Flutter or claim an executable client capability unless an
approved roadmap phase explicitly authorizes that work. Documentation can describe requirements and proposals, but
must label them accurately.

## Authority and dependency direction

- `kumwe/cms` is authoritative for server behavior, data, authorization, validation, workflow, transactions, audit, extension trust, public rendering, and wire contracts.
- `kumwe/dart-sdk` translates adopted machine contracts into typed transport and validated runtime contract values. It must not recreate domain policy or own Flutter screen models.
- The Flutter client will depend on released SDK packages for native API operations. Widgets, view models, and platform adapters must not call HTTP directly.
- Do not add an in-app WebView or embedded-server fallback. A clearly labeled external-system-browser handoff may point users to the independently available Kumwe website, but it is outside the client, does not count as parity, and must not receive native credentials.
- Never access the Kumwe database, server filesystem, internal PHP classes, or undocumented JSON fields from the client.

The complete boundary is recorded in [`docs/architecture.md`](docs/architecture.md) and [ADR-0001](docs/architecture/decisions/0001-core-api-dart-sdk-flutter-boundary.md).

## Product constraints

- Target native Linux, macOS, Windows, Android, and iOS.
- Do not add Flutter web.
- Preserve the separation between administrator, authenticated portal, and public trust boundaries.
- Native parity is outcome and contract parity, not a DOM or pixel clone.
- Do not move authorization, validation, workflow transitions, approval decisions, exact-value rules, or extension trust decisions into the client.
- Treat offline mutation as unsupported until core defines synchronization, conflict, numbering, authorization-freshness, and audit contracts.
- Keep every mutation retry-safe with the core-defined idempotency and optimistic-concurrency semantics.
- Prefer a clear unavailable state over silently approximating a server feature.

## Contract discipline

- Pin research, generated code, compatibility fixtures, and release support to an exact core revision and contract checksum.
- Consume versioned `kumwe/dart-sdk` packages; never generate or hand-maintain transport models in this repository. Generation belongs to the SDK and must use core's adopted canonical OpenAPI 3.1 contract.
- Honor `Authorization`, the exact `Kumwe-Site` context, `Idempotency-Key`, `ETag`/`If-Match`, `Retry-After`, and `application/problem+json` as defined by core.
- Treat undocumented response fields as unstable.
- Exercise runtime-discovered definitions through SDK conformance fixtures pinned to the installed site's contract generation, not through client-owned raw HTTP.
- Record core contract gaps as client requirements with evidence. Do not edit this repository as if that changes the server contract.

## Security and privacy

- Never commit tokens, passwords, signing keys, recovery codes, certificates, `.env` files, production URLs containing secrets, or captured private data.
- Use operating-system secure storage for long-lived credentials when implementation begins. Logs, analytics, crash reports, screenshots, and clipboard flows must redact secrets and policy-hidden fields.
- Do not embed administrator tokens or ship a shared application credential.
- Do not invent a login protocol. Native sign-in starts only after core exposes and documents a suitable user authorization and revocation flow.
- Fail closed on TLS, trust, site-context, contract-version, or authorization ambiguity.

## Accessibility, localization, and adaptive behavior

- WCAG 2.2 AA is the baseline for equivalent native journeys.
- Desktop journeys must be fully keyboard operable; mobile journeys must work with VoiceOver and TalkBack.
- Do not hard-code English, left-to-right assumptions, date/number formats, or color as the only state signal.
- Preserve exact decimal, money, and quantity values as contract types; never round through binary floating-point convenience APIs.
- Adapt by available space and input capability, not by maintaining separate business flows for each platform.

## Change and review rules

- Keep changes scoped and preserve unrelated work.
- Update relevant docs, ADRs, status, compatibility evidence, and tests in the same change as an implemented contract.
- An ADR records a durable decision; do not rewrite an accepted decision silently. Supersede it with a new ADR.
- Do not mark a roadmap gate complete without linked executable evidence.
- Avoid placeholders presented as production paths. Open work belongs in the roadmap/status documents with an owner or dependency.
- Do not commit generated build output, credentials, IDE state, or platform signing material.

When code exists, every change must use the narrowest relevant checks, followed by the repository's documented aggregate quality gate. Until then, documentation changes require link, terminology, status, and contradiction review.
