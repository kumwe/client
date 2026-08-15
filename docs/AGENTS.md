# Documentation instructions

These instructions apply to `docs/` and all descendants.

## Purpose

Documentation in this repository defines the client product, client-owned decisions, implementation gates, and evidence expectations. It does not redefine Kumwe core.

## Truth labels

Use precise status language:

- **Core evidence**: behavior verified in the pinned Kumwe core revision by source, test, generated contract, or stable documentation.
- **Client decision**: an accepted choice owned by this repository and recorded in an ADR when durable.
- **Requirement**: behavior needed before a gate can pass.
- **Proposal**: a candidate design that core or product owners have not accepted.
- **Implemented**: reserved for behavior present in this repository and supported by executable evidence.

Never use future-tense product intent as proof that a client capability exists. The current repository has no application or SDK.

## Core authority

- Link to an exact core revision when recording audit evidence.
- Prefer the generated OpenAPI document, runtime routes, application services, and executable tests over narrative summaries.
- Do not copy normative schemas, capability lists, validation rules, error catalogs, or extension manifest grammars into client docs. Link to the core authority and summarize only what is necessary to explain a client requirement.
- If core source and core documentation disagree, record the mismatch as a contract gap. Do not choose the more convenient interpretation.
- Historical ERP prompts are context and desired direction, not proof of shipped behavior. Current core source and evidence win.

## Document structure and style

- Lead with the decision, outcome, or status.
- State whether a section describes current evidence, a client requirement, or a proposal.
- Use tables for exact mappings and parity classifications.
- Use Mermaid only when a relationship is materially clearer than prose.
- Use repository-relative links for local documents and pinned HTTPS links for core evidence.
- Keep trust-boundary terminology consistent: **administrator**, **portal**, and **public**.
- Define parity as equivalent authorized outcomes and contract semantics, not markup reuse.
- Keep accessibility, localization, privacy, failure, and lifecycle behavior in the main acceptance criteria rather than optional appendices.

## ADRs

ADRs live in `docs/architecture/decisions/` and use four-digit sequence numbers. Each ADR contains title, status, date, context, decision, consequences, and alternatives. Accepted ADRs are immutable except for corrections that do not alter the decision. A changed decision requires a superseding ADR and reciprocal links.

## Roadmap lifecycle

The client roadmap describes client work and client-facing core dependencies only. It must not replace or paraphrase the core programme roadmap as if this repository controls it.

- [`roadmap/README.md`](roadmap/README.md) defines phases and gates.
- [`roadmap/STATUS.md`](roadmap/STATUS.md) records the current phase, evidence, blockers, and next decision.
- A gate is complete only when its acceptance evidence is linked.
- Completed implementation belongs in release notes/changelog once those artifacts exist; do not let status pages become unsupported capability claims.

## Review checklist

Before finishing a documentation change, verify:

- all local links resolve;
- all pinned core links use the audited or newly declared revision;
- every capability is labeled as evidence, requirement, proposal, or implemented behavior;
- no client document claims to change a core contract;
- platform scope still excludes Flutter web;
- security, accessibility, localization, and adaptive behavior are not weakened; and
- roadmap and current status agree.
