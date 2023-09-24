# Resonite Standalone Setup

How to have steam hour logging, mods, and NCR simultaneously

## Explanation

If you don't care about why we're doing things and just want the steps, skip to the [setup](#setup) section.

First, lets about how Resonite is shipped. Both the Steam and standalone versions come with a launcher. This is called ResoniteLauncher.exe. This is *only* a helper program that launches Resonite with extra [command line arguments](https://wiki.Resonite.com/Command_Line_Arguments). You do not need to launch Resonite via ResoniteLauncher.exe if you have a different way of passing in arguments, for example via Steam's launch options. Next, Resonite standalone also comes with a special ResoniteProLauncher.exe. This application updates the standalone install when opened and can launch Resonite, but notably **it cannot pass command line arguments**. This means while we need it for updating, we cannot launch modded Resonite using it.

Finally, lets talk Steam integration. As long as Steam is running *before* Resonite starts, Resonite will hook up with Steam. This enables Steam playtime logging and Steam networking. You do not actually need to launch via Steam, however launching via Steam will guarantee you don't forget to open your Steam client.

So to summarize:

Resonite.exe is the actual game. We need to pass special command line arguments to this. There are three commonly used launchers:

| Launcher            | Can update Resonite?        | Can pass command line arguments? |
| ------------------- | ----------------------- | -------------------------------- |
| ResoniteLauncher.exe    | No                      | Yes                              |
| Steam               | Steam version only      | Yes                              |
| ResoniteProLauncher.exe | Standalone version only | No                               |

Therefore, my suggestion is you use ResoniteProLauncher to keep your install updated and a non-Steam game shortcut within steam to actually launch Resonite.

## Setup

1. Grab and run [ResonitePublicSetup.exe](https://assets.Resonite.com/install/ResonitePublicSetup.exe)
2. Install it wherever you want to. The default `C:\Resonite` location is fine if you don't have multiple drives to worry about. If you do, check the [drive notes](directories.md/#drive-notes). **Do not merge your standalone install into your Steam install!** While merging installs can technically work it can easily create more problems than it solves.  
   ![ResonitePublicSetup.exe screenshot](img/ResonitePublicSetup.png)
3. Go to the directory you installed to and run `ResoniteProLauncher.exe`. Wait for it to finish patching. **Do not use either of the launch buttons!** We're only using this to update Resonite, not launch it.  
   ![ResoniteProLauncher.exe screenshot](img/ResoniteProLauncher.png)
4. Once the pro launcher says it's ready, simply exit it without launching.
5. Observe that an `app` directory has been created next to `ResoniteProLauncher.exe`. So, for example, it may be in `C:\Resonite\app`. This `app` directory contains the standalone Resonite install, and contains the `ResoniteLauncher.exe` and `Resonite.exe` that you are familiar with from the Steam install.
6. Go to steam and add a non-steam game.  
   ![add non-steam game screenshot](img/add_non_steam_game.png)
7. Hit the "browse" button, and go to `C:\Resonite\app\Resonite.exe` (or wherever is applicable given your install directory)
8. Hit the "add selected programs" button.
9. Right click the newly added game in your library, and go to "properties"  
   ![right click properties screenshot](img/non_steam_game_properties_1.png)
10. Configure the non-steam game with the same launch options you would use on the Steam version (`-LoadAssembly Libraries\ResoniteModLoader.dll`). Optionally, give it a more descriptive name and check the "Include in VR Library" checkbox.  
    ![non steam game properties screenshot](img/non_steam_game_properties_2.png)  
11. Install ResoniteModLoader into the standalone version [as you normally would for the steam version](../README.md#installation), but using your `C:\Resonite\app` directory instead.
12. Launch Resonite using your new non-steam game shortcut. Steam will track playtime on the Steam version of the game even though you are running the standalone version.

## Important Notes

- You will need to run `ResoniteProLauncher.exe` every time you want to update. You never need actually use its launch buttons, as it does not support launch options which are required for plugins/mods.
- The logs you're used to finding in `C:\Program Files (x86)\Steam\steamapps\common\Resonite\Logs` will go to `C:\Resonite\app\Logs` now.
