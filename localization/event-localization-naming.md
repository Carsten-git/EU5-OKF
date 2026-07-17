---
type: Reference
title: Event localization naming
description: Pair one localization yml file per event script file; include .entry keys for DHE.
resource: mod/northern_crusade_teu/main_menu/localization/english/events/DHE/
tags: [localization, events, dhe, naming]
timestamp: 2026-07-06T00:00:00+10:00
status: draft
---

# Pairing rule

EU5 expects event localization files to **match the event script filename**:

| Script | Localization |
|--------|----------------|
| `in_game/events/DHE/flavor_teu_nc_purpose.txt` | `main_menu/localization/english/events/DHE/flavor_teu_nc_purpose_l_english.yml` |
| `in_game/events/DHE/flavor_teu_nc_tannenberg.txt` | `…/flavor_teu_nc_tannenberg_l_english.yml` |

Do **not** bundle all mod events into one mega-yml if titles fail to resolve.

# Required keys per event

For event `flavor_teu_nc_purpose.100`:

```yaml
 flavor_teu_nc_purpose.100.title: "…"
 flavor_teu_nc_purpose.100.desc: "…"
 flavor_teu_nc_purpose.100.a: "…"          # option a
 flavor_teu_nc_purpose.100.entry: "$flavor_teu_nc_purpose.100.title$"  # DHE browser
```

The `.entry` key is used by the **Dynamic Historical Events** browser listing.

# Option key convention

Prefer vanilla-style letter suffixes for options:

| Script `name =` | Loc key |
|-----------------|---------|
| `flavor_x.1.a` | ` flavor_x.1.a: "…"` |
| `flavor_x.1.b` | ` flavor_x.1.b: "…"` |

Also add `.a.entry` short labels when mirroring vanilla DHE files.

**Watch-out (KI-069):** if the same option key is defined in **two** yml files (e.g. main `mod_l_english.yml` **and** `events/mod_events_l_english.yml`), `error.log` reports `Duplicate localization key` and the option can show as a raw key / empty hover. Define each key **once** — prefer the paired events file only.

Flat keys (`name = rgo_conversion_complete_option`) and vanilla `….a` both work when not duplicated. Vanilla OK pattern:

```yaml
 rgo_conversion.3.a: "[ROOT.GetCountry.Custom('common_string_ok')]"
 rgo_conversion.3.a.entry: "$common_string_alright$"
```

Custom suffixes (`….wheat`) for choice lists usually work.

# Modifier DESC immersion

`STATIC_MODIFIER_DESC_*` is player-facing flavor. Do **not** explain other modifiers, “separate timers”, or implementation details there — put that in mod `docs/` or tooltips that are clearly UI chrome only.

# Namespace

The `namespace = flavor_teu_nc_purpose` line in the script prefixes event IDs in code but loc keys use the **full** event id (`flavor_teu_nc_purpose.100`).

# See also

* [DHE browser visibility](/events/dhe-browser-visibility.md)
* [Event ID rules](/events/event-id-rules.md)

# Citations

[1] Mod: `northern_crusade_teu/main_menu/localization/english/events/DHE/flavor_teu_nc_purpose_l_english.yml` — `.title`, `.desc`, `.a`, `.entry` keys
[2] Mod: `northern_crusade_teu/in_game/events/DHE/flavor_teu_nc_purpose.txt` — paired script file and `namespace`
[3] Vanilla: `game/main_menu/localization/english/events/DHE/flavor_teu_l_english.yml` — filename pairing convention
