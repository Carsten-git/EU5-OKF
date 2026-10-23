---
type: Reference
title: Location-scoped modifier types
description: Categorized vanilla EU5 location-scoped modifier type list from modifier_type_definitions, including goods output, estate, pop, control, trade, military, disease, and miscellaneous local modifiers.
tags: [modifiers, location, vanilla, reference, economy, estates]
timestamp: 2026-10-23T15:54:00+11:00
status: complete
source_mod: vanilla
resource: main_menu/common/modifier_type_definitions/00_modifier_types.txt
---

# EU5 Vanilla Location-Scoped Modifier Types

Source: `C:\Program Files (x86)\Steam\steamapps\common\Europa Universalis V\game\main_menu\common\modifier_type_definitions\00_modifier_types.txt`

Total `game_data.category = location` modifier types: **423**

## Executive Summary

- Vanilla exposes a large location modifier surface: **423** location-scoped modifier types.
- The richest balance lever group is goods production: per-good output modifiers and per-good establishment speed modifiers. These are the main tools for static comparative advantage work.
- Estate-local levers exist, but they affect **estate power / wealth share / unrest**, not estate-specific local tax rates. There is no local equivalent of `nobles_estate_max_tax`.
- Population and pop-type controls are extensive: desired pop ratios, growth, migration, literacy, food consumption, assimilation/conversion blockers, and promotion/demotion speed.
- Economic-state levers include RGO size, production efficiency, market access, control, proximity, development, food storage, trade capacity, and building establishment.
- Military and strategic location levers are broad: forts, garrisons, attrition, movement, levies, recruitment/building permissions, supply, ports, and maritime behavior.
- Disease, religious movement, rebel, separatism, and piracy modifiers are available for local crisis or flavor systems.

## Category Counts

| Category | Count | Notes |
|---|---:|---|
| Goods Output | 76 | Per-good `local_<good>_output_modifier` and generic raw material output. |
| Goods Establishment Speed | 76 | Per-good establishment speed modifiers for production/building establishment. |
| Estates And Wealth Share | 17 | Local estate power, estate unrest, and peasant enfranchisement levers that affect who captures wealth or unrest. |
| Pop Type Composition And Pop Mechanics | 91 | Desired pop composition, migration, growth, literacy, food consumption, assimilation/conversion blockers, and general pop lifecycle modifiers. |
| RGO, Production, Construction, And Buildings | 29 | RGO size/time, production efficiency, construction speed, building levels, building establishment, town rights, and building-owner resource hooks. |
| Control, Proximity, Integration, Development, And Culture | 22 | Local control, proximity, integration, development, cultural influence/tradition, institutions, prosperity, devastation recovery. |
| Food And Storage | 9 | Food capacity, food production, food decay, food preservation, monthly food, and food stockpile capacity. |
| Trade, Market, Maritime, And Harbors | 21 | Market access, merchant capacity/power, trade center power, maritime presence, harbors, blockades, and privateer/vision/shipbuilding toggles. |
| Military, Forts, Movement, Levies, And Supply | 42 | Attrition, levies, manpower, sailors, garrisons, forts, movement cost/blocking, recruitment/building permissions, supply, and war score. |
| Disease And Religious Movements | 22 | Disease growth/impact/resistance plus Reformation movement growth/resistance. |
| Unrest, Rebels, Piracy, And Separatism | 4 | Unrest, separatism, rebel thresholds, slave satisfaction, pirate spawn, and related disorder levers. |
| Location Rules, Capacities, And Miscellaneous | 14 | Remaining location-scoped mechanics: generic capacity, parliament seat, cardinal, stockpile, construction slots, institution chance, etc. |

## Categorized Modifier Tables

### Goods Output

Per-good `local_<good>_output_modifier` and generic raw material output.

