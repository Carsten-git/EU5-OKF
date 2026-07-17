---
type: Reference
title: DHE browser visibility
description: What makes a mod event show in the Dynamic Historical Events list.
tags: [events, dhe, localization]
timestamp: 2026-07-06T00:00:00+10:00
resource: game/in_game/events/DHE/
status: draft
---

# Requirements checklist

1. `dynamic_historical_event { tag = … from = … to = … }` on the event
2. Loc key `<event_id>.entry` (often `"$<event_id>.title$"`)
3. Loc file paired with event script ([naming rule](/localization/event-localization-naming.md))
4. UTF-8 **BOM** on yml
5. `historical_info = <loc_key>` — **not** a raw string in the script

```txt
historical_info = flavor_teu_nc_tannenberg.1.historical_info
```

```yaml
 flavor_teu_nc_tannenberg.1.historical_info: "In 1410 the combined armies…"
```

# Game rules

"Immersion mode hide events" (or similar) may hide DHE entries — disable when testing.

# Vanilla coexistence

Vanilla TEU DHE events in `flavor_teu.txt` still appear; mod events add alongside them.

# Troubleshooting

| Symptom | Likely cause |
|---------|----------------|
| Event missing from list | No `dynamic_historical_event` block |
| Raw key as title | BOM / wrong yml filename |
| Parse failure | Inline `historical_info = "…"` instead of loc key |

# See also

* [Dynamic historical events](dynamic-historical-events.md)

# Citations

[1] Vanilla: `game/in_game/events/DHE/flavor_teu.txt` — `dynamic_historical_event`, `historical_info = flavor_teu.3.historical_info`
[2] Vanilla: `game/main_menu/localization/english/events/DHE/flavor_teu_l_english.yml` — `.entry` and `.historical_info` keys
[3] Mod: `mod/northern_crusade_teu/in_game/events/DHE/flavor_teu_nc_tannenberg.txt` — mod DHE + `historical_info` loc-key pattern
[4] Mod: `mod/northern_crusade_teu/main_menu/localization/english/events/DHE/flavor_teu_nc_tannenberg_l_english.yml`
[5] Vanilla: `game/main_menu/localization/english/game_rules_l_english.yml` — `rule_inmersion_mode`, `setting_inmersion_mode_hide_events`
[6] OKF: [Event localization naming](/localization/event-localization-naming.md) — script/yml filename pairing
