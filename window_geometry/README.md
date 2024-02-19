# Tutorial: Window Geometry

## Introduction

**Window geometry** refers to measurable physical properties of a window. In the context of this tutorial, those properties are:

* **Window X**, the distance from the left side of the screen to the left side of the window.
* **Window Y**, the distance from the top side of the screen to the top side of the window.
* **Window Width**, the amount of pixels from the left side of the window to the right side.
* **Window Height**, the amount of pixels from the bottom side of the window to the top side.
* **View Width**, the amount of pixels from the left side of the window to the right side, excluding window decorations.
* **View Height**, the amount of pixels from the bottom side of the window to the top side, excluding window decorations.

**Window decorations** are window features that are managed by the OS instead of by the application, such as borders, menus, etc. You may find different terminology used depending on the topic being discussed.

When a player first launches a game, the window's geometry is set to whatever the game developer chose as a reasonable default. The Width and Height components in the *game.project* file's Display section determine the window's initial size:

![display_resolution](https://github.com/klaytonkowalski/tutorial-defold/window_geometry/assets/images/display_resolution.png)

One of the first things a player will do is resize the window to better fit their preferences. This new window geometry should be saved and recalled by the application upon each successive launch. Imagine if every time you loaded your favorite game, a little window popped up in the top-left corner of your screen, requiring you to re-adjust it! That would be annoying.

## Prerequisites

This tutorial uses the following libraries, however alternatives may be available if you would like to use them instead:

* [Defold Persist](https://github.com/klaytonkowalski/defold-persist), which will help us save and load window geometry data.
* [DefOS](https://github.com/subsoap/defos), which will allow us access to OS-specific functions that the standard Defold API does not provide.

Only library features that are directly relevant to this tutorial will be explained.

We need to save the following window properties to a *settings* file:

```
local function create_save_files()
    -- Define default settings.
    local settings =
    {
        fullscreen = sys.get_config_string("display.fullscreen", false),
        maximized = false,
        window_x = 0,
        window_y = 0,
        view_width = sys.get_config_int("display.width", 960),
        view_height = sys.get_config_int("display.height", 540),
    }
    -- If a settings file does not already exist, then create it.
    persist.create("settings", settings)
end
```

We save `window_x` and `window_y`, but not `window_width` and `window_height`. This is because we don't care about the size of whatever window decorations are added by the OS. Instead, we save `view_width` and `view_height` because they are specific to the camera's viewport, which is relevant to how our graphics are displayed.

There are three geometry states that a window can be in, ordered by precedence:

1) Fullscreen
2) Maximized
3) Windowed

If the window should be in fullscreen mode, then there is no need to check if it's maximized. If the window is maximized, then there is no need to read its exact screen position.

The window always launches based on settings in the *game.project* file, so we need to apply the correct window geometry on startup:

```
local function load_window_settings()
    -- Load the settings file.
    local settings = persist.load("settings")
    -- Apply window geometry by precedence.
    if settings.fullscreen then
        defos.set_fullscreen(true)
    elseif settings.maximized then
        defos.set_maximized(true)
    else
        defos.set_window_size(settings.window_x, settings.window_y, )
        defos.set_view_size(settings.window_x, settings.window_y, settings.window_width, settings.window_height)
    end
end
```

Note that we use `defos.set_view_size()` instead of `defos.set_window_size()`. 

At this point, the default settings will always be applied since we have not yet saved any changes made by the player. These settings should be saved on application shutdown:

```
local function save_window_settings()

end
```