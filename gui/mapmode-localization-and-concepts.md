---
type: Playbook
title: Mapmode localization and game concepts
description: Loc keys, legends, strategy copy, custom textformatting colors, and game_concepts for utility map modes (Zorange's Mapmode Collection).
tags: [gui, map-modes, localization, game-concepts]
timestamp: 2026-07-13T17:10:00+10:00
status: complete
source_mod: zoranges.mapmode.collection
source_version: "1.0"
---

# Loc key set (per mode)

Assume mode id `zmc_building_cap_percentage`:

| Key | Role |
|-----|------|
| `mapmode_<id>_name` | Short picker name |
| `MAPMODE_<ID>` | Long mode description (tooltips / help) — ZMC embeds **Strategy** tips here |
| `MAPMODE_<ID>_TT_LAND` | Hover on land |
| `MAPMODE_<ID>_TT_WATER` | Hover on water |
| `MAPMODE_<ID>_TT_UNCOLONISED` | Hover on unowned land (when needed) |
| Extra `_TT_*` | Edge cases (0 growth forever, already at 100 dev, …) |
| `<id>_<band>_legend_key` | Matches each `legend_key = { desc = … }` |

Icons: `main_menu/gfx/interface/icons/map_modes/<mode_id>.dds` (same stem as mode key).

# Tooltip data binding

Prefer script values in loc:

```yml
MAPMODE_ZMC_BUILDING_CAP_PERCENTAGE_TT_LAND: "[ROOT.GetLocation.MakeScope.ScriptValue('zmc_free_building_slots')|Y] …"
```

Cached modes use variables:

```yml
MAPMODE_ZMC_URBANISATION_SUITABILITY_TT_LAND: "[ROOT.GetLocation.MakeScope.Var('zmc_local_urbanisation_suitability').GetValue|Y]."
```

Vanilla helper getters also appear (`GetModifierValueFixed`, `GetModifierValueAndPercentWithCountry`) — use when they already match the metric.

# Game concepts

Register under `main_menu/common/game_concepts/`:

```txt
zmc_urbanisation_suitability = {
	texture = "gfx/interface/icons/modifiers/_default.dds"
}
```

Loc:

- `game_concept_<id>` — short name
- `game_concept_<id>_desc` — full definition (ZMC explains inputs: topography, vegetation, climate, …)

Mode descriptions then link with `[zmc_urbanisation_suitability|e]` so players get encyclopedia-style popups.

# Custom textformatting colors

For gradient wording inside loc (e.g. “close to the cap”), ship `main_menu/gui/<mod>_textformatting.gui`:

```txt
textformatting = {
	format = {
		name = zmc_color_orange
		format = "color:{1,0.65,0};glow_color:{0,0,0,0};glow_offset:{0.0,0.0}"
	}
}
```

Use as `#zmc_color_orange text#!` in yml. Decimal RGB; optional glow RGBA.

# Honest caveats in copy

ZMC documents metric limitations in the mode description (e.g. closed/unemployed buildings not counted toward building-cap mapmode). Prefer that over silent wrong colors.

# Related

* [Custom map modes](/gui/custom-map-modes.md)
* [Live vs cached mapmode metrics](/gui/live-vs-cached-mapmode-metrics.md)
* [Multi-file loc split for UI mods](/localization/multi-file-loc-split-for-ui-mods.md)

# Citations

[1] ZMC `main_menu/localization/english/zmc_loc_l_english.yml`
[2] ZMC `main_menu/common/game_concepts/zmc_concepts.txt`
[3] ZMC `main_menu/gui/zmc_textformatting.gui`
