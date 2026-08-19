# ADR-0001: Core API, Dart SDK, and Flutter client boundary

- **Status:** Accepted
- **Date:** 2026-08-15
- **Decision owners:** Kumwe client maintainers
- **Core evidence baseline:** [`kumwe/app@4e5083b3`](https://github.com/kumwe/app/tree/4e5083b3fe43790605ae5c6c5bf8e392f9822efc)

## Context

Kumwe core already owns application behavior across server-rendered administrator, portal, public, REST, CLI, MCP, worker, and extension surfaces. Its API requires exact security context, supports ETags and idempotency, returns Problem Details, and can assemble runtime-specific business contracts from installed definitions and trusted extensions.

A Flutter client needs typed access to those contracts across five target operating systems. Calling HTTP directly from screens would duplicate headers, retries, errors, serialization, and compatibility decisions. Moving PHP business logic into Dart would create a second authority that can disagree about authorization, workflow, exact values, audit, and extension lifecycle. A hand-maintained model layer would also drift from generated OpenAPI.

The product decision is all-native: server-rendered pages are not embedded in the application. A missing API or semantic client contract blocks the native journey until core is mature enough to supply it.

## Decision

Adopt three responsibility layers with inward ownership:

1. **Kumwe core is authoritative.** It owns data, authentication and authorization, policy-filtered disclosure, validation, exact-value rules, workflow and approval, transactions, concurrency, idempotency, audit, reporting, extension trust/lifecycle, and canonical versioned contracts.
2. **A Flutter-independent Dart SDK owns transport.** Its generated API layer comes from the canonical OpenAPI 3.1 contract. A narrow handwritten runtime owns authorization/context injection, ETag and idempotency mechanics, Problem Details, retries, cancellation, streaming, capability/generation discovery, and conformance. It makes no business decisions.
3. **The Flutter application owns native presentation.** Application coordinators and widgets consume SDK types/operations; they do not call HTTP, read core internals, or reproduce core rules. Platform authority is isolated behind narrow adapters.

Additional constraints:

- generated API code is isolated from handwritten SDK code and is never manually patched;
- the SDK has no Flutter dependency;
- Flutter/application packages have no direct HTTP dependency;
- runtime-discovered definitions and extension client surfaces are interpreted through a bounded, versioned semantic model and remain policy-filtered by core;
- unknown/incompatible fields, actions, or surfaces fail explicitly rather than becoming generic JSON editors;
- every in-client surface is native and API-backed; no in-app WebView or hybrid HTML surface is permitted;
- an optional system-browser link is outside the client, receives no native credentials, and is never parity evidence; and
- native and reference adapters must prove equivalent stored outcomes, versions, audit attribution, errors, and denials through shared core behavior.

## Consequences

### Positive

- One protocol implementation serves every native platform and can be conformance-tested without Flutter.
- Schema drift becomes visible at generation/fixture time instead of appearing as screen-specific bugs.
- Core remains the security and business authority.
- Flutter code can focus on accessibility, adaptive behavior, platform integration, and user intent.
- Runtime-generated ERP/extension surfaces can scale without hard-coding each domain when semantic contracts exist.
- Missing core contracts remain honest blockers rather than being hidden by HTML embedding.

### Costs and constraints

- Core OpenAPI and discovery contracts must mature before broad client implementation.
- A generator cannot fix an ambiguous or false server contract; core changes and compatibility fixtures are dependencies.
- Dynamic runtime schemas require generation- or metadata-aware caches and more conformance coverage than a static API alone.
- Some existing browser-only journeys, including current high-impact step-up and public presentation, remain unavailable in the client until native contracts exist.
- The separate SDK adds package boundaries, release/version coordination, and testing work.
- Product teams cannot ship a quick screen by bypassing the SDK.

## Alternatives considered

### Direct HTTP from Flutter screens

Rejected. It scatters security headers, serialization, retries, idempotency, concurrency, and errors across UI code and makes parity untestable.

### A fully handwritten Dart client

Rejected. It invites schema drift and turns observation/documentation into a competing wire contract. Handwritten protocol infrastructure remains useful only around generated DTOs and operations.

### Share or port core business logic into Dart

Rejected. Client-side copies cannot be authoritative, become stale, and risk disclosing or accepting data that core would deny. Core application services remain the only execution path.

### Generate Flutter widgets directly from OpenAPI

Rejected. OpenAPI describes transport, not adequate interaction, accessibility, adaptive layout, workflow, sensitivity, or field-presentation semantics. A separate bounded runtime semantic contract is required.

### Embed server-rendered pages in a WebView

Rejected. It would make the client hybrid, blur credential/session boundaries, and disguise missing native contracts as parity. Users may independently open the existing website in their system browser, but that is outside the product surface.

## Verification

The decision is satisfied only when architecture and conformance tests demonstrate the dependency rules, generated provenance, exact protocol semantics, runtime schema handling, native-only surfaces, and equivalent reference/native outcomes described above. At the date of this ADR, those tests and implementation do not exist; the decision guides future phases.
