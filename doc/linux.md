# Linux Notes

ResoniteModLoader works on Linux, but in addition to the [normal install steps](../README.md#installation) there are some extra steps you need to take until Resonite issue [#2638](https://github.com/Resonite-Metaverse/ResonitePublic/issues/2638) is fixed.

The log directory on Linux is `$HOME/.local/share/Steam/steamapps/common/ResoniteVR/Logs`

If your log contains the following, you need to set up a workaround for the issue.

```log
System.IO.DirectoryNotFoundException: Could not find a part of the path "/home/myusername/.local/share/Steam/steamapps/common/ResoniteVR/Resonite_Data\Managed/FrooxEngine.dll".
```

To set up the workaround, run the following commands in your terminal:

```bash
cd "$HOME/.local/share/Steam/steamapps/common/ResoniteVR"
ln -s Resonite_Data/Managed 'Resonite_Data\Managed'
```
