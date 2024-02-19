# Tutorial: Window Geometry

## Introduction (1 of 5)

**Window geometry** refers to measurable physical properties of a window. In the context of this tutorial, those properties are:

* **Window X**, the distance from the left side of the screen to the left side of the window.
* **Window Y**, the distance from the top side of the screen to the top side of the window.
* **Window Width**, the amount of pixels from the left side of the window to the right side.
* **Window Height**, the amount of pixels from the bottom side of the window to the top side.
* **View Width**, the amount of pixels from the left side of the window to the right side, excluding window decorations.
* **View Height**, the amount of pixels from the bottom side of the window to the top side, excluding window decorations.

**Window decorations** are window features that are managed by the OS instead of by the application, such as borders, menus, etc. You may find different terminology used depending on the topic being discussed.

When a player first launches a game, the window geometry is set to whatever the game developer chose as a reasonable default in the Display section of the *game.project* file. One of the first things a player will do is resize the window to better fit their preferences. This updated window geometry should be saved and loaded by the application on each successive launch. Imagine if every time you loaded your favorite game, a little window popped up in the top-left corner of the screen, requiring you to re-adjust it! That would be annoying.

## Libraries (2 of 5)

This tutorial uses the following libraries, however alternatives may be available if you would like to use them instead:

* [Defold Persist](https://github.com/klaytonkowalski/defold-persist), which will help us save and load window geometry data.
* [DefOS](https://github.com/subsoap/defos), which will allow us access to OS-specific functions that the standard Defold API does not provide.

Only library features that are directly relevant to this tutorial will be explained.

## Saving Window Geometry (3 of 5)



## Loading Window Geometry (4 of 5)



## Demo (5 of 5)

