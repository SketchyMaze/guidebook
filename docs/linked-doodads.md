# Linked Doodads

[Doodads](doodads.md) such as Buttons and Switches know how to open Electric Doors via a process known as **linking.** A pair of linked doodads are able to exchange messages with one another, to inform their linked partner about events happening to itself. For example, when the Button is pressed in by the player, it sends a "power" signal to a linked Electric Door to tell it to open. When the button is released, it tells the linked door that power is gone, and the door closes.

## How to Link Doodads

![Link Tool](images/link-tool.png)

In the [Level Editor](custom-levels/index.md), click on the <img src="images/sprites/actor-tool.png" width="16" height="16"> **Doodad Tool** and drag a two doodads into your level, such as a Button and an Electric Door.

Then, select the <img src="images/sprites/link-tool.png" width="16" height="16"> **Link Tool**. When you mouse over the actors, a pink border appears instead of the usual orange.

Click one of the doodads, and then the other doodad. The first doodad will have a solid pink background while awaiting your click on the doodad to link it to. A glowing pink line will now be drawn connecting the two doodads.

You may use the <img src="images/sprites/actor-tool.png" width="16" height="16"> **Doodad Tool** to move the doodads to other places on your level, and they'll remain linked.

Currently, the only way to _unlink_ two doodads is to delete one of them. With the **Doodad Tool**, right-click on a doodad to remove it from your level, or drag it back into the Doodads window.

## Doodad Interactions

This section describes how the built-in doodads interact with one another when they're linked, and some example use cases. Custom doodads made by users should follow similar patterns; check the [PubSub event types](custom-doodads/scripts.md#official-standard-pub-sub-messages) used by built-in doodads, or invent your own custom event types!

### Start Flag

Link it with **any one doodad** and the that doodad will be the player
character for this level.

It is considered an error to link more than one doodad to the Start Flag.
It is undefined behavior which doodad "wins" in that case.

Upon level start, all actors linked to the Start Flag are destroyed.

### Anvil

If the Anvil receives **power** from any linked Button or Switch, it will teleport
back to its original starting location on the level. With this, players can make
a "Reset Button" for puzzle levels.

### Box

If the Box receives **power** from any linked Button or Switch, it will teleport
back to its original starting location on the level. With this, players can make
a "Reset Button" for puzzle levels.

### Buttons

Quick reference:

