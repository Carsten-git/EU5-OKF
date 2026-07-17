---
type: Reference
title: Scripted geography
description: Named geographic sets for gating buildings, diseases, and triggers without hardcoding province lists everywhere.
tags: [map, scripted-geography, triggers]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

`scripted_geography` definitions group regions/areas/provinces/locations under one name. Triggers then use:

```txt
is_in_scripted_geography = scripted_geography:MnT_tsetse_geography
```

# When to use

- Disease belts, building bans, exploration hazards spanning many locations.
- Cleaner than repeating huge OR lists in every trigger.

# Related

* [Environmental disease](/map/environmental-disease.md)
* [Scripted trigger basics](/scripted-triggers/scripted-trigger-basics.md)

# Citations

[1] MnT `in_game/common/scripted_geography/MnT_africa.txt`
[2] MnT `in_game/common/scripted_triggers/MnT_location_triggers.txt`
