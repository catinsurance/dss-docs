---
description: Metadata about your directory.
---

# DirectoryKey

A **DirectoryKey** houses metadata about your [`Directory`](directory.md). It is used by DSS to manage your menu.

It's unique in that **you** **must** **provide every property**, but only `Item` and `Main` need to be customized.

| Type     | Property                         |
| -------- | -------------------------------- |
| property | [`Directory`](directory.md) Item |
| property | `string` Main                    |
| property | `false` Idle                     |
| property | `1` MaskAlpha                    |
| property | `{}` Settings                    |
| property | `false` SettingsChanged          |
| property | `{}` Path                        |

## Required Properties

### MaskAlpha

Set this to 1.

### Settings

Set this to an empty table (`{}`).

### SettingsChanged

Set this to `false`.

### Path

Set this to an empty table (`{}`).

## Properties

### Item

[`Directory`](directory.md)

A reference to your [`Directory`](directory.md).

### Main

`string`

The index of your menu's main `Page`.