| Modifier | percent | boolean | color |
|---|---:|---:|---|
| `local_alum_output_modifier` | yes |  |  |
| `local_amber_output_modifier` | yes |  |  |
| `local_beer_output_modifier` | yes |  |  |
| `local_beeswax_output_modifier` | yes |  |  |
| `local_books_output_modifier` | yes |  |  |
| `local_camels_output_modifier` | yes |  |  |
| `local_cannons_output_modifier` | yes |  |  |
| `local_chili_output_modifier` | yes |  |  |
| `local_clay_output_modifier` | yes |  |  |
| `local_cloth_output_modifier` | yes |  |  |
| `local_cloves_output_modifier` | yes |  |  |
| `local_coal_output_modifier` | yes |  |  |
| `local_cocoa_output_modifier` | yes |  |  |
| `local_coffee_output_modifier` | yes |  |  |
| `local_copper_output_modifier` | yes |  |  |
| `local_cotton_output_modifier` | yes |  |  |
| `local_dyes_output_modifier` | yes |  |  |
| `local_elephants_output_modifier` | yes |  |  |
| `local_fiber_crops_output_modifier` | yes |  |  |
| `local_fine_cloth_output_modifier` | yes |  |  |
| `local_firearms_output_modifier` | yes |  |  |
| `local_fish_output_modifier` | yes |  |  |
| `local_fruit_output_modifier` | yes |  |  |
| `local_fur_output_modifier` | yes |  |  |
| `local_furniture_output_modifier` | yes |  |  |
| `local_gems_output_modifier` | yes |  |  |
| `local_glass_output_modifier` | yes |  |  |
| `local_goods_gold_output_modifier` | yes |  |  |
| `local_horses_output_modifier` | yes |  |  |
| `local_incense_output_modifier` | yes |  |  |
| `local_iron_output_modifier` | yes |  |  |
| `local_ivory_output_modifier` | yes |  |  |
| `local_jewelry_output_modifier` | yes |  |  |
| `local_lacquerware_output_modifier` | yes |  |  |
| `local_lead_output_modifier` | yes |  |  |
| `local_leather_output_modifier` | yes |  |  |
| `local_legumes_output_modifier` | yes |  |  |
| `local_liquor_output_modifier` | yes |  |  |
| `local_livestock_output_modifier` | yes |  |  |
| `local_lumber_output_modifier` | yes |  |  |
| `local_maize_output_modifier` | yes |  |  |
| `local_marble_output_modifier` | yes |  |  |
| `local_masonry_output_modifier` | yes |  |  |
| `local_medicaments_output_modifier` | yes |  |  |
| `local_mercury_output_modifier` | yes |  |  |
| `local_millet_output_modifier` | yes |  |  |
| `local_naval_supplies_output_modifier` | yes |  |  |
| `local_olives_output_modifier` | yes |  |  |
| `local_paper_output_modifier` | yes |  |  |
| `local_pearls_output_modifier` | yes |  |  |
| `local_pepper_output_modifier` | yes |  |  |
| `local_porcelain_output_modifier` | yes |  |  |
| `local_potato_output_modifier` | yes |  |  |
| `local_pottery_output_modifier` | yes |  |  |
| `local_raw_material_output` | yes |  |  |
| `local_rice_output_modifier` | yes |  |  |
| `local_saffron_output_modifier` | yes |  |  |
| `local_salt_output_modifier` | yes |  |  |
| `local_saltpeter_output_modifier` | yes |  |  |
| `local_sand_output_modifier` | yes |  |  |
| `local_silk_output_modifier` | yes |  |  |
| `local_silver_output_modifier` | yes |  |  |
| `local_slaves_goods_output_modifier` | yes |  |  |
| `local_steel_output_modifier` | yes |  |  |
| `local_stone_output_modifier` | yes |  |  |
| `local_sugar_output_modifier` | yes |  |  |
| `local_tar_output_modifier` | yes |  |  |
| `local_tea_output_modifier` | yes |  |  |
| `local_tin_output_modifier` | yes |  |  |
| `local_tobacco_output_modifier` | yes |  |  |
| `local_tools_output_modifier` | yes |  |  |
| `local_weaponry_output_modifier` | yes |  |  |
| `local_wheat_output_modifier` | yes |  |  |
| `local_wild_game_output_modifier` | yes |  |  |
| `local_wine_output_modifier` | yes |  |  |
| `local_wool_output_modifier` | yes |  |  |

