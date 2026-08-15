# Vision and objectives

## Vision

Kumwe native client will make a Kumwe installation feel at home on desktop and mobile without creating a second source of business truth. A user should be able to discover the work they are allowed to perform, complete it with native controls, and receive the same result that Kumwe would produce through its server-rendered or machine surfaces.

This is a client of the Kumwe platform, not a fork of the platform. Core remains responsible for the difficult guarantees: tenant and organization context, authorization before disclosure, exact-value behavior, validation, workflow, approval, concurrency, idempotency, audit, extension trust, and persistence.

## Product objectives

1. **Equivalent authorized outcomes.** Cover the administrator, authenticated portal, and public journeys that core exposes through stable contracts. The same actor, data, and action must produce the same stored result, version, audit attribution, and error category.
2. **A first-class native experience.** Use platform-appropriate windows, navigation, keyboard shortcuts, focus, screen readers, touch targets, notifications, file pickers, and secure storage. Do not wrap the entire existing website and call it a desktop application.
3. **Contract-driven breadth.** Discover published business definitions, fields, views, actions, relations, reports, and extension capabilities at runtime where core supplies metadata, instead of hard-coding every ERP domain.
4. **Safe evolution.** Generate a typed Dart transport layer from core's canonical OpenAPI contract, pin compatibility evidence to core revisions and checksums, and fail clearly when a server is outside the supported range.
5. **Honest gaps.** Keep a native journey blocked until a portable contract exists. An optional external-browser handoff may help a user reach the separate Kumwe website, but it is not part of the client and is never counted as parity.
6. **Inclusive by construction.** Treat WCAG 2.2 AA, keyboard use, screen-reader behavior, text scaling, localization, bidirectionality, reduced motion, and adaptive layouts as release criteria.
7. **Operationally trustworthy.** Protect credentials and restricted data, preserve retry/concurrency semantics, avoid secret-bearing telemetry, and give support teams actionable correlation and compatibility information.

## Guiding principles

- **Core decides; the client presents.** A control can be hidden for usability, but server authorization is always final.
- **Metadata may describe behavior; it does not replace behavior.** A generated form still submits to the same application service and policy checks as every other adapter.
- **Semantic parity beats markup parity.** Flutter controls should express the same field, state, and action meaning without reproducing Twig or Lit implementation details.
- **Secure failure beats guessed continuity.** Stale policy, unsupported contract versions, missing site context, or ambiguous mutation results fail closed and remain recoverable.
- **One adaptive product, not five OS-specific forks.** Platform integrations differ at the boundary; application use cases and semantics remain shared.
- **Public web remains valuable and separate.** The existing server-rendered site is the canonical browser experience; it is not embedded into the native product.

## Success measures

The product is not successful merely because it can issue API requests. Release evidence must show:

- representative journeys complete on each supported platform and produce the same core state as their reference surface;
- no native adapter bypasses the SDK or recreates server authorization and validation;
- all mutation journeys demonstrate idempotent retry and optimistic-conflict recovery;
- permission-denied records and fields do not leak through lists, caches, search, error text, accessibility labels, notifications, or telemetry;
- critical keyboard and screen-reader journeys pass the accessibility gate;
- supported locales, long text, right-to-left layout, and exact number/money/quantity rendering pass the localization gate;
- extension activation, disable, trust revocation, and contract-generation changes are reflected without stale executable client surfaces; and
- blocked native surfaces are reported explicitly and safely, without being counted as delivered through an external browser.

Quantitative latency, scale, crash-free, minimum-OS, and support-window targets are intentionally undecided in this foundation. They must be measured and accepted before a production release rather than invented here.

## Product relationship to the ERP programme

Kumwe core's ERP-readiness direction calls for independently installable components, immutable business definitions, generated administrator and opt-in portal surfaces, adapter parity, extension SDKs, and high-integrity operation. The native client should consume those public contracts once shipped and verified. It must not treat historical programme prompts as proof that a core feature exists, nor move ERP domain logic into Flutter.

Accounting, inventory, purchasing, sales, CRM, manufacturing, payroll, projects, assets, service, and point-of-sale remain extension-owned domains. The client provides reusable native presentation for published semantics; it does not hard-code those domains into its foundation.
