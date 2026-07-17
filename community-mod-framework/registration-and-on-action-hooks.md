---
type: Playbook
title: CMF registration and on-action hooks
description: Hook cmf_on_mod_registration and CMF game-start/load/transfer/human pulse on_actions.
tags: [cmf, on-actions, registration, multiplayer]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: community_mod_framework
---

CMF exposes **shared on_action parents**. Append your leaf on_actions the same way vanilla chains work — do not redefine CMF’s parent effects.

# Mod registration (standard entry)

Runs per human player on game start and when the mod menu opens. Use it to register CMM settings, action bar buttons, alerts, etc.

```txt
# in_game/common/scripted_effects/your_effects.txt
your_mod_register = {
	# cmm_register_* / cmf_add_action_bar_element / …
}

# in_game/common/on_action/your_on_actions.txt
cmf_on_mod_registration = {
	on_actions = {
		your_mod_on_register
	}
}

your_mod_on_register = {
	effect = {
		your_mod_register = yes
	}
}
```

# Game start & load hooks

All fire once in **no scope** (like vanilla `on_game_start`), except `*_human_country` variants (country scope, each human).

| Hook | When |
|------|------|
| `on_game_start_after_lobby` | New game, after country selection |
| `on_game_start_after_lobby_human_country` | Same, per human |
| `on_game_load` | Every save load (incl. from selection) |
| `on_game_load_human_country` | Same, per human |
| `on_game_load_after_lobby` | Load only after passing selection |
| `on_game_load_after_lobby_human_country` | Same, per human |

Prefer these over raw `on_game_start` when you need **post-lobby** / **human-only** timing. Complements [pulse orchestration](/on-actions/pulse-orchestration.md).

# Country transfer

`cmf_on_country_transfer` — player switches country. CMF auto-copies CMM settings. Scopes: `scope:old_country`, `scope:new_country`.

# Recurring human pulses

More efficient than every mod checking `is_ai = no`:

- `cmf_yearly_human_country_pulse`
- `cmf_monthly_human_country_pulse`

# Related

* [Community Mod Menu](/community-mod-framework/community-mod-menu.md)
* [Pulse orchestration](/on-actions/pulse-orchestration.md)
* [On game start](/on-actions/on-game-start.md)

# Citations

[1] https://raw.githubusercontent.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/main/docs/wiki/cmf.wiki — On-Action Hooks
[2] CMF wiki: On-Action Hooks