### Goods Establishment Speed

Per-good establishment speed modifiers for production/building establishment.

| Modifier | percent | boolean | color |
|---|---:|---:|---|
| `local_alum_establishment_speed` | yes |  |  |
| `local_amber_establishment_speed` | yes |  |  |
| `local_beer_establishment_speed` | yes |  |  |
| `local_beeswax_establishment_speed` | yes |  |  |
| `local_books_establishment_speed` | yes |  |  |
| `local_building_establishment_speed` | yes |  |  |
| `local_camels_establishment_speed` | yes |  |  |
| `local_cannons_establishment_speed` | yes |  |  |
| `local_chili_establishment_speed` | yes |  |  |
| `local_clay_establishment_speed` | yes |  |  |
| `local_cloth_establishment_speed` | yes |  |  |
| `local_cloves_establishment_speed` | yes |  |  |
| `local_coal_establishment_speed` | yes |  |  |
| `local_cocoa_establishment_speed` | yes |  |  |
| `local_coffee_establishment_speed` | yes |  |  |
| `local_copper_establishment_speed` | yes |  |  |
| `local_cotton_establishment_speed` | yes |  |  |
| `local_dyes_establishment_speed` | yes |  |  |
| `local_elephants_establishment_speed` | yes |  |  |
| `local_fiber_crops_establishment_speed` | yes |  |  |
| `local_fine_cloth_establishment_speed` | yes |  |  |
| `local_firearms_establishment_speed` | yes |  |  |
| `local_fish_establishment_speed` | yes |  |  |
| `local_fruit_establishment_speed` | yes |  |  |
| `local_fur_establishment_speed` | yes |  |  |
| `local_furniture_establishment_speed` | yes |  |  |
| `local_gems_establishment_speed` | yes |  |  |
| `local_glass_establishment_speed` | yes |  |  |
| `local_goods_gold_establishment_speed` | yes |  |  |
| `local_horses_establishment_speed` | yes |  |  |
| `local_incense_establishment_speed` | yes |  |  |
| `local_iron_establishment_speed` | yes |  |  |
| `local_ivory_establishment_speed` | yes |  |  |
| `local_jewelry_establishment_speed` | yes |  |  |
| `local_lacquerware_establishment_speed` | yes |  |  |
| `local_lead_establishment_speed` | yes |  |  |
| `local_leather_establishment_speed` | yes |  |  |
| `local_legumes_establishment_speed` | yes |  |  |
| `local_liquor_establishment_speed` | yes |  |  |
| `local_livestock_establishment_speed` | yes |  |  |
| `local_lumber_establishment_speed` | yes |  |  |
| `local_maize_establishment_speed` | yes |  |  |
| `local_marble_establishment_speed` | yes |  |  |
| `local_masonry_establishment_speed` | yes |  |  |
| `local_medicaments_establishment_speed` | yes |  |  |
| `local_mercury_establishment_speed` | yes |  |  |
| `local_millet_establishment_speed` | yes |  |  |
| `local_naval_supplies_establishment_speed` | yes |  |  |
| `local_olives_establishment_speed` | yes |  |  |
| `local_paper_establishment_speed` | yes |  |  |
| `local_pearls_establishment_speed` | yes |  |  |
| `local_pepper_establishment_speed` | yes |  |  |
| `local_porcelain_establishment_speed` | yes |  |  |
| `local_potato_establishment_speed` | yes |  |  |
| `local_pottery_establishment_speed` | yes |  |  |
| `local_rice_establishment_speed` | yes |  |  |
| `local_saffron_establishment_speed` | yes |  |  |
| `local_salt_establishment_speed` | yes |  |  |
| `local_saltpeter_establishment_speed` | yes |  |  |
| `local_sand_establishment_speed` | yes |  |  |
| `local_silk_establishment_speed` | yes |  |  |
| `local_silver_establishment_speed` | yes |  |  |
| `local_slaves_goods_establishment_speed` | yes |  |  |
| `local_steel_establishment_speed` | yes |  |  |
| `local_stone_establishment_speed` | yes |  |  |
| `local_sugar_establishment_speed` | yes |  |  |
| `local_tar_establishment_speed` | yes |  |  |
| `local_tea_establishment_speed` | yes |  |  |
| `local_tin_establishment_speed` | yes |  |  |
| `local_tobacco_establishment_speed` | yes |  |  |
| `local_tools_establishment_speed` | yes |  |  |
| `local_weaponry_establishment_speed` | yes |  |  |
| `local_wheat_establishment_speed` | yes |  |  |
| `local_wild_game_establishment_speed` | yes |  |  |
| `local_wine_establishment_speed` | yes |  |  |
| `local_wool_establishment_speed` | yes |  |  |

