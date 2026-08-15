# Security and authentication

## Position

The client is an untrusted presentation adapter from core's perspective. It may improve usability by hiding unavailable actions, but it cannot authorize a record, reveal a field, approve an action, validate a mutation, or establish trust in an extension. Core must enforce every security decision on every request.

At the current documentation-only stage, no native authentication flow has been selected or implemented.

## Current core evidence

At the audited revision, authenticated REST requests use an opaque bearer token plus exactly one canonical `Kumwe-Site` header. Tokens may expire, carry explicit capabilities, and are bound to site and optional organization/workspace, membership/policy/security generations, audience, purpose, family, and delegation constraints. Mutations use idempotency keys; versioned mutations use ETags or explicit positive versions; failures use Problem Details.

Core can issue, rotate, list metadata for, and revoke API tokens through authorized administration. Plaintext token material is returned once. This is an integration credential mechanism; by itself it is not a complete safe consumer sign-in experience for desktop/mobile.

Generated business high-impact approval decisions require fresh session-bound step-up at the audited revision. Bearer REST cannot manufacture or consume that proof. The native approval-decision journey is therefore blocked until core provides an explicit native authorization/step-up contract.

See the pinned core [REST authentication contract](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/docs/rest-api.md#authentication) and [business security model](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/docs/business-security.md).

## Native authentication gate

Implementation must not begin with copied administrator tokens or a guessed login endpoint. Core and client owners must accept a flow that defines:

- how an installation advertises its authorization endpoints and supported client versions;
- user-driven authorization, redirect/loopback/deep-link return, and phishing-resistant installation identity;
- public/native client registration and proof-key requirements where applicable;
- token audience, purpose, site, organization/workspace, capabilities, duration, refresh/rotation, and delegation rules;
- cancellation, denied consent, MFA/step-up, account recovery, password change, and security-epoch invalidation;
- sign-out from one device versus server revocation and emergency all-device revocation;
- portal and administrator identity/session separation;
- offline/expired behavior and reauthorization without data leakage; and
- a non-browser automation/service-token path that is never confused with an interactive user session.

OAuth 2.1 authorization code with PKCE or another established native-app pattern may be evaluated, but this client document does not create such a core contract.

## Credential handling requirements

- Store long-lived secrets only through the operating system's protected credential facility.
- Keep access tokens in memory where practical and minimize their lifetime and scope.
- Never place credentials in URLs, browser content, clipboard by default, general preferences, SQLite/plain files, crash reports, analytics, logs, screenshots, or support bundles.
- Bind stored credentials to the normalized installation identity and exact account/site context; prevent confused-deputy reuse at another origin.
- Protect token creation/rotation secret-once screens from screen capture where the platform permits, auto-hide values, and require an explicit user action to copy.
- Clear credentials, restricted caches, notifications, and resumable operations on sign-out, account removal, site change, or security invalidation as the accepted policy requires.
- Treat biometric/device unlock as local access control to stored material, not a replacement for core authentication or step-up.
- Never ship a shared administrator credential, private client secret, trust-all certificate switch, or hidden recovery account.

## Transport and installation trust

- HTTPS is mandatory outside a deliberate local-development profile.
- Certificate, hostname, redirect, mixed-content, and TLS failures fail closed. The product must not offer a persistent “ignore certificate errors” control.
- Normalize and display the installation origin before authorization. Follow redirects only according to an explicit policy that prevents token forwarding across origins.
- Do not invent certificate pinning without an operator rotation/recovery design. System trust plus optional managed enterprise trust may be safer than brittle static pins.
- Validate advertised contract identity, version, and checksum before generating or caching dynamic surfaces.
- Keep proxy configuration and custom trust roots managed, visible, and auditable; never silently weaken global transport policy.

## Authorization and non-disclosure

- Send explicit site and other contracted context on every authenticated request; never infer security context from UI selection alone.
- Treat capability metadata as a presentation hint. Always let core decide.
- Scope caches by installation, identity, site, organization/workspace, locale, definition/runtime generation, and policy/security generation.
- On permission or generation change, invalidate affected lists, counts, search indexes, relation choices, field metadata, notifications, and background work.
- Do not turn 403/404 distinctions into record enumeration. Preserve core's problem type and user-safe wording.
- Policy-hidden data must not survive in semantic labels, widget keys, debug inspectors, local search, telemetry dimensions, accessibility announcements, or restored navigation state.

## Mutation safety

- Generate a stable, high-entropy idempotency key per user intent and reuse it only for the byte-equivalent operation and authority context.
- Capture strong ETags and require them for contracted mutations. Never silently fetch-current-and-overwrite after a 412.
- Reconcile timeout/connection-loss ambiguity with idempotent replay or operation status; do not report failure when the server may have committed.
- Present destructive and high-impact confirmations with exact resource/action/context and preserve server-required step-up.
- Do not queue general offline writes. A future offline design must protect authorization freshness, numbering, audit identity, conflict handling, and idempotency expiry.
- Validate download filenames and content types, verify supplied checksums, respect expiry and authorization, and avoid automatically opening active content.

## External browser handoff

The client does not embed web content. It may open a validated HTTPS page in the system browser for unsupported work or use a future core-defined native authorization redirect. That browser is independent of the client and does not make the blocked journey part of the product.

- Never put native bearer tokens, cookies, hidden context, record data, or SDK operation payloads in the URL or browser.
- Do not use a script/native bridge or share a browser cookie store.
- The browser establishes its own session. A native authorization flow may return only a bounded one-time code/state value defined by core and exchanged through the SDK.
- Validate return origins, schemes, state, nonce, installation identity, and expiry; reject unsolicited or replayed deep links.
- Open extension pages only when core provides a currently trusted, authorized HTTPS target. A stale bookmark preserves no authority.

## Extension trust

Core documents trusted PHP extensions as in-process code, not a sandbox. The native client must not increase that authority by downloading or executing extension binaries, Dart code, scripts, or arbitrary client bundles. A future declarative surface is data interpreted by audited client components and remains subject to core signature, ownership, generation, and policy controls. See [Extension client surfaces](extensions.md).

## Privacy and observability

Telemetry is opt-in/managed according to a future privacy decision. Until accepted, collect no product analytics. Operational diagnostics must use allowlisted fields and bounded cardinality.

A support bundle may contain client/core versions, contract checksum, platform, sanitized network category, timestamps, feature identifiers, and correlation IDs. It must exclude credentials, cookies, authorization redirect codes/state, request/response bodies, record identifiers unless explicitly safe, personal data, restricted field labels/values, and local file paths.

## Threats the release gate must exercise

- malicious installation URL, redirect, TLS downgrade, and origin confusion;
- token theft from logs, clipboard, crash reports, backups, notifications, deep links, and local storage;
- stale membership/policy/security epoch and account disable during an active screen;
- cross-site, cross-organization, administrator/portal, and multi-account cache leakage;
- replay, duplicate taps, ambiguous timeout, stale ETag, and idempotency-key misuse;
- hidden-field inference through validation, counts, search, relations, errors, accessibility, and telemetry;
- malicious filenames/content types, oversized responses, decompression bombs, and unsafe external links;
- extension disable, trust revocation, generation replacement, and stale declarative surfaces; and
- rooted/jailbroken or compromised endpoints, with documented limits rather than unsupported guarantees.

Security claims require executable evidence and an explicit residual-risk statement. The client cannot make an insecure server or device secure.
