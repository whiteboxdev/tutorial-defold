# Tutorial: Window Geometry

This tutorial explains how to save, load, and apply window geometry data.

Please click the ☆ button on GitHub if this repository is useful or interesting. Thank you!

## Introduction (1 of 5)



## Libraries (2 of 5)

This tutorial uses the following libraries, however alternatives may be available:

* [Defold Persist](https://github.com/klaytonkowalski/library-defold-persist), which will help us save and load window geometry data.
* [DefOS](https://github.com/subsoap/defos), which will allow us access to OS-specific functions that the standard Defold API does not provide.

Only library features that are directly relevant to this tutorial will be explained.

## Saving Window Geometry (3 of 5)



## Loading Window Geometry (4 of 5)



## Demo (5 of 5)

This tutorial includes a demo project. Since this demo includes libraries, remember to select the Fetch Libraries menu item in the Defold editor before launching.

All of the relevant code is located in the *demo/example/main.script* file. Moving and resizing the window changes window geometry. On application shutdown, window geometry data is saved to disk. On application startup, window geometry data is loaded and applied to the window.

The following controls are available:

* `key_f11` toggles fullscreen mode.
* `key_esc` terminates the application. Use this to save window geometry data while in fullscreen mode.
