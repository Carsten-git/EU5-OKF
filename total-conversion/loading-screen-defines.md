---
type: Playbook
title: Loading-screen defines overrides
description: Override engine constants in loading_screen/common/defines by editing individual keys only.
tags: [defines, loading_screen, total-conversion]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: meiou_and_taxes
---

Global engine constants (markets, economy, estates, …) live under **`loading_screen/common/defines/`**, not `in_game`. Wrong root → overrides never apply.

# Discipline

1. Create `loading_screen/common/defines/MyMod_Defines.txt` (or similar).
2. Copy **only the keys you change**, inside the correct category block (`NMarket`, `NEconomy`, …).
3. Comment the vanilla value next to each edit for future merges.
4. **Never** paste an entire vanilla defines category — merge hell on every patch.

```txt
# Conceptual
NEconomy = {
	SOME_KEY = 0.5  # vanilla 1.0
}
```

# Also in loading_screen

Boot branding, custom loading GUI, version label widgets — keep gameplay scripts out of this root.

# Related

* [Three-root architecture](/total-conversion/three-root-and-override-ladder.md)
* [Mod folder structure](/getting-started/mod-folder-structure.md)

# Citations

[1] MnT `loading_screen/common/defines/MnT_Defines.txt`
[2] MnT file header comments — “Do NOT copy entire blocks”
