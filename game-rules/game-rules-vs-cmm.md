---
type: Playbook
title: Game rules vs CMM for player options
description: Choose vanilla game rules for campaign-start house rules; CMM for mid-game QoL toggles — and when to combine both.
tags: [game-rules, cmm, cmf, design, settings, ironman]
timestamp: 2026-07-13T16:50:00+10:00
status: complete
source_mod: m524.player_speed_game_rules
source_version: "1.0.2"
---

# Decision

| Need | Prefer |
|------|--------|
| Choice at **new game** that sticks with the save / ironman expectations | **Vanilla game rules** |
| Human-only **stat** buffs without scripting each country | Game rules + `apply_modifier = player:…` |
| Toggle **mid-campaign** (pause menu) without restart | **CMM** (CMF) or local fallback GUI |
| Works **without CMF** dependency | Game rules (no CMF) **or** dual CMM+fallback |
| Gate **AI script pulses** / content | Either: `has_game_rule` **or** CMM bool read in trigger |
| Host house rule in MP affecting everyone | Game rules **or** `cmm_register_global_*` |

# Why the older “TC only” note was wrong

Earlier Glorp extract framed vanilla game rules as total-conversion territory because Glorp itself never used them. **Additive** `main_menu/common/game_rules/` files work for small utility mods — confirmed by Player Speed Game Rules (`3755676844`).

Glorp’s “no lobby wizard” story remains true for **CMM-centric** UI mods; it is not “game rules unavailable.”

# Recommended pairing for gameplay mods

| Setting kind | Surface |
|--------------|---------|
| Broad vs historical conversion profile | Game rule (campaign identity) |
| AI conversion off / on (default off) | Game rule **or** CMM — prefer game rule if ironman/MP clarity matters; CMM if mid-game flip is required |
| Tooltip / HUD QoL | CMM |
| One-shot “I understand this mod” consent | Rare; avoid blocking play — document defaults |

# Implementation sketch (AI convert gate)

**Game-rule path (no CMF):**

1. Rule group with `default = …_off` and settings `…_off` / `…_on`.
2. AI pulse trigger: `has_game_rule = …_on` (or `NOT = { has_game_rule = …_off }` — pick one convention and stick to it).
3. Loc for rule + settings; no static modifier required if the only effect is script gating.

**CMM path:**

1. `cmm_register_bool_setting` with `default_value` matching product default-off.
2. Read alias / variable in AI pulse — [Player options without lobby](/community-mod-framework/player-options-without-lobby.md).

Do not implement both without a sync story (players hate two toggles that disagree).

# Related

* [Custom game rules](/game-rules/custom-game-rules.md)
* [Player-scoped modifiers from game rules](/game-rules/player-scoped-modifiers-from-rules.md)
* [Player options without lobby events](/community-mod-framework/player-options-without-lobby.md)
* [CMM settings catalog design](/community-mod-framework/cmm-settings-catalog-design.md)
* [Dual settings UI fallback](/community-mod-framework/dual-settings-ui-fallback.md)

# Citations

[1] Workshop Player Speed Game Rules `3755676844`
[2] Glorp / CMM articles in this bundle (mid-game toggles)
[3] Vanilla `has_game_rule` in script
