# Kumwe native client

[![Status: design foundation](https://img.shields.io/badge/status-design%20foundation-blue)](docs/roadmap/STATUS.md)
[![Target platforms](https://img.shields.io/badge/targets-desktop%20%26%20mobile-blue)](docs/platform-targets.md)

Kumwe native client defines the planned Flutter application for operating a Kumwe installation from desktop and mobile devices. It covers native presentation for administrator, portal and public experiences, with Core retaining authority over data, policy, validation, workflows and extension lifecycle.

## Availability

This repository contains product requirements and architecture decisions. It does **not** contain a Flutter application, generated API client, runnable binary, automated build or published client release. There is no application to install yet. The roadmap and contract requirements remain active development inputs.

The independent, Flutter-free transport foundation lives in [`kumwe/dart-sdk`](https://github.com/kumwe/dart-sdk). This client will consume released SDK packages when their Core contracts and compatibility checks are satisfied. Client readiness is assessed separately from Core and the PHP libraries.

## Platforms and product scope

The planned targets are Linux, macOS and Windows desktop, plus Android and iOS mobile. Flutter web is outside the scope.

Native parity means equivalent authorized outcomes, data, concurrency protection, audit attribution and error semantics. It does not require copying server-rendered markup. Browser-only journeys remain explicit external links and do not count as native parity.

See [product scope](docs/product-scope.md), [platform targets](docs/platform-targets.md) and [accessibility, localization and adaptive UX](docs/accessibility-localization-adaptive-ux.md).

## Contract with Core

| Layer | Responsibility |
| --- | --- |
| [Kumwe Core](https://github.com/kumwe/app) | Authoritative data, authentication and authorization, policy, validation, workflows, transactions, audit, extension lifecycle and versioned wire contracts |
| [Dart SDK](https://github.com/kumwe/dart-sdk) | Typed transport, site and authorization headers, Problem Details, ETags, idempotency and capability/version negotiation |
| Flutter client | Native presentation, adaptive navigation, accessibility, secure local credential handling and user-driven orchestration |

The client must not reimplement server business rules, access the database, or use undocumented transport fields. Offline mutation needs a Core-owned synchronization and conflict contract before it can be supported.

The maintained boundary and evidence requirements are in [architecture](docs/architecture.md), [contracts and source of truth](docs/contracts-and-source-of-truth.md), and [security and authentication](docs/security-and-authentication.md). [Native functional parity](docs/native-functional-parity.md) records the API audit at its exact Core revision; it is not a claim about every later Core release.

## Development

Start with the [current status and open dependencies](docs/roadmap/STATUS.md), [roadmap](docs/roadmap/README.md) and [architecture decisions](docs/architecture/decisions/). Implementation depends on the Core API contract, SDK compatibility, authentication, platform support and representative parity journeys.

Read [contributor instructions](AGENTS.md) and [documentation instructions](docs/AGENTS.md). Documentation changes require local-link, terminology, status and contradiction checks. Requirements and proposals must stay distinguishable from implemented behavior. Do not initialize Flutter or generate resource models before the documented contract gates are satisfied.

The [documentation index](docs/README.md) links the complete product and development documentation.
