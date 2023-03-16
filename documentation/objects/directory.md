---
description: A Directory holds your mod's menu.
---

# Directory

A **Directory** is unique to other objects in that it doesn't have any properties or methods. Instead, it is a dictionary where the index is an arbitrary name and the value is a Page.

```lua
local directory = {
    main = {
        -- code
    },
    settings = {
        -- more code
    },
}
```
