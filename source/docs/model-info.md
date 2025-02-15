---
title: Model Info
description: Information where you need it
extends: _layouts.documentation
section: content
---

# Model Info {#model-info}

Laravel Nvim can display the columns of a model with virtual text. This saves you the hassle of looking at a migration file or opening the database to see what fields exist for a DB entity. The plugin also lists constraints, the database used, and the table name.

![model_info](/assets/img/model_info.png)

# Configuration {#model-info-configuration}

You can disable it by changing the configuration:
```lua
{
  features = {
    model_info = {
      enable = true,
    },
  },
}
```

# Customization {#model-info-customization}

Similar to the route info, the model info can be customized.

```lua
local app = require("laravel").app

local model_info_view = {}

function model_info_view:get(model)
  local virt_lines = {
    { { "[", "comment" } },
    { { " Database: ", "comment" },  { model.database, "@enum" } },
    { { " Table: ", "comment" },     { model.table, "@enum" } },
    { { " Attributes: ", "comment" } },
  }

  for _, attribute in ipairs(model.attributes) do
    table.insert(virt_lines, {
      { "   " .. attribute.name,                                                     "@enum" },
      { " " .. (attribute.type or "null") .. (attribute.nullable and "|null" or ""), "comment" },
      attribute.cast and { " -> " .. attribute.cast, "@enum" } or nil,
    })
  end

  table.insert(virt_lines, { { "]", "comment" } })

  return {
    virt_lines = virt_lines,
    virt_lines_above = true,
  }
end

app:instance("model_info_view", model_info_view)
```

The plugin gets model information from the artisan command `model:show --json`.
You can explore it to see what variables are at your disposal.
