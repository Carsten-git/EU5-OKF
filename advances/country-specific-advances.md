---
type: Reference
title: Country-specific advances
description: Adding nation advances with has_or_had_tag potential, count_TAG naming, and localization.
resource: mod/northern_crusade_teu/in_game/common/advances/count_TEU_northern_crusade.txt
tags: [advances, country, TEU, mod-pattern]
timestamp: 2026-07-06T08:45:00+10:00
status: complete
---

Country-specific advances extend the research tree for one tag (or culture group) without editing vanilla files. Add a new `.txt` under `in_game/common/advances/` — the game merges all advance files at load time.

# Vanilla pattern: country_TAG.txt

Vanilla TEU advances live in `country_TEU.txt`. Each block uses a `teu_` prefix and `has_or_had_tag = TEU` in `potential`:

```txt
teu_crusader_discipline = {
	content_priority = 1100
	age = age_1_traditions
	potential = {
		has_or_had_tag = TEU
	}
	discipline = 0.05
	fort_limit = 1
	requires = feudalism_advance
}
```

`has_or_had_tag` (not bare `tag =`) keeps advances available after forming a new country from TEU — important for mods with formable tags.

Localization in `main_menu/localization/english/advances_l_english.yml`:

```yml
 teu_crusader_discipline: "Crusader Discipline"
 teu_crusader_discipline_desc: "Our grand order was forged in the flames…"
```

Every advance needs **both** `_desc` and title keys. Missing loc shows the raw id in the research panel.

# Mod pattern: count_TEU_northern_crusade.txt

`northern_crusade_teu` adds a parallel file (filename is descriptive; `count_` echoes vanilla `country_` convention):

```txt
teu_nc_last_crusade = {
	content_priority = 900
	age = age_1_traditions
	potential = {
		has_or_had_tag = TEU
	}
	global_pop_conversion_speed_modifier = 0.15
	requires = feudalism_advance
	starting_technology_level = 4
}

teu_nc_amber_monopoly = {
	content_priority = 890
	age = age_1_traditions
	potential = {
		has_or_had_tag = TEU
	}
	trade_income = 0.05
	requires = teu_nc_last_crusade
	starting_technology_level = 5
}
```

Design choices in this mod:

| Choice | Reason |
|--------|--------|
| `teu_nc_` prefix | Avoids colliding with vanilla `teu_*` keys |
| Separate file | Keeps generated content out of vanilla override paths |
| `requires = feudalism_advance` | Hooks into global tree like vanilla TEU advances |
| `starting_technology_level = 4/5` | TEU template starts at 3 — mod advances must be higher |
| `content_priority` 880–900 | Sorts mod nodes near vanilla TEU entries in the UI |

# Workflow for new country advances

1. **Read vanilla** — `country_<TAG>.txt` if it exists; otherwise pick a similar nation (`country_HUN.txt`).
2. **Create** `in_game/common/advances/<your_file>.txt` with `potential` scoped to your tag.
3. **Chain** `requires` to an existing global or vanilla-country advance so the node is reachable.
4. **Set** `starting_technology_level` above the country's template level.
5. **Localize** in `main_menu/localization/english/advances_l_english.yml` (UTF-8 BOM).
6. **Validate** modifier keys — `python tools/validate_mod.py --advances`.

Generated mods can emit advances from `tools/generate_game_content.py` using the same field set.

# Coexistence with vanilla

Adding `count_TEU_northern_crusade.txt` does **not** replace `country_TEU.txt`. Both files load; vanilla and mod advances appear in the same TEU tree. To override a vanilla advance definition entirely, use the same advance id in a file that loads later (alphabetically later filename wins among mods) or use `REPLACE:` syntax per wiki modding docs.

# See also

* [Advance file structure](advance-file-structure.md)
* [Starting technology level](starting-technology-level.md)
* [Advance triggers and modifiers](advance-triggers-and-modifiers.md)
* [Reading vanilla examples](/getting-started/reading-vanilla-examples.md)

# Citations

[1] `game/in_game/common/advances/country_TEU.txt`
[2] `northern_crusade_teu/in_game/common/advances/count_TEU_northern_crusade.txt`
[3] `game/main_menu/localization/english/advances_l_english.yml`
