---
description: Global methods obtained by initializing DSS.
---

# Global Methods

Global methods are functions found under the `ModReference` returned when initializing DSS. To initialize DSS, call the function returned by using `include` on DSS's core file (named `dssmenucore.lua` by default).

<details>

<summary>Initialize DSS</summary>

{% code lineNumbers="true" %}
```lua
-- Make sure you put the correct path to dssmenucore
local DSSInitializerFunction = include("dssmenucore")

-- The standard format for this variable is "Dead Sea Scrolls (Mod Name)"
local dssModName = "Dead Sea Scrolls (Awesome Mod)"

-- The mod with the highest version set will have their copy of DSS used
-- Don't change this unless you know what you're doing
local dssCoreVersion = 7

-- This function returns a table with some useful functions for DSS
local dssMod = DSSInitializerFunction(dssModName, dssCoreVersion, menuProvider)
```
{% endcode %}

</details>

Some global methods are in PascalCase while some are in camelCase. There is no logic behind it, so make sure you're spelling everything correctly.

Some methods are only meant to be used internally by the `dssmenucore.lua` file. These have been omitted as they have no use outside of this context.

| Return value | Function                                                                                                       |
| ------------ | -------------------------------------------------------------------------------------------------------------- |
| `void`       | [reloadButton](global-methods.md#reloadbutton)(`SettingsButton` button, `Page` item, `Menu` tbl)               |
| `void`       | [reloadButtons](global-methods.md#reloadbuttons)(`Directory` tbl, `Page?` item)                                |
| `void`       | [openMenu](global-methods.md#openmenu)(`Directory` tbl, `boolean` openedFromNothing = false)                   |
| `void`       | [closeMenu](global-methods.md#closemenu)(`Menu` tbl, `boolean` fullClose = false, `boolean` noAnimate = false) |
| `void`       | [back](global-methods.md#back)(`Directory` tbl)                                                                |
| `void`       | [AddChangelog](global-methods.md#addchangelog)(`string...`, `string` log)                                      |
| `void`       | [ResetBuzzerCheck](global-methods.md#resetbuzzercheck)()                                                       |

## Functions

### reloadButton()

`void reloadButton(SettingsButton button, Page item, Menu tbl)`

Runs the `load` callback of a `SettingsButton`.

### reloadButtons()

`void reloadButtons(Directory tbl, Page? item)`

If the `item` argument is provided, this will run the `reloadButton()` method on all `SettingsButton` in `item`. Otherwise, it'll run `reloadButton()` on every `SettingsButton` in `tbl`.

### openMenu()

`void openMenu(Directory tbl, boolean openedFromNothing = false)`

Opens `tbl`. If `openedFromNothing` is true, there will be no opening animation.

### closeMenu()

`void closeMenu(Directory tbl, boolean fullClose = false, boolean noAnimate = false)`

Closes `tbl` and opens the main page of `tbl`. If `fullClose` is true **AND** `noAnimate` is false, any panels will not be adjusted.

### back()

`void back(Menu tbl)`

Takes the player to the previous menu. This can also be called via the `action` property of a `Button`.

### AddChangelog()

`void AddChangelog(string..., string log)`

Adds a changelog to appear under the Changelogs menu, along with other mods that have added changelogs. For more information, [click here](../tutorials/adding-changelogs.md).

### ResetBuzzerCheck()

`void ResetBuzzerCheck()`

Resets the variable that tracks if the buzzer has been played yet in the room. The buzzer is a sound effect that plays when the player tries to open the menu while in a room that isn't "safe." More information can be [found here](../tutorials/gotchas.md#marking-an-entity-as-dss-safe).