### Estates And Wealth Share

Local estate power, estate unrest, and peasant enfranchisement levers that affect who captures wealth or unrest.

| Modifier | percent | boolean | color |
|---|---:|---:|---|
| `local_burghers_estate_power` | yes |  | bad |
| `local_burghers_estate_unrest` | yes |  | bad |
| `local_clergy_estate_power` | yes |  | bad |
| `local_clergy_estate_unrest` | yes |  | bad |
| `local_cossacks_estate_power` | yes |  | bad |
| `local_cossacks_estate_unrest` | yes |  | bad |
| `local_crown_estate_power` | yes |  |  |
| `local_crown_estate_unrest` | yes |  | bad |
| `local_dhimmi_estate_power` | yes |  | bad |
| `local_dhimmi_estate_unrest` | yes |  | bad |
| `local_nobles_estate_power` | yes |  | bad |
| `local_nobles_estate_unrest` | yes |  | bad |
| `local_peasant_enfranchisment` | yes |  |  |
| `local_peasants_estate_power` | yes |  | bad |
| `local_peasants_estate_unrest` | yes |  | bad |
| `local_tribes_estate_power` | yes |  | bad |
| `local_tribes_estate_unrest` | yes |  | bad |

### Pop Type Composition And Pop Mechanics

Desired pop composition, migration, growth, literacy, food consumption, assimilation/conversion blockers, and general pop lifecycle modifiers.

