---
description: Learn how to add new color palettes to DSS.
---

# Adding menu palettes

## Intro

Color palettes are basically different themes for DSS. The user can choose between a healthy variety of color palettes in the Dead Sea Scrolls menu settings, but developers can add their own color palettes very easily.

## Adding a color palette

You can add color palettes using the `AddPalettes` function of the `DeadSeaScrollsMenu` global. `AddPalettes` takes a table of color palettes. Below is an example of how to define a color palette.

```lua
DeadSeaScrollsMenu.AddPalettes({
    {
        -- the name of your palette
        Name = "lavender",

        -- Below you will define the colors your palette will use
        -- They are defined as a list of 3 values of red, green, and blue
        -- Try using Google's RGB color picker to find some good colors

        -- What color is the paper?
        {128, 98, 189},

        -- What color is normal text?
        {20, 16, 43},

        -- What color is highlighted text?
        {28, 15, 24}
    }
})
```

<figure><img src="../.gitbook/assets/image (6).png" alt="A purple palette named Lavender is displayed"><figcaption><p>Graphic design is hard.</p></figcaption></figure>

You can download the example mod and inspect how it works below.

{% embed url="https://mega.nz/file/ROIGEQSB#TjkQGqV3CT06yQfXe1ZLX52c9gawLe3dOdpkQ4e6g_M" %}
