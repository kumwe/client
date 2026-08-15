# Accessibility, localization, and adaptive UX

## Baseline

Every supported native journey targets WCAG 2.2 AA and platform accessibility conventions. Accessibility, localization, and adaptive layout are functional parity criteria because they determine whether a user can complete the same authorized outcome.

No client UI exists yet. This document defines release requirements.

## Accessibility requirements

### Structure and semantics

- Give every screen a meaningful title and landmark/heading structure.
- Expose labels, values, roles, state, required/invalid status, descriptions, errors, and action consequences through Flutter semantics.
- Preserve logical reading and traversal order independently of visual reflow.
- Group table/card rows and repeated field structures without creating an unusable announcement stream.
- Announce asynchronous success, validation, conflict, approval, progress, and error changes without stealing focus unexpectedly.
- Never put sensitive hidden data in semantic labels, tooltips, debug descriptions, or offstage widgets.

### Keyboard and alternative input

- Every desktop operation is possible without a pointer.
- Focus is visible, ordered, trapped only in a true modal, restored after dismissal, and moved to the first relevant error or new content when appropriate.
- Provide platform-appropriate shortcuts for navigation, save, search, refresh, close/cancel, and command discovery; never override essential system or assistive-technology keys.
- Hover-only and drag-only interactions have keyboard/touch alternatives. Relation ordering and file drop require explicit accessible controls.
- Touch targets meet platform and WCAG size expectations with sufficient spacing.

### Visual and motion

- Text and meaningful UI meet AA contrast in every theme, focus, disabled, error, selection, and high-contrast state.
- Do not use color, position, shape, or animation as the only state signal.
- Support platform text scaling without clipped content, unreachable actions, or forced horizontal scrolling for ordinary forms.
- Respect reduced-motion, reduced-transparency, bold-text, contrast, and system color preferences where available.
- Animation does not delay essential work, flash, or obscure operation status.

### Screen-reader qualification

- Test VoiceOver on macOS and iOS, Narrator on Windows, Orca on supported Linux environments, and TalkBack on Android.
- Automated semantics checks are necessary but not sufficient; critical journeys require human review with each supported reader/input combination.
- An external-browser link is not accessibility or parity evidence for a blocked native journey.

## Localization architecture

Client interface strings and server-authored content have different authorities:

| Content | Authority | Client responsibility |
|---|---|---|
| Native shell and client-only messages | This repository's versioned localization catalog | Stable message IDs, ICU/plural/select formatting, translator context, completeness checks |
| Core problem/user-facing messages | Core stable problem type and localization contract | Prefer client-localized known stable types only when semantics are exact; preserve safe server detail and correlation ID |
| Business definition labels/help/options | Policy-filtered core/extension definition metadata | Render the resolved locale/fallback without inventing translations |
| CMS/public content | Core content locale and translation-group delivery contract | Preserve locale, fallback, alternate, canonical-path, and publication semantics |
| Dates/numbers/money/quantity | Exact wire values plus locale/definition metadata | Format for display; submit canonical exact strings/objects unchanged |

The client may use Dart/Flutter localization tooling and ARB catalogs when implementation starts, but that file format is not itself the cross-system contract.

## Locale requirements

- Preserve well-formed locale tags and do not collapse regional/script distinctions without a core-defined fallback rule.
- Make installation/user/content locale resolution visible and deterministic.
- Support runtime language change where practical without losing form state or executing an action twice.
- Use locale-aware dates, times, calendars, numbers, plural/select grammar, collation, and search behavior only where the contract permits; retain canonical wire values.
- Display server and local time zones explicitly for schedules, publication windows, audit events, and deadlines.
- Never parse exact decimals, money, or quantity through binary floating point. Currency and unit remain part of the value.
- Allow long translations and unbroken identifiers without clipping controls or hiding required context.
- Mirror layout and direction for right-to-left locales while preserving intentional direction for identifiers, code, URLs, account numbers, and mixed-direction values.
- Localize accessibility labels, shortcut descriptions, errors, empty states, notifications, file names where safe, and external-browser handoff text—not only visible buttons.
- Unknown message/enum/field kinds use an explicit safe fallback that retains support detail; they do not expose raw developer JSON as the ordinary UI.

## Translation and content parity dependencies

The audited core carries locale and translation-group identity on content and renders alternate/fallback presentation on the server. Native public parity still requires a structured contract for:

- supported site locales and default/fallback policy;
- current effective locale and direction;
- translation-group members visible to the caller;
- localized slugs/canonical paths and hreflang alternatives;
- per-locale workflow/publication availability;
- extension-contributed interface and content catalogs; and
- version/generation invalidation of resolved messages and metadata.

Until that contract exists, the client language-selector journey is blocked. The separate Kumwe website continues to own its browser language selector.

## Adaptive experience

Adaptive behavior responds to space and input capability while retaining one semantic journey.

- Compact mode uses sequential collection/detail/form routes with preserved back state.
- Expanded mode may use navigation rail/sidebar and split collection/detail panes.
- Extra-wide mode may add context/history/inspector panes only when focus order and reading order remain coherent.
- Data-heavy views retain sorting, filters, selection, totals, bulk actions, and column meaning when reflowed to cards or stacked fields.
- Forms keep label-help-error relationships and operation controls visible at high text scale and small windows.
- Destructive confirmations, native step-up states, export progress, and conflict comparison adapt without changing safety semantics.
- Platform navigation conventions differ, but deep-link identity, unsaved-change handling, and authorization refresh remain consistent.

## Content and rich text

Rich content must declare its representation and safety provenance:

- structured semantic blocks map to audited native renderers;
- bounded rich-text markup may be parsed into audited native text/block components only when the contract defines the supported subset and sanitization provenance;
- arbitrary HTML, scripts, and styles are never rendered or moved into the native widget tree; and
- unsupported blocks show an accessible unavailable state. An optional external-system-browser link remains outside the client and does not satisfy parity.

Images require meaningful author-provided alternatives where available; decorative media is marked as such. Video/audio need captions/transcripts and controllable playback according to their contracts.

## Release evidence

For every critical parity journey, record:

- keyboard-only completion on desktop;
- the supported platform screen-reader path and announcements;
- 200% text zoom or the platform-equivalent supported scaling target;
- normal/high-contrast and light/dark theme states;
- reduced motion;
- narrow, standard, and expanded layout sizes;
- longest supported translations and a right-to-left locale;
- locale-sensitive exact money/quantity/date/time fixtures; and
- validation, denial, conflict, offline, loading, and asynchronous result states.

Automated semantics, contrast, golden, and overflow tests belong in CI once code exists. Stable screenshots support visual review but do not replace assistive-technology or outcome evidence.
