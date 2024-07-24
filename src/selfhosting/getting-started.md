---

# Getting Started

This guide will help you set up your own server with our mod. Follow these steps to get started.

## Installation

1. **Download Server Files**  
   Visit our [GitHub Page](https://github.com/RimworldTogether/Rimworld-Together/releases/latest) and download the server files. Choose the version compatible with your operating system (Linux or Windows). Please note that macOS is not supported, and there are no plans to support it in the future.

## QuickStart

### Setting Up

1. **Create a Folder**  
   On your desktop, create a folder and extract the server files into it.

2. **Launch the Server**  
   Open the folder and run the server application. A terminal window will open, and the server should start automatically.

3. **Join the Server**  
   You can now join your server through the in-game menu. For detailed instructions on joining, refer to our client guide.

### World Creation

- **Initial Setup**  
   The first player to connect will be responsible for creating the world. It’s recommended to generate the world with the minimum enforced modlist to avoid potential issues.

## DLCs

DLC files are available on our [GitHub Page](https://github.com/RimworldTogether/Rimworld-Together) for server-side use only. Treat DLCs as mods.

## Configuration Files

### ActionValues File

- **Purpose**: Adjust the cost in silver for specific actions in the game, such as spying on other players.

### DifficultyValues File

- **Purpose**: Configure custom difficulty settings.
- **Key Settings**:
  - `UseCustomDifficulty`: Enable or disable custom difficulty.
  - `ThreatScale`: Intensity of threats.
  - `AllowBigThreats`, `AllowViolentQuests`, `AllowIntroThreats`: Control whether large threats, violent quests, and intro threats are allowed.
  - Additional settings for crop yield, research speed, and raid loot.

### EventValues File

- **Purpose**: Set the cost in silver for various in-game events, such as raids or infestations.

### Market File

- **Purpose**: Allows players to list and buy items globally. Requires a comms console to be used in-game.

### RoadValues File

- **Purpose**: Defines types of roads and their costs.
  - `AllowDirtPath`, `AllowStoneRoad`, `AllowAsphaltPath`: Specifies which roads are permitted.
  - `DirtPathCost`, `StoneRoadCost`, `AsphaltHighwayCost`: Sets the construction cost for each road type.

### ServerConfig File

- **Purpose**: Configure server settings.
  - `IP`: The server’s IP address (use `0.0.0.0` if unsure).
  - `Port`: Port used by the server (default is `25555`).
  - `MaxPlayers`: Maximum number of simultaneous players.
  - `MaxTimeoutInMS`: Time a client can be unresponsive before disconnection (in milliseconds).
  - `VerboseLogs`, `ExtremeVerboseLogs`, `DisplayChatInConsole`: Manage logging and chat display settings. Useful for debugging and troubleshooting.
  - `UseUPnP`: Enables or disables UPnP for automatic port forwarding.
  - `SyncLocalSave`: Synchronizes local saves with the server.
  - `AllowCustomScenarios`: Determines if custom scenarios are allowed.
  - `TemporalActivityProtection`, `TemporalEventProtection`, `TemporalAidProtection`: Configures protection against specific temporal activities, events, and aid.

### ServerValues File

- **Purpose**: Allows or disallows custom scenarios (e.g., crashlanded, mechanitor).

### SiteValues File

- **Purpose**: Adjusts costs and production values for various site types. More information can be found on our [Site Page](https://rimworldtogether.github.io/Guide/features/sites.html).

### Whitelist File

- **Purpose**: Configure player access using a whitelist. Add in-game usernames to the list.

### WorldConfig File

- **Purpose**: Stores world data. Avoid editing this file while the server is running.

## Troubleshooting

For additional help and information, join our [Discord Server](https://discord.gg/NCsArSaqBW).

---
