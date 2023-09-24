# Going Into Detail: How ResoniteModLoader Interfaces with Resonite

ResoniteModLoader interfaces with Resonite in two main places: the hook and the compatibility hash.

## The Hook

The hook is the point where Resonite "hooks" into ResoniteModLoader, allowing it to begin execution.

Typically, [plugins] use [components] to execute custom code. This is limiting as they can only begin execution once a world loads. This usually involves putting a plugin's component into your local home.

The ResoniteModLoader plugin uses a different mechanism. Instead of using a component, it uses a connector. Resonite loads connectors during its initial setup, at which point they can begin execution. As this system is independent of world loading it is more reliable. The connector implementation is in [ExecutionHook.cs](../ResoniteModLoader/ExecutionHook.cs).

This connector-based hook does not modify the Resonite application, and only uses public Resonite APIs.

## The Compatibility Hash

[Resonite plugins][plugin], when loaded, alter your client's compatibility hash from what a vanilla client would have. You cannot join a session unless your compatibility hash is an exact match with the host.

Plugins are intended to add new features such as [components] and [LogiX nodes][logix] to the [data model], and altering the data model breaks multiplayer compatibility. This is why the compatibility hash exists, and why plugins alter it.

ResoniteModLoader does not change the data model, therefore it is 100% network compatible with vanilla Resonite. Furthermore, mods cannot alter the [data model] because ResoniteModLoader does not perform important component post-processing.

In order to make a client using ResoniteModLoader compatible with vanilla Resonite the compatibility hash must be restored to its default value. [ResoniteVersionReset.cs](../ResoniteModLoader/ResoniteVersionReset.cs) does the following:

1. Finds the original protocol version number by scanning FrooxEngine for a particular integer. This involves interpreting the IL bytes as instructions, which could be considered disassembling.
2. Calculates the default compatibility hash using the integer we just found.
3. Restores version and compatibility hash fields to their original values via [reflection](https://docs.microsoft.com/en-us/dotnet/framework/reflection-and-codedom/reflection). This does not modify Resonite code, but it does change the values of fields that are not intended to be changed.

[plugin]: https://wiki.Resonite.com/Plugins
[plugins]: https://wiki.Resonite.com/Plugins
[component]: https://wiki.Resonite.com/Component
[components]: https://wiki.Resonite.com/Component
[logix]: https://wiki.Resonite.com/LogiX
[data model]: https://wiki.Resonite.com/Core_Concepts#Data_Model

## Other Minor Ways ResoniteModLoader Interfaces with Resonite

- Hooking into the splash screen to show mod loading progress
- Hooking into Resonite shutdown to trigger one last mod configuration save