* When linked with an **[Electric Door](#electric-door)**: opens the door while the button is pressed.
* When linked with a **[Sticky Button](#sticky-button)**: when the button emits power to a pressed Sticky Button, the sticky button pops back up.

Technical details on what PubSub messages the Buttons subscribe to:

* When a mobile doodad touches the button, it presses down and publishes a **power** (true) message. Most doodads interpret this to be a power source, and a true power will make [Electric Doors](#electric-door) open.
* When the button stops being collided with, it pops back up and publishes a **power** (false) message. This will generally make doodads power down and the Electric Door closes.
* When the button _receives_ a **sticky:down** (true) event from a linked [Sticky Button](#sticky-button), the Button too presses in and remains pressed in solidarity with the Sticky Button.

### Sticky Button

The Sticky Button is just like the Buttons except it stays pressed forever once touched by the player or other mobile doodad. It emits a **power** (true) button when pressed, as Buttons do, but does not pop back up again.

Sticky Button specific interactions:

* When linked to **[Buttons](#buttons)**: when the Sticky Button is pressed down it will also tell all linked buttons to press down as well, making them all behave as sticky buttons and remain pressed forever.
* When linked to other **Sticky Buttons**: the buttons will mutually exclusively pop each other's buttons back up when pressed, by emitting power signals to the linked buttons.

When a pressed Sticky Button receives a **power** (true) signal, it pops up back up. This means that if a Sticky Button is linked to another Sticky Button, they would mutually exclusively pop each other's buttons back up when pressed.

### Switches

The various on/off switches emit power signals to linked doodads.

* When linked to an **[Electric Door](#electric-door)**: it will always toggle the door's state, opening if it was closed and vice versa, regardless of the on/off value of the Switch.
* When linked to a **[Sticky Button](#sticky-button)**: if the Sticky Button were pressed in, it will pop back up.
* When linked to **[Buttons](#buttons)** or other **Switches**: the Switch will sync its power state to any linked power source that sends a new state.

The technicals:

* When the switch is touched by the player or a mobile doodad, it sends two PubSub messages to linked doodads:
    * **switch:toggle** (bool), sending the switch's own value (true="on", false="off")
    * **power** (bool), sending the switch's "on" vs. "off" state.
* The **power** signals work how you'd expect; they would open or close doors, pop up sticky buttons, etc.
* The **switch:toggle** though is emitted _just before_ power, so sensitive doodads can behave more smartly to the toggle nature of switches regardless of the on/off state. See [Electric Door](#electric-door).
* When the **power** signal is _received_ by the Switch, for example because a linked Button was pressed, the Switch will set its power state to match the sender's.

### Electric Door

The Electric Door can only be opened by being powered by a linked doodad, such as a Button or a Switch that emits a **power** event.

Quick reference:

* When linked with **[Buttons](#buttons)**: when the Button is pressed, the door opens; when the button is released, the door shuts.
* When linked with **[Sticky Buttons](#sticky-buttons)**: same as with Buttons.
* When linked with **[Switches](#switches)**: the Electric Door will always _toggle_ its state regardless of the power value from the switch. If the door is opened, it will close, and vice versa.

The technicals:

* When the Electric Door receives the **switch:toggle** signal from [Switches](#switches):
    * It will ignore the _immediately following_ **power** signal, as this will be sent from the Switch immediately following the toggle.
    * Instead of following the power signal as normal, the door will always just toggle its state to its opposite.
* When the Electric Door receives the **power** signal from any linked doodad.
    * If the sender is saying **power** (true), the door will open.
    * If the sender is saying **power** (false), the door will close.
    * If the sender is a Switch, this message is ignored in favor of the toggle behavior.

### Electric Trapdoor

The Electric Trapdoor is basically a horizontal version of the
[Electric Door](#electric-door).

* Opens when it receives power from a Button, closes when power is removed.
* Always toggles state when powered from a Switch regardless of the Switch's
  actual power status.

### Warp Doors

Warp Doors let the player fast travel across the map by sending them to a linked Warp Door.

Quick reference:

* When not linked to another door: the door will be "locked" and can not be opened by the player.
* When linked to one other **Warp Door** of any kind: the Player will open this Warp Door and come out the linked door.
* When linked to **multiple Warp Doors**, the Player will exit one of them non-deterministically (undefined behavior currently).
* When linked to a **Warp Door (Blue)** or **Warp Door (Orange)**: the player will always exit from the other door, even if the door is not active. This can be used to create one-way doors.

The technicals (useful if you want to create your own custom warp doors):

* When the player actives the door with the Space key or similar:
    * If there is no linked door, flash "This door is locked." on screen.
    * Animate: freeze the player, open the door, hide the player, shut the door.
    * Publish a **warp-door:incoming** (Actor) event to the linked door so it can receive the actor (player character).
    * Pre-emptively move the (hidden) player to the X,Y position of the linked door on the level.
* When a door receives the **warp-door:incoming** event from a linked door:
    * Unfreeze the incoming actor (the player character)
    * Animate: open the door, make the player visible, shut the door.
* Only the Player character can open warp doors, not other mobile doodads.
* Blue and Orange doors listen for the **broadcast:state-change** event from [State Buttons](#boolean-state-doodads).

### Boolean State Doodads

The blue/orange "ON/OFF" button that toggles boolean state doodads globally across your level.

**State Doodads do NOT need to be linked to one another.** They all automatically communicate with each other globally on your level. The following doodads are State Doodads and do **NOT** need to be linked:

* State Button (the ON/OFF block)
* Blue and Orange State Blocks
* Warp Door (Blue)
* Warp Door (Orange)

Toggling the State Button will globally swap the "solid/invisible" states of the blue and orange doodads to their opposite settings.

The technicals:

* All State Doodads are in the OFF / false state to begin with.
* When a **State Block** is hit by the player:
    * It toggles its current state (doodad-local), from OFF to ON.
    * It emits a broadcast PubSub message **broadcast:state-change** sending its new state.
* The **State Block** also subscribes to the **broadcast:state-change** channel. So, when a different State Block is toggled by the player, _all_ State Blocks toggle and keep in sync.
* The various State Doodads listen for the **broadcast:state-change** event and enable or disable themselves accordingly:
    * In the default OFF state, Blue doodads are active and Orange are inactive.
    * In the ON state, Orange doodads are active and Blue are inactive.
