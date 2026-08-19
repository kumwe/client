# Programme status

| | |
|---|---|
| **Status date** | 2026-08-18 |
| **Client phase** | Phase 0 — Product truth and decisions |
| **Core audit baseline** | [`kumwe/app@4e5083b3`](https://github.com/kumwe/app/tree/4e5083b3fe43790605ae5c6c5bf8e392f9822efc) |
| **Implementation** | Not started |

## Executive status

This repository contains a documentation-first client foundation and two accepted architecture decisions. It
contains no embedded Dart SDK, Flutter project, generated client, automated test, build, or runnable product
capability. The sibling SDK repository has an executable protocol foundation, but no generated resource client or
qualified compatibility profile yet.

The product's sign-in is now decided and recorded across all three repositories:
[ADR-0002](../architecture/decisions/0002-authentication-link-one-client-and-the-account-switcher.md) selects the
authentication link with an area chooser, guest arrival, a multi-deployment account switcher, persistent
sessions, and the web-session handoff; core records the matching decision D17 (ADR 0009, ledger lane N
`V3-NC-001` … `V3-NC-004`) and the SDK carries the revised wire proposals plus endpoint-free flow primitives.
Implementation remains gated exactly as before — nothing is buildable until core adopts the contracts.

The baseline is downstream preparatory work for a **proposed Kumwe Version 3 Native Client Platform**. It does not
change or provide completion evidence for core Version 2, Gate A, or Gate B. The SDK foundation is developed
separately in [`kumwe/dart-sdk`](https://github.com/kumwe/dart-sdk); neither repository can adopt a core programme
or close a core-owned gate.

Phase 0 is **under review**. Gate 0 has not been assessed. Gate A is blocked on verified core contract maturity; all later gates are blocked in sequence.

The core [`docs/roadmap/STATUS.md`](https://github.com/kumwe/app/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/docs/roadmap/STATUS.md) present at the audit revision also reports its own Phase 0 truth/contracts work and public contract classification as open. That core status is independently governed and must be rechecked at the revision selected for Gate A; this client status does not supersede it.

## Phase board

| Phase | State | Gate | Primary blocker |
|---|---|---|---|
| 0 — Product truth and decisions | Under review | Gate 0 not assessed | Product/core-owner review and open decision ownership |
| 1 — Core contract maturity | Not started | Gate A not assessed | Gate 0; core-owned API/auth/public/media/extension gaps |
| 2 — Dart SDK and conformance | Parallel foundation active | Gate B not assessed | Gate A; generated clients and adopted runtime/auth contracts |
| 3 — Native shell/platform foundation | Not started | Gate C not assessed | Gates 0, A, and B |
| 4 — CMS administrator parity | Not started | Gate D not assessed | Gate C and capability-specific core contracts |
| 5 — Generated business/portal parity | Not started | Gate E not assessed | Gate D and native step-up/portal contracts |
| 6 — Native public/extension surfaces | Not started | Gate F not assessed | Public delivery/presentation and client-surface contracts |
| 7 — Production/release qualification | Not started | Gate G not assessed | All claimed feature/platform gates |

## What the baseline establishes

- The client is Flutter/Dart and targets Linux, macOS, Windows, Android, and iOS; Flutter web is excluded.
- Every in-client feature is native and API-backed. There is no WebView/hybrid fallback.
- External-system-browser links are outside the client, receive no native credentials, and do not satisfy parity.
- Parity means equivalent authorized outcomes and contract semantics, not DOM or pixel identity.
- Kumwe core is authoritative; a Flutter-independent Dart SDK owns transport; Flutter owns presentation.
- General offline mutation, copied business/security rules, arbitrary extension code, and direct database/internal access are excluded.
- Current public-site, native-auth/step-up, media, contract-truth, and extension-surface gaps are explicit blockers.

These are documentation/architecture decisions only, not implemented features.

## Audit result at the pinned core revision

| Area | Result |
|---|---|
| Authenticated CMS/admin API | Broad route coverage; production SDK blocked by contract drift/completeness and native auth decisions |
| Generated business API | Strongest native foundation; runtime-generated schemas exist, but high-impact decision step-up is browser-session-bound at the audit |
| Public site | Authoritative HTML rendering exists; no complete anonymous structured resolved-page/presentation contract, so native public parity is blocked |
| Media | Public asset streaming and server administrator UI exist; no complete REST management family found |
| Extensions | Rich trusted server contribution system exists; no portable declarative Flutter client-surface contract found |
| OpenAPI | Canonical artifacts exist, but the audit found concrete runtime/schema drift for content and menu-item presentation plus incomplete responses |

See [Native functional parity](../native-functional-parity.md) for evidence.

## Gate 0 review items

- Confirm the representative parity journeys and which core web-only/operator responsibilities intentionally remain outside the native product.
- Assign owners for the core contract capability groups and decide how client requirements enter core governance.
- Accept or revise the native authorization/step-up requirements (the authentication-link selection is recorded in ADR-0002; the step-up half remains open).
- Decide minimum platform versions, distribution order, and support lifecycle—or assign them to the qualification phase explicitly.
- Decide compatibility support range, analytics/privacy posture, push/deep-link scope, and read-cache policy.
- Review ADR-0001 and the all-native/no-WebView constraint.

## Core blockers before generated resource clients

1. Correct and fixture-test static/dynamic OpenAPI against runtime responses.
2. Publish installation, capability, version, and native authorization/step-up discovery — the selected authentication-link flow with areas, guest arrival, and the web-session handoff, now seeded in core's roadmap as lane N (`V3-NC-001` … `V3-NC-004`, decision D17/ADR 0009) with the wire proposals in `kumwe/dart-sdk`.
3. Complete bounded CMS collections and media-management contracts needed by selected administrator journeys.
4. Publish policy-filtered portal projection for every selected generic business operation.
5. Publish anonymous public page/navigation/localization/presentation/media/search/form contracts for selected public scope.
6. Publish a signed, owned, lifecycle-aware declarative extension client-surface contract.
7. Publish compatibility/deprecation and conformance fixtures for the supported client range.

## Immediate next action

Review and accept the Phase 0 documents, then open core-owned contract work from the evidence-backed capability
groups while the SDK foundation matures in parallel. Do not initialize Flutter or generate endpoint/resource models
until Gate A supplies a truthful consumable contract and the SDK G2–G4 inputs mapped to client Gate B are stable.

## Evidence rule

This status advances only with links to exact core/client revisions and executable evidence. A historical prompt, mocked API, screenshot, external-browser journey, or documentation assertion cannot complete a gate.