| Modifier | percent | boolean | color |
|---|---:|---:|---|
| `cap_maximum_population_growth_at_zero` |  | yes | bad |
| `free_capacity_attracts_pops` |  | yes |  |
| `growth_is_primary_culture` |  | yes |  |
| `local_burghers_assimilation_blocked` |  | yes | bad |
| `local_burghers_conversion_blocked` |  | yes | bad |
| `local_burghers_desired_pop` |  |  |  |
| `local_burghers_desired_pop_scaled` | yes |  |  |
| `local_burghers_food_consumption` | yes |  | bad |
| `local_burghers_max_literacy` |  |  |  |
| `local_burghers_migration_allowed` |  | yes |  |
| `local_burghers_pop_growth` | yes |  |  |
| `local_clergy_assimilation_blocked` |  | yes | bad |
| `local_clergy_conversion_blocked` |  | yes | bad |
| `local_clergy_desired_pop` |  |  |  |
| `local_clergy_desired_pop_scaled` | yes |  |  |
| `local_clergy_food_consumption` | yes |  | bad |
| `local_clergy_max_literacy` |  |  |  |
| `local_clergy_migration_allowed` |  | yes |  |
| `local_clergy_pop_growth` | yes |  |  |
| `local_laborers_assimilation_blocked` |  | yes | bad |
| `local_laborers_conversion_blocked` |  | yes | bad |
| `local_laborers_desired_pop` |  |  |  |
| `local_laborers_desired_pop_scaled` | yes |  |  |
| `local_laborers_food_consumption` | yes |  | bad |
| `local_laborers_max_literacy` |  |  |  |
| `local_laborers_migration_allowed` |  | yes |  |
| `local_laborers_pop_growth` | yes |  |  |
| `local_life_expectancy` |  |  |  |
| `local_max_literacy` |  |  |  |
| `local_migration_attraction` |  |  |  |
| `local_migration_speed` |  |  |  |
| `local_migration_speed_modifier` | yes |  |  |
| `local_monthly_literacy` |  |  |  |
| `local_nobles_assimilation_blocked` |  | yes | bad |
| `local_nobles_conversion_blocked` |  | yes | bad |
| `local_nobles_desired_pop` |  |  |  |
| `local_nobles_desired_pop_scaled` | yes |  |  |
| `local_nobles_food_consumption` | yes |  | bad |
| `local_nobles_max_literacy` |  |  |  |
| `local_nobles_migration_allowed` |  | yes |  |
| `local_nobles_pop_growth` | yes |  |  |
| `local_peasants_assimilation_blocked` |  | yes | bad |
| `local_peasants_conversion_blocked` |  | yes | bad |
| `local_peasants_desired_pop` |  |  |  |
| `local_peasants_desired_pop_scaled` | yes |  |  |
| `local_peasants_food_consumption` | yes |  | bad |
| `local_peasants_max_literacy` |  |  |  |
| `local_peasants_migration_allowed` |  | yes |  |
| `local_peasants_pop_growth` | yes |  |  |
| `local_pop_assimilation_speed` |  |  |  |
| `local_pop_assimilation_speed_modifier` | yes |  |  |
| `local_pop_conversion_speed` |  |  |  |
| `local_pop_conversion_speed_modifier` | yes |  |  |
| `local_pop_demand` | yes |  |  |
| `local_pop_demotion_speed` |  |  | bad |
| `local_pop_demotion_speed_modifier` | yes |  | bad |
| `local_pop_food_consumption` | yes |  | bad |
| `local_pop_join_rebel_threshold` | yes |  | bad |
| `local_pop_leave_rebels_threshold` | yes |  | bad |
| `local_pop_promotion_speed` |  |  |  |
| `local_pop_promotion_speed_modifier` | yes |  |  |
| `local_pop_promotion_speed_scaled` |  |  |  |
| `local_population_capacity` |  |  |  |
| `local_population_capacity_modifier` | yes |  |  |
| `local_population_growth` | yes |  |  |
| `local_slaves_assimilation_blocked` |  | yes | bad |
| `local_slaves_conversion_blocked` |  | yes | bad |
| `local_slaves_desired_pop` |  |  |  |
| `local_slaves_desired_pop_scaled` | yes |  |  |
| `local_slaves_food_consumption` | yes |  | bad |
| `local_slaves_max_literacy` |  |  |  |
| `local_slaves_migration_allowed` |  | yes |  |
| `local_slaves_pop_growth` | yes |  |  |
| `local_soldiers_assimilation_blocked` |  | yes | bad |
| `local_soldiers_conversion_blocked` |  | yes | bad |
| `local_soldiers_desired_pop` |  |  |  |
| `local_soldiers_desired_pop_scaled` | yes |  |  |
| `local_soldiers_food_consumption` | yes |  | bad |
| `local_soldiers_max_literacy` |  |  |  |
| `local_soldiers_migration_allowed` |  | yes |  |
| `local_soldiers_pop_growth` | yes |  |  |
| `local_tribesmen_assimilation_blocked` |  | yes | bad |
| `local_tribesmen_conversion_blocked` |  | yes | bad |
| `local_tribesmen_desired_pop` |  |  |  |
| `local_tribesmen_desired_pop_scaled` | yes |  |  |
| `local_tribesmen_food_consumption` | yes |  | bad |
| `local_tribesmen_max_literacy` |  |  |  |
| `local_tribesmen_migration_allowed` |  | yes |  |
| `local_tribesmen_pop_growth` | yes |  |  |
| `local_upper_class_capacity_modifier` | yes |  |  |
| `total_population_capacity_modifier` | yes |  |  |

