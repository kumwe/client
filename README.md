# Kumwe native client

Kumwe native client is the planned Flutter application for operating a Kumwe installation from desktop and mobile devices. It is intended to cover the authorized outcomes of Kumwe's administrator, authenticated portal, and public-site experiences while keeping Kumwe core as the only authority for data, policy, validation, workflows, and extension lifecycle.

> [!IMPORTANT]
> This repository is currently a **documentation-first foundation**. It contains no Flutter application, Dart SDK, generated API client, runnable binary, or implemented product capability. Statements about the intended product are requirements or design decisions, not claims of shipped behavior.

This repository is downstream product work for a **proposed Kumwe Version 3 Native Client Platform**. It does not
change, extend, or supply completion evidence for Kumwe core Version 2, Gate A, or Gate B. The independent
Flutter-free SDK foundation lives in [`kumwe/dart-sdk`](https://github.com/kumwe/dart-sdk); this client will consume
released SDK packages once core-owned contracts and SDK compatibility gates are satisfied.

## Product intent

The client will target:

- Linux, macOS, and Windows desktop;
- Android and iOS mobile; and
- native, accessible, adaptive interfaces built with Flutter and Dart.

Flutter web is intentionally out of scope. Kumwe already owns server-rendered public, portal, and administrator web surfaces; a second browser application would duplicate those surfaces without solving the native-client problem.

"Parity" means that a permitted user can reach the same business outcome with the same data, authorization decision, validation, concurrency protection, audit attribution, and stable error semantics. It does **not** mean copying Twig markup, Lit components, CSS, or DOM structure into Flutter. The client will wait for portable semantic/API contracts rather than embed server-rendered pages. The existing Kumwe website remains independently available in a system browser, but leaving the client does not count as client parity.

## Responsibility boundary

| Layer | Owns | Must not own |
|---|---|---|
| Kumwe core | Authoritative data and policy; authentication and authorization; validation; workflows; transactions; audit; extension trust and lifecycle; public web rendering; canonical OpenAPI and runtime capability contracts | Flutter state, platform widgets, or client-only navigation |
| [`kumwe/dart-sdk`](https://github.com/kumwe/dart-sdk) | Typed transport models; request construction; site and authorization headers; Problem Details; ETags; idempotency; capability/version negotiation | Business-policy decisions, hidden defaults, or UI |
| Flutter client | Native presentation; adaptive navigation; accessibility; secure local credential handling; local ephemeral state; user-driven orchestration | Reimplementation of server rules, authoritative persistence, or direct database access |

See [Architecture](docs/architecture.md) and [ADR-0001](docs/architecture/decisions/0001-core-api-dart-sdk-flutter-boundary.md).

## What the API investigation established

The baseline investigation is pinned to [`kumwe/cms@4e5083b3`](https://github.com/kumwe/cms/tree/4e5083b3fe43790605ae5c6c5bf8e392f9822efc). At that revision, core exposes substantial authenticated REST coverage for CMS content, content models and workflows, menus, identity and tokens, settings, extensions, automation, and generic generated business resources. The generated business contract is particularly suitable for a metadata-driven native client.

The same investigation found that a one-to-one **native public-site renderer is not yet supported by a complete structured delivery contract**. The authoritative public site is assembled by server handlers, the public page locator, presentation services, Twig templates, theme assets, translations, and extension contributions. Those journeys remain blocked in the client until APIs exist for resolved public pages, nested navigation, effective presentation, SEO, media, search/forms/feeds, and portable extension surfaces.

The detailed, evidence-based classification is in [Native functional parity](docs/native-functional-parity.md).

## Documentation map

- [Vision and objectives](docs/vision-and-objectives.md)
- [Product scope and non-goals](docs/product-scope.md)
- [Native functional parity](docs/native-functional-parity.md)
- [Features and surfaces](docs/features-and-surfaces.md)
- [Architecture and responsibility boundary](docs/architecture.md)
- [Platform targets](docs/platform-targets.md)
- [Security and authentication](docs/security-and-authentication.md)
- [Accessibility, localization, and adaptive UX](docs/accessibility-localization-adaptive-ux.md)
- [Contracts and source-of-truth lifecycle](docs/contracts-and-source-of-truth.md)
- [Extension client surfaces](docs/extensions.md)
- [Roadmap](docs/roadmap/README.md) and [current status](docs/roadmap/STATUS.md)
- [Architecture decisions](docs/architecture/decisions/)

## Current status

The repository is in Phase 0: product contracts and architectural decisions. No implementation phase has started. Before Flutter code is initialized, the core/API contract gate, SDK boundary, authentication flow, platform support policy, and representative parity journeys must be approved. See [Programme status](docs/roadmap/STATUS.md).

## Contributing

Read the repository [agent and contributor instructions](AGENTS.md), then the narrower [documentation instructions](docs/AGENTS.md) before changing documentation. A contribution must not imply that a planned capability is implemented. Client documents may identify core gaps and propose contracts, but normative Kumwe contracts remain in the core repository.
