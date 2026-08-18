# Kumwe native client roadmap

## Purpose

This roadmap sequences client work and the client-facing core dependencies required for honest native parity. It does not replace Kumwe core's roadmap or make proposals in this repository normative for core.

It is preparatory planning for a **proposed Kumwe Version 3 Native Client Platform**. Kumwe core Version 2 and its
Gate A/Gate B remain unchanged and independently governed. The Gate A/Gate B labels below are working gates for
this proposed native-client programme; they are not claims about, evidence for, or modifications to the Version 2
gates. Core maintainers must adopt any future Version 3 gate in the core roadmap before it becomes authoritative.

The product is deliberately waiting for mature, testable core contracts. No phase may bypass a blocked contract with an in-app WebView, scraped HTML, direct database access, or duplicated business rules. An external-system-browser link is outside the client and cannot satisfy a gate.

Current position is recorded in [`STATUS.md`](STATUS.md).

## Gate discipline

- A phase describes work; its gate describes the evidence required to expand scope.
- Gate completion requires linked executable evidence at exact core, SDK, client, and platform versions.
- A route existing is not enough: schemas, security, errors, pagination, lifecycle, compatibility, and representative outcomes must agree.
- Requirements owned by core are proposed and tracked here only as dependencies. They are completed only by evidence in the core repository/release.
- Features remain “not implemented” until client code and tests land, regardless of documentation completeness.
- Release order can change, but later phases cannot weaken an earlier gate.

## Phases and gates

### Phase 0 — Product truth and decisions

Deliver:

- vision, product scope, parity definition, surface inventory, and non-goals;
- pinned API/public-site feasibility audit;
- core → Dart SDK → Flutter boundary ADR;
- platform, security/authentication, accessibility/localization/adaptive, contract lifecycle, and extension-surface requirements; and
- roadmap/status governance.

**Gate 0 — Foundation accepted**

- Product owners accept the all-native/API-only scope and no-Flutter-web decision.
- Core and client owners accept the responsibility boundary and definition of parity.
- Audit findings have exact core evidence and no historical prompt is mistaken for shipped behavior.
- Open decisions and core dependencies have owners or explicit decision forums.
- Documentation is contradiction- and link-reviewed.

Gate 0 authorizes contract work, not Flutter initialization by itself.

### Phase 1 — Core contract maturity

Core-owned dependencies to verify or deliver:

- complete truthful OpenAPI 3.1 with response schemas, examples, headers, Problem Details, security schemes, drift fixtures, and compatibility policy;
- installation/client/version/capability discovery and a supported native user authorization, revocation, logout, and step-up contract — for the selected sign-in ([ADR-0002](../architecture/decisions/0002-authentication-link-one-client-and-the-account-switcher.md)) that means the authentication link with area binding, the non-enumerating guest arrival lifecycle, rotating refresh families, and the single-use web-session handoff, tracked in core's ledger as `V3-NC-001` … `V3-NC-004`;
- bounded pagination/search/filter/sort and media management needed by administrator journeys;
- complete generated-business definition/record/action/relation/report/export contracts for administrator and explicitly exposed portal actors;
- anonymous structured public page, route, navigation, localization, presentation, SEO, media, search, and form contracts for selected public scope;
- signed, owned, lifecycle-aware declarative extension client-surface contracts; and
- deterministic generation/checksum/invalidation behavior across trusted runtime and definition changes.

**Gate A — Core is consumable**

- Representative live responses validate against the same installation's base and generated OpenAPI.
- Positive and denied identities demonstrate non-enumeration and field/action omission.
- ETag, idempotency, operation status, error, retry, exact-value, locale, and lifecycle fixtures pass.
- Native authentication/step-up and all selected parity surfaces have stable supported contracts.
- No selected journey relies solely on browser/session handlers or server markup.
- Core publishes a client compatibility/deprecation policy and a versioned conformance fixture set.

### Phase 2 — Dart SDK and conformance

