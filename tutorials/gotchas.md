---
description: This page lists many "gotchas" or common mistakes that may throw you off.
---

# Gotchas

## Editing `dssmenucore.lua`

Most of the time you'll never need to edit the core file of DSS. The callbacks and various global methods are more than enough for the average mod. Most custom behavior can be achieved with the `generate` and `postrender` callbacks.

You _can_ edit `dssmenucore.lua`, but there are some caveats.

### You cannot-

* You cannot change the `DeadSeaScrollsMenu` global (also referred to as `dssmenu` in the code).
* You cannot change anything within the `InitializeMenuCore()` function.
* You cannot change anything that runs for the core specifically.

Everything else is free to change as you like.

## Strings must be in lowercase

All strings to be displayed by DSS must be provided in lowercase, as the font used by DSS doesn't support UPPERCASE.

You can easily change the case of strings by using the methods below.

```lua
local foo = "Hello, world!"

local lowercase = string.lower(foo) -- The same as `foo:lower()`
local uppercase = string.upper(foo) -- The same as `foo:upper()`

print(lowercase) -- hello, world!
print(uppercase) -- HELLO, WORLD!
```

## Marking an entity as "DSS-safe"

By default, the DSS menu isn't able to be opened in rooms with active enemies. Most of the time this is desirable so that the player doesn't accidentally open the menu while in combat. Though sometimes, you may have an NPC in your mod that shouldn't be considered an enemy.

You can mark an entity as "safe" by setting `DSSMenuSafe` to true in the entity's data returned by `:GetData()`.

```lua
-- `npc` should be the reference to the EntityNPC you want to be marked as safe.
npc:GetData().DSSMenuSafe = true
```
