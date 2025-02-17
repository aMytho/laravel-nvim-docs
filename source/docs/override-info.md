---
title: Override Info
description: Information where you need it
extends: _layouts.documentation
section: content
---

# Override Info {#override-info}

Laravel Nvim can indicate when a method is overriding a base class.

> This was requested by a member of our community. If you have a feature request, [open an issue](https://github.com/adalessa/laravel.nvim)!

![override](/assets/img/override_info.png)

The symbol for an override can be customized. By default, it used the Neovim override symbol.

```lua
vim.fn.sign_define("LaravelOverride", { text = "@", texthl = "String" })

```

Feel free to change the text and the highlight to match your theme and style.

# Configuration {#override-info-configuration}

You can disable this feature by changing the configuration:
```lua
{
  features = {
    override = {
      enable = true,
    },
  },
}
```
