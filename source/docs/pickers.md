---
title: Pickers
description: The best way to interact with laravel
extends: _layouts.documentation
section: content
---

# Pickers {#pickers}

A picker is a core part of neovim configuration. This plugin makes heavy use of them to search through commands, resources, routes, and more.

To use a picker with Laravel Nvim, install one of the below providers.

## Supported Providers
- [Telescope](https://github.com/nvim-telescope/telescope.nvim)
- [Snacks](https://github.com/folke/snacks.nvim)
- [FzfLua](https://github.com/ibhagwan/fzf-lua)
- [Select](https://github.com/nvim-telescope/telescope-ui-select.nvim)

## Available Pickers
- artisan
- commands
- composer
- history
- make
- related
- resources
- routes

## Config
To enable the picker, set the picker to enabled and choose one of the below options.
```lua
opts = {
    features = {
        pickers = {
            enable = true,
            provider = "telescope|snacks|fzf-lua|ui.select", -- Choose ONE of these.
        }
    },
}
```

To call the pickers can use the normal commands `Laravel artisan`
The commands will use the config provider when no sub command is provided.
