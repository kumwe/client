# Architecture decision records

Architecture decision records capture durable client-owned choices. Kumwe core contracts remain in the core repository; an ADR here can decide how the client consumes them, not change their meaning.

| ADR | Status | Decision |
|---|---|---|
| [0001](0001-core-api-dart-sdk-flutter-boundary.md) | Accepted | Keep core authority, typed Dart transport, and Flutter presentation in separate dependency layers |
| [0002](0002-authentication-link-one-client-and-the-account-switcher.md) | Accepted | Sign in with the authentication link; ship one client with an area chooser and a multi-deployment account switcher |

Accepted decisions are superseded with a new ADR rather than rewritten in place.
