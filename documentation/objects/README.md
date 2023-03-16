---
description: DSS' various objects.
---

# Objects

Technically, there are no objects in DSS. Objects such as a Button are more like structures in which data is formatted. In a sense, each "object" uses the table "constructor." If you're familiar with TypeScript, it's like using an interface.

This is fairly confusing to explain, so we just call the different table structures objects. It's close enough to the truth and conveys the message adequately.

Below are the different "objects."

* [Directory](directory.md) - the table defining the pages of your menu
* [DirectoryKey](directorykey.md) - metadata about your menu
* Page - a page of your menu
* Button - a button in a page
  * SettingsButton - a type of Button made for settings (does nothing on its own)
    * SettingsButtonKeybind - a SettingsButton for binding keys
    * SettingsButtonChoice - a multiple choice SettingsButton
    * SettingsButtonNumber - choose between a range of numbers
    * SettingsButtonSlider - like a SettingsButtonNumber except that it uses a slider
* Palette - a palette that changes how DSS looks

[See here](../../meta/missing-definitions.md) for a list of objects that still need documentation.
