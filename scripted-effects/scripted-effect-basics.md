---
type: Reference
title: Scripted effect basics
description: Defining and calling reusable effects in common/scripted_effects/.
resource: game/in_game/common/scripted_effects/
tags: [scripted-effects, syntax]
timestamp: 2026-07-06T09:00:00+10:00
status: complete
---

# Location

`in_game/common/scripted_effects/<mod>.txt`

# Definition

```txt
teu_nc_change_purpose = {
	change_variable = { name = teu_nc_order_purpose add = $value$ }
	change_variable = { name = teu_nc_purpose_monthly_delta add = $value$ }
	teu_nc_apply_purpose_tier_flags = yes
}
```

# Invocation

```txt
teu_nc_change_purpose = { value = 5 }
```

```txt
teu_nc_apply_purpose_tier_flags = yes
```

# Parameters

Use `$param$` placeholders in the definition; pass named arguments at call sites.

# See also

* [Scripted trigger basics](/scripted-triggers/scripted-trigger-basics.md)
* [Variables and monthly mechanics](variables-and-monthly-mechanics.md)

# Citations

[1] [Vanilla scripted effects directory](game/in_game/common/scripted_effects/)
[2] [Mod scripted effects](mod/northern_crusade_teu/in_game/common/scripted_effects/teu_nc_effects.txt)
[3] [Scripted trigger basics](/scripted-triggers/scripted-trigger-basics.md)
