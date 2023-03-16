---
description: Missing definitions.
---

# Missing Definitions

These docs are not complete, so naturally there are certain definitions that are missing.

## Object Definitions

Technically, there are no objects in DSS. Objects such as a Button are more like structures in which data is formatted. In a sense, each "object" uses the table "constructor." This has the potential to be pretty confusing for a beginner, so we just simplify it by calling them objects. It works in practice and is descriptive enough.

Here are a list of objects that are not documented:

* Button - how a button is formatted
* SettingsButton (derived from Button) - a button that is used as a setting
* All objects derived by SettingsButton
* Page - a page in your menu
* Palette - provided in an array to the AddPalettes function and houses information about a palette
