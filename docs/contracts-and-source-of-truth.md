# Contracts and source-of-truth lifecycle

## Authority

Kumwe core is the sole authority for server behavior and wire contracts. This repository can consume, test, and propose changes to those contracts; it cannot redefine them.

The Flutter-independent [`kumwe/dart-sdk`](https://github.com/kumwe/dart-sdk) consumes adopted core contracts and
owns Dart transport/package behavior. It can publish proposal schemas and conformance requirements for discussion,
but a proposal becomes a Kumwe wire contract only when core adopts, versions, tests, and releases it.

When sources disagree, use this order and open a mismatch rather than silently choosing:

| Priority | Source | What it proves |
|---|---|---|
| 1 | Current core runtime behavior plus executable security/application tests | What the server actually enforces at that revision |
| 2 | Canonical OpenAPI emitted/committed by that same core revision and installation generation | Supported machine shape and operations |
| 3 | Core public compatibility fixtures and version/deprecation records | Stability promise across releases |
| 4 | Stable core documentation | Human guidance and intended use |
| 5 | Client ADRs and requirements | How this repository consumes or requests the contract |
| 6 | Historical prompts, plans, screenshots, and observed private JSON | Context only; never a production contract |

Runtime behavior winning a disagreement does not make undocumented behavior safe to consume. The correct response is to repair the core contract/documentation and add a regression fixture before generating a client.

## Initial baseline

The initial client investigation is pinned to:

- core repository: [`kumwe/app`](https://github.com/kumwe/app);
- core revision: [`4e5083b3fe43790605ae5c6c5bf8e392f9822efc`](https://github.com/kumwe/app/tree/4e5083b3fe43790605ae5c6c5bf8e392f9822efc);
- static contract: [`api/openapi/kumwe-v1.json`](https://github.com/kumwe/app/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/api/openapi/kumwe-v1.json); and
- stable API guide: [`docs/rest-api.md`](https://github.com/kumwe/app/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/docs/rest-api.md).

Installed sites may assemble `/api/v1/openapi.json` from active trusted runtime and published business definitions. A static repository contract alone cannot describe every runtime-contributed schema. Compatibility evidence must therefore include both the core base contract and representative installed-site generated contracts.

## Contract classes

The client depends on several related contracts. They should not be collapsed into one undocumented “API version.”

| Contract | Example responsibility | Required identity |
|---|---|---|
| Base wire contract | Routes, headers, DTOs, Problem Details, security schemes | Core semantic version/revision and OpenAPI checksum |
| Runtime-generated contract | Published definitions, fields, views, actions, relations, reports | Site, trusted runtime generation, definition generation, authorization generation, checksum |
| Capability/discovery contract | Supported client features, auth flow, limits, locales, extension surface versions | Installation and site identity plus generation/version |
| Data concurrency contract | ETags, record versions, idempotency, operation status | Resource/operation identity and authority context |
| Presentation contract | Semantic field/widgets, design tokens, public page projection | Contract version, theme/surface owner and generation |
| Extension client contract | Signed declarative routes/surfaces/actions/assets | Extension owner, release digest, runtime generation, schema version |
| Client compatibility policy | Which core/contract generations a client release supports | Client version and tested core range |

Core remains normative for every class it publishes. The client can record its supported subset and explicit unavailable behavior.

## Source lifecycle

The intended lifecycle is:

1. core changes an application capability and its stable delivery contract together;
2. core generates deterministic OpenAPI, compatibility fixtures, examples, and contract checksum;
3. core tests representative runtime responses against that contract, including negative/security cases;
4. `kumwe/dart-sdk` pins the new core/contract input and records the compatibility decision;
5. the SDK's Dart transport layer is regenerated reproducibly, with a clean diff and no hand-edited generated files;
6. handwritten SDK runtime maps transport mechanics without changing business meaning;
7. SDK conformance runs against core fixtures and at least one live installed-site generated contract;
8. this client pins a qualified SDK artifact and runs Flutter parity journeys that compare reference and native outcomes; and
9. each release records its supported core/SDK range, checksums, generated artifacts, tests, blocked surfaces, and optional external links.

A core change is not complete for the client merely because code generation succeeds. Security/non-enumeration, retry, ETag, unknown-field, locale, accessibility metadata, and extension lifecycle fixtures must also pass.

## Generation policy

Generation is owned by `kumwe/dart-sdk`. Its qualification evidence must show:

- generation input is checked in or fetched by exact immutable digest, never “latest”;
- the generator, templates, formatter, and Dart language/runtime versions are pinned;
- generated files carry provenance and are reproducible byte-for-byte;
- generated code is isolated from handwritten SDK code;
- no generated file is manually fixed; repair the generator or core contract;
- `additionalProperties`, nullable fields, unions, discriminators, formats, exact numeric strings, binary bodies, and headers receive explicit conformance tests;
- operation names and schema collisions fail the core contract build rather than being renamed unpredictably in Dart; and
- secret responses and redacted/permission-dependent projections receive distinct types or explicit handling where the contract supports it.

The SDK package version does not masquerade as the core version. Release metadata records both.

## Compatibility rules the client requires

Core must publish the normative compatibility and deprecation rules. The client gate expects at least:

- additive optional response fields do not break tolerant readers, while generated strict validation is tested deliberately;
- removing/renaming fields, changing requiredness, narrowing formats, changing enum meaning, altering auth/context, or changing status/problem semantics is treated as breaking unless versioned;
- unknown enum/field/widget/action kinds have a declared safe client behavior;
- deprecation has machine-readable replacement and removal windows;
- runtime-generated schemas are immutable for their named generation;
- extension disable/trust revocation invalidates contributed contracts immediately and cannot leave a callable stale route;
- strong validators and contract checksums change when their represented semantics change; and
- old supported client releases can reject a new incompatible server before sending mutations.

The client should prefer explicit capability negotiation over version-number guessing.

## Current blockers found by the audit

At the pinned revision, the following prevent an honest production SDK or complete native client:

1. static OpenAPI/runtime response drift for content and menu-item presentation fields;
2. incomplete response schemas on some route families;
3. no complete installation/native-auth discovery contract;
4. no anonymous resolved public-page/navigation/presentation delivery contract;
5. no complete REST media-management surface;
6. incomplete content collection pagination/search/filter semantics for a large native workspace;
7. no portable extension client-surface contract; and
8. browser-session-only step-up for high-impact approval decisions.

The detailed evidence and capability groups are in [Native functional parity](native-functional-parity.md). These are requirements against core or product design; writing them here does not make them part of Kumwe core.

## Proposed discovery information

Before authorization or feature routing, a client needs a small, non-secret, cacheable discovery document. This is a proposal for core owners to design, not a schema defined by this repository. It should convey, at minimum:

- canonical installation identity and allowed API/browser origins;
- core product and compatibility version;
- base and generated contract URLs/checksums/generations;
- supported native authorization, logout, and step-up methods — for the selected sign-in this means
  advertising the `authentication_link` profile and the enabled areas (administrator, portal), per
  [ADR-0002](architecture/decisions/0002-authentication-link-one-client-and-the-account-switcher.md);
- site/context discovery rules;
- public delivery, media, localization, extension client-surface, push, deep-link, and external-browser-link capabilities;
- request/body/page limits relevant before login; and
- minimum/maximum supported client contract versions with an external support/upgrade URL that receives no native credentials.

Sensitive capabilities and policy-filtered definitions remain authenticated resources.

A machine-readable draft of this document and of the authentication-link, guest-arrival, and web-session
handoff wire shapes exists as the `kumwe/dart-sdk` proposal corpus (`kumwe.native-discovery` and
`kumwe.native-authorization`, both `0.2.0-proposal.2` at this writing). Those proposals are non-authoritative
until core adopts descendants of them; this document cites them as the current design input, not as contracts.

## Contract validation matrix

Each supported client release must validate:

| Dimension | Minimum evidence |
|---|---|
| Base core | Oldest and newest supported core versions plus next-version compatibility fixture |
| Database/runtime | Representative installed site with dynamic business and extension definitions |
| Actor | Administrator, portal, restricted identity, expired/revoked identity, and anonymous public where applicable |
| Context | Multiple sites and organizations/workspaces with cross-context denial |
| Protocol | Happy path, all documented problem statuses, ETag, idempotency replay/mismatch, retry/back-pressure, cancellation, and ambiguous timeout |
| Data | Null/optional, unknown additive fields, maximum bounds, exact values, Unicode, long/RTL text, binary/artifact responses |
| Lifecycle | Definition change, extension activation/disable/revocation, authorization generation change, and server upgrade |
| Security | Hidden record/field/action/count/error and secret-once response non-leakage |

## Change records

Client-owned durable decisions live in ADRs. Core contract changes live in core release/compatibility records and are linked from client compatibility evidence. Roadmap status may point to open gaps but must not copy an entire normative schema or carry completed behavior indefinitely.

When a mismatch is found, record:

- exact core revision and runtime generation;
- route/method, request headers/body, response status/headers/schema category with secrets removed;
- expected authoritative source and observed source;
- security and client impact;
- proposed owner (core, generator, SDK, app, or documentation); and
- a fixture that will prevent recurrence.
