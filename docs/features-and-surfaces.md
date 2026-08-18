# Features and surfaces

## Reading this inventory

This is the intended product surface, not an implementation checklist claiming that features exist. “API position” summarizes the initial audit at core commit `4e5083b3`; it must be revalidated against the installed site's canonical contract before work begins.

## Shell and connection

| Journey | Native expectation | API position |
|---|---|---|
| Add installation | Validate HTTPS endpoint, discover supported contract/generation, advertised areas, and explain compatibility | Discovery contract required |
| Authorize user | Authentication link per [ADR-0002](architecture/decisions/0002-authentication-link-one-client-and-the-account-switcher.md): choose area, enter email, complete from the emailed link's deep-link return or the manual cross-device code | Current REST documents opaque bearer tokens; the link flow is selected product behavior and remains gated on core adoption (`V3-NC-001`) |
| Guest arrival | An unknown address signs in to the arrival page: the deployment was notified, positioning is pending, activation arrives by email | Blocked on the core guest lifecycle (`V3-NC-003`); the arrival page is the pending account's only surface |
| Account switcher | Hold many accounts across deployments and areas; switch instantly; add with URL + area + email; remove with revocation first | Client-owned roster over the SDK account directory; sign-in itself gated as above |
| Select site/workspace/organization | Show only contexts the identity may use; invalidate cached state on change | Exact `Kumwe-Site` is required; richer discovery must be verified |
| Session and account | Stay signed in until sign-out; revoke, reauthorize, handle expiry/security epoch changes | Persistent sessions ride the proposed rotating refresh family; end-user lifecycle remains gated on the same core adoption |
| Open in website | Continue a browser-only journey in the system browser already signed in | Blocked on the single-use web-session handoff (`V3-NC-004`); never parity evidence, no tokens in the browser |
| Capability-driven shell | Navigation is a projection of allowed server capabilities and client-supported surfaces | Core has capability/definition metadata in several domains; one complete client manifest is not yet established |
| Diagnostics | Show client/core versions, contract checksum, correlation ID, clock/network state, and safe support export | Contract/version endpoint and redaction format required |

## Administrator surface

| Area | Planned journeys | API position |
|---|---|---|
| Dashboard | Actionable status, recent work, failures, and deep links | No single dashboard projection; compose only from bounded documented resources |
| CMS content | Browse, read, create, edit, trash, restore, transition, publication schedule, translation status | CRUD/workflow REST exists; collection/query and OpenAPI drift need closure; locale/translation-group API is missing |
| Content models/workflows | Inspect and, where safely representable, create/publish versions | REST exists; native authoring semantics must be proven against schemas |
| Navigation | Menus, hierarchy, ordering, targets, page presentation bindings, conflict recovery | Management REST exists; template/color-scheme bindings cannot currently be mutated through it |
| Media | Browse, preview, metadata, upload/replace/delete, select from generated fields | No complete REST media management family found; native journey blocked |
| Site presentation | Homepage, primary menu, logo/footer, schemes, layout/theme choices exposed by contract | Settings REST covers a subset; effective theme/template catalogs need discovery |
| Identity and access | Users, groups, memberships, scoped grants, token metadata, issue/rotate/revoke with secret-once UX | Broad REST coverage exists; high-risk workflows need step-up and dedicated redaction |
| Extensions | Inventory, trust/status diagnostics, activate, disable, uninstall, surface support | Lifecycle REST exists; installation stays administrator/host boundary; client surfaces absent |
| Automation | Schedules, recent jobs, retry/cancel, operation status | REST coverage exists; schemas and permission-filtered presentation must be verified |
| Reports/exports | Discover, parameterize, run, monitor, download, expire safely | Generated business report/export REST exists |
| Security/operations | Audit verification, keys, recovery, monitoring, backup/deployment controls | Many are deliberately host/CLI/server operations and may remain outside native scope |

## Generated business administrator surface

The dynamic business family is expected to provide reusable native views for:

