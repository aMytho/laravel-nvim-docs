---
title: Route Info
description: Information where you need it
extends: _layouts.documentation
section: content
---

# Route Info {#route-info}

Laravel Nvim can display the route info for a controller right in your editor!
This allows you to see the parameters, middleware, and route information above the corresponding controller method.

The plugin uses `virtual_text` to display it directly in the editor.

![route_info_top](/assets/img/route_info_top.png)

This is the default style, but it can be changed to be on the right side by changing your config:

```lua
{
  features = {
    route_info = {
      enable = true,
      view = "right",
    },
  },
}
```

![route_info_right](/assets/img/route_info_right.png)

If you are using a really wide monitor, I recommend putting it on the right-hand side. It is a more efficient use of space.

You can also disable it entirely.
```lua
{
  features = {
    route_info = {
      enable = false,
      view = "right",
    },
  },
}
```


# Customization {#route-info-customization}

As we know customization is the sauce of neovim, you will for sure want to have different styles, different
colors.
The plugin will allow you to simply set what ever you prefer using the container system.

To modify it, you change your configuration after laravel has been loaded or add a `user_provider` in the
configuration (recommended), but all the options are at your use.

```lua
local app = require("laravel").app

local route_info_view = {}

function route_info_view:get(route)
      return {
        virt_text = {
          { "[",                                                "comment" },
          { "Method: ",                                         "comment" },
          { table.concat(route.methods, "|"),                   "@enum" },
          { " Uri: ",                                           "comment" },
          { route.uri,                                          "@enum" },
          { "]",                                                "comment" },
        },
      }
end

app:instance("route_info_view", route_info_view)
```

After this code is executed, the plugin will use this new definition of the route_info_view, no need to reboot.
This makes testing faster, as you just need to focus the buffer again to trigger the autocommand.
This verions looks like

![route_info_custom](/assets/img/route_info_custom.png)


The plugin gets the route information from the artisan command `route:list --json`. You can explore it to see what variables
are available.

# Missing Method {#route-info-missing-method}

Laravel Nvim will automatically detect routes that point to a controller method that doesn't exist. This is helpful for finding typo's in route names and missing implementations.
This info will be communicated via a Vim diagnostic.

![route_info_missing_route](/assets/img/route_info_missing_route.png)
