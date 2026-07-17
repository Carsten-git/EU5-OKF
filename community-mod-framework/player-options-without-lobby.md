---
type: Playbook
title: Player options without lobby events
description: How EU5 mods expose player choices when there is no new-game setup wizard — CMM defaults, init hooks, globals (pattern #31).
tags: [cmf, cmm, on-actions, lobby, design, glorp]
timestamp: 2026-07-12T22:45:00+10:00
status: complete
source_mod: glorp.ui
source_version: "1.3.10.1"
---

Glorp pattern **#31**: EU5 has **no custom “lobby wizard”** for arbitrary mod setup UIs. Glorp ships **zero** `in_game/events/`, **no** game-rules files, and **no** country-selection scripts — yet offers ~15 player toggles via **in-game CMM** with registration-time defaults.

Vanilla **Game Rules** *are* a campaign-start options surface — mods can add rules additively ([custom game rules](/game-rules/custom-game-rules.md)). Use this playbook when choosing **CMM / mid-game** options; use game rules when the choice belongs on the rules screen.

# Option surfaces (ranked)

| Surface | When it runs | Best for |
|---------|--------------|----------|
| **CMM `default_value`** | Registration (`cmf_on_mod_registration` — game start + mod menu open) | UI toggles, QoL, per-human preferences |
| **`cmf_on_callback` + aliases** | Player changes setting mid-game | Sync vars / refresh GUI |
| **`on_game_start_after_lobby_human_country`** | Once per human after country pick | Init computed state (HUD aggregates, caches) |
| **`on_game_load_after_lobby_human_country`** | Every load after lobby | Re-init after save |
| **`cmm_register_global_*`** | Host-editable in MP | House rules affecting everyone |
| **Vanilla game rules (additive)** | New-game rules screen | Campaign-start house rules, ironman-visible defaults, `player:`/`ai:` modifiers — [custom game rules](/game-rules/custom-game-rules.md) |
| **Lobby banner** | Pre-game lobby only | Marketing / visibility — **not** choices |
| **Events at `on_game_start`** | Immediate campaign start | Strong flavor, one-shot prompts — use sparingly |

Glorp uses CMM rows (no game rules). Utility mods may use **additive game rules** without being a total conversion — see [Game rules vs CMM](/game-rules/game-rules-vs-cmm.md).

# What Glorp does NOT do

Confirmed absent in workshop `3601047146`:

* `in_game/events/` directory
* Pre-game modal “configure Glorp” flow
* Per-save setup event chains
* Country-specific start options

Players configure **after** unpause via ESC → Community Mod Menu (or Glorp fallback window).

# CMM defaults = “out of box” experience

`default_value` on bools and `default_index` on dropdowns **are** your implicit new-game choices:

```txt
cmm_register_bool_setting = {
	mod_id = glorpui
	setting_id = disableTopBarIncome
	default_value = 1
}
cmm_sync_bool_alias_inverted = { ... }
```

`default_value = 1` + inverted alias ⇒ top-bar income **hidden until player enables it** — no lobby question required.

Document defaults in Workshop text: “By default, Glorp hides X; enable in ESC → Mod Menu → Top Bar.”

# Init hooks ≠ player choices

Glorp `on_game_start_after_lobby_human_country` runs `glorpui_sync_avg_stats` — **computed HUD data**, not configuration:

```txt
on_game_start_after_lobby_human_country = {
	on_actions = { glorpui_on_human_country_value_sync_init }
}
```

Use init hooks when settings **imply** derived state (recalculate caches), not to replace CMM.

# Multiplayer split

| Setting type | Who edits |
|--------------|-----------|
| Per-country bool/slider/dropdown | Each human (CMM) |
| `cmm_register_global_*` | Host only |
| Pause menu CMM open | Host uses `CMM_SetHostAndRegisterCoreMod`; clients use `CMM_RegisterCoreMod` — [CMM pause menu entry](/community-mod-framework/cmm-pause-menu-entry.md) |

# Applying to gameplay mods (e.g. RGO conversion)

| Player question | Implementation |
|-----------------|----------------|
| Enable AI conversions? | **Game rule** (`has_game_rule`) **or** CMM bool — see [Game rules vs CMM](/game-rules/game-rules-vs-cmm.md); default off via `default =` / `default_value` |
| Broad vs historical profile? | Prefer **game rule** (campaign identity) |
| Show 25y cooldown in tooltip? | CMM bool + alias; tooltip reads `has_variable` |
| Harder settling penalty? | Avoid CMM — balance belongs in script; use version notes |
| Host disables conversions in MP | Game rule **or** `cmm_register_global_bool_setting` |

Do **not** block first conversion behind a startup event unless you want explicit one-time consent — game-rule / CMM defaults are enough for QoL mods.

# UX copy for Workshop

> **Game rules:** set at new game under Game Rules (if your mod adds any).  
> **CMM settings:** ESC → Pause → **Community Mod Menu** → *[Mod name]*. Defaults: [list]. CMF required for shared menu; [fallback if any].

# Related

* [CMM settings catalog design](/community-mod-framework/cmm-settings-catalog-design.md)
* [Registration and on-action hooks](/community-mod-framework/registration-and-on-action-hooks.md)
* [Community Mod Menu](/community-mod-framework/community-mod-menu.md)
* [CMM pause menu entry](/community-mod-framework/cmm-pause-menu-entry.md)
* [Custom game rules](/game-rules/custom-game-rules.md)
* [Game rules vs CMM](/game-rules/game-rules-vs-cmm.md)

# Citations

[1] Glorp — no `in_game/events/`; CMM in `glorpui_cmm_effects.txt`
[2] Glorp `in_game/common/on_action/glorpui_value_sync_on_actions.txt`
[3] CMF wiki — Basic Setup, Reading Setting Values
[4] [Glorp UI file inventory](/references/glorp-ui-file-inventory.md)
[5] Workshop Player Speed Game Rules `3755676844` — additive game rules
