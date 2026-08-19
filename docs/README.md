# Client documentation

This directory is the product and architecture baseline for the planned Kumwe native client. Nothing here is evidence of a runnable client unless it links to implementation and executable verification.

## Product

| Document | Purpose |
|---|---|
| [Vision and objectives](vision-and-objectives.md) | Desired outcome, principles, and measures of success |
| [Product scope and non-goals](product-scope.md) | Included trust boundaries, parity definition, and explicit exclusions |
| [Native functional parity](native-functional-parity.md) | Pinned API/site audit, current feasibility, blocked surfaces, gaps, and acceptance model |
| [Features and surfaces](features-and-surfaces.md) | Planned journeys across administrator, portal, public, and system surfaces |
| [Platform targets](platform-targets.md) | Desktop/mobile target policy and the no-Flutter-web decision |
| [Accessibility, localization, and adaptive UX](accessibility-localization-adaptive-ux.md) | Cross-platform experience requirements |

## Architecture and contracts

| Document | Purpose |
|---|---|
| [Architecture](architecture.md) | Core → Dart SDK → Flutter client responsibility boundary |
| [Security and authentication](security-and-authentication.md) | Trust boundaries, current API facts, and native-auth requirements |
| [Contracts and source-of-truth](contracts-and-source-of-truth.md) | Contract authority, generation lifecycle, compatibility, and drift controls |
| [Extension client surfaces](extensions.md) | Native extension model, blocked-surface behavior, and required future core contracts |
| [ADR-0001](architecture/decisions/0001-core-api-dart-sdk-flutter-boundary.md) | Accepted SDK boundary decision |

## Delivery

| Document | Purpose |
|---|---|
| [Roadmap](roadmap/README.md) | Phases, gates, and exit criteria |
| [Current status](roadmap/STATUS.md) | What exists, what does not, blockers, and immediate next work |

## Evidence baseline

The initial investigation is pinned to [`kumwe/app@4e5083b3`](https://github.com/kumwe/app/tree/4e5083b3fe43790605ae5c6c5bf8e392f9822efc). Core remains authoritative. The repository's historical ERP-readiness prompts supplied product direction, but they are not treated as shipped contracts.

Read [`AGENTS.md`](AGENTS.md) before changing these documents.
