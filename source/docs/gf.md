---
title: GF
description: gf for laravel
extends: _layouts.documentation
section: content
---

# GF {#gf}

gf is one of the most common keymaps for vim, with it you can go to the file under the cursor.
This plugin can use it to go to Laravel views, configs, envs, and routes.

You only need to add a keymap to your config

```lua
{
    "gf",
    function()
        if require("laravel").app("gf").cursor_on_resource() then
            return "<cmd>Laravel gf<CR>"
        else
            return "gf"
        end
    end,
    noremap = false,
    expr = true,
}
```
