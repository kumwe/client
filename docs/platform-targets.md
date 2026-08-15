# Platform targets

## Decision

The intended product targets native Linux, macOS, Windows, Android, and iOS from one Flutter codebase. Flutter web is not a target.

No platform binary exists today. Minimum OS versions, supported Linux distributions, CPU architectures, store availability, update mechanisms, and support lifetimes are requirements for a later qualification gate, not implied commitments.

## Target matrix

| Platform | Intended interaction profile | Qualification concerns |
|---|---|---|
| Linux desktop | Keyboard/mouse, resizable and multi-window workflows, filesystem integration | Supported distributions/desktop environments, packaging/signing, keyring behavior, text rendering |
| macOS desktop | Keyboard/trackpad, menus, windows, file and URL handoff | Notarization, sandbox/keychain, universal binaries, accessibility API, updater policy |
| Windows desktop | Keyboard/mouse, windows, file and URI integration | Signing, installer/update policy, credential vault, high contrast, scale factors |
| Android | Touch, variable screens, back navigation, intents, share/file providers | Keystore, API level, process death, network security, TalkBack, background and download policy |
| iOS | Touch, safe areas, navigation conventions, share/file providers | Keychain, app transport security, review constraints, VoiceOver, background and external-auth return |

Tablet and foldable layouts are adaptive mobile/large-screen states, not separate products. Desktop narrow windows and split-screen mobile layouts are first-class test sizes.

## Why no Flutter web

Kumwe core already has authoritative public, portal, and administrator browser surfaces built with server-rendered Twig and focused Lit enhancement. A Flutter web client would:

- duplicate browser navigation, rendering, accessibility, localization, security, and deployment work;
- still fail to reproduce arbitrary Twig/extension presentation natively;
- create a second web compatibility and caching surface; and
- divert effort from platform integration that justifies a native client.

Public/browser users continue to use the core web application. The native client does not embed it. A clearly labeled link may open the independent site in the system browser for work that is not available natively, but that external journey is not client parity and uses no native credential injection.

## Qualification sequence

All five target operating systems are intended targets, but qualifying them simultaneously from day one would hide contract problems behind platform noise. The proposed sequence is:

1. prove SDK conformance without Flutter;
2. prove one narrow vertical slice on Linux, macOS, and Windows with the same fixtures;
3. prove the same slice on Android and iOS, including process death and external-auth return;
4. expand feature parity only after the adaptive component set and platform harness are stable; and
5. release a platform only when its distribution, signing, update, security, accessibility, and recovery gates pass.

This is a delivery proposal, not a decision that desktop must ship before mobile. Product owners may change release order without changing the shared architecture.

## Shared versus platform-specific behavior

The following must be shared:

- core use cases and typed SDK operations;
- capability and definition interpretation;
- application state and error categories;
- parity fixtures;
- semantic design tokens and accessibility intent; and
- localization catalogs and exact-value presentation rules.

The following belong behind platform adapters:

- secure credential storage;
- external-system-browser launch and deep-link return, without credential injection;
- file selection, save location, downloads, and share sheets;
- notifications, background execution, app links, and URL schemes;
- window management, menus, system tray/dock integration, and updater; and
- signing, packaging, store, and enterprise distribution.

Platform conventions may alter presentation and navigation mechanics. They may not alter business rules or contract semantics.

## Adaptive layout requirements

- Layout decisions use available width, height, text scale, pointer/hover capability, and platform conventions rather than a simple mobile/desktop boolean.
- Collection/detail journeys support compact single-pane and expanded split-pane arrangements without losing selection, focus, or unsaved input.
- Data tables become semantically equivalent lists/cards only when sorting, selection, actions, totals, and accessibility remain available.
- Dialogs must remain usable at minimum supported window sizes and maximum supported text scale.
- Hover is an enhancement, never the only path to information or action.
- Touch targets and keyboard density can differ while preserving the same operation and confirmation semantics.

## Platform support gate

Before a platform is called supported, the repository must record:

- exact OS, architecture, Flutter/Dart, and distribution matrix;
- installation, upgrade, downgrade/refusal, uninstall, data cleanup, and crash recovery behavior;
- credential-store and native authorization redirect/deep-link evidence;
- signing/notarization/store provenance and update rollback policy;
- keyboard/screen-reader/text-scale/high-contrast evidence as applicable;
- parity journey and network-failure results; and
- known bounded limitations with an owner and review date.
