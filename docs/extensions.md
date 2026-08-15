# Extension client surfaces

## Position

Kumwe extensions can extend core server behavior and server-rendered interfaces. They do not automatically extend a Flutter application. Native extension parity requires an explicit, versioned, declarative client-surface contract; arbitrary PHP, Twig, Lit, JavaScript, CSS, or downloadable Dart code is not portable or safe to execute in the client.

The machine-readable proposal and SDK-side lifecycle are developed in
[`kumwe/dart-sdk`](https://github.com/kumwe/dart-sdk/blob/main/docs/extension-client-surfaces.md). This document owns
the Flutter host requirements; the two repositories must link the same adopted core contract version rather than
copying divergent schemas.

Until that contract exists, an extension surface is either:

- covered by the generic core API and the client's audited semantic components;
- clearly unavailable in the native client.

## Current core evidence

At the audited revision, core supports signed/trusted packages, immutable runtime publications, lifecycle/trust enforcement, owner-aware capabilities and contributions, administrator and portal routes/navigation/templates/assets, field-presentation contributions, custom business views/actions, and durable integration/report/job contributions. Core describes supported PHP extensions as **trusted in-process code**, not a sandbox.

Those server contributions are valuable inputs but not a Flutter contract:

- a Twig template produces browser markup, not a semantic widget tree;
- a Lit custom element is JavaScript bound to browser APIs;
- a PHP handler runs inside the server and can only be reached through a published route/application contract;
- an extension asset may be visual or executable browser content with no native meaning; and
- hiding a server navigation item does not communicate a native route, field renderer, or offline policy.

See the pinned core [extension architecture](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/docs/architecture/extensions.md) and [generated business surfaces](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/docs/architecture/generated-business-surfaces.md).

## Portability levels

| Level | Extension contribution | Native treatment |
|---|---|---|
| Generic data | Published definition uses core field/action/relation/report types already supported by the SDK/client | Render through audited native generated surfaces |
| Declarative native | Extension publishes a signed compatible client-surface declaration using known semantic components and typed operations | Validate, policy-filter, and render natively |
| Unsupported | No compatible contract or unsafe/unknown component/version | Show an explicit unavailable state; do not render or approximate it |

An extension can use different levels for different surfaces. Native support must never be inferred from extension activation alone.

Core may provide a safe external website URL for unsupported work. The client may open it in the system browser without forwarding native credentials, but that separate-browser journey is not an extension client surface and does not count as parity.

## Proposed client-surface contribution

This section is a requirement proposal for a future core-owned contract, not a manifest schema. Core should define and version the exact grammar.

A portable contribution needs to bind:

- extension owner, package release digest, trusted runtime generation, contract version, and lifecycle status;
- stable surface and route identifiers within the owner's namespace;
- trust boundary: administrator, portal, or public;
- required capability/policy target and explicit portal/public exposure;
- localized navigation label/icon semantics, ordering, and deep-link identity;
- semantic layout/view type and bounded component tree;
- typed data queries and application actions that reference published core operations, never arbitrary URLs/SQL/callbacks;
- field/parameter/result schemas, exact-value types, validation metadata, sensitivity, and disclosure/editability rules;
- accessibility semantics, focus/order intent, error/status regions, and responsive hints;
- presentation tokens/assets with content type, dimensions, checksum, variants, and fallback—not arbitrary executable CSS/script;
- native support version range and explicit unavailable-version behavior; and
- cache/invalidation behavior tied to authorization, definition, and trusted runtime generations.

The signed declaration must be reconciled with registered server contributions during activation so a package cannot advertise a native action that is absent, differently authorized, or owned by another extension.

## Semantic component rules

A client may support a closed, versioned catalog such as collection, detail, form, section, field, relation, status, action, approval, report, export, and safe rich-content regions. Exact names and schemas belong to core's future contract.

Every component must:

- reference typed data already filtered by core;
- have bounded nesting, count, text, asset, query, and action limits;
- degrade explicitly when the client lacks its version;
- carry enough semantics for keyboard, screen reader, text scale, localization, and RTL;
- avoid raw executable code, untrusted HTML, SQL, PHP service IDs, filesystem paths, and arbitrary native method calls; and
- preserve core mutation, approval, ETag, idempotency, Problem Details, and operation-status behavior.

Extension-specific business logic remains in the extension's server application handler behind the same core authorization and transaction boundaries. The client renders inputs and results; it does not download the handler.

## Field presentation

Core field metadata can enable a native editor only when its semantics are complete. A field contribution should declare:

- stable field kind/version and contexts (list/detail/create/update/relation/filter/report);
- canonical value and retained-input shapes;
- display/editability/sensitivity behavior already enforced by core;
- suitable known widget semantics, choices/reference search, constraints, units/precision, and localized labels/help;
- accessibility and unsupported-version behavior; and
- whether safe generic read-only rendering is permitted when editing is unsupported.

Unknown fields must not fall back to an unbounded JSON editor. They remain unavailable unless the contract explicitly permits a complete safe read-only native projection.

## Actions and navigation

- Native navigation is derived only from active, trusted, policy-visible contributions compatible with the client.
- A hidden item is not security; opening a deep link repeats capability, route, generation, and policy checks.
- Actions reference typed core application operations with declared read-only/destructive/idempotent/high-impact semantics.
- High-impact actions preserve step-up and maker/checker approval. A declarative confirmation cannot replace core proof.
- External URLs, downloads, and uploads require explicit safe target types and platform policy. An external browser leaves the client and is never parity.
- Deep links include no secret or hidden record data and must re-resolve the current contribution after launch.

## Lifecycle behavior

The client must key extension metadata and state by installation, site/context, extension owner/release, trusted runtime generation, definition generation, actor, and authorization generation.

On disable, uninstall, replacement, trust revocation, or generation mismatch:

- remove navigation and prevent new route execution;
- cancel or reconcile in-flight reads safely;
- do not assume an already-sent mutation was rolled back;
- invalidate schemas, choices, presentation metadata, cached results, and external-link targets;
- keep local drafts only if policy permits, mark them unavailable, and never auto-submit after reactivation; and
- show a non-enumerating unavailable state rather than stale content.

Core owns data-preservation and purge policy. The client does not delete extension-owned server data because a surface disappeared.

## Security model

- The client executes only its reviewed Flutter/Dart binary. It does not load extension code, dynamic libraries, scripts, or over-the-air Dart.
- Core validates package signature/trust and publishes the declarative contribution; the client additionally validates contract version, checksum, limits, ownership, and supported semantics.
- Declarative metadata is untrusted input for parsing/rendering even when its owner is trusted server code; enforce size/depth/resource limits and safe URL/asset handling.
- Do not expose SDK, secure storage, filesystem, clipboard, camera, microphone, notifications, or native commands through a general extension bridge.
- Optional external website links open in the independent system browser without native bearer tokens, cookies, hidden context, or platform bridges.
- Extension telemetry cannot add arbitrary event names/properties or restricted values; only client-owned allowlisted diagnostics are emitted.

## Extension author expectations

Once core publishes the contract, an extension claiming native support should provide:

- compatible declarative native contribution and explicit unsupported-client behavior;
- conformance fixtures for positive/negative identities and all declared contexts;
- long/RTL strings, high text scale, keyboard, screen-reader, compact/expanded layout evidence;
- exact-value, null/optional, maximum-bound, validation, conflict, retry, and unknown-client-version cases;
- activate/disable/reactivate/upgrade/trust-revoke lifecycle tests; and
- compatibility policy across core contract and client semantic-component versions.

The client repository should maintain a neutral conformance extension rather than depending on a production ERP domain as its only proof.

## Required unavailable-surface UX

When a native surface is unavailable, explain only what the actor is permitted to know:

- an optional “Open the website” link when core supplies an authorized HTTPS target, clearly labeled as leaving the client and not available offline or as parity;
- “Update the client” when the server declares a compatible newer client contract;
- “This extension does not provide a native surface” when that fact is safe to disclose; or
- the same not-found/unavailable result used for denied or inactive contributions when disclosure would enumerate policy or trust state.

Never synthesize an approximate action from a label or undocumented URL.