This phase is delivered in the separate [`kumwe/dart-sdk`](https://github.com/kumwe/dart-sdk) repository and
consumed here as a versioned package artifact. An internal pre-1.0 artifact may satisfy this integration gate when
its public API and compatibility evidence are pinned; stable SDK Gate G6 is not a prerequisite for initializing the
client. The client repository may carry compatibility evidence, but it does not own or duplicate the SDK.

Deliver:

- reproducible pinned OpenAPI generation;
- Flutter-independent generated transport and handwritten SDK runtime;
- secure credential-provider abstraction, exact context, ETag, idempotency, Problem Details, retry/cancellation, streaming, and capability/generation support;
- representative base and runtime-generated contract fixtures; and
- compatibility manifest for supported core versions.

**Gate B — SDK qualified**

For cross-repository mapping, this gate requires SDK G2 (generated invariant alpha), G3 (dynamic runtime alpha), and
G4 (native authorization/context beta) for the selected initial slice. SDK G5 profile qualification grows with
later client parity phases, and SDK G6 stable release is a production-release dependency rather than an
initialization dependency.

- Generated output is reproducible and contains provenance.
- Runtime fixtures, negative/security cases, unknown additive fields/enums, exact values, uploads/downloads, and lifecycle invalidation pass.
- Architecture tests prevent Flutter and business-policy dependencies.
- The SDK supports oldest/newest declared core versions or rejects them before mutation with an actionable error.

### Phase 3 — Native shell and platform foundation

Deliver:

- Flutter application initialization only after Gates 0, A, and B;
- installation/account/context shell, native authorization, secure storage, routing, adaptive navigation, localization, themes, diagnostics, and external-link policy;
- shared native semantic component library and platform adapters; and
- Linux, macOS, Windows, Android, and iOS test harnesses.

**Gate C — Native foundation qualified**

- One read and one retryable versioned mutation complete on all five target operating systems through the SDK.
- Sign-in/out/revocation, cross-account/site cache isolation, process death/restart, and ambiguous timeout behave safely.
- Keyboard, screen-reader, text-scale, RTL/long-text, compact/expanded layout, and high-contrast evidence passes for the slice.
- No UI/application package calls HTTP or embeds web content.

### Phase 4 — CMS administrator parity

Deliver selected native administrator journeys for content, content models/workflows, navigation, media, settings/presentation, identity/access, extensions lifecycle, automation, and diagnostics.

**Gate D — CMS administrator parity**

- Reference and native journeys produce equivalent state, versions, audit, errors, idempotent replay, and conflict behavior.
- Large collections remain bounded; dynamic fields and media selectors are complete and accessible.
- Secret-once, destructive, lifecycle, trust-revocation, and permission-change cases pass.
- Any administrator feature lacking a complete native contract remains outside the claimed surface.

### Phase 5 — Generated business and portal parity

Deliver native definition-driven workspaces, collections, detail/forms, relations, lines, history, actions, approvals/step-up, operation status, reports, and exports for administrator and explicitly enabled portal actors.

**Gate E — Business parity**

- A neutral extension fixture completes the same lifecycle through reference and native adapters.
- Row/field/action policy, exact money/quantity/decimal, concurrency, idempotency, approval, step-up, reports, exports, and extension lifecycle pass.
- Portal operations remain opt-in and separate from administrator identity/cache/navigation.
- Unsupported custom fields/surfaces fail explicitly and never become raw JSON editors.

### Phase 6 — Native public and extension surfaces

Deliver native public routing/homepage, navigation, localization, structured content/media, SEO/share, search/forms where selected, semantic presentation tokens, and compatible declarative extension surfaces.

**Gate F — Public and extension parity**

- Public native results match core resolution, publication, canonical path, locale alternatives, navigation, and semantic presentation fixtures.
- Search/forms meet abuse, privacy, validation, upload, accessibility, and localization requirements.
- Extension activate/upgrade/disable/revoke removes and restores native surfaces at exact runtime generations.
- Unknown surface versions remain unavailable; neither server HTML nor arbitrary code is executed in the client.

### Phase 7 — Production and release qualification

Deliver distribution/signing/update channels, platform support matrix, threat and privacy review, performance/capacity budgets, resilience, accessibility/localization evidence, compatibility and upgrade fixtures, observability/support tooling, and release provenance.

**Gate G — Release qualified**

- Every claimed surface and platform passes its parity, security, accessibility, localization, adaptive, lifecycle, update, recovery, and compatibility evidence.
- Release artifacts are signed, reproducible where practical, scanned, checksummed, and linked to exact source/dependencies/contracts.
- No known repository-owned critical/high issue remains; bounded residual/external risks are explicit.
- Product documentation lists only shipped capabilities, supported core/platform ranges, and honest limitations.

## Cross-phase proof journeys

The same neutral fixtures should grow across phases instead of creating unrelated demonstrations:

- connect/authorize/select site and context;
- content create/edit/conflict/transition/translation/navigation/public resolution;
- generated business create/relate/reorder/action/approval/report/export;
- denied actor/field/action/count/search and cross-site/organization attempts;
- ambiguous network result/idempotent replay, server upgrade, definition replacement, extension trust revocation; and
- keyboard/screen-reader/text-scale/RTL and compact/expanded layouts on every supported platform.

## Roadmap changes

Change phase/gate definitions only with an ADR or explicit product decision when scope or architecture changes materially. Update [`STATUS.md`](STATUS.md) in the same change. Never mark a core dependency complete solely from a client mock or local schema copy.
