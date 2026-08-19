# ADR-0002: Authentication-link sign-in, one client with an area chooser, and the account switcher

- **Status:** Accepted
- **Date:** 2026-08-18
- **Decision owners:** Product owner; Kumwe client maintainers
- **Core evidence baseline:** [`kumwe/app@4e5083b3`](https://github.com/kumwe/app/tree/4e5083b3fe43790605ae5c6c5bf8e392f9822efc), with the decision cross-referenced in core decision D17 ([ADR 0009](https://github.com/kumwe/app/blob/master/docs/roadmap/decisions/0009-native-client-platform-and-the-authentication-link.md)) and SDK [ADR 0007](https://github.com/kumwe/dart-sdk/blob/main/docs/decisions/0007-authentication-link-is-the-primary-sign-in.md)

## Context

The security baseline said no native authentication flow had been selected. The product owner has now selected
one, recorded in all three repositories: sign-in is the **authentication link** (the pattern the industry calls
a magic link). On the web, the administrator and portal areas are separate URL paths with separate password
logins; a native client has no URLs, Kumwe is self-hosted so every deployment has its own origin, and one
person may hold accounts on several deployments at once. The open product questions this ADR settles: what the
client's sign-in journey is, whether administrator and portal justify two separate client applications, and how
several deployments coexist in one installation of the client.

This ADR decides product mechanism and client-owned structure. It does not create a core contract: the wire
design lives in the `kumwe/dart-sdk` proposal corpus (`kumwe.native-authorization` `0.2.0-proposal.2`,
`kumwe.native-discovery` `0.2.0-proposal.2`), core adoption is tracked in core's lane N (`V3-NC-001` …
`V3-NC-004`), and implementation here still starts only after core exposes the supported flow.

## Decision

1. **Sign-in is the authentication link.** Add deployment (enter the HTTPS URL; the client reads the
   discovery document from that exact origin) → choose the area the deployment advertises (administrator or
   portal) → enter email address → open the emailed single-use link, which returns to the client and
   completes sign-in through the SDK's proof-key-bound ticket. Reading email on another device, the person
   types the link landing page's short completion code into the requesting client. The client never collects
   a Kumwe password; password login remains a web-only surface.
2. **One client application, not two.** The area is chosen at sign-in, per account — the native equivalent of
   the web's `/administrator` and `/portal` paths. Trust separation is preserved where it matters: every
   account is keyed by deployment origin **and** area **and** credential, sessions and caches never cross
   account keys, and an administrator account and a portal account on the same deployment are simply two
   entries in the switcher. Two separate applications would double distribution, signing, store review,
   platform qualification and update channels while providing no isolation the account key does not already
   provide. A future ADR may still add a second, differently-branded distribution of the same codebase if an
   operator-policy need appears; nothing here forecloses it.
3. **The account switcher is a first-class shell surface.** The client manages many accounts across many
   self-hosted deployments (the pattern password managers use for multiple vaults): a non-secret roster
   (deployment display name, area, address, account state) with instant switching, while token material
   stays in the platform credential store keyed per account. Adding an account never asks for more than a
   URL, an area and an email address. Removing an account attempts server-side revocation first, then
   removes credential material, roster entry and every cache under that account key.
4. **Guest arrival is a state, not a boundary.** An unknown address still completes sign-in and lands on the
   guest arrival page: the deployment has been told of the arrival, and the account waits to be positioned.
   Guest is the `pending` account state inside the chosen area's trust boundary — not a fourth boundary —
   and the arrival page is the only surface a pending account has. Positioning is announced by email;
   the client refreshes into full capability without a new sign-up journey.
5. **Sessions persist until sign-out.** Staying signed in is the default and rides on the rotating refresh
   family; access tokens stay short-lived. Sign-out, server-side revocation, security-epoch advance or
   refresh refusal ends the session honestly, and re-entry is the same link flow.
6. **"Open in website" uses the web-session handoff.** Where a journey is browser-only, the client offers to
   open the deployment's website already signed in through core's single-use, short-lived handoff URL.
   Native tokens and cookies never enter the browser, the browser session is core's own, and a browser
   journey is never parity evidence — exactly as ADR-0001 already requires.

## Consequences

### Positive

- One sign-in journey on every platform, with no password surface in the client to phish, cache or leak.
- Registration and sign-in collapse into one flow; the guest state gives arriving users a truthful home
  while administrators keep full control of positioning.
- The account model (origin + area + credential) gives multi-deployment, multi-area use the same isolation
  discipline the security baseline already demands of caches and credentials.
- The area chooser preserves the web's administrator/portal separation without splitting the product.

### Costs and constraints

- Everything waits on core adoption: mailer, link endpoints, guest lifecycle, refresh rotation, discovery
  areas and the handoff are core-owned (`V3-NC-001` … `V3-NC-004`); the client cannot ship sign-in first.
- Deep-link/app-link association per platform becomes a release-blocking platform task, and the cross-device
  manual code is a required accessibility of the flow, not an optional nicety.
- Email deliverability becomes part of the sign-in experience; the client must present "the link did not
  arrive" honestly (resend with rate limits, check-spam guidance) without revealing account existence.
- The switcher multiplies conformance surface: every cache, notification and restored navigation state must
  prove it is bound to one account key.

## Alternatives considered

### Two separate client applications (administrator client and portal client)

Rejected as the default. It doubles every distribution and qualification cost, forces people with both roles
to install two applications, and adds no isolation beyond what per-account keying already enforces. Kept
available as a future branding/distribution decision over the same codebase.

### Password login in the client

Rejected. Duplicates a credential surface core deliberately keeps in its own browser UI, contradicts the
standing "never collect a Kumwe password" rule, and would still need a second factor story the link makes
unnecessary.

### Links that sign in whoever opens them (no proof-key binding)

Rejected. Email interception or forwarding would become account takeover. The SDK's ticket binds completion
to the requesting client; the deliberate cost is the manual completion code for cross-device reading.

### A separate registration form for unknown addresses

Rejected. It invites address enumeration, duplicates the flow, and contradicts the guest-arrival experience
the product owner specified.
