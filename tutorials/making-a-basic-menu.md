---
description: Learn how to make a basic menu with DSS.
---

# Making a basic menu

## Intro

We're going to make a simple menu with an options page featuring an on/off switch, as well as a page where you can heal every player at the click of a button. We'll be storing data using the [`:SaveData()`](https://wofsauge.github.io/IsaacDocs/rep/ModReference.html#savedata) method of our [`ModReference`](https://wofsauge.github.io/IsaacDocs/rep/ModReference.html).

## Setting up your mod

Before we can use DSS, we first need to register our mod in our `main.lua` file. While we're at it, let's define a couple functions that'll handle saving and loading our mod's data.

DSS expects us to save the menu-related data ourselves. If your mod has its own save manager in it, them use those! Otherwise, you can use the code below.

<details>

<summary>Setup code</summary>

```lua
local mod = RegisterMod("Awesome Mod", 1)
local json = require("json")

local saveDataMod = RegisterMod("Awesome Mod Save Data", 1)
saveDataMod.MenuSaveData = nil -- The variable that will hold the save data that DSS uses

function mod.LoadSaveData()
    if not saveDataMod.MenuSaveData then -- If we have no save data loaded
        if mod:HasData() then
            saveDataMod.MenuSaveData = json.decode(mod:LoadData())
        else
            saveDataMod.MenuSaveData = {}
        end
    end

    return saveDataMod.MenuSaveData
end

function mod.StoreSaveData()
    mod:SaveData(json.encode(saveDataMod.MenuSaveData))
end
```

</details>

## Setting up DSS

As the developer, you get to choose how your save data is loaded and saved. This means that you need to define the functions DSS uses to save and load its own data.\
\
Let's define a table that'll contain all of the aforementioned functions. We'll store this table in a variable named `menuProvider`. Make sure to customize these functions to suit your mod's save data structure.

<details>

<summary>Pre-initialization code</summary>

```lua
-- This variable and all functions contained within it are required for DSS to run.
local menuProvider = {}

function menuProvider.SaveSaveData()
    mod.StoreSaveData()
end

function menuProvider.GetPaletteSetting()
    return mod.LoadSaveData().MenuPalette
end

function menuProvider.SavePaletteSetting(var)
    mod.LoadSaveData().MenuPalette = var
end

function menuProvider.GetHudOffsetSetting()
    if not REPENTANCE then
        return mod.LoadSaveData().HudOffset
    else
        return Options.HUDOffset * 10
    end
end

function menuProvider.SaveHudOffsetSetting(var)
    if not REPENTANCE then
        mod.LoadSaveData().HudOffset = var
    end
end

function menuProvider.GetGamepadToggleSetting()
    return mod.LoadSaveData().GamepadToggle
end

function menuProvider.SaveGamepadToggleSetting(var)
    mod.LoadSaveData().GamepadToggle = var
end

function menuProvider.GetMenuKeybindSetting()
    return mod.LoadSaveData().MenuKeybind
end

function menuProvider.SaveMenuKeybindSetting(var)
    mod.LoadSaveData().MenuKeybind = var
end

function menuProvider.GetMenuHintSetting()
    return mod.LoadSaveData().MenuHint
end

function menuProvider.SaveMenuHintSetting(var)
    mod.LoadSaveData().MenuHint = var
end

function menuProvider.GetMenuBuzzerSetting()
    return mod.LoadSaveData().MenuBuzzer
end

function menuProvider.SaveMenuBuzzerSetting(var)
    mod.LoadSaveData().MenuBuzzer = var
end

function menuProvider.GetMenusNotified()
    return mod.LoadSaveData().MenusNotified
end

function menuProvider.SaveMenusNotified(var)
    mod.LoadSaveData().MenusNotified = var
end

function menuProvider.GetMenusPoppedUp()
    return mod.LoadSaveData().MenusPoppedUp
end

function menuProvider.SaveMenusPoppedUp(var)
    mod.LoadSaveData().MenusPoppedUp = var
end
```

</details>

It's time to initialize DSS. We'll include `dssmenucore` and use the function it returns to initialize DSS.

<details>

<summary>Initialization code</summary>

```lua
-- Make sure you put the correct path to dssmenucore
local DSSInitializerFunction = include("dssmenucore")

-- The standard format for this variable is "Dead Sea Scrolls (Mod Name)"
local dssModName = "Dead Sea Scrolls (Awesome Mod)"

-- The mod with the highest version set will have their copy of DSS used
-- Don't change this unless you know what you're doing
local dssCoreVersion = 6

-- This function returns a table with some useful functions for DSS
local dssMod = DSSInitializerFunction(dssModName, dssCoreVersion, menuProvider)
```

</details>

## Creating your menu's main page

Creating a menu is a simple and intuitive process. Your entire menu will be defined within a main "directory," which is just a table. Let's create our directory, then make the main page that will appear when your menu is opened.

<details>

<summary>Main page code</summary>

```lua
local directory = {}

-- Index the directory with the name of the page you're creating
-- The name of your pages are arbitrary, but make them something descriptive
directory.main = {

    -- ALL STRING SHOULD BE IN LOWERCASE!
    -- The title is what the user will see at the top of the page
    title = "awesome mod",

    -- buttons defines every button in our menu
    -- Make sure you define them in order of how you want them to appear
    buttons = {
        {
            -- The str tag defines the text our button will show
            str = "resume game",

            -- The action tag refers to a pre-defined function
            -- This function will run when the button is pressed
            -- "resume" closes the menu and resumes the game
            -- Check out the documentation on the action tag for more information
            action = "resume"
        },
        {
            str = "heal isaac",

            -- The dest tag let's a button take the user to another page in the menu
            -- Later we're going to create a page named "heal" where you can heal all players
            dest = "heal"
        },
        {
            str = "settings",

            -- Later we're going to create a page named "settings"
            dest = "settings"
        },

        -- There are a few default buttons provided in the dssMod table
        -- These buttons will handle generic menu features, like changelogs
        -- They'll only be visible in your menu if it is the only one active
        -- Otherwise, they'll appear in the outermost DSS menu
        -- This one leads to the changelogs menu, which contains changelogs defined by all mods
        dssMod.changelogsButton,
    },

    -- A tooltip can be set on either an item or a button
    -- It will display in the corner of the menu if the button with it is selected
    -- For items, it'll display only if there are no buttons with a tooltip
    -- This default tooltip tells the user how to navigate the menu
    tooltip = dssMod.menuOpenToolTip
}
```

</details>

## Creating a page to heal all players

We're going to define another page in our menu just like before. This page will have only two buttons: one button that will take you to the previous menu, and one that will play a sound effect and heal all players when pressed.

<details>

<summary>Healing page code</summary>

```lua
directory.heal = {
    title = "heal isaac",

    buttons = {
        {
            str = "heal",

            -- func is a callback that will run when our button is pressed
            -- It provides a bunch of arguments, but we don't need them for our purposes
            -- For more information, check out the documentation on it
            func = function()
                SFXManager():Play(SoundEffect.SOUND_VAMP_GULP)

                for i = 0, Game():GetNumPlayers() - 1 do
                    local player = Isaac.GetPlayer(i)
                    local maxHearts = player:GetEffectiveMaxHearts()

                    player:AddHearts(maxHearts)
                end
            end,

            -- A tooltip is completely configurable, just like the main page 
            -- strset is a list of strings where each string will be on its own line
            tooltip = {strset = {"heal all of", "your friends!"}}
        },
        {
            -- We can have an empty line by simply making an invisible button
            str = "",

            -- nosel makes it so the button cannot be selected
            nosel = true
        },
        {
            str = "close menu",

            func = function (button, page, item)
                dssMod.closeMenu(item, true)
            end
        },
        {
            str = "",

            nosel = true
        },
        {
            str = "back",

            -- You can set the font size of buttons by using the fsize tag
            -- 3 is the biggest, 1 is the smallest
            fsize = 2,

            action = "back"
        }
    }
}
```

</details>

## Creating a settings page

The final page of our menu will be the settings page. There will only be one setting, which can either be on or off.\
\
To achieve this, we're going to make a settings button. There are many different types of settings buttons. For our purposes, we want to make a multiple choice button that lets us choose either "on" or "off."

<details>

<summary>Settings page code</summary>

```lua
directory.settings = {
    title = "settings",

    buttons = {
        {
            str = "arbitrary switch",

            -- choices is a list of strings that are the different choices for your button
            choices = {"on", "off"},

            -- setting is the index of the choice the player has selected
            -- We set it to 1 so that the default option is "on"
            -- This tag will update depending on what choice the player has selected
            setting = 1,

            -- variable is what DSS uses to store the state of the button
            -- Set it to any arbitrary string, just make sure that it is unique
            variable = "arbitraryChoiceOption",

            -- When the menu is opened, "load" will be called on all settings-buttons
            -- This function should return what the button's current setting should be
            -- This generall means loading whatever data you have stored for the setting
            load = function ()
                -- If we have no data, it'll return 1 because of the "or"
                return mod.GetDssData().SwitchState or 1
            end,

            -- When the menu is closed, "store" will be called on all settings-buttons
            -- This function should save the button's current setting
            -- The button's current setting is passed as the first argument
            store = function (var)
                mod.GetDssData().SwitchState = var
            end,

            tooltip = {strset = {"which do you", "prefer?"}}
        },
        
        -- These are the settings found on the outermost menu of DSS
        -- They'll only be visible in your menu if it is the only one active
        -- You should put them somewhere in your menu
        dssmod.gamepadToggleButton,
        dssmod.menuKeybindButton,
        dssmod.paletteButton,
        dssmod.menuHintButton,
        dssmod.menuBuzzerButton,
    }
}
```

</details>

## Implementing our menu

We're almost done! The last thing we need to do is to actually add our menu to DSS.\
\
First, we must make a directory key. This is a table used by DSS and tells it about our directory.

<details>

<summary>Directory key code</summary>

```lua
local directoryKey = {
    Item = directory.main, -- This is the initial item of the menu, generally you want to set it to your main item
    Main = 'main', -- The main item of the menu is the item that gets opened first when opening your mod's menu.

    -- These are default state variables for the menu; they're important to have in here, but you don't need to change them at all.
    Idle = false,
    MaskAlpha = 1,
    Settings = {},
    SettingsChanged = false,
    Path = {},
}
```

</details>

Finally, let's add our menu using the `AddMenu` function of the `DeadSeaScrollsMenu` global.

<details>

<summary>Add menu code</summary>

```lua
DeadSeaScrollsMenu.AddMenu("Awesome Mod", {
    -- The Run, Close, and Open functions define the core loop of your menu
    -- Once your menu is opened, all the work is shifted off to your mod running these function
    -- This allows each mod to have its own independently functioning menu.

    -- The DSSInitializerFunction returns a table with defaults defined for each function
    -- These default functions are good enough for most mods
    -- If you do want a completely custom menu, making own functions is the way to do it
    
    -- This function runs every render frame while your menu is open
    -- It handles everything
    Run = dssMod.runMenu,

    -- This function runs when the menu is opened
    -- Generally it initializes the menu
    Open = dssMod.openMenu,

    -- This function runs when the menu is closed
    -- Generally it handles the storing of save data and general shutdown logic.
    Close = dssMod.closeMenu,

    -- This will hide your mod behind an "other mods" button if enabled
    -- It only activates if other mods with DSS are enabled
    -- It's a good idea to enable this if you don't expect players to use your menu often
    UseSubMenu = false,

    Directory = directory,
    DirectoryKey = directoryKey
})
```

</details>

## Conclusion

With all of this, you now have your first menu up and running.

<figure><img src="../.gitbook/assets/ezgif.com-gif-maker (6).gif" alt="Going through the various buttons and pages that we made and showing them functioning correctly"><figcaption><p>Hooray!</p></figcaption></figure>

You can download the example mod and inspect how it works below. It includes the code found in the "[Adding changelogs](adding-changelogs.md)" tutorial and the "[Adding menu palettes](adding-menu-palettes.md)" tutorial.

{% embed url="https://mega.nz/file/ROIGEQSB#TjkQGqV3CT06yQfXe1ZLX52c9gawLe3dOdpkQ4e6g_M" %}
Awesome Mod download.
{% endembed %}
