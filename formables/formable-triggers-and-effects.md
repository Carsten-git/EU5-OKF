---
type: Playbook
title: Formable triggers and effects
description: Writing potential and allow blocks, form_effect pipelines, and scripted gates for formable countries.
tags: [formables, triggers, form_effect, has_reform]
timestamp: 2026-07-06T08:42:00+10:00
resource: game/in_game/common/formable_countries/
status: draft
---

Formables use the same trigger/effect DSL as events. Root scope is always the **active country**.

# potential vs allow

| Block | Question answered |
|-------|-------------------|
| `potential` | Should this formable appear in the list at all? |
| `allow` | Can the player click **Form** right now? |

```txt
potential = {
	has_or_had_tag = TEU
}
allow = {
	custom_tooltip = {
		text = teu_nc_form_gate_odr_tag_tt    # "Playing as the Teutonic Order (TEU)."
		tag = TEU
	}
	teu_nc_can_form_odr = yes                 # scripted trigger, called DIRECTLY (see pitfall below)
	stability >= 20
	NOT = { is_subject = yes }
	owns = location:konigsberg
	at_war = no
}
```

When `allow` fails, the UI shows why — use `custom_tooltip = { text = loc_key … }` on **leaf conditions** for readable requirements.

## Pitfall: outer custom_tooltip hides everything inside (KI-052)

Do **not** wrap a whole scripted trigger in one `custom_tooltip` in `allow`:

```txt
# WRONG — the outer text replaces ALL detailed tooltips inside teu_nc_can_form_odr,
# so the player never sees the Tannenberg / Purpose / reform requirements.
custom_tooltip = {
	text = teu_nc_can_form_odr_tt    # "Form the Orderstaat — …" reads like a circular precondition!
	teu_nc_can_form_odr = yes
}
```

Call the scripted trigger directly and keep `custom_tooltip` on the individual conditions inside it. Phrase every tooltip as a **requirement** ("Has won a decisive victory at Tannenberg."), never as an action ("Form the Prussian League") — action phrasing shows up as a formable being its own precondition.

## Pitfall: broad potential needs tag gates in allow (KI-053)

Widening `potential` for branch preview (below) removes the old tag gating. Without an explicit tag gate in `allow`, TEU could immediately form step-2 formables like the Prussian League. Add a tag gate with a tooltip that also tells the player *who* forms it ("Playing as the #Y Orderstaat#!.").

# Showing locked formables (greyed out)

Vanilla defaults `potential_requires_own = yes` — player may not see a formable until owning land in required areas. For **branch preview** from day one:

```txt
capital_required = no
potential_requires_own = no
potential = {
	OR = {
		has_or_had_tag = TEU
		has_or_had_tag = ODR
		# … all branch tags in the mod tree
	}
}
```

Strict gates stay in `allow` with `custom_tooltip` — player sees the full tree with ✓/✗ requirements.

# Game rule: plausible formables

`rule = plausible` formables are **hidden** when the game rule is set to **Only historical**. Document this for players testing trade branches (PRL, BDM, ENR, BLC).

# Scripted trigger pattern (mod)

Move complex logic to `in_game/common/scripted_triggers/`:

```txt
teu_nc_can_form_odr = {
	custom_tooltip = {
		text = teu_nc_can_form_odr_tannenberg_tt
		has_variable = teu_nc_tannenberg_victory
	}
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

Reference in formable:

```txt
allow = {
	teu_nc_can_form_odr = yes    # direct call — inner custom_tooltips render individually
	# … hard requirements (ownership, war, subject status)
}
```

This separates **flavor milestones** (Tannenberg, Purpose meter, reform enacted) from **map requirements** (owns key locations, area fraction).

# Vanilla reference: Prussia secularization

`PRU_f` combines reform and estate checks:

```txt
allow = {
	NOT = {
		has_reform = government_reform:military_order_reform
		has_estate_privilege = estate_privilege:clergy_powerful_dioceses
	}
	has_estate_privilege = estate_privilege:nobles_land_rights
	"estate_power(estate_type:clergy_estate)" < 0.20
	"estate_power(estate_type:nobles_estate)" > 0.20
	at_war = no
}
```

TEU must transform away from the military-order model before forming Prussia.

# form_effect pipelines

## Government transformation

```txt
form_effect = {
	change_government_type = government_type:teu_nc_military_state
	add_reform = government_reform:teu_nc_military_state_reform
	set_country_rank_effect = { rank = country_rank:rank_kingdom }
	add_prestige = prestige_mild_bonus
	trigger_event_silently = { id = flavor_teu_nc_odr.1 days = 3 }
	set_variable = { name = teu_nc_odr_missions_unlocked value = 1 }
}
```

Order matters for player-visible sequencing: type change → signature reform → rank → rewards → follow-up event.

## Rank and modifiers only

```txt
form_effect = {
	set_country_rank_effect = { rank = country_rank:rank_empire }
	add_country_modifier = { modifier = teu_nc_catholic_empire years = -1 }
}
```

## Territory integration

```txt
form_effect = {
	every_owned_location = {
		limit = {
			area = area:baltic_area
			integration_level = conquered
		}
		change_integration_level = integrated
	}
}
```

## Conditional vanilla logic (Prussia)

```txt
form_effect = {
	if = {
		limit = {
			government_type != government_type:republic
			government_type != government_type:monarchy
		}
		change_government_type = government_type:monarchy
		add_legitimacy = legitimacy_radical_bonus
	}
	if = {
		limit = {
			NOT = { is_member_of_international_organization = international_organization:hre }
		}
		set_country_rank_effect = { rank = country_rank:rank_kingdom }
	}
}
```

# Branching formable trees

`northern_crusade_teu` chains formables by tag history and variables:

| Formable | `potential` highlights | Unlocks |
|----------|------------------------|---------|
| `ODR_f` | `has_or_had_tag = TEU` | Military state path |
| `HPR_f` | ODR or TEU + refused secularization | Sacral monarchy |
| `HPE_f` | `has_or_had_tag = HPR` | Empire tier |
| `BDM_f` | `has_or_had_tag = ODR` | Federal bailiwicks reform |
| `PRL_f` | TEU or ODR | Merchant republic branch |
| `BLC_f` | `has_or_had_tag = PRL` | Maritime federation endgame |

Use `has_or_had_tag` so formation remains visible after tag switch; use `tag = X` inside `allow` when only the current tag may click.

# Debugging checklist

1. **Not in list** — check `potential`, `potential_requires_own`, game rule `rule`, and `level`.
2. **Greyed out** — check `allow`, location fraction, `capital_required`, subject/war flags.
3. **Clicks but nothing** — verify `tag` exists in country history/database; check error log.
4. **Blocked entirely** — active reform with `blocked_from_forming_countries = yes` (e.g. papacy).

# See also

* [Formable countries overview](formable-countries-overview.md)
* [Formable localization](formable-localization.md)
* [Government reforms](/governments/government-reforms.md)

# Citations

[1] Vanilla: `game/in_game/common/formable_countries/00_formable_countries.txt` — `PRU_f` `allow` / `form_effect` (reform + estate gates, conditional government change)
[2] Mod: `mod/northern_crusade_teu/in_game/common/formable_countries/teu_nc_formables.txt` — `ODR_f`, `HPR_f`, branching `potential` / `form_effect`
[3] Mod: `mod/northern_crusade_teu/in_game/common/scripted_triggers/teu_nc_triggers.txt` — `teu_nc_can_form_odr` scripted gate
[4] Vanilla: `game/in_game/common/government_reforms/theocracy.txt` — `military_order_reform` blocked by `PRU_f`