### RGO, Production, Construction, And Buildings

RGO size/time, production efficiency, construction speed, building levels, building establishment, town rights, and building-owner resource hooks.

| Modifier | percent | boolean | color |
|---|---:|---:|---|
| `building_enslavement_power` |  |  |  |
| `building_missionary_effort` |  |  |  |
| `building_missionary_effort_scaled` | yes |  |  |
| `building_owner_maritime_presence` |  |  |  |
| `building_owner_overlord_maritime_presence` |  |  |  |
| `constructions_stalled` |  | yes |  |
| `free_building_levels` |  |  |  |
| `gold_to_building_owner` |  |  |  |
| `gold_to_building_owner_overlord` |  |  |  |
| `local_build_buildings_efficiency` | yes |  | good |
| `local_build_new_buildings_efficiency` | yes |  | good |
| `local_building_establishment_reduction` |  |  | bad |
| `local_cloth_guild_building_levels` |  |  |  |
| `local_construction_speed` | yes |  |  |
| `local_fine_cloth_guild_building_levels` |  |  |  |
| `local_jewelry_guild_building_levels` |  |  |  |
| `local_market_upgrade_building_levels` |  |  |  |
| `local_marketplace_building_levels` |  |  |  |
| `local_max_rgo_size` |  |  |  |
| `local_max_rgo_size_modifier` | yes |  |  |
| `local_mills_build_buildings_efficiency` | yes |  | good |
| `local_port_build_buildings_efficiency` | yes |  | good |
| `local_possible_town_rights` |  |  |  |
| `local_production_efficiency` | yes |  |  |
| `local_rgo_build_time` | yes |  | bad |
| `manpower_to_building_owner` |  |  |  |
| `sailors_to_building_owner` |  |  |  |
| `scaled_gold_to_building_owner` | yes |  |  |
| `scaled_gold_to_building_owner_overlord` | yes |  |  |

### Control, Proximity, Integration, Development, And Culture

Local control, proximity, integration, development, cultural influence/tradition, institutions, prosperity, devastation recovery.

| Modifier | percent | boolean | color |
|---|---:|---:|---|
| `local_cultural_influence` |  |  |  |
| `local_cultural_tradition` |  |  |  |
| `local_devastation_recovery` | yes |  |  |
| `local_distance_from_capital_speed_propagation` | yes |  |  |
| `local_institution_growth_modifier` | yes |  |  |
| `local_integration_speed` |  |  |  |
| `local_integration_speed_modifier` | yes |  |  |
| `local_max_control` | yes |  |  |
| `local_max_rural_control` | yes |  |  |
| `local_max_urban_control` | yes |  |  |
| `local_monthly_control` | yes |  |  |
| `local_monthly_control_decline` | yes |  | bad |
| `local_monthly_development` |  |  |  |
| `local_monthly_development_modifier` | yes |  |  |
| `local_monthly_prosperity` | yes |  |  |
| `local_monthly_rural_control` | yes |  |  |
| `local_monthly_urban_control` | yes |  |  |
| `local_prosperity_decay` | yes |  | bad |
| `local_proximity_source` |  |  |  |
| `local_proximity_source_modifier` | yes |  |  |
| `promote_institution_chance` |  |  |  |
| `propagating_zone_of_control` |  | yes |  |

### Food And Storage

Food capacity, food production, food decay, food preservation, monthly food, and food stockpile capacity.

| Modifier | percent | boolean | color |
|---|---:|---:|---|
| `hostile_food_multiplier` | yes |  |  |
| `local_food_capacity` |  |  |  |
| `local_food_capacity_modifier` | yes |  |  |
| `local_food_decay` | yes |  | bad |
| `local_food_preservation_efficiency_modifier` | yes |  | good |
| `local_food_production_mult` | yes |  |  |
| `local_monthly_food` |  |  |  |
| `local_monthly_food_modifier` | yes |  |  |
| `maximum_stockpile_capacity` |  |  |  |

