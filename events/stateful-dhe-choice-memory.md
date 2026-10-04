---
type: Playbook
title: Stateful DHE arcs and choice memory
description: Build historical event arcs that survive tag changes and let later options inspect earlier choices.
resource: game/in_game/events/DHE/flavor_BYZ.txt
tags: [events, dhe, variables, branching, tag-change]
timestamp: 2026-09-30T17:58:00+10:00
status: complete
---

# Two kinds of state

A multi-decade event arc can read:

1. **live game state** — opinion, liberty desire, prosperity, control, food, war, manpower, societal values; or
2. **choice memory** — country variables recording what an earlier option meant.

Live state makes events systemic, but later wars and recovery can erase the consequences of an earlier choice. Choice memory is appropriate when a later event must know that the player conciliated, coerced, prepared, or made a named promise even after ordinary values changed.

# Numeric choice-memory pattern

EU5 country variables can act as small counters:

```txt
hidden_effect = {
	change_variable = {
		name = my_arc_conciliation
		add = 1
	}
}
```

Later option:

```txt
option = {
	name = my_arc.10.a
	trigger = {
		has_variable = my_arc_conciliation
		var:my_arc_conciliation >= 3
	}
	# visible effects
}
```

Vanilla compares numeric variables directly (`var:num_of_scientific_breakthroughs >= 4`) and uses variable-gated options in long DHE chains. Use a few semantically stable counters, not one flag for every line of prose.

# Boolean facts and mutually exclusive outcomes

Use a boolean variable for a categorical fact that cannot be reconstructed:

```txt
set_variable = my_arc_expedition_survivor
```

For exclusive end states, set the chosen state and remove incompatible states:

```txt
hidden_effect = {
	set_variable = my_arc_compact_outcome
	remove_variable = my_arc_collapse_outcome
	remove_variable = my_arc_partition_outcome
	remove_variable = my_arc_military_outcome
}
```

This follows vanilla's mutually exclusive variable branches. Clean temporary counters after the final event when later content no longer needs them.

# Keep technical state out of player prose

Effects in `hidden_effect` do not print in the option tooltip. Pair them with a localized custom tooltip:

```txt
custom_tooltip = {
	text = my_arc_strengthens_compact_tt
	hidden_effect = {
		change_variable = {
			name = my_arc_conciliation
			add = 1
		}
	}
}
```

Do not expose raw `has_variable` requirements in player-facing trigger lists. Wrap technical branch requirements in localized tooltips that explain the in-world condition.

# Retaining a country arc after tag change

Register every current tag that may schedule the event, then gate the arc by country history:

```txt
dynamic_historical_event = {
	tag = AAA
	tag = BBB
	from = 1400.1.1
	to = 1450.1.1
	monthly_chance = 5
}

trigger = {
	has_or_had_tag = AAA
	# current-state requirements
}
```

This gives the event to:

- current AAA;
- BBB that was formerly AAA.

It excludes unrelated BBB countries that never had tag AAA. Vanilla `flavor_byz.11` registers BYZ and VEN and combines current identity with history checks.

# Choice-memory arc vs live-state anthology

Prefer **choice memory** when:

- a climax must reward a promise made decades earlier;
- ordinary values can recover for unrelated reasons;
- the player should recognize an authored political route.

Prefer **live state** when:

- the event is an independent historical scene;
- current simulation conditions matter more than causality;
- lower custom-state maintenance is important.

Hybrid use is normal: remember only categorical promises and identities, while reading current prosperity, diplomacy, manpower, and war state.

# Checklist

- [ ] Use `fire_only_once = yes` for one-time historical scenes.
- [ ] Combine a historical window with current-state triggers.
- [ ] Register every possible current tag and gate by `has_or_had_tag`.
- [ ] Keep counters few, named, and country-scoped.
- [ ] Make each choice produce visible ordinary effects as well as hidden memory.
- [ ] Localize hidden branch requirements.
- [ ] Remove temporary variables when the arc ends.
- [ ] Give AI options viable `ai_chance` weights.

# Related

* [Dynamic historical events](/events/dynamic-historical-events.md)
* [Event triggers and options](/events/event-triggers-and-options.md)
* [Hidden effects and AI chance](/events/hidden-effects-and-ai-chance.md)
* [DHE browser visibility](/events/dhe-browser-visibility.md)

# Citations

[1] Vanilla `in_game/events/DHE/flavor_BYZ.txt` — multiple DHE tags, `has_or_had_tag`, numeric variable branches and follow-ups.
[2] Vanilla `in_game/events/DHE/flavor_TUR.txt` — mutually exclusive branch variables.
[3] Vanilla `in_game/events/institution_events.txt` — direct numeric `var:* >= N` comparisons.
[4] Vanilla `in_game/events/DHE/flavor_ENG.txt` — option-level triggers.
