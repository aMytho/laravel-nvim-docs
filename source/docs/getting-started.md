---
title: Getting Started
description: Getting started with Laravel NVIM
extends: _layouts.documentation
section: content
---

# Getting Started {#getting-started}

The goal of the plugin is to offer a solution out of the box, just install the plugin, set your keymaps and start using it.

## Install {#getting-started-install}

To install this plugin, add it to your Neovim package manager. 

### Lazy nvim

Here is an example with the [Lazy.nvim](https://github.com/folke/lazy.nvim) package manager.

```lua
{
  "adalessa/laravel.nvim",
  dependencies = {
    "tpope/vim-dotenv",
    "nvim-telescope/telescope.nvim",
    "MunifTanjim/nui.nvim",
    "kevinhwang91/promise-async",
  },
  cmd = { "Laravel" },
  keys = {
    { "<leader>la", ":Laravel artisan<cr>" },
    { "<leader>lr", ":Laravel routes<cr>" },
    { "<leader>lm", ":Laravel related<cr>" },
  },
  event = { "VeryLazy" },
  opts = {},
  config = true,
}
```

Default plugin options:
```lua
opts = {
  lsp_server = "phpactor",
  features = {
    route_info = {
      enable = true,
      view = "top",
    },
    model_info = {
      enable = true,
    },
    override = {
      enable = true,
    },
    pickers = {
      enable = true,
      provider = 'telescope',
    },
  },
  ui = require("laravel.options.ui"),
  commands_options = require("laravel.options.command_options"),
  environments = require("laravel.options.environments"),
  user_commands = require("laravel.options.user_commands"),
  resources = require("laravel.options.resources"),
  providers = {
    require("laravel.providers.laravel_provider"),
    require("laravel.providers.repositories_provider"),
    require("laravel.providers.override_provider"),
    require("laravel.providers.completion_provider"),
    require("laravel.providers.route_info_provider"),
    require("laravel.providers.tinker_provider"),
    require("laravel.providers.telescope_provider"),
    require("laravel.providers.fzf_lua_provider"),
    require("laravel.providers.ui_select_provider"),
    require("laravel.providers.user_command_provider"),
    require("laravel.providers.status_provider"),
    require("laravel.providers.diagnostics_provider"),
    require("laravel.providers.model_info_provider"),
    require("laravel.providers.composer_info_provider"),
    require("laravel.providers.history_provider"),
  },
  user_providers = {}, -- Custom providers, see below
}
```


> Tip: A powerful way of customizing and extending it is setting providers as Laravel does.

---

### Providers {#getting-started-providers}

One of the goals of the plugin is to be a good experience and extensible as laravel is as a php framework.

You can create your own provider as a lua file.
For example, here is the main provider for the plugin: 
```lua
---@class LaravelProvider
local laravel_provider = {}

---@param app LaravelApp
function laravel_provider:register(app)
  app:bindIf("api", "laravel.api")
  app:bindIf("tinker", "laravel.tinker")
  app:bindIf("templates", "laravel.templates")

  -- SERVICES
  app:bindIf("artisan", "laravel.services.artisan")
  app:bindIf("class", "laravel.services.class")
  app:bindIf("composer", "laravel.services.composer")
  app:bindIf("php", "laravel.services.php")
  app:bindIf("runner", "laravel.services.runner")
  app:bindIf("ui_handler", "laravel.services.ui_handler")
  app:bindIf("view_finder", "laravel.services.view_finder")
  app:bindIf("views", "laravel.services.views")

  app:singeltonIf("cache", "laravel.services.cache")
  app:singeltonIf("env", "laravel.services.environment")
end

---@param app LaravelApp
function laravel_provider:boot(app)
  app("env"):boot()

  require("laravel.treesitter_queries")

  local group = vim.api.nvim_create_augroup("laravel", {})

  vim.api.nvim_create_autocmd({ "DirChanged" }, {
    group = group,
    callback = function()
      app("env"):boot()
    end,
  })
end

return laravel_provider
```

Simply add them to the `user_providers` config and they will be loaded.

register as laravel it's a place where you can define elements for the container system, even you
can override current implementation given that the ones defined by the plugin are only conditional.

---

### App {#getting-started-app}

Inspire by Laravel the plugin has an `app` helper that allows you to quickly use it in keymaps, plugins, etc.
Having access and control of it at your disposal.

To access it, just require laravel and take `app` property. This is the recomended way of using it.
```lua
local app = require("laravel").app

-- get all commands, print the total amount
app("commands_repository"):all():thenCall(function(commands)
  vim.print(#commands)
end)
```

### Promises {#getting-started-promises}

Something to note from the previous example is the approach that I took using promises.
This is due to the fact that the calls needed to be async.
Promises allow for more straight-forward expression. More can be found [here](https://github.com/kevinhwang91/promise-async)

This may take some time to grasp, but it is a proven approach.
Examples of it can be found are all around the plugin code.
