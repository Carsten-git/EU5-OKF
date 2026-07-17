---
type: Reference
title: Player-scoped modifiers from game rules
description: apply_modifier = player:modifier_id applies static modifiers only to human countries — AI does not receive the bonus.
tags: [game-rules, modifiers, player, ai, scope]
timestamp: 2026-07-13T16:50:00+10:00
status: complete
source_mod: m524.player_speed_game_rules
source_version: "1.0.2"
---

# Problem

A QoL or house-rule bonus should affect **human** countries only. Applying a global country modifier via script to everyone would buff AI as well.

# Pattern

Game-rule settings support:

```txt
apply_modifier = player:<static_modifier_id>
apply_modifier = ai:<static_modifier_id>
apply_modifier = all:<static_modifier_id>
```

Documented in vanilla `_game_rules.info`. Player Speed Game Rules uses **`player:`** exclusively so assimilation / conversion / integration / tribesmen speed buffs hit humans only.

# Worked example (abbreviated)

**Rule setting** (`main_menu/common/game_rules/…`):

```txt
m524_culture_assimilation_speed_x2 = {
	apply_modifier = player:m524_player_culture_assimilation_x2
	flag = blocks_achievements
	flag = general_rule
}
```

**Static modifier** (`main_menu/common/static_modifiers/…`):

```txt
m524_player_culture_assimilation_x2 = {
	game_data = {
		category = country
	}
	global_pop_assimilation_speed_modifier = 1.0
}
```

Vanilla setting for the same rule has **no** `apply_modifier` — “Vanilla” means no bonus.

# Multiplier math lesson

Displayed “x2 / x4 / …” labels are product copy. Engine values are often **additive modifiers** (e.g. `1.0` → +100% for “x2”, `3.0` → +300% for “x4”). Always verify against the modifier type’s semantics; do not assume `value = multiplier`.

# When to use `ai:` or `all:`

| Scope | Typical use |
|-------|-------------|
| `player:` | Human-only QoL / difficulty helpers |
| `ai:` | AI difficulty / aggression style modifiers (vanilla `ai_difficulty`) |
| `all:` | Campaign-wide modifiers that should hit every country |

For **behaviour gates** (e.g. “AI may convert RGO”) prefer `has_game_rule = <setting>` in script rather than a no-op modifier — see [Custom game rules](/game-rules/custom-game-rules.md).

# Related

* [Custom game rules](/game-rules/custom-game-rules.md)
* [game_data category](/modifiers/game-data-category.md)
* [Modifier stat keys](/modifiers/modifier-stat-keys.md)

# Citations

[1] Workshop `3755676844` — `apply_modifier = player:m524_player_*`
[2] Vanilla `_game_rules.info` — `player`, `ai`, `all` categories
[3] Vanilla `player_difficulty` / `ai_difficulty` in `00_game_rules.txt`