- definition/workspace discovery;
- searchable, filterable, sortable, cursor-paginated collections;
- detail, create, and update forms;
- typed field widgets, exact values, references, media choices, and ordered lines;
- record history and lifecycle actions;
- relationship browse/add/remove/reorder;
- policy-visible custom collection and record views;
- ordinary actions and approval requests;
- approval inbox/detail; high-impact decisions remain blocked until a native-safe step-up contract exists;
- operation status;
- reports, projections where exposed, and export artifacts; and
- explicit empty, loading, validation, conflict, denial, unavailable-definition, stale-generation, and retry states.

Core metadata is necessary but not sufficient: server responses must continue to omit denied records and fields, and every mutation must reach the shared application service. Client-side metadata must never become an authorization cache.

## Portal surface

| Journey | Native expectation | Contract requirement |
|---|---|---|
| Portal sign-in/context | Separate identity/session boundary from administrator; the same authentication-link flow with `portal` as the chosen area | Native-safe portal authorization required; otherwise blocked |
| Opt-in workspaces | Only explicitly portal-exposed definitions and operations appear | Policy-filtered portal exposure metadata |
| Records and relations | List/detail/create/update/relation/history exactly where allowed | Same application semantics, portal-specific contract projection |
| Actions and approvals | Ordinary actions and fresh step-up decisions through native contracts | Typed action/approval and non-replayable native step-up contract |
| Reports/exports | Only explicitly exposed, policy-filtered reports and artifacts | Portal-scoped report metadata and downloads |
| Extension portal pages | Declarative native surface only | Extension client-surface contract and lifecycle signal; otherwise blocked |

Installing an extension never implies portal exposure. A native client must honor the same opt-in operation allow-list and non-enumerating errors as core.

## Public surface

| Journey | Native expectation | Current API position |
|---|---|---|
| Home and routed pages | Resolve current public page, canonical URL, layout semantics, content, and cache validators | Blocked on public delivery contract |
| Navigation/breadcrumbs | Nested public tree, current item, safe targets, localized labels | Blocked; management API is not equivalent |
| Language selector | Supported locale, alternates, fallback, localized paths, direction | Blocked; translation delivery contract required |
| Rich content/modules | Native semantic components from a bounded portable contract | Blocked where only server HTML or arbitrary modules exist |
| Media | Display authorized public variants with correct type, size, and accessibility text | Public asset URLs exist; metadata/variant contract needed |
| Search | Bounded, localized, abuse-resistant public results | No general contract found |
| Forms | Typed accessible forms, upload, validation, consent, anti-abuse, receipt | No general contract found |
| SEO/share | Canonical metadata and share payload; sitemap/feed remain server responsibilities | Public HTML/`robots.txt` now; structured metadata contract needed for native use |
| Extension pages | Native only when extension declares a compatible semantic surface | Blocked otherwise |

For any blocked journey, the client may provide a clearly labeled link to the independent Kumwe website in the system browser. That external work is not an in-client feature or parity evidence and receives no native credentials.

## Cross-cutting behaviors

Every surface must include:

- connectivity, timeout, cancellation, retry, and server-maintenance states;
- 401 reauthorization, 403 denial, non-enumerating 404, 409 in-progress/replay conflict, 412 stale version, 413 size limit, and 422 field validation presentation based on Problem Details;
- strong ETag storage and conflict resolution without last-write-wins shortcuts;
- stable idempotency keys and replay recognition for retryable mutations;
- pagination and incremental loading without hidden unbounded fetches;
- sensitive-data redaction in state restoration, notifications, logging, screenshots, clipboard, and crash reports;
- accessibility semantics and predictable focus after navigation, validation, dialogs, and asynchronous updates;
- locale-aware exact values without changing their wire representation; and
- extension/definition lifecycle invalidation so stale screens cannot keep executing removed contributions.

## Intentionally non-native or undecided surfaces

Some core responsibilities may remain outside the client even at maturity: server installation, database migration and repair, low-level backup/restore, process/container management, recovery mode, production key custody, and installation of local extension artifacts. A future product decision may add safe remote diagnostics, but a desktop UI must not turn host-level authority into an undocumented API.
