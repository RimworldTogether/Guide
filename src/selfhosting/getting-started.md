# Getting Started

We'll explain the process on how to setup your own server in this page.

To get started, you can start by installing the [server files](https://github.com/RimworldTogether/Rimworld-Together/releases/latest). Choose the appropriate version for your operation system, either Linux or Windows. We do not currently offer support for mac OS nor do we plan to in the future.

## QuickStart
This guide will assume you're running a windows installation.

Start by creating a folder on your desktop and extracting the server inside of it, you should now be able to launch the server.

A bunch of files should have been created, and a terminal window should have popped up. From there, the server should be up and running. You can now join your server through the in game menu, please refer to our client guide for more information.

The first client to join the server will be responsible for creating the world. We heavily recommend only generating the world with the minimum enforced modlist (see how to add enforced mods bellow) to avoid any problems.

## Configuration
Most of the files can be ignored, the relevant ones are listed bellow.

### DLCs
You can find the DLC files on our [GitHub Page](https://github.com/RimworldTogether/Rimworld-Together) for server side use only.

DLCs are considered mods, so you can treat them as mods. 

### Mods

Full guide on mod support can be found on our [Mod Support Page](/selfhosting/mod-support.md).

### Core

#### ActionValues
You can change the cost in silver of actions in this file. Currently, you can use them on players.

#### DifficultyValues
You can set all the values for your enforced difficulty if are currently using that configuration. You can find all those stats under the custom storyteller option in RimWorld.

#### EventValues
You can change the cost in silver of events in this file. You can call those events on players.

#### ServerConfig
You can change various settings about the server, such as:

* The port being used by the server
* The IP being used by the server (when in doubt, leave it to `0.0.0.0`)
* The maximum amount of players connected at once.
* The maximum amount of time a client can go without responding the server in milliseconds. You can increase it if you have an unstable connection.
* Verboselogs gives you additional logging information on the server
* DisplayChatInconsole does just that, the in game chat will be displayed on the console.

#### ServerValues
You can change if you want to allow custom scenarios for clients (ex :crashlanded, mechanitor, naked brutality).

#### SiteValues
You can change the various cost and production of sites in this file. You can find out more information about sites on out [Site Page](/features/sites.md).

#### Whitelist
The file lets you configure if you want a whitelist or not. Simply put the in game usernames (the one used to log in) in between the brackets.

#### WorldConfig
This file stores all the world data. We do not recommend editing it mid run.

## Making your server public

Full guide on going public can be found on our [Going Public Page](./selfhosting/going-public.md).

## TroubleShooting

For additional help with troubleshooting, please join our [Discord Server](https://discord.gg/VFFSvfJTQD).