### Trade, Market, Maritime, And Harbors

Market access, merchant capacity/power, trade center power, maritime presence, harbors, blockades, and privateer/vision/shipbuilding toggles.

| Modifier | percent | boolean | color |
|---|---:|---:|---|
| `blockade_force_required` |  |  |  |
| `blocks_privateer_raids` |  | yes |  |
| `blocks_vision_from_land` |  | yes |  |
| `blocks_vision_from_sea` |  | yes |  |
| `can_build_ships_in_this_location` |  | yes |  |
| `harbor_suitability` |  |  |  |
| `local_maritime_presence` |  |  |  |
| `local_market_access` | yes |  |  |
| `local_merchant_capacity` |  |  |  |
| `local_merchant_capacity_modifier` | yes |  |  |
| `local_merchant_power` |  |  |  |
| `local_port_cost_distance_impact` | yes |  | bad |
| `local_ship_build_speed` | yes |  |  |
| `local_trade_center_power` | yes |  |  |
| `local_trade_embark_disembark_efficiency` | yes |  | good |
| `local_trade_protection_factor` | yes |  |  |
| `local_trades_per_burgher` | yes |  |  |
| `merchant_capacity_from_building` |  |  |  |
| `merchant_power_from_building` |  |  |  |
| `natural_harbor_suitability` | yes |  |  |
| `owner_gets_vision_when_occupied` |  | yes |  |

### Military, Forts, Movement, Levies, And Supply

Attrition, levies, manpower, sailors, garrisons, forts, movement cost/blocking, recruitment/building permissions, supply, and war score.

| Modifier | percent | boolean | color |
|---|---:|---:|---|
| `can_recruit_regiment_in_this_location` |  | yes |  |
| `fort_level` |  |  |  |
| `friendly_disembark_time_modifier` | yes |  | bad |
| `friendly_movement_cost` | yes |  | bad |
| `hostile_disembark_time_modifier` | yes |  |  |
| `hostile_fleet_attrition` |  |  | bad |
| `hostile_movement_cost` | yes |  | bad |
| `local_army_attrition` |  |  | bad |
| `local_army_levy_size_modifier` | yes |  |  |
| `local_calvinism_movement_growth_modifier` | yes |  |  |
| `local_calvinism_movement_resistance_modifier` | yes |  |  |
| `local_defensive` | yes |  |  |
| `local_forced_attrition` |  |  | bad |
| `local_fort_maintenance_efficiency` | yes |  | good |
| `local_frontage_allowed` |  |  |  |
| `local_garrison_growth` | yes |  |  |
| `local_garrison_size` |  |  |  |
| `local_hostile_attrition` |  |  | bad |
| `local_levy_recruitment_speed_modifier` | yes |  |  |
| `local_levy_size_modifier` | yes |  |  |
| `local_lutheranism_movement_growth_modifier` | yes |  |  |
| `local_lutheranism_movement_resistance_modifier` | yes |  |  |
| `local_manpower` |  |  |  |
| `local_manpower_modifier` | yes |  |  |
| `local_may_build_nahuatl_units` |  | yes |  |
| `local_may_build_north_american_units` |  | yes |  |
| `local_navy_attrition` |  |  | bad |
| `local_navy_levy_size_modifier` | yes |  |  |
| `local_repair_speed` | yes |  |  |
| `local_sailors` |  |  |  |
| `local_sailors_modifier` | yes |  |  |
| `local_supply_limit_modifier` | yes |  |  |
| `local_war_score_efficiency` | yes |  |  |
| `max_attrition` |  |  | bad |
| `max_regiments_trained_at_same_time` |  |  |  |
| `max_siege_memory` |  |  |  |
| `may_build_sofa_units` |  | yes |  |
| `minimum_fort_level` |  |  |  |
| `movement_blocked` |  | yes |  |
| `movement_cost` | yes |  | bad |
| `occupation_time` | yes |  |  |
| `supply_limit` |  |  |  |

