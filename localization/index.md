# Localization

EU5 localization is strict about encoding and file naming. Most “mod works but text is wrong” bugs trace here.

* [UTF-8 BOM requirement](utf8-bom-requirement.md) — files without BOM may load with zero keys
* [Localization key conventions](localization-key-conventions.md) — prefixes, suffixes, pairing rules
* [Event localization naming](event-localization-naming.md) — one yml per event script file
* [Main menu event localization mirror](main-menu-event-localization-mirror.md) — `main_menu/localization/english/events/` layout (MnT)
* [Advance and mission localization](advance-and-mission-localization.md) — advances, mission trees, tasks
* [Static modifier localization](static-modifier-localization.md) — `STATIC_MODIFIER_NAME_` / `STATIC_MODIFIER_DESC_`
* [Dynamic text in loc](dynamic-text-in-loc.md) — `ROOT.GetVariable`, custom loc, scopes in tooltips
* [Multi-file loc split for UI mods](multi-file-loc-split-for-ui-mods.md) — concern × language split (#17 Glorp)
