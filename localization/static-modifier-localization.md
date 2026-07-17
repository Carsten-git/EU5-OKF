---
type: Reference
title: Static modifier localization
description: Keys STATIC_MODIFIER_NAME_ and STATIC_MODIFIER_DESC_ for country modifiers.
resource: game/main_menu/localization/english/static_modifiers_l_english.yml
tags: [localization, modifiers]
timestamp: 2026-07-06T09:00:00+10:00
status: complete
---

# Key pattern

For modifier id `teu_nc_purpose_meter` defined in `main_menu/common/static_modifiers/`:

```yaml
 STATIC_MODIFIER_NAME_teu_nc_purpose_meter: "Purpose: [ROOT.GetVariable('teu_nc_order_purpose').GetValue|0]/100 ([ROOT.Custom('teu_nc_purpose_tier_name')])"
 STATIC_MODIFIER_DESC_teu_nc_purpose_meter: "Whether Christendom still accepts our crusading mission…"
```

Prefix is always `STATIC_MODIFIER_NAME_` / `STATIC_MODIFIER_DESC_` + **modifier id**.

# Display vs gameplay

You may define multiple modifiers:

- **Meter** — always-on readout (name can show dynamic score)
- **Tier display** — small thematic stat so the tier appears in the list
- **Tier gameplay** — real bonuses/penalties

See [Displaying hidden mechanics](/modifiers/displaying-hidden-mechanics.md).

# Timed location modifiers

For **location** cooldowns/flags in `Location.GetTimedModifiers`, NAME/DESC alone is not enough — empty modifiers are often omitted. See [Timed location modifier visibility](/modifiers/timed-location-modifier-visibility.md).

# See also

* [Static modifiers](/modifiers/static-modifiers.md)
* [Dynamic text in loc](dynamic-text-in-loc.md)
* [Timed location modifier visibility](/modifiers/timed-location-modifier-visibility.md)

# Citations

[1] [Vanilla static modifier loc](game/main_menu/localization/english/static_modifiers_l_english.yml)
[2] [Mod Purpose meter keys](mod/northern_crusade_teu/main_menu/localization/english/teu_nc_l_english.yml)
[3] [Static modifiers](/modifiers/static-modifiers.md)
