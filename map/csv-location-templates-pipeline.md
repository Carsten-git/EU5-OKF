---
type: Playbook
title: CSV location templates pipeline
description: Treat location_templates as generated output — CSV overlay + Python patch for climate (or other) fields.
tags: [map, tooling, location-templates, pipeline]
timestamp: 2026-07-11T11:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Editing `climate = …` (or similar) across tens of thousands of locations by hand is not viable. Treat **`location_templates.txt` as generated code**.

# Pipeline

| Stage | Input | Tool | Output |
|-------|-------|------|--------|
| 1 | Geo hierarchy / definitions | Optional parser → CSV | Location list |
| 2 | External GIS/spreadsheet | `location, climate` CSV | Assignment table |
| 3 | Source templates + CSV | Replace script | `in_game/map_data/location_templates.txt` |
| 4 | In-game | Screenshot mapmodes / `debug_color` | QA |

# Script pattern (MnT)

`tools/climate_replace_from_csv_in_location_templates/main.py`:

1. Read CSV rows `location, climate`
2. Walk source template lines; on `location_name = {` blocks, regex-replace `climate = \w+`
3. Write the shipped `location_templates.txt`

Run tools with cwd under `tools/` (or sibling of `shared/`). See [Toolchain](/tooling/total-conversion-toolchain.md).

# Record shape

```txt
stockholm = {
	topography = flatland
	vegetation = grasslands
	climate = hemiboreal_climate
	# …
}
```

# Lessons

- Keep human-editable **source** + **CSV overlay** + **deterministic script**.
- Workshop publishes may omit the CSV; the script still documents the intended workflow.
- Same pattern works for religion/culture/raw_material batch fixes — one field at a time is safest.

# Citations

[1] MnT `tools/climate_replace_from_csv_in_location_templates/main.py`
[2] MnT `in_game/map_data/location_templates.txt`
[3] [Köppen climates](/map/koppen-climates.md)
