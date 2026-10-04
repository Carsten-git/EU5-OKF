---
type: Playbook
title: Main menu event localization mirror
description: Where event loc lives in EU5 mods — main_menu/events/ pairing, in_game mirrors, hidden-event keys, and subdirectory layout.
tags: [localization, events, main_menu, in_game, mirror, bom]
timestamp: 2026-07-20T21:45:00+10:00
status: complete
source_mod: meiou_and_taxes
source_version: "0.1"
---

EU5 loads **event scripts** from `in_game/events/` but resolves most **event localization** from `main_menu/localization/`. Agents often put event yml under `in_game/localization/` and get unresolved keys, empty options, or raw `hidden_event.t` in popups.

MnT and TEU both use the **main_menu events subtree** pattern.

# Golden rule

| Content | Path |
|---------|------|
| Event logic | `in_game/events/…/*.txt` |
| Event loc (primary) | `main_menu/localization/english/events/…/*_l_english.yml` |
| Event loc (mirror, optional) | `in_game/localization/english/events/…/` — duplicate keys when tooltips need in-session reload |

Script filename ↔ loc filename pairing: [Event localization naming](/localization/event-localization-naming.md).

# MnT layout (reference)

```
in_game/events/
  mnt_information.txt          # namespace mnt_information
  estates/MnT_cossacks_estate_events.txt
  exploration/MnT_exploration_monthly_events.txt
  …

main_menu/localization/english/events/
  MnT_generic_event_localization_l_english.yml   # hidden_event.t, welcome popup
  disasters/MnT_decline_of_empire_l_english.yml
```

**Subdirectories mirror** — disasters loc under `events/disasters/` when script lives in `in_game/events/disasters/`.

# Required keys

For event `mnt_information.1`:

```yaml
l_english:
 hidden_event.t: "Hidden Event"
 hidden_event.d: "This is a hidden event…"
 mnt_information.1.title: "MEIOU & Taxes"
 mnt_information.1.desc: "Welcome…"
 mnt_information.1.opta: "Let's go!"
```

Shared **hidden event** strings (`hidden_event.t` / `hidden_event.d`) belong in a generic events yml so `title = hidden_event.t` resolves for all `hidden = yes` events.

# When to mirror under `in_game/localization/`

| Mirror to in_game? | Situation |
|--------------------|-----------|
| **Yes** | Game rules, static modifiers, debug telemetry loc ([KI-074](/validation/known-issues.md), [KI-078](/validation/known-issues.md)) |
| **Usually no** | Standard flavor events — main_menu alone is enough if BOM is correct |
| **Yes (duplicate keys)** | Player reports option text missing mid-session after hot reload — duplicate paired file |

Same keys in both trees must be **identical**; drift causes duplicate-key warnings ([KI-069](/validation/known-issues.md)).

# Encoding and verification

1. Save every `*_l_english.yml` as **UTF-8 with BOM** ([UTF-8 BOM requirement](/localization/utf8-bom-requirement.md)).
2. Full quit to desktop after loc edits ([KI-030](/validation/known-issues.md)).
3. Grep `error.log` for `Unrecognized loc key` on your event ids.
4. For hidden telemetry, verify expanded strings in log — not raw loc keys ([KI-078](/validation/known-issues.md)).

# Checklist (new event)

- [ ] Script under `in_game/events/` with `namespace` and `.title` / `.desc` keys
- [ ] Paired yml under `main_menu/localization/english/events/` (matching subfolder)
- [ ] Option keys match script `name =` (`opta`, `.a`, `.b`, …)
- [ ] `hidden_event.t` defined if using hidden events
- [ ] UTF-8 BOM on yml
- [ ] DHE `.entry` keys if listing in browser ([DHE browser visibility](/events/dhe-browser-visibility.md))

# Related

* [Event localization naming](/localization/event-localization-naming.md)
* [Custom game rules](/game-rules/custom-game-rules.md) — main_menu + in_game mirror table
* [Mod folder structure](/getting-started/mod-folder-structure.md)
* [Script telemetry via hidden events](/validation/script-telemetry-via-hidden-events.md) — §4 loc mirror

# Citations

[1] MnT-EU5 `main_menu/localization/english/events/MnT_generic_event_localization_l_english.yml`
[2] MnT-EU5 `main_menu/localization/english/events/disasters/MnT_decline_of_empire_l_english.yml`
[3] Mod `northern_crusade_teu/main_menu/localization/english/events/DHE/` — TEU pairing
[4] [Game rules custom](/game-rules/custom-game-rules.md) — mirror pattern for non-event keys
