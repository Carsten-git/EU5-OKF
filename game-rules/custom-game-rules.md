---
type: Reference
title: Custom game rules (additive)
description: How utility mods add new game rules under main_menu/common/game_rules without replacing vanilla — settings, flags, loc keys, and has_game_rule.
tags: [game-rules, main_menu, lobby, settings, ironman]
timestamp: 2026-07-13T16:50:00+10:00
status: complete
source_mod: m524.player_speed_game_rules
source_version: "1.0.2"
---

EU5 exposes a **vanilla Game Rules** screen at campaign setup. Mods can **add** new rule groups via files under `main_menu/common/game_rules/` — this is **not** total-conversion-only. Workshop utility mods (e.g. Player Speed Game Rules) ship additive rules alongside vanilla.

# Problem

Players need campaign-wide house rules (defaults that stick with the save, ironman-visible, chosen before play) without building a custom lobby wizard or depending on CMF/CMM.

# File layout

| Path | Role |
|------|------|
| `main_menu/common/game_rules/<mod>_….txt` | Rule groups + settings |
| `main_menu/common/static_modifiers/<mod>_….txt` | Modifiers referenced by `apply_modifier` |
| `main_menu/localization/<lang>/<mod>_…_l_<lang>.yml` | Rule/setting labels for the rules UI |
| `in_game/localization/<lang>/<mod>_…_l_<lang>.yml` | Same keys for in-game tooltips / static modifier names |

Schema comments: vanilla `game/main_menu/common/game_rules/_game_rules.info`.

# Rule schema (minimal)

```txt
my_mod_ai_convert = {
	default = my_mod_ai_convert_off

	my_mod_ai_convert_off = {
		flag = general_rule
	}

	my_mod_ai_convert_on = {
		flag = general_rule
		# optional: apply_modifier = ai:some_mod — or gate script with has_game_rule
	}
}
```

| Field | Meaning |
|-------|---------|
| Outer key | Rule group id → loc `rule_<outer_key>` |
| `default = <setting>` | Selected setting at new game |
| Setting block | Loc `setting_<setting_key>` + `setting_<setting_key>_desc` |
| `apply_modifier = <scope>:<modifier_id>` | Auto-apply static modifier — scopes: `player`, `ai`, `all` |
| `flag = …` | Engine / achievement / UI flags (see below) |
| `defines = { … }` | Optional define overrides while active |

# Flags (from `_game_rules.info` + worked examples)

| Flag | Use |
|------|-----|
| `general_rule` | Appears in the general rules category (Player Speed uses this on every setting) |
| `flavour_rule` | Flavour category (vanilla formables) |
| `blocks_achievements` | Achievement lock when this setting is active |
| Engine-named flags | e.g. `no_ahistorical_formable_countries`, `harsh_ai` — only when you intentionally hook engine behaviour |

Utility multipliers that change balance should set `blocks_achievements` on non-vanilla settings.

# Script check: `has_game_rule`

Vanilla script tests the **setting key**, not the outer rule group:

```txt
has_game_rule = institution_spawn_random_city
```

Use this to gate AI pulses, eligibility, or content when a setting does not (only) apply a modifier.

# Localization keys

| Key pattern | Example |
|-------------|---------|
| `rule_<rule_group>` | `rule_m524_culture_assimilation_speed` |
| `setting_<setting>` | `setting_m524_culture_assimilation_speed_x2` |
| `setting_<setting>_desc` | Longer description under the setting |
| `STATIC_MODIFIER_NAME_<modifier_id>` | Name shown if the modifier appears in UI |

Prefix rule/setting/modifier ids with a mod token (`m524_`, `sire_`, …) to avoid clashes.

## Localization file pairing (required for rules UI)

| Script | Loc (main_menu) | Loc (in_game mirror) |
|--------|-----------------|----------------------|
| `main_menu/common/game_rules/rgo_conv_game_rules.txt` | `main_menu/localization/english/rgo_conv_game_rules_l_english.yml` | `in_game/localization/english/rgo_conv_game_rules_l_english.yml` |

Workshop **Player Speed Game Rules** ships the **same keys in both** `main_menu` and `in_game` localization folders. Do not bury game-rule keys only in a general mod yml.

**Encoding:** all `*_l_*.yml` must be **UTF-8 with BOM** (`utf-8-sig`). Without BOM the engine often loads **zero keys** from that file — Game Rules then show raw `setting_*` ids ([KI-010](/validation/known-issues.md), [KI-074](/validation/known-issues.md)).

**After loc edits:** full quit to desktop and relaunch EU5 ([KI-030](/validation/known-issues.md)) — hot reload is unreliable for yml.

# Additive load behaviour

- New files under `game_rules/` **add** rules; you do not need to copy `00_game_rules.txt`.
- Prefer one file per mod (or per feature family) with namespaced keys.
- Confidence: **high** for additive rules + player modifiers (worked workshop example). Mid-campaign rule change / save migration of rule choices: treat as **unvalidated** until tested for your use case.

# Related

* [Player-scoped modifiers from game rules](/game-rules/player-scoped-modifiers-from-rules.md)
* [Game rules vs CMM](/game-rules/game-rules-vs-cmm.md)
* [Static modifiers](/modifiers/static-modifiers.md)
* [Player options without lobby events](/community-mod-framework/player-options-without-lobby.md)

# Citations

[1] Workshop `3450310/3755676844` — Player Speed Game Rules v1.0.2 (`m524.player_speed_game_rules`)
[2] Vanilla `game/main_menu/common/game_rules/_game_rules.info`
[3] Vanilla `game/main_menu/common/game_rules/00_game_rules.txt`
[4] Vanilla `has_game_rule` usage in `in_game/common/on_action/_hardcoded.txt`, institutions
