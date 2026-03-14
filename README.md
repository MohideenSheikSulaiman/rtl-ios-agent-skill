# rtl-ios-agent-skill

An **agent-agnostic iOS RTL skill** for implementing production-quality Right-to-Left (RTL) layout support. Works with Claude Code, Cursor, and Codex. Covers SwiftUI, UIKit, BiDi text, locale-aware formatting, and automated project auditing.

> RTL bugs are invisible until a user with an RTL language opens your app. This skill makes RTL a first-class concern from day one.

[![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-8A2BE2.svg)](https://claude.ai)
[![Agent Agnostic](https://img.shields.io/badge/Agent-Agnostic-green.svg)]()
[![Swift 5.0+](https://img.shields.io/badge/Swift-5.0%2B-orange.svg)](https://swift.org)
[![iOS 13+](https://img.shields.io/badge/iOS-13%2B-blue.svg)](https://developer.apple.com/ios/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Structure

```
rtl-ios-agent-skill/
├── AGENTS.md                     ← Entry point for Cursor, Codex, and other agents (repo root)
├── README.md                     ← You are here
├── LICENSE
└── rtl-ios-skill/                ← Installed by npx skills
    ├── SKILL.md                  ← Entry point for Claude Code
    ├── Documentation/
    │   ├── SwiftUI-RTL.md        ← SwiftUI patterns, modifiers, transitions
    │   ├── UIKit-RTL.md          ← UIKit constraints, cells, navigation
    │   ├── BiDi-Text.md          ← BiDi text, formatters, String Catalogs
    │   └── Testing-RTL.md        ← Simulator setup, XCTest, QA checklist
    └── Tools/
        └── rtl_audit.sh          ← Static analyzer for RTL anti-patterns
```

---

## Installation

### All Agents (Recommended)

```bash
npx skills add MohideenSheikSulaiman/rtl-ios-agent-skill
```

Auto-installs to Claude Code, Cursor, Codex, and any other detected agent in your project.

### Target a Specific Agent

```bash
# Claude Code only
npx skills add MohideenSheikSulaiman/rtl-ios-agent-skill --agent claude-code

# Cursor only
npx skills add MohideenSheikSulaiman/rtl-ios-agent-skill --agent cursor

# Codex only
npx skills add MohideenSheikSulaiman/rtl-ios-agent-skill --agent codex
```

### Project-Level Install

Run this from your iOS project root to scope the skill to that project only:

```bash
cd /path/to/MyiOSApp
npx skills add MohideenSheikSulaiman/rtl-ios-agent-skill
```

The skill will be added to your project's local agent config (e.g. `.claude/skills/` for Claude Code) and will not affect other projects on your machine. Commit this to version control so your whole team gets the skill automatically.

### Global Install

```bash
npx skills add MohideenSheikSulaiman/rtl-ios-agent-skill -g
```

---

## Usage

Once installed, your agent activates automatically when you mention RTL, Arabic, Hebrew, i18n, BiDi, or mirrored layouts:

```
"Add RTL support to my SwiftUI app"
"My Arabic layout is broken — fix it"
"Audit my project for RTL issues"
"Set up Arabic localization with String Catalogs"
"Write a UITableViewCell that works in RTL"
"My push animation direction is wrong in Hebrew"
```

### Run the Audit Tool

```bash
./rtl-ios-skill/Tools/rtl_audit.sh /path/to/MyApp
```

Scans Swift files and reports:
- 🔴 **Critical** — `leftAnchor`/`rightAnchor`, hardcoded `.left`/`.right` alignment
- 🟡 **Warning** — absolute transforms, `UIEdgeInsets`, unannotated forced-LTR views
- 🔵 **Info** — formatters missing `.locale = .current`

---

## Compatibility

The skill generates code compatible with **Swift 5.0+** and **iOS 13+**. Where APIs differ by version, the agent outputs the appropriate variant automatically:

| Feature | Compatibility | Notes |
|---------|--------------|-------|
| SwiftUI layout direction | Swift 5.1 / iOS 13+ | `@Environment(\.layoutDirection)` |
| `NSDirectionalEdgeInsets` | iOS 11+ | Preferred over `UIEdgeInsets` |
| `.natural` text alignment | iOS 9+ | Always use over `.left` / `.right` |
| `#Preview` macro | Swift 5.9 / Xcode 15+ | Falls back to `PreviewProvider` |
| `String(localized:)` | Swift 5.9 / Xcode 15+ | Falls back to `NSLocalizedString` |
| `formatted(.currency(...))` | iOS 15+ | Falls back to `NumberFormatter` |
| String Catalogs (`.xcstrings`) | Xcode 15+ | Falls back to `.strings` / `.stringsdict` |

Swift 5.9+ features are wrapped in `#if swift(>=5.9)` / `#else` blocks. iOS 15+ APIs use `@available(iOS 15, *)` with a fallback.

---

## Supported RTL Languages

| Language | Locale |
|----------|--------|
| Arabic | `ar_SA`, `ar_EG`, `ar_AE` |
| Hebrew | `he_IL` |
| Persian / Farsi | `fa_IR` |
| Urdu | `ur_PK` |

---

## License

MIT
