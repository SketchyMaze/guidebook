# Creating Custom Doodads

Sketchy Maze is designed to be modder friendly and provides tools to help
you create your own custom doodads to use in your levels.

You can draw the sprites for the doodad either in-game or using an external
image editor. Then, you can program their behavior using JavaScript to make them
"do" stuff in-game and interact with the player and other doodads.

* Drawing your Doodad's Sprites
    * [In-Game](edit-in-game.md)
    * [In an External Program](edit-external.md)
* Program its Behavior
    * [JavaScript](scripts.md)

## Naming Convention

It is strongly encouraged that you name your custom doodad files with a
prefix or something to _namespace_ them apart from the built-in doodads.

For example: if you made a custom doodad named "door-red.doodad" it would
actually conflict with the [built-in doodad](../doodads.md) of the same
name, and your custom doodad would take priority over the built-in one.

By prefixing your custom doodad filenames with your initials or name, you
will minimize the likelihood that your doodad conflicts with a built-in
from a future game release _or_ with custom doodads made by other players
than yourself.

Future versions of the game will likely prevent saving a new doodad with
the same filename of a built-in one.

## Profile Directory

Custom doodads and levels will go in your [Profile Directory](../profile-directory.md),
into folders named "doodads" and "levels" respectively.

To share your custom doodads with others, you can copy the `.doodad` files out
of your doodads folder. To install doodads made by others, place their `.doodad`
files into your doodads folder, and they will appear in-game to drag and drop
them into your level.

## doodad (Command Line Tool)

Your copy of the game should have shipped with a `doodad` command-line tool
bundled with it. On Windows it's called `doodad.exe` and should be in the same
folder as the game executable. On Mac OS, it is located inside the .app bundle.

The `doodad` tool provides a command-line interface to create and inspect
doodad and level files from the game. You'll need to use this tool, at the very
least, to attach a JavaScript to your doodad to make it "do" stuff in-game.

You can create a doodad from PNG images on disk, attach or view the JavaScript
source on them, and view/edit metadata.

```bash
# (the $ represents the shell prompt in a command-line terminal)

# See metadata about a doodad file.
$ doodad show /path/to/custom.doodad

# Create a new doodad based on PNG images on disk.
$ doodad convert frame0.png frame1.png frame2.png output.doodad

# Add and view a custom script attached to the doodad.
$ doodad install-script index.js custom.doodad
$ doodad show --script custom.doodad
```

More info on the [`doodad` program](../doodad-tool.md) here.
