# Profile Directory

Sketchy Maze stores your user-created levels and custom doodads in your operating system's profile directory for your account.

This will typically be found at the following locations based on your platform:

*   **Windows:** `%APPDATA%` or `C:\Users\%USER%\AppData\Roaming\doodle`
*   **Mac OS:** `$HOME/Library/Application Support/doodle`
*   **GNU/Linux:** `$XDG_CONFIG_HOME/doodle` or `$HOME/.config/doodle`
*   **Linux (Flatpak):** `$HOME/.var/app/com.sketchymaze.Doodle/config/doodle`

## Opening your Profile Directory

The in-game Settings window has a button that will open your Profile Directory in your operating system's default file browser. You will see the folders labeled "levels" and "doodads" which is where your custom created content will live.

To install custom content created by other players, copy them into these folders.

## Contents of your Profile Directory

There are a few interesting folders:

*   **levels:** This is where your user-created .level files are saved to. You can also copy levels made by other players into this folder.
*   **doodads:** This is where your custom doodads go. You can copy doodads made by other players here and use them in your own levels.
*   **levelpacks:** This is where you can place custom [Level Packs](custom-levels/levelpacks.md). You can create your own or download packs created by other players and place them in this folder.
*   **screenshots:** the Giant Screenshot feature of the level editor will place its generated pictures here.

And some interesting files:

*   **logfile.txt:** holds the log output from your last run of the game.
*   **settings.json:** holds your game settings and is unique to your installation of the game.
*   **savegame.json:** holds your progress through Story Mode level packs including high scores on the levels.
*   **license.key:** if you have [registered the game](register.md) this is a copy of your license key file.

Note: to back up your savegame.json you must also copy the settings.json file, or otherwise the game will not accept the savegame.json if the settings file doesn't "match" it.

## See Also

*   [Publishing Levels](custom-levels/publishing.md): embed custom doodads directly _inside_ your level for easy sharing with others!