### Disease And Religious Movements

Disease growth/impact/resistance plus Reformation movement growth/resistance.

| Modifier | percent | boolean | color |
|---|---:|---:|---|
| `local_bubonic_plague_growth_modifier` | yes |  |  |
| `local_bubonic_plague_impact_modifier` | yes |  | bad |
| `local_bubonic_plague_resistance_modifier` | yes |  |  |
| `local_disease_resistance` | yes |  |  |
| `local_egyptian_plague_growth_modifier` | yes |  |  |
| `local_egyptian_plague_impact_modifier` | yes |  | bad |
| `local_egyptian_plague_resistance_modifier` | yes |  |  |
| `local_influenza_growth_modifier` | yes |  |  |
| `local_influenza_impact_modifier` | yes |  | bad |
| `local_influenza_resistance_modifier` | yes |  |  |
| `local_malaria_growth_modifier` | yes |  |  |
| `local_malaria_impact_modifier` | yes |  | bad |
| `local_malaria_resistance_modifier` | yes |  |  |
| `local_measles_growth_modifier` | yes |  |  |
| `local_measles_impact_modifier` | yes |  | bad |
| `local_measles_resistance_modifier` | yes |  |  |
| `local_smallpox_growth_modifier` | yes |  |  |
| `local_smallpox_impact_modifier` | yes |  | bad |
| `local_smallpox_resistance_modifier` | yes |  |  |
| `local_typhus_growth_modifier` | yes |  |  |
| `local_typhus_impact_modifier` | yes |  | bad |
| `local_typhus_resistance_modifier` | yes |  |  |

### Unrest, Rebels, Piracy, And Separatism

Unrest, separatism, rebel thresholds, slave satisfaction, pirate spawn, and related disorder levers.

| Modifier | percent | boolean | color |
|---|---:|---:|---|
| `local_pirate_spawn_chance` |  |  | bad |
| `local_separatism` | yes |  | bad |
| `local_slave_pop_satisfaction` | yes |  |  |
| `local_unrest` | yes |  | bad |

### Location Rules, Capacities, And Miscellaneous

Remaining location-scoped mechanics: generic capacity, parliament seat, cardinal, stockpile, construction slots, institution chance, etc.

| Modifier | percent | boolean | color |
|---|---:|---:|---|
| `cannot_upgrade_location` |  | yes | bad |
| `gives_cardinal` |  | yes | neutral |
| `has_parliament_seat` |  | yes |  |
| `local_great_pestilence_growth_modifier` | yes |  |  |
| `local_great_pestilence_impact_modifier` | yes |  | bad |
| `local_great_pestilence_resistance_modifier` | yes |  |  |
| `local_heathen_pop_conversion_speed_modifier` | yes |  |  |
| `local_heretic_pop_conversion_speed_modifier` | yes |  |  |
| `local_mercenaries_modifier` | yes |  | neutral |
| `local_road_building_time` | yes |  | bad |
| `local_tribal_promotion` | yes |  |  |
| `max_constructions_at_same_time` |  |  |  |
| `max_ships_built_at_same_time` |  |  |  |
| `road_building_blocked` |  | yes | bad |

## Extraction Notes

- Included: modifier types whose block contains `game_data = { category = location }`.
- The categories are editorial groupings for modding use; they are not engine categories.
- `percent = yes` and `boolean = yes` are copied from vanilla modifier type metadata.
- `color` is copied when present. Blank fields mean vanilla did not set that metadata field.


# Citations

[1] `game/main_menu/common/modifier_type_definitions/00_modifier_types.txt` — parsed for modifier blocks whose `game_data.category = location`.
[2] `game/main_menu/localization/english/modifier_types_l_english.yml` — human-readable modifier naming and descriptions when needed.
[3] `game/main_menu/localization/english/game_concepts_l_english.yml` — estate wealth share, tax base, control, and local economy concepts.
