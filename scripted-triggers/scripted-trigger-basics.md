---
type: Reference
title: Scripted trigger basics
description: Defining and calling reusable triggers in common/scripted_triggers/.
resource: game/in_game/common/scripted_triggers/
tags: [scripted-triggers, syntax]
timestamp: 2026-07-06T08:40:00+10:00
status: draft
---

# Location

`in_game/common/scripted_triggers/<mod>.txt`

Vanilla examples: `country_triggers.txt`, `flavor_kor_triggers.txt`. Mod example: `northern_crusade_teu/in_game/common/scripted_triggers/teu_nc_triggers.txt`.

# Definition

A scripted trigger is a named block of ordinary trigger script. The engine inlines it wherever you reference the name.

```txt
teu_nc_has_steadfast_purpose = {
	OR = {
		has_variable = teu_nc_purpose_steadfast
		has_variable = teu_nc_purpose_zealous
	}
}
```

# Invocation

Use the trigger name as a key with value `yes` (or pass arguments — see below):

```txt
trigger = {
	teu_nc_has_steadfast_purpose = yes
}

visible = {
	teu_nc_can_form_odr = yes
}
```

Works in any trigger context: event `trigger`, mission `visible` / `enabled`, formable requirements, on-action `trigger`, and nested `limit` blocks.

# Parameters

Vanilla `readme.txt` documents `$param$` placeholders for reusable triggers:

```txt
my_scoped_trigger = {
	$target$ = {
		prestige = $value$
	}
	is_enemy_of = $target$
}
```

Call site:

```txt
my_scoped_trigger = { target = scope:my_scope value = 13 }
```

Arguments are text substitutions. Typos produce missing-trigger errors in the log.

# `custom_tooltip` inside triggers

Scripted triggers are a good place to bundle player-facing requirement text. Northern Crusade formable gates combine real conditions with tooltip lines:

```txt
teu_nc_can_form_odr = {
	custom_tooltip = {
		text = teu_nc_can_form_odr_purpose_tt
		teu_nc_has_steadfast_purpose = yes
	}
	custom_tooltip = {
		text = teu_nc_can_form_odr_reform_tt
		has_reform = government_reform:teu_nc_centralized_command
	}
}
```

See [Common trigger patterns](common-trigger-patterns.md) for `custom_description` naming pitfalls (do not reuse the trigger key as the tooltip text key).

# See also

* [Common trigger patterns](common-trigger-patterns.md)
* [Scripted effect basics](/scripted-effects/scripted-effect-basics.md) — paired reusable effects
* [Event triggers and options](/events/event-triggers-and-options.md)
* [Vanilla file locations](/references/vanilla-file-locations.md)

# Citations

[1] Vanilla: `in_game/common/scripted_triggers/readme.txt`
[2] Mod: `northern_crusade_teu/in_game/common/scripted_triggers/teu_nc_triggers.txt`
