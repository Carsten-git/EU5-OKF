---
type: Playbook
title: CMF action bar and custom alerts
description: Register shared action-bar buttons and dismissable alert-bar notifications with cmf_on_callback.
tags: [cmf, action-bar, alerts, gui]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: community_mod_framework
---

# Action bar

Shared bar; players can reposition and toggle buttons via CMF’s own CMM entry.

```txt
cmf_add_action_bar_element = { element = cmf_action_bar_element_example }
cmf_remove_action_bar_element = { element = cmf_action_bar_element_example }
```

Loc keys: `<root>_color`, `_icon`, `_name`, `_tooltip` (and root key itself). Colors like `gold`; icons like `@advance!`.

Hide entire bar from GUI: `GetVariableSystem.Set('cmf_hide_action_bar', 'yes')` / `Clear(...)`.

Optional Scripted GUI: `is_shown` / `is_valid` after `cmf_register_scripted_gui`.

# Custom alerts

Country-scope effects:

```txt
cmf_show_alert = { alert = cmf_alert_example }
cmf_remove_alert = { alert = cmf_alert_example }
# trigger:
cmf_is_alert_active = { alert = cmf_alert_example }
```

Loc: `<root>_color` (`blue|orange|red|red_war|black|yellow|green|purple`), `_icon`, `_name`, `_tooltip`.

Left-click runs callback and removes the alert; right-click dismisses.

# Clicks

Both use `cmf_on_callback` with `var:cmf_callback` = element/alert flag. Same pattern as CMM setting changes.

# Related

* [Registration hooks](/community-mod-framework/registration-and-on-action-hooks.md)
* [Community Mod Menu](/community-mod-framework/community-mod-menu.md)

# Citations

[1] https://raw.githubusercontent.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/main/docs/wiki/cmf.wiki — Action Bar, Custom Alerts
[2] Example mod scripted GUIs under `submods/cmf-example-mod`
