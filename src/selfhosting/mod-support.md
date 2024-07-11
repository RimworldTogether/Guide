# Mod Support

The first thing you probably want to configure are your mods. Currently, you can have enforced, optional or forbidden:

* **Enforced mods** will be required for connection. Otherwise, the client will not be able to connect
* **Optional mods** are not required for connection and the client will be able to connect without them
* **Forbidden mods** are banned, which means the client will not be able to connect with them. Useful for servers without any mods restriction.

You can find your mod files in your workshop folder, usually located in your steam folders under:

**Windows**: `C:\Program Files (x86)\Steam\steamapps\workshop\content\294100`

**Linux**: `~/.steam/steam/steamapps/workshop/content/294100`

You can use this [tool](https://github.com/Byte-Nova/Library/releases/latest) to rename your mods to their in game name instead of ids. <u>Make sure to use it on a copy of your mods, not you actual workshop folder.</u>

We heavily recommend to have any mods that affect the world map or factions to be **Enforced**, as they can break things otherwise.

You can find a list of our current incompatibility list [here](https://github.com/RimworldTogether/Rimworld-Together/blob/development/IncompatibilityList.md).

