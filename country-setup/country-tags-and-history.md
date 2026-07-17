---
type: Reference
title: Country tags and history
description: Three-letter tags, scoped references c:TAG, and country history localization.
resource: game/in_game/setup/countries/
tags: [country-setup, tags, localization, history]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

# Country tags

EU5 uses three-letter tags (`TEU`, `POL`, `LIT`) as stable country identifiers in script and setup.

| Syntax | Context | Example |
|--------|---------|---------|
| `tag = TEU` | Trigger on current country | Pulse, mission `enabled` |
| `c:TEU` | Country scope reference | `c:TEU = { teu_nc_init_purpose_for_country = yes }` |
| `country_exists = c:POL` | Existence check | `flavor_teu.1` trigger |
| `has_or_had_tag = TEU` | Tag now or after formable | Advance `potential` |

Tags are **not** defined in mod events — they come from vanilla setup plus any new countries you add to setup files.

# Country metadata (`setup/countries/`)

Regional files under `in_game/setup/countries/` declare per-tag **presentation** data used across the game:

```txt
TEU = {
	color = map_TEU
	color2 = rgb { 102 105 104 }
	culture_definition = prussian
	religion_definition = catholic
	description_category = military
	difficulty = 3
	unit_color0 = white
	unit_color1 = black
	unit_color2 = white
}
```

Vanilla: `baltics.txt`. See [Setup countries files](setup-countries-files.md) for how this differs from bookmark start data.

# Country history text

Long nation-select / intro prose uses customizable localization + yml.

**Routing** — `in_game/common/customizable_localization/country_history.txt`:

```txt
text = { localization_key = country_history_TEU trigger = { tag = TEU } }
```

**Body** — `main_menu/localization/english/country_history_l_english.yml`:

```yaml
 country_history_TEU: "The glorious #italic Order of Brothers of the German House…"
```

Vanilla TEU history references `GetCountry('TEU')`, `ShowAreaName('prussia_area')`, and `GetCharacter('teu_werner_von_orseln')` for dynamic sentences.

# Modding history for new formables

If your mod adds formable tags (`ODR`, `HPR`, …):

1. Add `country_history_<TAG>` keys in localization (or reuse a generic culture entry in `country_history.txt`).
2. Add a `text = { localization_key = … trigger = { tag = ODR } }` line to a mod copy or additive custom loc file.
3. Keep alphabetical / grouped order in vanilla-style files to ease merges.

Northern Crusade reuses TEU for the start bookmark; branch tags mostly inherit mechanics via `has_or_had_tag` and variables rather than new history paragraphs.

# AI personality assignment

Historical AI personalities are **not** in `setup/countries/` — they live in `main_menu/setup/start/26_ai_personalities.txt`:

```txt
TEU = { ai_personality = ai_cautious }  # Teutonic Order
```

Hooked at game start via [on-actions overview](/on-actions/on-actions-overview.md).

# See also

* [Setup countries files](setup-countries-files.md)
* [Country-scoped custom loc](/customizable-localization/country-scoped-custom-loc.md)
* [Common trigger patterns](/scripted-triggers/common-trigger-patterns.md)

# Citations

[1] Vanilla: `in_game/setup/countries/baltics.txt`, `country_history_l_english.yml`
[2] Vanilla: `main_menu/setup/start/26_ai_personalities.txt`
