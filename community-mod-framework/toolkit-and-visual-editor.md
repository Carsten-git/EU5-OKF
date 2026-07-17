---
type: Reference
title: CMF toolkit and CMM visual editor
description: Community Mod Toolkit — CMM Visual Editor, workshop uploader, and translation tools.
tags: [cmf, cmt, tooling, visual-editor]
timestamp: 2026-07-11T12:00:00+10:00
status: complete
source_mod: community_mod_framework
---

The **Community Mod Toolkit (CMT)** companions CMF with authoring automation.

# CMM Visual Editor

Browser-based designer that generates effects, scripted GUIs, localization, and on_action for CMM — no hand-scripting required. Supports all setting types, live preview, import of existing mods.

Launch (Windows PowerShell one-liner from CMF docs):

```powershell
curl.exe -sL https://raw.githubusercontent.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/main/tools/cmm-visual-editor.bat -o "$env:TEMP\cmm-visual-editor.bat"; & "$env:TEMP\cmm-visual-editor.bat"
```

Repo tools live under `tools/cmm-visual-editor/` in the CMF GitHub repo.

# Other CMT pieces

| Tool | Role |
|------|------|
| Upload tool | Steam Workshop deploy with submod support |
| Translation tool | Loc via DeepL or Gemini for EU5 languages |
| Mod template | Structured starter layout |

Companion repo: [community-mod-toolkit](https://github.com/Europa-Universalis-5-Modding-Co-op/community-mod-toolkit).

# Related

* [Community Mod Menu](/community-mod-framework/community-mod-menu.md)
* [Total conversion toolchain](/tooling/total-conversion-toolchain.md) — MnT-style balance tools (different purpose)

# Citations

[1] https://raw.githubusercontent.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/main/docs/wiki/cmm.wiki — CMM Visual Editor
[2] https://github.com/Europa-Universalis-5-Modding-Co-op/community-mod-framework/wiki/Community-Mod-Toolkit
[3] https://github.com/Europa-Universalis-5-Modding-Co-op/community-mod-toolkit
