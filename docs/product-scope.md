# Product scope and non-goals

## Scope statement

The product is one Flutter codebase for native Linux, macOS, Windows, Android, and iOS clients. Every in-client surface is native and consumes supported public Kumwe contracts. Server-rendered pages are not embedded.

The long-term scope covers three separate trust boundaries:

| Surface | Intended client role | Exposure rule |
|---|---|---|
| Administrator | Native operational and configuration journeys for authenticated, authorized administrators | Capability and policy filtered; never inferred from a visible menu item |
| Portal | Native self-service and delegated business journeys for authenticated portal identities | Opt-in per definition, operation, and policy; deny by default |
| Public site | Native public reading only where semantic delivery contracts exist; otherwise the journey is blocked | Anonymous/public exposure only; never reuse administrator credentials |

These surfaces may share presentation primitives, but they must not share sessions, cached restricted data, or route assumptions across trust boundaries.

## Definition of native functional parity

A client journey has native functional parity only when all of the following are true:

1. the same authorized actor and explicit site/organization context can discover and invoke it;
2. inputs preserve the core schema, exact-value types, field visibility/editability, and bounded choices;
3. core performs the same validation, authorization, workflow, approval, transaction, audit, idempotency, and concurrency behavior;
4. output preserves versions, stable identifiers, redaction, pagination, error categories, and operation status;
5. lifecycle changes such as permission loss, definition replacement, extension disable, or trust revocation remove access without stale client authority; and
6. the native journey meets the platform accessibility, localization, and adaptive UX gate.

Visual similarity can help users transfer knowledge, but it is not functional parity. A pixel match cannot compensate for a different authorization or data outcome.

## In-scope capability groups

Subject to core contracts and roadmap gates, the product may include:

- installation discovery, compatibility inspection, sign-in, sign-out, session/token revocation, and site/workspace selection;
- CMS content browse/read/create/update/trash/restore and workflow transitions;
- content type and workflow inspection/management where the wire contract is sufficient for a safe native editor;
- menu and navigation management;
- users, groups, scoped grants, token metadata, and safe one-time token issuance flows for authorized administrators;
- site settings and presentation configuration;
- extension inventory, diagnostics, activation, disable, and uninstall; package installation only if core later exposes a safe supported upload contract;
- schedules, jobs, operation status, reports, exports, and safe plan previews;
- runtime-discovered business definitions, records, relations, history, actions, approvals, reports, and exports;
- portal journeys that core explicitly exposes to the authenticated portal actor;
- public page reading, navigation, localization, media, search, forms, feeds, and SEO-aware sharing when structured public contracts exist; and
- versioned declarative extension-contributed client surfaces that pass trust, ownership, compatibility, security, and accessibility checks.

The detailed surface inventory is in [Features and surfaces](features-and-surfaces.md). Inclusion here is a product requirement, not evidence that the current API or client implements the feature.

## Non-goals

The following are outside the product boundary unless a future ADR explicitly changes them:

- Flutter web;
- an in-app WebView, embedded server page, or hybrid HTML/native feature surface;
- a replacement for Kumwe core, its server-rendered web experiences, or its deployment/operator tools;
- direct database, filesystem, PHP service, Twig template, or internal runtime access;
- client-side replicas of authorization, workflow, approval, audit, numbering, validation, or extension trust;
- compiling or translating arbitrary PHP, Twig, Lit, JavaScript, CSS, or extension templates into Flutter widgets;
- silently converting unsupported extension pages into generic forms;
- shipping a built-in administrator token, shared service account, or custom undocumented login protocol;
- trusting the native UI to enforce secrets, row visibility, field visibility, or destructive-action policy;
- general offline mutation or conflict resolution before core supplies a complete synchronization contract;
- production ERP domain rules hard-coded into the client foundation;
- installing extensions from arbitrary server file paths or bypassing core package signature/trust policy;
- a promise of exact public-theme pixel parity on every platform; and
- unsupported claims such as “bank-grade,” certified, compliant, or fully secure without specific controls and evidence.

## Deliberate deferrals

The following need product or core decisions before implementation:

- the native end-user authorization flow and token audience;
- minimum operating-system and Flutter versions;
- support-window and core/client compatibility policy;
- push notification and deep-link contracts;
- background synchronization and read-only caching policy;
- media upload/download management contracts;
- public semantic delivery and extension client-surface contracts;
- analytics/telemetry policy and data residency; and
- desktop distribution, mobile-store release, signing, and update channels.

Deferral means “not yet decided,” not “implicitly available.” Each item appears in a roadmap gate.
