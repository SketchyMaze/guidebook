# Drawing a Doodad In-Game

Project: Doodle has support for drawing your custom doodad sprites in-game,
although for now you may find it more comfortable to use an
[external image editor](edit-external.md) instead.

To start a new doodad, open the game and enter the level editor. Select the
"File -> New Doodad" menu at the top of the screen. You will be prompted for
the square dimensions of your doodad (i.e. `100` for a 100x100 sprite) and
you can begin editing.

![Screenshot of the Doodad editor](../images/doodad-editor.png)

## Layers

A key difference between Levels and Doodads are that Doodad drawings can have
multiple **layers**. For doodads these are used to store multiple frames of
animation or different states, such as an opened vs. closed door.

Clicking the **Lyr.** button on the left toolbar or the "Tools -> Layers"
menu will open the Layers window where you can switch your editor between
layers, add and rename them. Layers can be toggled by the doodad's
[JavaScript code](scripts.md) by index number or by name, so giving each layer
a descriptive name is useful.

Doodads saved in-game go in your [user config directory](../profile-directory.md)
on your system.

## Future Planned Features

Creating doodads in-game is intended to be a fully supported feature. The
following features are planned to be supported:

* Implement some features only available on the `doodad` tool using in-game
  UI, such as attaching JavaScripts to the doodad.
