# Tutorial: Window Geometry

This tutorial explains how to save, load, and apply window geometry.

Please click the ☆ button on GitHub if this repository is useful or interesting. Thank you!

## Introduction (1 of 5)

One of the first actions a player takes upon launching a game for the first time is toggle fullscreen, maximize, or resize the window to fit well on whatever screen dimensions they're using. These size-related properties are collectively known as **window geometry**.

Imagine if every time you launched your favorite game, it popped up in the top-left corner of the screen as a tiny 320 x 180 window. That would be annoying and might give you a reason not to play the game in the future, or even leave a negative review! We want to provide a fun experience to players as much as possible, so we should learn how to properly save, load, and apply window geometry.

## Libraries (2 of 5)

This tutorial uses the following libraries, however alternatives may be available:

* [Defold Persist](https://github.com/whiteboxdev/library-defold-persist), which will help us save and load window geometry.
* [DefOS](https://github.com/subsoap/defos), which will allow us access to OS-specific functions that the standard Defold API does not provide.

Only library features that are directly relevant to this tutorial will be explained.

## Loading Geometry (3 of 5)

The following terms are important to understand when thinking about window geometry:

* **Window X** is the amount of pixels from the left side of the screen to the left side of the window.
* **Window Y** is the amount of pixels from the top side of the screen to the top side of the window.
* **Window Width** is the amount of pixels from the left side of the window to the right side of the window.
* **Window Height** is the amount of pixels from the top side of the window to the bottom side of the window.
* **View Width** is the amount of pixels from the left side of the viewport to the right side of the viewport.
* **View Height** is the amount of pixels from the top side of the viewport to the bottom side of the viewport.

![geometry.png](https://github.com/whiteboxdev/tutorial-defold/blob/main/window_geometry/assets/images/geometry.png)

It is impossible to know what the player's window size will be because every OS attaches its own default **window decorations**, which include everything outside the viewport, such as the title bar, border, menu items, etc. The Width and Height components in the Display section of the *game.project* file only specify the viewport dimensions, which do not include window decorations. Because of this, the view width and view height properties do not need to be saved. They can be pinged by calling the standard `sys.get_config_int()` function or some other function if you're using a custom camera library.

Window geometry needs to be loaded on application startup, which translates the the top-level `init()` function:

```
function init()
    create_settings()
    load_settings()
end
```

First we create a *settings* file that contains default window geometry:

```
-- Creates a settings file that contains default window geometry.
-- If the file already exists, then this function does nothing.
local function create_settings()
    local window_x, window_y, window_width, window_height = defos.get_window_size()
    local default_settings =
    {
        fullscreen = sys.get_config_string("display.fullscreen", "0") == "1" and true or false,
        maximized = false,
        window_x = window_x,
        window_y = window_y,
        window_width = window_width,
        window_height = window_height
    }
    persist.create("settings", default_settings)
end
```

We don't want to write an inaccurate value to the *settings* file, so we need to ping every value that might have a different value than we expect. Fullscreen mode is unpredictable because it can be toggled by the developer in the *game.project* file. Window position is unpredictable because it is sometimes up to the OS where a window is spawned on the screen. Window size is unpredictable because we don't know what kind of window decorations the OS might attach.

Next we load whatever window geometry is stored in the *settings* file:

```
-- Loads window geometry from the settings file.
local function load_settings()
    local settings = persist.load("settings")
    if settings.fullscreen then
        defos.set_fullscreen(true)
    elseif settings.maximized then
        defos.set_maximized(true)
    else
        defos.set_window_size(settings.window_x, settings.window_y, settings.window_width, settings.window_height)
    end
end
```

The order of precedence is important here. For example, if the window should be in fullscreen mode, then maximized mode and windowed mode should not be applied, otherwise the window might flicker around the screen a few times or adopt incorrect geometry.

## Saving Geometry (4 of 5)

Window geometry doesn't have to be saved as soon as the window is resized, since the window state is not lost until the application is terminated. This kind of on-demand saving would also be more difficult to manage because the relevant code might be needed in several different files. Instead, we should only save window geometry in the top-level `final()` function:

```
function final()
    save_settings()
end
```

We then write the fullscreen mode, maximized state, window position, and window dimensions to the *settings* file and save the changes.

```
-- Saves window geometry to the settings file.
local function save_settings()
    persist.write("settings", "fullscreen", defos.is_fullscreen())
    persist.write("settings", "maximized", defos.is_maximized())
    local window_x, window_y, window_width, window_height = defos.get_window_size()
    persist.write("settings", "window_x", window_x)
    persist.write("settings", "window_y", window_y)
    persist.write("settings", "window_width", window_width)
    persist.write("settings", "window_height", window_height)
    persist.save("settings")
end
```

Now whenever the player launches the game, the window immediately re-shapes itself to match the most recent window geometry settings.

## Demo (5 of 5)

This tutorial includes a demo project. Since this demo includes libraries, remember to select the Fetch Libraries menu item in the Defold editor before launching.

All of the relevant code is located in the *demo/example/main.script* file. On application shutdown, window geometry is saved to disk. On application startup, window geometry is loaded and applied.

The following controls are available:

* `key_f11` toggles fullscreen mode.
* `key_esc` terminates the application. Use this to save window geometry while in fullscreen mode.
