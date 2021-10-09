# Doodad Program

The game ships with a command-line program called `doodad` which assists in
creating and managing custom doodads and levels.

The `doodad` tool can show and set details on .doodad and .level files used by
the game, create new doodads from PNG images and attach custom JavaScript source
to program behavior of doodads.

## Where to Find It

The `doodad` tool should be in the same place as the game executable.

On Windows, the program is called `doodad.exe` and comes in the zip file next
to the game executable, `doodle.exe`.

On Linux, it will typically be at `/opt/sketchymaze/doodad` if you installed
the game from a .rpm or .deb package, or else for Flatpak it's included within
the app bundle and invoked like so:

    $ flatpak run com.sketchymaze.Doodle doodad --help

On Mac OS, it is found inside the .app bundle; right-click the 'Sketchy Maze.app'
to find the option to browse inside the .app bundle.

## Usage

Run `doodad --help` to get usage information.

The program includes several sub-commands, such as `doodad convert`. Type a
subcommand and `--help` to get help on that command, for example:

```bash
doodad convert --help
```

---

## Examples

Here are some common scenarios and use cases for the doodad tool.

### Show Level or Doodad Information

Shows metadata and details about a level or doodad file.

```bash
# Usage:
doodad show [doodad or level filename]
```

Examples:

```bash
### About a doodad file
$ doodad show button.doodad
===== Doodad: button.doodad =====
Headers:
  File version: 1
  Game version: 0.7.0
  Doodad title: Button
        Author: Noah
        Locked: true
        Hidden: false
   Script size: 922 bytes

Palette:
  - Swatch name: Color<#000000+ff>
    Attributes:  solid
    Color:       #000000
  - Swatch name: Color<#666666+ff>
    Attributes:  none
    Color:       #666666
  - Swatch name: Color<#999999+ff>
    Attributes:  fire
    Color:       #999999

Layer 0: button1
Chunks:
  Pixels Per Chunk: 37^2
  Number Generated: 1
  Coordinate Range: (0,0) ... (36,36)
  World Dimensions: 36x36
  Use -chunks or -verbose to serialize Chunks

Layer 1: button2
Chunks:
  Pixels Per Chunk: 37^2
  Number Generated: 1
  Coordinate Range: (0,0) ... (36,36)
  World Dimensions: 36x36
  Use -chunks or -verbose to serialize Chunks

### About a level file
$ doodad show 'Tutorial 2.level'
===== Level: Tutorial 2.level =====
Headers:
  File version: 1
  Game version: 0.7.0
   Level title: Lesson 2: Keys & Doors
        Author: Noah P
      Password: 
        Locked: false

Palette:
  - Swatch name: rock
    Attributes:  solid
    Color:       #996600
  - Swatch name: grass
    Attributes:  solid
    Color:       #00ff00
  - Swatch name: stone
    Attributes:  solid
    Color:       #888888
  - Swatch name: water
    Attributes:  water
    Color:       #0099ff
  - Swatch name: spikes
    Attributes:  fire
    Color:       #ff0000
  - Swatch name: hot lava
    Attributes:  fire
    Color:       #ff3300

Level Settings:
  Page type: Bounded
   Max size: 2550x3300
  Wallpaper: legal.png

Attached Files:
  None

Actors:
  Level contains 35 actors
  Use -actors or -verbose to serialize Actors

Chunks:
  Pixels Per Chunk: 128^2
  Number Generated: 206
  Coordinate Range: (-128,0) ... (2559,3327)
  World Dimensions: 2687x3327
  Use -chunks or -verbose to serialize Chunks
```

---

### Attach and Export Doodad Scripts

Doodads are programmed [in JavaScript](custom-doodads/scripts.md) and the
script can be attached and read using the doodad program.

Usage:

```bash
# Set the doodad script from filename.js
doodad install-script filename.js custom.doodad

# View the script from a doodad file
doodad show --script custom.doodad
```

Example:

```javascript
$ doodad show --script key-blue.doodad
// key-blue.doodad.js
function main() {
	var color = Self.GetTag("color");
	var quantity = color === "small" ? 1 : 0;

	Events.OnCollide(function(e) {
		if (e.Settled) {
			Sound.Play("item-get.wav")
			e.Actor.AddItem(Self.Filename, quantity);
			Self.Destroy();
		}
	})
}
```

---

### Edit Level or Doodad Properties

The `doodad edit-level` and `doodad edit-doodad` commands can set certain
properties on these types of drawings.

Example:

```bash
$ doodad edit-level --title "My First Level" example.level
```

Available properties that can be modified are as follows:

