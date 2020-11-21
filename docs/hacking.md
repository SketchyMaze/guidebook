# Hacking

This page discusses some advanced features of the game.

I've always loved it when developers kept debugging features in their released
games, and playing around with those and figuring out what makes the game tick.
I purposely left some debug features in the game that you can play around with.

## Developer Console

Pressing the `Enter` key at any time will open the developer console at the
bottom of the screen (all gameplay logic is paused while the console is open).

In the console you can type anything from simple commands, to hidden cheat
codes, to JavaScript commands to operate on some of the game's internal code!

![Screenshot of the developer console](../images/shell.png)

Pressing `Enter` again without typing a command will close the console.

## Commands

At the white **&gt;** prompt you can type a command. Typing `help` will show
a listing of available commands; typing `help` and then a command name will
show further usage of that command. For example, `help echo`.

The answer to your command is "flashed" in blue text at the bottom of the
screen and the developer console is closed. Pressing `Enter` will re-open the
console and show the recent history, including the answer to your last command.

```
>help echo
Usage: echo <message>
Flash a message back to the console
```

The following commands are supported:

* `help`, `help <command>`

    Shows the list of commands, or further help on a specific command.

* `echo <message>`

    Flashes your custom message on the bottom of the screen.

* `alert <message>`

    Pop up an alert box modal with a custom message.

* `new`

    Go to the "New Drawing" screen.

* `save [filename]`

    Save the current drawing. If the drawing has not been saved
    before, a filename is required, including the `.level` or
    `.doodad` suffix.

* `edit <filename>`

    Open a file for editing. The filename is a path on disk relative
    to the game's working directory.

* `play <filename>`

    Open a file for playing. The filename is a path on disk relative
    to the game's working directory. A wrong filename will play a
    new, blank level where Boy just falls to the bottom of the map.

* `close`

    Close the current level being edited and return to the title screen.

* `quit`, `exit`

    Close the developer console (an empty command would also work).

## Cheat Codes

Typing these messages in the console will toggle various mundane cheat
codes within the game:

* `unleash the beast`

    Do not cap the frames per second target of 60, allowing the game
    to run as fast as it's capable of. May or may not actually work.

* `don't edit and drive`

    While playing a level, this makes the level canvas editable and
    you can draw new pixels by clicking. Note that drawn pixels do not
    "commit" to the level until you release the cursor.

* `scroll scroll scroll your boat`

    While playing a level, this allows scrolling the level with arrow
    keys as if you're editing it. The camera still keeps the player
    character in view.

* `import antigravity`

    While playing a level, this turns off gravity for the player
    character. In this state the arrow keys can freely move the
    character in any direction. [Relevant xkcd](https://xkcd.com/353/)

* `ghost mode`

    Disable collision detection for the player character. This
    will also enable antigravity, otherwise you would fall to the
    bottom of the level.

* `give all keys`

    Gives all four colored keys to the player.

* `drop all items`

    Removes all keys and items from the player's inventory.

## JavaScript Shell

The developer console also features a JavaScript shell, which exposes
many of the game's internal data types and functions that can be
useful when debugging the game, or just fun to see what you can
break within the game!

In the developer console, the `eval` or `$` command will run a single
line of JavaScript code.

```
>$ 2 + 2
4
>$ d.Flash("This is %s", d.Title())
This is Project: Doodle v0.4.0-alpha
```

The following native objects are exposed to the JavaScript shell:

* `d` is the master game object.
* `function RGBA(red, green, blue, alpha uint8)` creates a native
  Color type, each value is range 0 to 255
* `function Point(x, y int)` creates a native Point type.
* `function Rect(x, y, w, h int)` creates a native Rect type.
* `function Tree(ui.Widget)` prints a tree of UI widgets drawn on the
  screen -- if you can find the widgets somewhere under `d`

```
>$ RGBA(255, 153, 0, 230).String()
Color<#ff9900+e6>
>$ Object.keys(d)
Debug,Engine,Scene,ConfirmExit,DrawCollisionBox,DrawDebugOverlay,...
>$ typeof(d.Debug)
boolean
>$ typeof(d.Flash)
function
>$ d.Flash("Flash a custom message, like the `echo` command")
undefined
>$ d.EditDrawing("filename.level")
```

It helps if you run Project: Doodle itself from a command line terminal,
so you can see its developer console output also on your terminal
window. Using `Object.keys(d)` will show all the exported functions and
variables from the internal game state.

Understanding that my game's [rendering engine](https://git.kirsle.net/go/render) and
[user interface toolkit](https://git.kirsle.net/go/ui) are open source projects
you can have fun reconfiguring widgets to change colors or whatever.

![Screenshot of the JavaScript REPL](../images/jsrepl.png)
