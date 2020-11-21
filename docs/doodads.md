# Doodads

Project:Doodle comes with several built-in doodads that you can use in your
levels. You may also [create your own](custom-doodads/index.md) custom doodads
and program them to do whatever you want!

---

## Flags

The **Start Flag** sets the player spawn point in your level. There should be
only one start flag.

The **Exit Flag** sets a goal point for the level. The player must touch this
flag to win the level.

---

## Trapdoors

Trapdoors come in four varieties: **Left**, **Right**, **Up**, or down (**Trapdoor**).

A trapdoor is a one-way passage. If the player or other mobile doodad touches
the door from the "correct" side, the door will swing open. After the mobile
doodad has passed, the door will swing shut again.

When the door is shut, you can not open it from the "wrong" side and it behaves
as a solid wall. If the door is open you may run in from the wrong side.

---

## Locked Doors & Keys

There are four pairs of **Colored Locked Doors and Keys** you can use on your level.

Colored doors are locked and behave as a solid wall until the player or another
mobile doodad "picks up" the Key of the same color. The doors may then be
permanently unlocked if the player walks into them while holding the key.

Should the player lose the keys later, previously opened doors will remain
unlocked but the player will need to find another key to open more doors.

Each key/door pair also has a distinct shape:

* **Red Key** (triangle)
* **Green Key** (cross)
* **Yellow Key** (star)
* **Blue Key** (diamond)

---

## Electric Door

The sci-fi electric door can only be opened when it receives a "power" signal
from a linked button or switch. See [Linked Doodads](#linked-doodads) below.

When the door receives a "power: on" signal it will open and allow passage to
the player or other mobile doodads. When it receives a "power: off" signal it
will close.

---

## Buttons

**Buttons** will emit a "power: on" signal to all doodads that they are
[Linked](#linking-doodads) to when the button is pressed by the player or
another mobile doodad.

When the button stops being pressed, it will emit a "power: off" signal to
all connected doodads, which will generally close electric doors.

Buttons come in three varieties:

* **Button:** a button with a grey arrow that pops back up when pressed.
* **Button Type B:** a variation without the grey arrow but behaves the same.
* **Sticky Button:** a button with a red arrow that stays pressed in once pressed.

When a sticky button is pressed, it emits a "power: on" signal once and stays
pressed in forever. If the Sticky Button itself _receives_ "power: on" from another
button, it will pop back up.

---

## Switches

**Switches** will emit a `power: on` signal to [linked](#linked-doodads) doodads
when touched by the player or other mobile doodad, and then a `power: off`
signal when touched again.

They come in various aesthetic flavors:

* Wall switch (left, right)
* Floor switch
* On/Off "background" wall switch facing the screen

---

## Crumbly Floor

The **Crumbly Floor** behaves as a solid ceiling when hit from below, and a
solid floor when walked on from above, but watch out! The floor will shake
and collapse after a moment beneath your feet.

The floor will respawn after a while and forbids passage from the underside
of the doodad.

---

## State Blocks

**State Blocks** are blue and orange squares that keep opposite state from one
another. In one state the block is solid, in another it is passable.

The ON/OFF block will toggle all state blocks on the level to their opposite
setting whenever it's touched by the player or other mobile doodad.

---

## Red Azulian (Test Mob)

The red Azulian is a test mobile character. Not really an enemy, as he doesn't
care about the player.

The Azulian's A.I. just has it run left and right until it meets resistance.
It can pick up keys, activate buttons and switches that it passes by, and can
unlock doors.

This mob will probably go away in future releases of the game and will remain
in the code as a hidden easter egg.
