---
type: Playbook
title: Timed location modifier visibility
description: Make timed location modifiers appear in Location.GetTimedModifiers with end date and icon.
tags: [modifiers, location, timed-modifiers, gui, localization]
timestamp: 2026-07-11T21:00:00+10:00
status: complete
source_mod: rgo_conversion
source_version: "0.1.1"
---

Timed location modifiers show in the **location panel** as `timed_modifier_icon` chips (`Location.GetTimedModifiers`), with a progress ring and tooltip duration **Until $UNTIL|Y$** (`TimedModifier.GetDuration`). They do **not** appear as Expand-RGO-style markers on the map — see [Construction map markers](/buildings/construction-map-markers.md).

# Why a cooldown “exists” but is invisible

`has_location_modifier = my_cooldown` can be true while **`GetTimedModifiers` omits** the entry when the modifier is effectively empty.

Empty-only definitions often fail:

```txt
my_cooldown = {
	game_data = { category = location }
	# no stats, no modifier_icons id entry → chip missing
}
```

# Checklist so the chip shows

1. **`game_data = { category = location }`**
2. **At least one of:**
   - A real (even tiny) stat line, e.g. `local_monthly_development_modifier = -0.001`
   - An id-keyed icon in `main_menu/common/modifier_icons/*.txt` (vanilla pattern: `hre_internal_peace_enforced`)
3. Loc: `STATIC_MODIFIER_NAME_<id>` and `STATIC_MODIFIER_DESC_<id>` (UTF-8 BOM)
4. Apply with duration: `add_location_modifier = { modifier = … years/months = … mode = replace }`

```txt
# main_menu/common/static_modifiers/…
my_cooldown = {
	game_data = { category = location }
	local_monthly_development_modifier = -0.001
}

# main_menu/common/modifier_icons/…
my_cooldown = {
	positive = "gfx/interface/icons/modifier_types/global_raw_material_output.dds"
	negative = "gfx/interface/icons/modifier_types/global_raw_material_output.dds"
}
```

# UI where players find it

* Location view timed-modifier strip (single icon, or count → list if many)
* Tooltip: name, effects, **Until \<date\>**, flavor DESC
* Province hover can list province timed modifiers; location chips are on the location panel

# Do not put live countdowns in STATIC_MODIFIER_DESC

Modifier DESC is flavor; the engine injects duration via `TimedModifier.GetDuration`. Dynamic `ROOT`/`GetPlayer` in static modifier loc usually fails — see [Dynamic text in loc](/localization/dynamic-text-in-loc.md).

# Two timers, two chips (RGO Conversion)

Do **not** merge convert-lock and economic hangover into one modifier if durations differ:

| Modifier | Duration | Role |
|----------|----------|------|
| `rgo_conv_settling` | 10y | `local_raw_material_output = -0.10` |
| `rgo_conv_cooldown` | 25y | Convert button lock (`has_location_modifier`); must stay visible for full 25y |

Empty lock timers are often hidden by `GetTimedModifiers` — give the lock chip an icon **and** a real (even mild) timed effect so **Until \<date\>** appears for the full duration. Keep the −10% output only on the settling modifier. Player-facing `STATIC_MODIFIER_DESC` should be immersive flavor — do not cross-reference the other modifier by implementation name.

# Chip visible but no “Until \<date\>” (KI-070)

`STATIC_MODIFIER_NAME` / `DESC` can look fine while **`TimedModifier.GetDuration`** is empty.

Proven visibility line for convert-locks (RGO Conversion):

```txt
local_monthly_development_modifier = -0.001
```

That tiny development line is what previously made **Until \<date\>** reliable. Replacing it with empty / migration-only, or leaving a stale instance from an empty definition, can drop the duration line even when the chip still shows.

After changing a modifier definition, clear and re-apply:

```txt
remove_location_modifier = my_cooldown
add_location_modifier = {
	modifier = my_cooldown
	years = 25
	mode = add_and_extend
}
```

Vanilla often uses `mode = add_and_extend` for location timers. Re-complete the project (or otherwise re-apply) so the instance is created with a real end date.

# See also

* [Static modifier localization](/localization/static-modifier-localization.md)
* [game_data category](/modifiers/game-data-category.md)
* [Custom modifier type registration](/modifiers/custom-modifier-type-registration.md) — icons folder
* [KI-067](/validation/known-issues.md)

# Citations

[1] Vanilla `location_window.gui` — `LocationView.GetLocation.GetTimedModifiers`
[2] Vanilla `modifier_icons` entry `hre_internal_peace_enforced`
[3] RGO Conversion `rgo_conv_cooldown` — empty modifier was invisible until icon + tiny stat
