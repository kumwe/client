# Native functional parity

## Conclusion

At the audited core revision, a high-quality Flutter client is feasible for many authenticated CMS and business operations, but a complete one-to-one native reproduction of every Kumwe surface is **not** yet possible from stable structured API contracts alone.

- **Available now in principle:** broad authenticated native workflows backed by documented REST resources, especially generated business definitions and records.
- **Independently available outside the client:** Kumwe's existing server-rendered website remains usable in a system browser, but it is not an in-client surface and does not count as parity.
- **Blocked for native implementation:** features whose only complete representation exists in PHP application services, Twig/Lit presentation, private repositories, administrator-only browser handlers, or extension code without a portable client contract.
- **Not an acceptable workaround:** scraping HTML into Flutter models, reading core tables, generating Dart from observed JSON that contradicts OpenAPI, or recreating server authorization and validation.

This is a research baseline, not a completeness certification. It is pinned to [`kumwe/cms@4e5083b3`](https://github.com/kumwe/cms/tree/4e5083b3fe43790605ae5c6c5bf8e392f9822efc).

The evidence is repository source, route wiring, handlers, stable documentation, OpenAPI, and tests at that revision. No particular deployed installation was exercised, so environment configuration, site data, active extension generations, and deployment-only behavior are not certified here. Gate A requires live conformance against representative installed sites.

## What “one-to-one” can honestly mean

| Parity level | Definition | Current feasibility |
|---|---|---|
| Outcome parity | Same authorized use case, stored result, version, audit attribution, and error semantics | Feasible for API-covered operations after SDK/auth gates |
| Semantic UI parity | Native controls derived from stable fields, views, actions, relations, constraints, and states | Strong fit for generated business resources; partial elsewhere |
| Visual parity | Same theme, layout, extension templates, and assets | Not a client goal as pixel identity; a native semantic equivalent needs a presentation contract |
| Implementation parity | Same Twig/Lit/PHP implementation inside Flutter | Neither useful nor technically portable |

The product targets the first two. It does not embed server-rendered pages to claim the third.

## Evidence from the audited core

### Public rendering is a server application, not a public JSON resource

Core declares public routes for `/`, `/robots.txt`, `/pages/{slug}`, `/media/{id}/{name}`, `/assets/extensions/{path:.+}`, and a final `/{path:.+}` catch-all in [`ContainerFactory`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/src/Kernel/ContainerFactory.php). [`PublicPageLocator`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/src/Site/Application/PublicPageLocator.php) resolves the homepage, menu-derived paths, and stable slug permalinks. [`HomePageHandler`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/src/Http/Handler/HomePageHandler.php) and [`PublishedContentHandler`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/src/Http/Handler/PublishedContentHandler.php) then render HTML.

The effective page also depends on [`ContentPresenter`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/src/Presentation/ContentPresenter.php), rich-text formatting, translation presentation, [`PublicNavigation`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/src/Navigation/Application/PublicNavigation.php), [`SitePresentation`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/src/Presentation/Application/SitePresentation.php), the selected theme/Twig environment, and active extension assets. That resolved model is not exposed as a documented anonymous JSON endpoint.

Consequently, requesting raw content plus settings is not equivalent to requesting the public page. It omits resolution and precedence decisions that core currently makes only on the server.

### Authenticated REST coverage is substantial

The stable core guide documents bearer authentication, exact `Kumwe-Site` context, idempotency, optimistic concurrency, Problem Details, and resources for:

- content, content types, and workflows;
- menus and menu items;
- users, groups, grants, and tokens;
- settings and presentation configuration;
- extension inventory and lifecycle;
- schedules, jobs, and safe plan previews; and
- generated business definition discovery, record browse/search/create/update/delete/archive/restore, history, relations, actions, approval requests, operation status, reports, and exports.

See the pinned [`docs/rest-api.md`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/docs/rest-api.md) and canonical [`api/openapi/kumwe-v1.json`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/api/openapi/kumwe-v1.json).

Generated business resources are the best current foundation for dynamic native surfaces because definition discovery exposes policy-filtered fields, views, actions, and relations and routes use shared application behavior across adapters. Even here, high-impact approval decisions require a fresh browser session-bound step-up proof at the audited revision; bearer REST can request an approval but cannot approve, reject, revoke, or consume the proof. The native decision journey is blocked until core publishes a native-safe step-up contract. An optional external-browser link reaches the separate web product and is not client parity.

Route coverage is not collection or authoring parity. At the pinned revision, [`ContentCollectionHandler`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/src/Delivery/Http/Api/Content/ContentCollectionHandler.php) hard-codes a maximum of 100 items and accepts only `include_deleted`; it has no cursor, total, search, filter, or sort contract. The content HTTP adapters expose no locale/translation-group assignment or group-member discovery even though those concepts exist in the domain and public presenter. The menu-item HTTP adapters do not accept page `template` or `color_scheme` bindings even though runtime records and the server administrator support them. These are native parity gaps, not work for a Dart heuristic.

### Contract generation is not yet safe to assume blindly

The audit found concrete static OpenAPI/runtime drift at the pinned revision:

- Runtime [`ContentRecord::toArray()`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/src/Content/Application/ContentRecord.php) includes `publication_window`, pinned content/workflow versions, `site`, and optional locale/translation-group fields. The static `Content` schema instead declares top-level `publish_at` and `unpublish_at` and omits several runtime fields.
- Runtime [`MenuItemRecord::toArray()`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/src/Navigation/Application/MenuItemRecord.php) includes `template` and `color_scheme`; the static `MenuItem` schema omits them.
- [`MenuItemCollectionHandler`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/src/Delivery/Http/Api/Navigation/MenuItemCollectionHandler.php) and [`MenuItemResourceHandler`](https://github.com/kumwe/cms/blob/4e5083b3fe43790605ae5c6c5bf8e392f9822efc/src/Delivery/Http/Api/Navigation/MenuItemResourceHandler.php) neither accept nor forward those two presentation bindings, so the API cannot reproduce the corresponding server-admin mutation.
- Several endpoints lack sufficiently complete response schemas for deterministic Dart generation.

These are blockers for a production SDK even when a route exists. Gate A therefore requires response-shape fixtures that validate representative runtime responses against the exact OpenAPI supplied by the same installation.

## Capability classification

| Capability group | Structured API at audit | Native feasibility | Gap or caution |
|---|---|---|---|
| Content CRUD and workflow | Yes | Feasible in part after contract correction and SDK | List is fixed at 100 without cursor/search/filter/sort; translation-group authoring/discovery is absent; dynamic fields need semantic editors |
| Content types/workflows | Yes | Feasible for schema-supported operations | Native definition authoring requires complete constraints and safe upgrade semantics |
| Menus/items | Yes | Feasible in part after response/request parity is verified | REST cannot mutate template/color-scheme bindings and does not return the public resolved nested navigation projection |
| Users/groups/grants/tokens | Yes | Feasible for carefully scoped admin journeys | Secret-once token responses and revocation need dedicated UX; no embedded shared token |
| Site settings/presentation | Yes | Feasible for configuration | Configuration does not equal effective page rendering; theme/template catalog may be incomplete |
| Extensions lifecycle | Partial | Inventory/activate/disable/uninstall feasible | REST intentionally does not install arbitrary package paths; client UI contributions are not portable |
| Automation | Yes for schedules/jobs controls | Feasible | Background process/job-specific surfaces depend on published schemas/capabilities |
| Business definitions/records | Strong generic API | Best native candidate | Dynamic contract by site/generation; high-impact decision journeys are blocked on native step-up |
| Reports/exports | Yes for generated business reports | Feasible with secure artifact handling | Permission, expiry, content type, filenames, and storage failure must remain server authoritative |
| Administrator media library | No complete REST management family found | Blocked | Public asset streaming exists, but browse/upload/replace/delete metadata contracts are missing |
| Public page resolution | No public delivery JSON | Blocked | Needs route resolution, publication, canonical URL, layout, navigation, locale, and presentation projection |
| Public navigation | Management API only | Blocked for parity | Needs anonymous, nested, publication-filtered projection with active/current state |
| Public localization | Domain stores locale/group; HTML emits effective translation presentation | Blocked | REST cannot currently manage/discover content translation groups; public delivery also needs fallbacks, hreflang/canonical semantics, and locale negotiation |
| SEO/social metadata | No complete structured delivery contract found | Blocked | Needs title/description/canonical/robots/Open Graph/schema/sitemap policy as data where native use is required |
| Public search | No public search contract found | Blocked | Needs bounded public query, result projection, locale, pagination, abuse controls |
| Forms/submissions | No general public contract found | Blocked | Needs schema, CSRF/bot protection, upload, validation, privacy, consent, and receipt semantics |
| Feeds/sitemap | `robots.txt` exists; no general feed/sitemap API found | Blocked or not applicable | These remain server/browser responsibilities unless a native discovery use case is defined |
| Extension custom UI | Server registries/Twig/Lit/PHP | Blocked | Needs signed, versioned declarative client-surface contribution; arbitrary code is not portable |

“Feasible” in this table means the server contract can plausibly support the journey. It does not mean the client has implemented it.

## External browser is not parity

The client may offer a clearly labeled system-browser link for work that remains available only on the independent Kumwe website. The browser authenticates independently; the client must not forward native bearer tokens, cookies, hidden context, or restricted data. Leaving the app is a convenience for blocked work, not an implementation of that journey, and it cannot satisfy a roadmap or parity gate.

## What is impossible without new contracts

The client cannot independently and correctly derive:

- the effective public page for a path, including current publication state and canonical URL;
- theme/Twig output or extension-rendered module content as native semantic widgets;
- complete public navigation, locale alternatives, SEO metadata, or server fallback decisions;
- server-side policy-hidden fields, records, actions, counts, or errors that an endpoint does not project;
- administrator media management from a public file URL;
- a secure native user login/step-up flow from opaque administrator-issued bearer tokens alone; or
- the semantics of extension-specific custom UI from a PHP handler or Twig template.

Adding Dart heuristics for these gaps would produce a second, divergent product rather than parity.

## Required core capability groups

These are client-facing requirements proposed for core; they are not contracts created by this document.

1. **Contract truth and compatibility:** complete OpenAPI 3.1 response bodies, examples, Problem Details, security schemes, conditional headers, capability/version metadata, and executable drift fixtures.
2. **Installation discovery and native authorization:** supported server metadata, site/workspace discovery, the authentication-link sign-in with area binding and non-enumerating guest arrival, refresh/rotation/revocation, logout, the single-use authenticated web-session handoff, native step-up, and minimum client/core version policy.
3. **Public delivery:** anonymous resolved-page-by-path/slug/homepage resources with canonical URL, publication state, locale, translation alternatives, structured content, layout semantics, and caching validators.
4. **Presentation:** effective site/page presentation, theme identity/version, color and typography tokens, asset references, bounded native component semantics, and explicit unsupported-rich-content behavior.
5. **Navigation and content scale:** nested public navigation, breadcrumbs/current state, cursor pagination, bounded search/filter/sort, and change validators.
6. **Localization:** supported locales, negotiation, translation groups/members, fallback resolution, localized slugs, direction, and stable interface message identifiers where needed.
7. **Media:** policy-filtered browse/get/upload/replace/delete, metadata, variants, content type/size/checksum, resumability if supported, and authorized download semantics.
8. **Public capabilities:** search, forms/submissions, consent/anti-abuse, feeds/sitemaps, SEO/social metadata, and feature discovery—only for product use cases that core explicitly supports.
9. **Extension client surfaces:** signed, owned, versioned declarative routes/views/widgets/actions plus lifecycle removal and explicit unsupported-version behavior.

## Visual parity risks

Exact visual parity is fragile even after a presentation API exists:

- Flutter and browser font shaping, line breaking, form controls, and accessibility scaling differ.
- Desktop window sizes and mobile safe areas do not correspond to CSS breakpoints.
- Arbitrary extension CSS/JavaScript cannot be translated safely or deterministically.
- Rich HTML needs a constrained semantic native representation; otherwise that content remains blocked in the client.
- Platform conventions sometimes conflict with the public theme; copying the web control can reduce native usability.

The acceptance strategy should therefore compare semantic design tokens and reference journeys within a tightly defined native component set. The separate website is not a client visual-parity fixture.

## Parity acceptance method

Every claimed parity journey needs a fixture containing:

- core revision, runtime/OpenAPI generation, client version, platform, locale, and trust boundary;
- actor, site, organization/workspace, capabilities, policy generation, and test data;
- reference adapter outcome and native outcome;
- stored record/version/relation/workflow/audit checks;
- denial and non-enumeration checks;
- retry, stale ETag, session expiry, permission change, and extension lifecycle checks; and
- keyboard, screen-reader, text-scale, RTL/long-text, and adaptive-layout evidence.

A screenshot alone is never parity evidence.