* **edit-doodad**
    * `--title value`: set the doodad's title (display name).
    * `--author value`: set the author's name (default is your OS username).
    * `--tag value, -t value`: set a custom tag (key=value format) on your doodad.
    * `--hide, --unhide`: edit the Hidden attribute on a doodad. Hidden doodads
      don't appear in the Doodad Dropper window of the level editor.
    * `--lock, --unlock`: edit the Locked attribute on a doodad. Locked doodads
      can not be opened for editing in-game.
* **edit-level**
    * `--title value`: set the level's title.
    * `--author value`: set the author's name (default is your OS username).
    * `--password value`: set the password for the level (not currently used).
    * `--type value`: set the page type, one of: Bounded, Unbounded, NoNegativeSpace,
      Bordered.
        * Note: Bordered is not yet implemented, and behaves the same as Bounded.
    * `--max-size WxH`: set the page size for Bounded levels, like 2550x3300.
    * `--wallpaper name.png`: set the wallpaper image filename.
    * `--lock, --unlock`: edit the Locked attribute on a level. Locked levels
      can not be opened for editing in-game.
    * `--remove-actor <name or id>`: remove an actor from your level by its name
      or its UUID. For example: `--remove-actor trapdoor.doodad` removes every
      instance of trapdoor.doodad from the level geometry.

---

### Remove actors from your level

In case you inherit a level that needs custom doodads that you don't have, you'll
get errors in the level editor about the missing doodads. One way to remedy this
is to delete the offending doodads from the map, but you can't do this in the
editor because the doodads don't have a sprite in your map.

First, use the `doodad show --actors example.level` command and look for the
Actors segment of the result, e.g.:

```bash
$ doodad show --actors example.level
...
Actors:
  Level contains 16 actors
  List of Actors:
  -  Name: key-blue.doodad
     UUID: 15f09c12-5d00-4654-9725-8e1ba10004d7
       At: 362,1348
  -  Name: trapdoor-down.doodad
     UUID: 24f85095-d13c-42e2-9156-01cb4b84723c
       At: 897,398
  -  Name: crumbly-floor.doodad
     UUID: 9ba40fc2-acc7-4e6d-821a-f0248c2ad7e1
       At: 1243,1742
...
```

You can then delete these actors by either their Name (filename) or their UUID
value. Deleting by name means that all instances of that doodad will be removed
from the map.

```bash
$ doodad edit-level --remove-actor crumbly-floor.doodad example.level
```

---

### Convert To/From Images

The `doodad convert` command can turn PNG or BMP images into doodads or
level files, and vice versa!

```bash
# Usage:
doodad convert [options] <input files.png> <output file.doodad>
```

Only PNG or bitmap images are supported.

#### Creating a Doodad from PNG images

Suppose you have PNG images named "frame0.png" through "frame3.png" and want
to create a doodad from those images. This will convert them to the doodad
file "custom.doodad":

```bash
# Convert PNG images into a doodad.
doodad convert frame0.png frame1.png frame2.png frame3.png custom.doodad

# The same, but also attach custom tags with the doodad.
# The doodad script can check its tags and you can have one
# common script for multiple variations of a doodad, e.g.,
# all four of the built-in Colored Locked Doors share a script.
doodad convert --tag color=blue frame{0,1,2,3}.png custom.doodad

# Convert the doodad back into an image.
# NOTE: only the 1st frame (frame0) can be exported, currently.
doodad convert custom.doodad frame0.png
```

The order of the given PNG images will be the order of the doodad layers
created; the first image will be Layer 0, the second Layer 1, and so on.
The names of the image files will be the names of those layers, minus the
.png or .bmp file extension.

#### Creating a Level from a PNG image

A level file can be created _from_ a PNG image:

```bash
# Usage:
doodad convert [options] input.png output.level

# Set which color is 'transparent' (to show the level wallpaper behind)
doodad convert --key '#ffffff' input.png output.level
```

Some considerations about this feature:

* The Palette will be created from each **distinct** color value found in the
  original PNG image. The names of each color will be named after their hex
  color value, and no attributes are applied by default. You will need to
  edit the level palette to mark colors as solid, fire, water, etc.
* The `--key` option (default #ffffff, white) sets the background color; pixels
  of this color in the input PNG will be 'transparent' in the level data, showing
  the wallpaper image behind.

#### Convert a level to a PNG image

You can also convert a .level file into a PNG (or bitmap) image, creating a
sort of "large screenshot" encompassing the entire level geometry.

```bash
# Usage:
doodad convert my.level output.png
```

Some considerations about this feature:

* Doodads are **not** included in the output image; only the level geometry
  itself is.
* Brush Patterns are not applied in the output image; each color swatch in
  your level will represent as solid pixel colors in the output image.

The image created by this command _could_ be fed back in to re-create the
level from that image, albeit with lots of information lost in the process,
such as the names and properties of Palette swatches and all the doodad
placements.
