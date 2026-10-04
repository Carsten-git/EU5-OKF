# Tooling

Dev pipelines for total conversions: encoding checks, codegen, telemetry, balance spreadsheets.

* [Total conversion toolchain](total-conversion-toolchain.md) — BOM finder, `$GOOD$` codegen, `error_log` telemetry, shared config
* [GitHub CI mod hygiene](github-ci-mod-hygiene.md) — PR BOM, LF, changelog checks (MnT)
* [Steam Workshop BBCode changelog](steam-workshop-bbcode-changelog.md) — `STEAM_UPDATE_*.bbcode`, plain fallback
* [Error log cleaner and rotation](error-log-cleaner-rotation.md) — dedupe, `!! NEW !!`, rotation
* [Script logging and telemetry](../validation/script-logging-and-telemetry.md) — binding matrix, script vs UI prices
* [Script telemetry via hidden events](../validation/script-telemetry-via-hidden-events.md) — hidden-event recipe
* [Data-binding macros](data-binding-macros.md) — macros for `[bracket]` bindings in telemetry scripted effects
* [Trait filter codegen](trait-filter-codegen.md) — generate character filters from vanilla traits (#11 Glorp)
